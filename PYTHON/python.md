## Bắt đầu
Bạn cần cấu hình `Makefile`
### Makefile
``` python 
# Makefile
help:
	@echo "  start         run server"
	@echo "  install       install dependencies"
	@echo "  lint          check style with flake8"
	@echo "  autopep8      auto format code pep8 with autopep8"
	@echo "  test          run all test suites"
	@echo "  coverage      export test coverage report"
	@echo "  config        initialize config files"
	@echo "  upgrade       migrate database"

start:
	ENV_FOR_DYNACONF=development python run.py server

install:
	pip install -r requirements/dev.txt

lint:
	flake8 --exclude=migrations,venv --max-line-length=120 .

autopep8:
	autopep8 --in-place --recursive --exclude=migrations,venv --max-line-length=120 .

test:
	ENV_FOR_DYNACONF=testing python -m unittest -v

coverage:
	ENV_FOR_DYNACONF=testing coverage run -m unittest
	ENV_FOR_DYNACONF=testing coverage html --include='src/app/*' -d .framgia-ci-reports/coverage

upgrade:
	flask db upgrade --directory src/db/migrations

downgrade:
	flask db downgrade --directory src/db/migrations

config:
	cp src/configs/.secrets.toml.example src/configs/.secrets.toml
	cp .env.example .env

```

Cấu hình file `framgia-ci.yml`
``` yaml 
# framgia-ci.yml
project_type: python
build:
  general_test:
    image: python:3.7
    environment:
      FLASK_APP: src.setup
    services:
      redis:
        image: redis:alpine
      mailcatcher:
        image: schickling/mailcatcher
      mysql_test:
        image: mysql:5.7
        environment:
          MYSQL_DATABASE: mysql_test
          MYSQL_USER: root
          MYSQL_PASSWORD: root
          MYSQL_ROOT_PASSWORD: root
    prepare:
      - make config
      - make install
      - framgia-ci test-connect mysql_test 3306 60
test:
  unittest:
    command: make test
  pep8:
    command: make autopep8
  lint:
    command: make lint
  coverage:
    command: make coverage
deploy:
cache:

```


