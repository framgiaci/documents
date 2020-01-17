# Integrate Sun*CI into your Project

## framgia-ci.yml

To intergrate with `Sun*CI`, your project must contain `framgia-ci.yml` file. You can automatically create this file by using [Framgia CI CLI Tool](https://github.com/framgiaci/framgia-ci-cli) or make it by yourself with the file's content below: 

```yaml
project_type: php
build:
  general_test:
    image: framgiaciteam/laravel-workspace:latest
    services:
      mysql_test:
        image: mysql:5.7
        environment:
          MYSQL_DATABASE: homestead
          MYSQL_USER: homestead
          MYSQL_PASSWORD: secret
          MYSQL_ROOT_PASSWORD: root
      mogodb:
        image: mongo:latest
        environment:
          MONGO_INITDB_ROOT_USERNAME: root
          MONGO_INITDB_ROOT_PASSWORD: mogo_test
    prepare:
      - cp .env.civ3.example .env
      - php artisan config:clear
      - composer install
      - framgia-ci test-connect mysql_test 3306 60
      - php artisan migrate --database=mysql_test
      - framgia-ci run --logs
test:
  eslint:
    ignore: false
    command: eslint --format=checkstyle
      --output-file=.framgia-ci-reports/eslint.xml
      resources/assets/js/ --ext .js
  phpcpd:
    ignore: true
    command: phpcpd --log-pmd=.framgia-ci-reports/phpcpd.xml app
  phpmd:
    ignore: true
    command: phpmd app xml cleancode,codesize,controversial,design,naming,unusedcode --reportfile .framgia-ci-reports/phpmd.xml
  pdepend:
    ignore: true
    command: pdepend --summary-xml=.framgia-ci-reports/pdepend.xml
      --jdepend-chart=.framgia-ci-reports/pdepend.svg
      --overview-pyramid=.framgia-ci-reports/pyramid.svg
      app
  phpmetrics:
    ignore: true
    command: phpmetrics --report-html=.framgia-ci-reports/metrics.html
      --report-xml=.framgia-ci-reports/metrics.xml
      app
  phpcs:
    ignore: false
    command: echo '' | phpcs --standard=Framgia --report-checkstyle=.framgia-ci-reports/phpcs.xml --ignore=app/Supports/* app
  phpunit:
    ignore: false
    command:
      - phpdbg -qrr vendor/bin/phpunit -d memory_limit=1024M
        --coverage-clover=.framgia-ci-reports/coverage-clover.xml
        --coverage-html=.framgia-ci-reports/coverage
deploy:
  rocketeer:
    image: framgiaciteam/deployer:2.0
    when:
      branch: master
    run: php rocketeer.phar deploy --on=staging --no-interaction
cache:
  composer:
    folder: vendor
    file: composer.lock
  nodejs:
    folder: node_modules
    file: yarn.lock
```

1. `project_type` <br/>
   Your project's type. For example: `php`, `ruby`, `android`..v.v.
2. `build` <br/>
    This block contain command for running tools , test command.
    * `general_test`:name of this test block.It can be: `unit_test`, `feature_test`Etc.  Any keyword you want.
        + `image: framgiaciteam/laravel-workspace:latest` <br/>
        Image's name that you use to start a container and run all commands defined inside. In this example, it's `framgiaciteam/laravel-workspace:latest` developed by `Sun*CI` CI/CD team. 
        You can create your own docker image. Publish on Dockerhub and use it for your project.
         `image: framgiaciteam/my_docker_image:1.1.0`
            > By default, `Sun*CI` will add `latest` if you're not specify any tag.
        + `services` <br/>
        If your project needs to connect to database for running test (unit test, test migrate, test connnect DB etc.), you must specify those services on this block. In this example, Laravel need to connect to `mysql` database, so `services` block will be look like this: <br/>
            ```yml
                services:
                    mysql_test:
                        image: mysql:5.7
                        environment:
                        MYSQL_DATABASE: homestead
                        MYSQL_USER: homestead
                        MYSQL_PASSWORD: secret
                        MYSQL_ROOT_PASSWORD: root
            ```
             
            * `mysql_test` - service's name
            
            * `image` - docker image's name that you want to start service container.
            
            * `environment` - environment variables that container needs. You can find these environment variables on description of docker image.In this example is `mysql`: [https://hub.docker.com/_/mysql/](). 
            
             >Note : You better connect to services by its name ( quite similar with docker compose ).For example, if service's name is `mysql_test`, config database host name in .env file should be `DB_HOST=mysql_test`.
             
        + `prepare`: a set of commands that you want to run in your project. For example : <br/>
            ```
                - composer install
            ```

3. `deploy`
    This block define your plugins for do some jobs after running test.For example in some cases you want to auto deployment after succeeded build. Currently, `Sun*CI` supporting auto deployment with:
    * rocketeer
    * capistrano
    * docker (Automatically build project into a docker image, push to dockerhub then pull and update to your server
    
    Automatically deployment need a set of ssh keys ( public keys and private keys) for secure reasons to `ssh` to your server and do some specific jobs). So you must use one of images below:
    * framgiaciteam/deployer:2.0 - using for PHP project use rocketeer
    * framgiaciteam/rails-deployer:1.0 - using for Rails project use capistrano
    * docker:dind - This image allow you to use docker inside docker and totally isolated with host machine.
    * framgiaciteam/ssh-able:latest -  Allow you to ssh to other servers.

    #### -
    Beside using our default images, you can use some custom images. We provide some evironment variables for you to create your own one :
     ```
        PLUGIN_REPO_NAME=project_name,
        PLUGIN_REPO_FULLNAME=project_fullname,
        PLUGIN_REPO_OWNER=project_owner,
        PLUGIN_REPO_LINK=https_link_to_github_repo,
        PLUGIN_REPO_BRANCH=curent_build_branch,
        PLUGIN_COMMIT_SHA=commit_sha,
        PLUGIN_COMMIT_MESSAGE=commit_mesage
        PLUGIN_EVENT=github_event,
        PLUGIN_COMMIT_AUTHOR=author_commit,
        PLUGIN_COMMIT_AUTHOR_EMAIL=author_email,
        PLUGIN_BUILD_STATUS=build_status,
        PLUGIN_BUILD_NUMBER=build_nummer,
        PLUGIN_PULL_REQUEST_NUMBER=oull_request_number,
        PLUGIN_REPO_CLONE_URL=ssh_clone_url,
        PLUGIN_PROJECT_TYPE=type_of_project,
        PLUGIN_REPO_DEFAULT_BRANCH=repo_default_branch,
        PLUGIN_ESLINT=eslint_test_result,
        PLUGIN_PDEPEND=pdepend_test_result,
        PLUGIN_PHPCPD=phpcpd_test_result,
        PLUGIN_PHPCS=phpcs_test_result,
        PLUGIN_PHPMD=phpmd_test_result,
        PLUGIN_PHPMETRICS=phpmetrics_test_result,
        PLUGIN_PHPUNIT=phpunit_test_result,
     ```
    Below is a simple example about the way you can use these environment variables in your plugin.
    https://github.com/framgiaci/chatwork-deploy-only-plugin/blob/master/commands.sh
    ( This plugin send message result of automatically deployment to chatwork box )
    
    This's a example of using auto deployment with roketeer:
    ```yml
    rocketeer:
    image: framgiaciteam/autodeploy:latest
    when:
        branch: master
    run: php rocketeer.phar deploy --on=staging --no-interaction
    ```
    + `rocketeer`: Plugin's name
    + `image`: Plugin's image
    +  `when`: Define when will your plugin start running<br/>
        + `branch:master`: run in branch master
        + `status:success`: run when build succeeded.
    + `run`: Your deploy's commands

    Another example of using auto deployment with Docker:
    ```
        dockerd:
            image: docker:dind
            when : 
            status: [success, failed]
            commands:
            - docker login -u=$$docker_username -p=$$docker_password
            - docker build -t $$release_image /workdir
            - docker push $$release_image
        ssh:
            image: framgiaciteam/ssh-able:latest
            when : 
            status: [success, failed]
            commands:
            - ssh -T $$user@$$serverIP docker pull $$release_image
    ```
    First of all, we use image `docker:dind` to build and push image to dockerhub. Then use `framgiaciteam/ssh-able:latest` to `ssh` to server and run `docker pull image`.
4. `Cache`: <br/>
    `Sun*CI` only run cache when your cached folder changed. For example:
    ```yml
        cache:
        git:
            folder: .git
        composer:
            folder: vendor
            file: composer.lock
        nodejs:
            folder: node_modules
            file: yarn.lock
    ```
    This is Laravel project's example. `vendor` and `node_modules` only change when you add or remove some packages. All those change has been writen in `composer.lock` file and `yarn.lock` file. `Sun*CI` decide to save cache folder base on the changes of these file.
    
* `test` For more detail, take a look at [Framgia CI CLI Tool Documents](https://github.com/framgiaci/framgia-ci-cli)

## Notes when you active and setting projects

### Active project and sync members
- To `active` a project,access `Available repository` page -> `Fetch repositories` -> Click on repository need to active -> `Create Hook`.

        > Note: The Project only be actived by admin of repository.

- After actived project. Access `Detail` page -> `setting` and click `Re - SYNC REPO'S MEMBER` incase other members not able to see build's information.

### Receive notification about build's results.
- By default, when you active a project, `Sun*CI` chatbot will be added to your project. But if you want to get notification from this chatbot, you need to turn on  chatwork notification. Click on `Detail` -> `NOTIFICATION` -> turn on `CHATWORK NOTIFICATION` and add chatwork chatroom ID to `Room Lists`.
- You can also create your own chat bot by add your chatbot key to `Chatwork Bot`
- Receive build's result via email: add email address to `Email Lists` in `SETTING -> NOTIFICATION`
- Receive notification via slack: To get notification via slack, you need to turn on slack's notification and add all necessary information to `Slack Notification`.

### Public key
- `Sun*CI` provide a public key. You can use this key to add to your server (you must use this key if you want to use auto deployment services). Click on `Detail` -> `SETTING` -> `SECRET`. Copy to use this key.

## Notes when intergrate with Ruby projects.
### Gems Cache
- Ruby projects usually install gems in `/usr/local/bin/gems` and share this folder with other projects in host machine. On the other hand,`Sun*CI` run builds in totally isolated environment, each build in a project run in different environment with the other, that why you can not keep that gem folder in containers. So when you intergrate a Ruby project with `Sun*CI`, please install gem in folder by specify `install path` like: 

        ```bash
            bundle install --path vendor/bundle
        ```
### Install rspec, brakeman, rake, reek...
- In some case, `bundle install` successfully but rspec still `not found`, you need to install directly gems into Dockerfile like this:
```
FROM ruby:2.4.0
RUN apt-get install -y libpq5 libpq-dev
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash -
RUN apt-get install -y nodejs
# Install eslint from https://www.npmjs.com/package/eslint
RUN npm install -g eslint
RUN eslint --version
RUN apt-get -y update && apt-get -y install ruby-full
RUN gem install rspec \
    scss_lint \
    brakeman \
    bundle-audit \
    reek \
    rails_best_practices \
    simplecov \
    robocop \
    rake
WORKDIR /
```

## Display Code coverage 
To display code coverage,you need to config block `test` in `framgia-ci.yml` and export output coverage file into `.framgia-ci-reports/coverage` folder, by this way, `Sun*CI` can read the coverage results.

### PHPUnit
```yaml
test:
  phpunit:
    ignore: false
    command:
      - phpdbg -qrr vendor/bin/phpunit -d memory_limit=1024M
        --coverage-clover=.framgia-ci-reports/coverage-clover.xml
        --coverage-html=.framgia-ci-reports/coverage
```

### Jest

```yaml
test:
  jest:
    ignore: false
    command:
      - yarn jest --coverage
```

In `jest.config.js` you need to config Jest's output html to `Sun*CI` folder:
```javascript 1.8

module.exports = {
  coverageDirectory: '.framgia-ci-reports/coverages/jest',
  coverageReporters: ['html'],
  collectCoverageFrom: [
    'src/**/*.{js,jsx}',
  ],
  coverageThreshold: {
    global: {
      statements: 70,
      branches: 70,
      functions: 70,
      lines: 70,
    },
  },
  moduleDirectories: ['node_modules', 'app'],
  moduleNameMapper: {
    '.*\\.(css|less|styl|scss|sass)$': '<rootDir>/internals/mocks/cssModule.js',
    '.*\\.(jpg|jpeg|png|svg|gif|eot|otf|webp|ttf|woff|woff2|mp4|webm|wav|mp3|m4a|aac|oga)$':
      '<rootDir>/tests/mocks/image.js',
  },
  setupFilesAfterEnv: [
    '<rootDir>/test/testing/test-bundler.js',
    'react-testing-library/cleanup-after-each',
  ],
  setupFiles: ['raf/polyfill'],
  testRegex: 'tests/.*\\.test\\.js$',
  snapshotSerializers: [],
}
```

