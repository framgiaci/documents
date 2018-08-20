# FramgiaCI official documents

## [English version]()  - Comming soon
## [Japanes version]() - Comming soon

## framgia-ci.yml: Một yêu cầu thiết lập quan trọng để có thể chạy framgia-ci là bạn phải có file cấu hình `framgia-ci.yml` trong project của bạn. Bạn có thể tạo một file bằng cách thủ công với nội dụng như dưới đây. Hoặc có thể [Framgia CI CLI Tool](https://github.com/framgiaci/framgia-ci-cli) và sử dụng tool này để tự động sinh file `framgia-ci.yml`. 

```yml
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
      - php -dzend_extension=xdebug.so vendor/bin/phpunit
        --coverage-clover=.framgia-ci-reports/coverage-clover.xml
        --coverage-html=.framgia-ci-reports/coverage
deploy:
  rocketeer:
    image: framgiaciteam/autodeploy:latest
    when:
      branch: master
    run: php rocketeer.phar deploy --on=staging --no-interaction
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

1. Key `project_type` <br/>
    Kiểu project của bạn. Ví dụ như `php`, `ruby`, `android`..v.v.
2. Key `build` <br/>
    Đây sẽ là nơi bạn chạy các tools, câu lệnh test cho project của bạn.
    * `general_test` Tên của phần chạy test này. Có thể là `unit_test`, `feature_test`, bất kỳ keyword nào tùy ý.
        + `image: framgiaciteam/laravel-workspace:latest` <br/>
        Tên `image` mà bạn sẽ sử dụng để start lên `containers` lên và chạy các câu lệnh test trong đó. Ở ví dụ trên là image `framgiaciteam/laravel-workspace:latest`. Bạn có thể tạo một docker image. Public nó lên Dockerhub và sử dụng nó cho project của bạn 
         `image: framgiaciteam/my_docker_image:1.1.0`
            > Vui lòng chỉ rõ image tag. Nếu không, framgia-ci mặc định sẽ sử dụng tag `latest` cho image của bạn
        + `services` <br/>
        Project của bạn cần kết nối đến database để thực hiện chạy test (unit test, test migrate, test connect db...) 
        Đây là nơi bạn cần định nghĩa các services cần thiết đó.
        Ờ file `framgia-ci.yml` mẫu trên. Ứng dụng Laravel cần test kết nối đến `mysql` database và chạy thử migrate. Nên chúng ta định nghĩa một service tên là `mysql_test`: <br/>
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
            Trong đó: <br/>
            * `mysql_test` là tên service
            * `image` - Tên docker image mà chúng ta sẽ start service container lên
            * `environment` - Là các biến mooit trương `ENV` mà chúng ta cần cho container. Các environments cần thiết này bạn có thể tìm thấy ở phần readme của các docker image. Ví dụ mysql image chẳng hạn: [https://hub.docker.com/_/mysql/](). Trường hợp bạn tự build image thì chắc bạn sẽ nắm rõ các biến môi trường này rồi.
                services:
        > Quan trọng ! Hãy thực hiện kết nối đến services thông qua tên services ! (Tương tự như khi bạn dùng docker compose vậy). Ví dụ tên service là `mysql_test` thì trong file .env phần config connect database bạn đổi database host name thành tên service `DB_HOST=mysql_test`
3. `deploy` Đây là nơi bạn sẽ định nghĩa các plugin của bạn.
    ```yml
    rocketeer:
    image: framgiaciteam/autodeploy:latest
    when:
        branch: master
    run: php rocketeer.phar deploy --on=staging --no-interaction
    ```
    + `rocketeer`: Tên plugin
    + `image`: Plugin image
    +  `when`: Định nghĩa khi nào pluing của bạn sẽ chạy<br/>
        + `branch:master`: Chạy khi build ở branch master
        + `status:success`: Chạy khi bản build `success`
    + `run`: Câu lệnh bạn sẽ chạy bên trong plugin container
4. `Cache`: <br/>
    FramgiaCI chỉ thực hiện cache khi folder bạn cần cache thực sự thay đổi. Ví dụ
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
    Ví dụ trên đây dành cho một project Laravel. Các folder như `vendor`, `node_modules` chỉ thay đổi khi bạn thêm hoặc bỏ bớt một hay nhiều packages ra khỏi project. Những thay đổi này được ghi lại ở các file tương ứng là `composer.lock` (compose install) và `yarn.lock` (yarn install). FramgiaCI quan sát sự thay đổi trên các file này để quyết định cache lại folder bằng phiên bản mới hay không.
    Trường hợp bạn muốn cache folder mà không có file tương ứng. Thì chỉ chỉ đơn giản là điền folder thôi và không cần file nữa:
    ```yml
        git:
            folder: .git
    ``` 
* `test` Tham khảo tại [Framgia CI CLI Tool Documents](https://github.com/framgiaci/framgia-ci-cli)