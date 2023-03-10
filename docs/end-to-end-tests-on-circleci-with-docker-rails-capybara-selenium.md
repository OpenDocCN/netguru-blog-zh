# 使用 Docker - Rails、Capybara 和 Selenium 对 CircleCI 进行端到端测试

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/end-to-end-tests-on-circleci-with-docker-rails-capybara-selenium>

 web 开发世界变化很快。我觉得我们正通过将后端部分提取到一个 API 中来将整体 web 应用程序一分为二。尽管这个解决方案有很多优点，但它也带来了一些困难。其中之一是端到端测试。

以适当的方式执行这些测试可能真的很棘手，尤其是当我们对外部主机上的过程没有太多控制时，例如在 CircleCI 构建期间。

我决定与你分享我所学到的——如何在 CircleCI 上设置端到端测试。下面的说明可以很容易地应用于除 Rails 和 React 之外的任何框架组合。然而，当前形式的文章是这样引用配置的:

*   两个独立的后端(Rails)和前端(React——类型并不重要)应用在独立的 repos 上

*   Rails 中的测试由 RSpec 使用 Capybara + Selenium + ChromeDriver 执行

*   CircleCI 配置假定包含 Docker 和 docker-compose

*   使用当前 CircleCI 设置和用于筹备和生产的文档化应用程序，在现有项目上执行设置流程

我想写一篇简单的文章，但我会尽可能多地涵盖在设置过程中可能出现的“如果”、“等等，会是什么”等问题。showed 文件中的大多数行对于这个设置来说都是必不可少的，并给出了注释和解释。这里的大图是允许在后端编写集成测试(位于规范/特性中)，在没有 Docker 的情况下在本地测试它们，并在 CircleCI 上的前端应用构建期间运行。

## **本地测试-后端**

### **环境设置**

确保给`Gemfile`添加合适的宝石:

```
...
group :test do
	...
	gem "capybara", "~> 2.7", ">= 2.7.1"
	gem "chromedriver-helper", "~> 1.0"
	gem "rspec_junit_formatter"  # Preparing proper output for CircleCI test metadata
	gem "selenium-webdriver", "~> 2.53", ">= 2.53.4"
end 
```

然后在`bundle install`之后，让我们在两个文件中添加水豚和 Selenium 的配置:

```
require "capybara/rails"
require "selenium-webdriver"

...

Capybara.register_driver :chrome do |app|
  Capybara::Selenium::Driver.new(
    app,
    browser: :chrome,
    desired_capabilities: { "chromeOptions" => { "args" => %w[window-size=1024,768] } },
  )
end

Capybara.register_driver :selenium do |app|
  Capybara::Selenium::Driver.new(app, browser: :chrome)
end

Capybara.configure do |config|
  config.default_max_wait_time = 10
  config.default_driver        = :selenium
end

Capybara.app_host = "http://localhost:3000"
Capybara.javascript_driver = :chrome
Capybara.server_port = 5001 # We don't want it to collide with standard rails server on port 5000
Capybara.server_host = "0.0.0.0" # Start server on localhost as meta-address
Capybara.server = :puma, { Silent: true } # Supress puma STDOUT in console

... 
```

```
require "capybara/rspec" 
```

因为我们的测试将放在:`spec/features`circle ci 上的测试套件将会失败。我们不想在后端的 CircleCI 上配置 e2e 测试。这就是为什么我们需要在`circle.yml`中覆盖测试命令:

```
machine:
  services:
    - redis
  environment:
    ES_JAVA_OPTS: "-Xms2g -Xmx2g"
    _JAVA_OPTIONS: "-Xms1024m -Xmx2048m"
    CONTINUOUS_INTEGRATION: true
dependencies:
  post:
    - if [[ ! -e elasticsearch-5.5.1 ]]; then wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.5.1.tar.gz && tar -xvf elasticsearch-5.5.1.tar.gz; fi
    - elasticsearch-5.5.1/bin/elasticsearch: { background: true }
test:
  post:
    - bundle exec codeclimate-test-reporter
    - bundle exec bundle-audit check --update
    - bundle exec brakeman -f plain
  override:                                                              # ADD this section
    - bundle exec rspec --exclude-pattern "spec/features/**/*.rb"        # We override Circle's test command here to explicitly exclude features specs
deployment:
  master:
    branch: master
    commands:
      - cd deployment; bundle install; bundle exec cap staging deploy 
```

## **本地测试-前端**

### **环境设置**

这个很简单——几乎不需要任何设置。

您唯一要做的事情就是将请求指向特定的后端地址(在我们的例子中是`rails_spec.rb`中所述的`http://localhost:5001`)。这是因为我们希望前端应用程序在测试期间使用与 rspec 相同的数据库——只有在这种情况下测试才是可行的。

这一步取决于您在项目中使用的技术。可以肯定地说，大多数 javascript 项目都有`package.json`文件。通常我们通过命令启动本地服务器:

`$ yarn start`

既然我们不想搞乱开发环境，我们可以在`package.json`中添加自定义的`start-test`命令:

```
"scripts": {
    "build": "react-scripts build",
    ...
    "start": "react-scripts start",
    "start-test": "REACT_APP_API_BASE_URL=http://localhost:5001 react-scripts start",
    ... 
```

我们的前端应用程序依赖于 env 变量`REACT_APP_API_BASE_URL`,该变量用于构建到 rails API 的引用链接。有了以上内容，我们现在可以使用:

`$ yarn start-test`

在你的项目中，你必须弄清楚如何设置它。可能性:

*   应用程序在某个时候使用的任何其他 env 变量——设置服务器启动的适当环境。:`APP_ENV=test`或`NODE_ENV=test`

*   设置。环境变量同上的 env 文件

*   API 相关服务/组件中的某种`if clause`

## **是活的吗？**

如果您已经正确地设置了所有的东西，那么您就可以在本地运行 e2e 测试了。简单的`is it working?`测试用例:

```
require 'rails_helper'

feature 'Home page', js: true do
  scenario 'visit home page' do
    visit '/'
    expect(current_path).to eq '/'
    expect(page.first('span').text).to eq("About Us")
  end
end... 
```

在我们启动前端的本地服务器后，如前一节所述，我们将通过以下方式手动运行规范:

`$ rspec spec/features`

现在让我们转到 **Crème de la crème** - CircleCI 设置！

## **CircleCI -先决条件**

让我们从确定您将处理的文件开始:

`Dockerfile`-docker 使用的文件，包含 docker 构建特定容器所需的所有信息和命令(容器就像一个小环境，有自己的依赖项、变量和配置)。现在看看你的文档。它没有任何扩展。**重要**检查你当前的`Dockerfile`是否有两个(或更多)`FROM`指令。如果是，那么这是一个 CircleCI 方面的问题。看，这被称为多阶段构建，这很棒(它使用一个图像，从它获取一些东西，从另一个图像添加一些东西，并构建一个图像，从这个图像你可以设置你的容器。在单阶段中，你必须从头开始构建单独的图像——它们会占用更多的空间和时间)，但 CircleCI 不是这样。多阶段构建需要 Docker v17.05 及以上版本(Circle 1.0 上的标准 Docker 要老很多)。我们可以强制使用那个版本，但是只能在 CircleCI v2.0 上。所以基本上，如果你有 CircleCI v1.0 和多阶段构建 docker file——你就有一个问题要解决。 [Dockerfile 参考](http://web.archive.org/web/20220924170825/https://docs.docker.com/engine/reference/builder/)

如果你有这样命名的文件，那么你可以使用 CircleCi v1.0。这个版本更容易设置。对于 v2.0，有一个`.circleci/config.yml`文件，它的结构是不同的。如果你愿意，你可以按照下面的步骤从 1.0 迁移到 2.0:[从 1 迁移到 2](http://web.archive.org/web/20220924170825/https://circleci.com/docs/2.0/migrating-from-1-2/) 但是我个人不推荐，除非你真的知道你在做什么。然而，使用 2.0 有很多优点——新版本的`Docker`和`docker-compose`支持像`exec`这样有用的命令。 [Circle.yml 参考](http://web.archive.org/web/20220924170825/https://circleci.com/docs/1.0/configuration/)

`docker-compose.yml` -由`docker-compose`工具使用的文件。您可以将该文件视为一个容器列表(通过引用 Dockerfile 或 Docker Hub 中的图像来指定),这些容器具有各自的配置。这是最重要的文件，你必须在其中放置你需要的安装程序的每个部分:rails，front，postgres，redis 等等。 [Docker-compose.yml 参考](http://web.archive.org/web/20220924170825/https://docs.docker.com/compose/compose-file/)。

## **CircleCI - Backend**

### **设置环境**

在我看来，这里最好的方法是为 rails 应用程序添加特定的环境，专门为 CircleCI 上的端到端测试定制。我选的名字:`e2e`。这很方便，实际上可以是任何名字。首先:

*   将`config/environments/test.rb`复制为`config/environments/e2e.rb`

*   在`Gemfile`中，将`:e2e`添加到为`:test`指定的每个组，以及组定义中。

*   如果您有秘密，那么您应该在`config/secrets.yml`中为`e2e`创建一个名称空间(从开发中复制定义)，并在加密密钥中添加适当的`secret_key_base`(在我们的项目中，我们通过`EDITOR=nano rails secrets:edit`编辑秘密)

*   请记住，在使用`Rails.env.test?`或类似设置/检查的地方，您可能也想指定在`e2e`环境下如何操作

*   如果您使用任何类型的请求模仿/抑制/模仿(如 [webmock](http://web.archive.org/web/20220924170825/https://github.com/bblimke/webmock) 或 [vcr](http://web.archive.org/web/20220924170825/https://github.com/vcr/vcr) )，您将需要将虚拟网络中的容器名称列入白名单，例如`frontend`、`elasticsearch`、`postgres`、`redis`等。

创建新文件`config/database.e2e.yml`:

```
e2e:
  adapter:  postgresql
  host:     postgres
  encoding: unicode
  database: your_project_test
  pool:     5
  username: postgres
  password: your_password 
```

创建`Dockerfile.e2e`(或者复制项目中的其他`Dockerfile`)。我们被安置在`docker/Dockerfile.e2e`。你的`Dockerfile`可能看起来像下面这个。您也可以使用 [Dockerfile reference](http://web.archive.org/web/20220924170825/https://docs.docker.com/engine/reference/builder/) 从头创建一个。

```
FROM quay.io/netguru/baseimage:0.10.1                                 # Image containing all dependecies like imagemagick etc

ENV RUBY_VERSION 2.4.2                                                # To get proper ruby

## Install Ruby & Dependencies
RUN \                                                                 # Install ruby
  apt-get update -q && \
  apt-get install -q libcurl3 && \
  apt-get install -q -y cron && \
  ruby-install --system --cleanup ruby $RUBY_VERSION -- --disable-install-rdoc && \
  gem install bundler

## Copy Gemfile & bundle
ADD Gemfile* $APP_HOME/                                               # Gemfile installation
RUN bundle install --jobs=8 --retry=3

## Add rest of code
ADD . $APP_HOME/                                                      # Copy to APP_HOME folder

ENV RAILS_ENV e2e                                                     # Set env to e2e
ENV WEB_CONCURRENCY 2                                                 # Puma concurrency
ENV AVAILABLE_MEMORY 1200                                             # How much memory the container can use
ENV RAILS_SERVE_STATIC_FILES true                                     # Serve static assets
ENV REDIS_URL redis://redis:6379/0                                    # Set Redis url and port

ADD ./config/database.e2e.yml /app/config/database.yml                # Copy database config
RUN mkdir /app/rspec_output                                           # Create directory for rspec results

RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*    # Cleaning up

# Elasticsearch
ENV ELASTICSEARCH_URL=http://elasticsearch:9200                       # Our case - setup ElasticSearch url and port

EXPOSE 5001                                                           # Container will listen on this port 
```

在本地将 e2e 特定的配置添加到`rails_helper.rb` ，ENV 是‘测试’，而在 CircleCI 上是‘e2e’。我们不想自动启动测试服务器。它将以不同的方式启动:

```
Capybara.register_driver :chrome do |app|
  Capybara::Selenium::Driver.new(
    app,
    browser: :chrome,
    desired_capabilities: { "chromeOptions" => { "args" => %w[window-size=1024,768] } },
  )
end

if Rails.env.test?
  Capybara.register_driver :selenium do |app|
    Capybara::Selenium::Driver.new(app, browser: :chrome)
  end

  Capybara.configure do |config|
    config.default_max_wait_time = 10
    config.default_driver        = :selenium
  end

  Capybara.app_host = "http://localhost:3000"
  Capybara.javascript_driver = :chrome
  Capybara.server_port = 5001
  Capybara.server_host = "0.0.0.0"
  Capybara.server = :puma, { Silent: true }
else
  args = ["--no-default-browser-check", "--start-maximized"]
  caps = Selenium::WebDriver::Remote::Capabilities.chrome("chromeOptions" => { "args" => args })
  Capybara.register_driver :selenium do |app|
    Capybara::Selenium::Driver.new(
      app,
      browser: :remote,
      url: "http://selenium:4444/wd/hub",
      desired_capabilities: caps,
    )
  end

  Capybara.configure do |config|
    config.default_max_wait_time = 15
    config.default_driver        = :selenium
  end

  Capybara.app_host = "http://frontend:3000"    # Containers communicate by aliases, hence 'frontend'
  Capybara.javascript_driver = :selenium
  Capybara.run_server = false                   # To ensure everything is in sync don't start puma automatically
end 
```

将启动 specs 所需的所有命令放在一个脚本中是一个非常好的主意。我们可以在`docker/e2e.sh`中创建一个。这个 bash 脚本将由一个`docker-compose`在容器中启动。

```
#!/bin/bash

echo "Setting up and running e2e tests!"

# Create database and seed (since we might want to include some data needed for our app to work properly like oauth clients
rails db:setup

# Start puma server in e2e environment on meta-address host on port 5001 in detached mode
RAILS_ENV=e2e puma -b tcp://0.0.0.0:5001 -d

# Start feature specs and output results to xml file
rspec --format progress --format RspecJunitFormatter --out ./rspec_output/rspec.xml spec/features 
```

就是这样！我们完成了后端。

## **CircleCI -前端**

### **设置环境**

在项目的主目录中创建新的`docker-compose.ci.yml`文件，或者复制并重命名当前文件。`docker-compose-staging.yml`。你需要设置一些东西:后端，卷，端口，别名等适当的依赖关系。一切都在下面解释。最后，该文件应该大致如下所示:

```
version: '2'                                       # Version of composer
services:
  backend:                                         # Backend container
    build:
      context: ~/backend                           # Backend should be pulled by git to this location
      dockerfile: docker/Dockerfile.e2e            # The location of Dockerfile, relative to context above
    depends_on:                                    # The containers below will be resolved and loaded before backed
      - redis
      - postgres
      - frontend
      - selenium
      - elasticsearch
    ports:
      - "5001:5001"                                # Expose host_port:container_port
    volumes:
      - "/rspec_output:/app/rspec_output"          # Mount  /rspec_output (on host) to /app/rspec_output (in container)
    environment:
      - RAILS_ENV=e2e                              # This env var will be available to any container instance - used to run rspec in e2e env
      - RAILS_MASTER_KEY                           # If no value is passed this var is going to be read from machine ENV - very useful. Just set RAILS_MASTER_KEY for secrets in CircleCI build config page
    networks:                                      # Virtual network for containers
      main:                                        # Name of the network
        aliases:
          - backend                                # Alias for reference purposes
    command: bash -c "bash /app/docker/e2e.sh"     # Start container by a bash script (container will be alive as long as this script is running)
  frontend:                                        # Frontend container
    build:
      context: .
      dockerfile: Dockerfile.e2e
    command: nginx
    logging:                                       # Disable spamming irrelevant messages from this container
      driver: none
    networks:
      main:
        aliases:
          - frontend                               # Container name in virtual network that can be addressed
    ports:
      - "3000:3000"
    volumes:                                       # Used to mount ./tmp on machine to /app/dist in container
      - "./tmp:/app/dist"
  selenium:                                        # Selenium container
    image: selenium/standalone-chrome
    ports:
      - "4444:4444"
    logging:                                       # Disable spamming irrelevant messages from this container
      driver: none
    networks:
      main:
        aliases:
          - selenium                               # Container name in virtual network that can be addressed
  elasticsearch:                                   # Elasticsearch container (not required) - we used it in project
    image: docker.elastic.co/elasticsearch/elasticsearch:5.5.1
    volumes:
      - "~/es_data:/usr/share/elasticsearch/data"
    environment:
      - "xpack.security.enabled=false"
      - "transport.host=localhost"                 # This line might be needed to disable errors with circleCI 1.0 container memory amount
      - "bootstrap.system_call_filter=false"       # This line might be needed to disable errors with circleCI 1.0 container memory amount
    ports:
      - "9300:9300"
    logging:                                       # Disable spamming irrelevant messages from this container
      driver: none
    networks:
      main:
        aliases:
          - elasticsearch                          # Container name in virtual network that can be addressed
  postgres:                                        # Postgres container
    image: postgres:9.5                            # Postgres version, installed from image on Docker Hub
    environment:
      - POSTGRES_PASSWORD=password                 # There can't be 'blank' password, use one specified in database.e2e.yml
    logging:                                       # Disable spamming irrelevant messages from this container
      driver: none
    networks:
      main:
        aliases:
          - postgres                               # Container name in virtual network that can be addressed
  redis:                                           # Redis container
    image: redis:4.0.6                             # Redis version, installed from image on Docker Hub
    logging:                                       # Disable spamming irrelevant messages from this container
      driver: none
    networks:
      main:
        aliases:
          - redis                                  # Container name in virtual network that can be addressed

networks:                                          # Set virtual network up
  main: 
```

如果您需要另一个服务(在容器中),只需以类似的方式将其添加到列表中，注意订单。

现在到前端`Dockerfile`。和以前一样——复制当前的`Dockerfile`或者在项目的根目录下创建一个。不幸的是，我们有多阶段建造`Dockerfile`和`CircleCI v1.0`，所以我不得不把多阶段转换成单阶段。我举个例子。起初我们有这个文件:

```
## specify node version
FROM quay.io/netguru/ng-node:6 as builder    # First 'FROM', as builder is used as reference below

## add necessary environments
ENV NODE_ENV staging                         # Set env var

## add code & build app
ADD . $APP_HOME                              # Copy / add external source to image's filesystem
RUN yarn install                             # Install all dependencies for frontend app
RUN yarn build                               # Compile frontend build

## Real app image
FROM nginx:alpine as app                     # Second 'FROM', now it's multi-stage build file

## Copy build/ folder to new image
COPY --from=builder /app/build /app/dist     # Copy file from one image to another by reference - we will have to get rid of it
COPY nginx.conf /etc/nginx/nginx.conf        # Copy nginx config to nginx directory

EXPOSE 3000                                  # Container will listen on this port at runtime 
```

现在它必须分成两个文件:

```
## Real app image
FROM nginx:alpine                       # Remove 'as app'

COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 3000 
```

```
## specify node version
FROM quay.io/netguru/ng-node:6                    # Remove 'as builder'

## add necessary environments
ENV NODE_ENV e2e                                  # Set env
ENV REACT_APP_API_BASE_URL http://backend:5001    # Set env var to point to backend within build. It can be also done by specific command in package.json

## add code & build app
ADD . $APP_HOME
RUN yarn install                                  # Install dependencies
RUN yarn build 
```

如果你仔细观察，你会发现我删除了`COPY --from=builder /app/build /app/dist`——这将以不同的方式完成。

上述变化将迫使两个新的步骤。在`circle.yml`中，我们必须在`pyenv rehash`后添加`dependencies/pre`:

```
 dependecies:
  pre:
    ...
    - pyenv rehash
    - docker build -t frontend_img -f Dockerfile.e2e.build .    # Builds image from Dockerfile.e2e.build (with our front app)
    - docker create --name frontend_pre frontend_img            # Creates container from image
    - docker cp frontend_pre:/app/build ./tmp                   # Copies /app/build directory (compiled app) from a container to ./tmp in CircleCI machine
    ... 
```

如果您的配置是:

*   `multi-stage` + `v1.0` -将多级转换为单级
*   确保你使用了正确的 Docker 版本和 CircleCI 的`Edge`版本
*   `single-stage` + `v2.0` -什么都不做，就用那个`Dockerfile`
*   `single-stage` + `v1.0` -什么都不做，就用那个`Dockerfile`

最后一步是配置`circle.yml`，设置命令的优先级，并指示 Docker 的用法。最后`circle.yml`应该是这样的:

```
machine:
  node:
    version: 8.2.1
  pre:
    - mkdir ~/.cache/yarn
    - curl -sSL https://s3.amazonaws.com/circle-downloads/install-circleci-docker.sh | bash -s -- 1.10.0          # Download Docker 1.10.0 (linked to docker-compose v2) for CircleCI
    - >-
      GIT_SSH_COMMAND='ssh -i ~/.ssh/backend-e2e-tests-deploy-key'
      git clone git@github.com:netguru/project.git ~/backend && cd ~/backend                   # Download backend to ~/backend. The SSH for backend had to be placed in CircleCI build setup page
    - set -o allexport; source "${HOME}/${CIRCLE_PROJECT_REPONAME}/.env"; set +o allexport
  environment:
    PATH: "${PATH}:${HOME}/${CIRCLE_PROJECT_REPONAME}/node_modules/.bin"
  services:
    - docker                                                    # Enable Docker for this build

dependencies:
  pre:
    - sudo mkdir /rspec_output && mkdir $CIRCLE_TEST_REPORTS/rspec && mkdir ~/es_data # make directories for mounting volumes and directory for test metadata. es_data is related to elasticsearch container
    - pip install docker-compose                                # Update docker-compose
    - pyenv rehash                                              # Refresh docker-compose link
    - docker build -t frontend_img -f Dockerfile.e2e.build .    # Build first part of former multi-stage Dockerfile
    - docker create --name frontend_pre frontend_img            # Create container from image above
    - docker cp frontend_pre:/app/build ./tmp                   # Copy compiled frontend app build from container to machine ./tmp dir
    - docker-compose -f docker-compose.ci.yml build             # Build all containers in docker-compose.ci.yml
  cache_directories:
    - ~/.cache/yarn
  override:
    - yarn install

test:
  override:
    - docker-compose -f docker-compose.ci.yml up --exit-code-from backend # Start containers from docker-compose. This will run in foreground and output exit code of bash script
    - ? case $CIRCLE_NODE_INDEX in                                        # Standard frontend tests
        0) yarn test --runInBand ;;
        1) yarn eslint && yarn stylelint ;; esac
      : parallel: true
    - cp /rspec_output/rspec.xml $CIRCLE_TEST_REPORTS/rspec/rspec.xml     # Copy rspec results to CircleCI test metadata folder

deployment:
  master:
    branch: master
    commands:
      - cd deployment; bundle install; bundle exec cap staging deploy 
```

就是这样，在这一点上，一切都应该是安全和健全的。如果没有，你的构建就不能让 ssh 循环运行。

## **有用的命令**

```
$ docker ps                                         # show running containers
$ docker ps -a                                      # show all containers
$ docker ps -n=-1                                   # show n last created containers
$ docker start                                      # start container
$ docker stop                                       # stop container
$ docker cp :                                       # copy file/directory from container to destinated dir in machine
$ docker cp  :                                      # copy file/directory from machine to container (don't get deceived, read 'understanding volumes' article)
$ docker images                                     # list all images
$ docker rm -f                                      # remove container
$ docker rmi                                        # remove image
$ docker-compose run  <command></command>                              # run a command in container specified in docker-compose file. Watch out - this command creates new containers each time. Moreover it doesnt create a network!
$ docker-compose exec  <command></command>                             # execute a command in container that is already running (unavailable in older versions of Docker (CircleCI v1.0))
$ docker-compose up                                 # start containers from docker-compose file by specified commands (creates networks). Containers are running as long as commands which run them 
```

## **有用的链接**

[docker-撰写运行参考](http://web.archive.org/web/20220924170825/https://docs.docker.com/compose/reference/run/)

[码头 cp 参考](http://web.archive.org/web/20220924170825/https://docs.docker.com/engine/reference/commandline/cp/)

[关于多阶段构建的文章](http://web.archive.org/web/20220924170825/https://engineering.busbud.com/2017/05/21/going-further-docker-multi-stage-builds/)

[另一篇关于多阶段构建的文章](http://web.archive.org/web/20220924170825/https://dzone.com/articles/multi-stage-docker-image-build-for-java-apps)

[另一篇关于多阶段构建的文章](http://web.archive.org/web/20220924170825/https://docs.docker.com/engine/userguide/eng-image/multistage-build/)

[了解容器中的体积](http://web.archive.org/web/20220924170825/http://container-solutions.com/understanding-volumes-docker/)

[在容器间复制数据](http://web.archive.org/web/20220924170825/https://medium.com/@gchudnov/copying-data-between-docker-containers-26890935da3f)

## **常见问题解答**

设置很难吗？

可能是吧。一切都取决于你所拥有的 CircleCi 版本，这限制了与 Docker(旧版本)的交互以及你的后端需要多少依赖容器。其中一些可能很难正确设置。

我应该考虑迁移到 CircleCI v2.0 吗？

肯定！这种构建需要很长时间来执行。有了新版本的 Circle，你可能会大大加快速度，并使用新的 Docker(更少的错误，一些命令实际上工作，而不仅仅是失败)。

**如果我有 CircleCI v2.0 文件怎么办？**

那你应该高兴才对。你在上面看到的一切仍然有效，只是你不必担心多阶段。只需遵循本指南[使用 CircleCI v2.0](http://web.archive.org/web/20220924170825/https://circleci.com/blog/multi-stage-docker-builds/) 构建多级 docker，并使用 2.0 样式 [CircleCI v2.0 参考](http://web.archive.org/web/20220924170825/https://circleci.com/docs/2.0/)重写我们的`circle.yml`附加行

## **总结**

我希望我已经设法描述了一切，使您能够在您的项目中自己设置端到端测试。

如果有什么不清楚的地方，请随时提问。也许文章不清楚或需要改进？也指出来。我相信我们学习的最好方式是通过一个错误，我们都可以从中受益。

后期正打算准备 CircleCI v2.0 的后续，敬请期待！