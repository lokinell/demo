# demo
This application was generated using JHipster 5.3.1, you can find documentation and help at [https://www.jhipster.tech/documentation-archive/v5.3.1](https://www.jhipster.tech/documentation-archive/v5.3.1).

## Development

To start your application in the dev profile, simply run:

    ./gradlew


For further instructions on how to develop with JHipster, have a look at [Using JHipster in development][].



## Building for production

To optimize the demo application for production, run:

    ./gradlew -Pprod clean bootWar

To ensure everything worked, run:

    java -jar build/libs/*.war


Refer to [Using JHipster in production][] for more details.

## Testing

To launch your application's tests, run:

    ./gradlew test

For more information, refer to the [Running tests page][].

### Code quality

Sonar is used to analyse code quality. You can start a local Sonar server (accessible on http://localhost:9001) with:

```
docker-compose -f src/main/docker/sonar.yml up -d
```

Then, run a Sonar analysis:

```
./gradlew -Pprod clean test sonarqube
```

For more information, refer to the [Code quality page][].

## Using Docker to simplify development (optional)

You can use Docker to improve your JHipster development experience. A number of docker-compose configuration are available in the [src/main/docker](src/main/docker) folder to launch required third party services.

For example, to start a mysql database in a docker container, run:

    docker-compose -f src/main/docker/mysql.yml up -d

To stop it and remove the container, run:

    docker-compose -f src/main/docker/mysql.yml down

You can also fully dockerize your application and all the services that it depends on.
To achieve this, first build a docker image of your app by running:

    ./gradlew bootWar -Pprod buildDocker

Then run:

    docker-compose -f src/main/docker/app.yml up -d

For more information refer to [Using Docker and Docker-Compose][], this page also contains information on the docker-compose sub-generator (`jhipster docker-compose`), which is able to generate docker configurations for one or several JHipster applications.

## Continuous Integration (optional)

To configure CI for your project, run the ci-cd sub-generator (`jhipster ci-cd`), this will let you generate configuration files for a number of Continuous Integration systems. Consult the [Setting up Continuous Integration][] page for more information.

[JHipster Homepage and latest documentation]: https://www.jhipster.tech
[JHipster 5.3.1 archive]: https://www.jhipster.tech/documentation-archive/v5.3.1

[Using JHipster in development]: https://www.jhipster.tech/documentation-archive/v5.3.1/development/
[Service Discovery and Configuration with the JHipster-Registry]: https://www.jhipster.tech/documentation-archive/v5.3.1/microservices-architecture/#jhipster-registry
[Using Docker and Docker-Compose]: https://www.jhipster.tech/documentation-archive/v5.3.1/docker-compose
[Using JHipster in production]: https://www.jhipster.tech/documentation-archive/v5.3.1/production/
[Running tests page]: https://www.jhipster.tech/documentation-archive/v5.3.1/running-tests/
[Code quality page]: https://www.jhipster.tech/documentation-archive/v5.3.1/code-quality/
[Setting up Continuous Integration]: https://www.jhipster.tech/documentation-archive/v5.3.1/setting-up-ci/


## 如何使用phabricator做code review
如何避免arc diff玷污现有节点
创建专用于评审的分支

git branch review
git checkout review
arc diff <xxx> 或 arc diff --preview <xxx> //创建评审单或预审单（到pha网站上进行下一步的操作，可用于ubuntu下不能自动补全人名的环境）
git checkout master
git branch -D review //评审单一旦创建，review分支就没有存在的必要性了
如何创建只包含部分文件的评审单
可能只希望评审方案文件（假设： design.md），但commit中包含相关的图片、svg、等文件，不需要提交到pha，如下处理：

git branch review <oneOldCommit> //从 design.md 创建或修改前的节点创建一个分支
git checkout review
git checkout master design.md //将master分支上的 design.md check 到 review 分支
git commit -am "review for design.md"
arc diff HEAD^ 或 arc diff --preview HEAD^
git checkout master
git branch -D review
