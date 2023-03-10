# CI/CD 你最好的朋友 CircleCI 是什么？

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/what-is-circleci>

 在一个敏捷的世界中，我们必须快速适应变化，但是对于有固定期限的工程团队来说，这可能是一个挑战。CircleCI 是一款顶级的 CI/CD 工具。

Automation 和 CircleCI 在这里登场——一款顶级 CI/CD 工具。下面，我们探索什么是 CircleCI，以及更多。

自动化有助于加快工作，其中很大一部分是 CI/CD。

第一部分，CI，代表持续集成。这个概念鼓励开发人员定期将他们正在处理的代码与共享存储库集成在一起——越频繁越好。当代码合并时，就会触发自动化测试和构建。

第二部分，CD，模棱两可。它代表持续交付或持续部署。两者都意味着先前集成过程的进一步自动化。连续交付是指在软件应用程序中自动创建图像。

这些映像存在于某种存储库中，可以手动部署。对于连续部署，该手动步骤也涵盖在自动化过程中。

我们所概述的可能看起来像许多不必要的自动化大惊小怪，但是这些实践是值得投资的。有了 CI/CD 平台，软件团队可以更快地得到关于 bug 的通知，有助于交付更稳定的产品，同时增加团队的信心和生产力。为了让您更好地理解，我们来看看 CircleCI 持续集成工具。

## 什么是 CircleCI？

CircleCI 是一个 CI/CD 解决方案，有助于提高您团队的效率。它提供了一个托管的云服务，也有一个自托管的选项。你可以通过 CircleCI 的网站访问它——有一个免费层，所以很容易试用。

多年来，CircleCI 已经成为最受欢迎的 CI/CD 平台之一。下面，我们来看看它的特性和示例管道，看看为什么这么多公司选择 CircleCI 来自动化他们的工作流程。

## CircleCI 的组件

一般来说，大多数 CI 工具都有类似的工作方式。尽管如此，熟悉 CircleCI 生态系统还是很重要的，这样你就能理解本文后面的示例代码片段。

### 步伐

步骤是工作流的基本构件。大多数时候，它们由可执行的 bash 命令组成。其他时候，可以使用 CircleCI 提供的预定义步骤，涵盖常见用例——例如，从存储库签出代码或处理管道缓存。

### 乔布斯

作业有两个配置部分。首先，他们需要一个步骤列表。当执行作业时，这些步骤按顺序作为一个单元运行。第二，执行一个作业需要一个执行者。CircleCI 支持许多平台——[Docker、](http://web.archive.org/web/20221220010244/https://www.netguru.com/blog/python-docker-tutorial) Windows、macOS 和虚拟机。

```
 #...
jobs:
 build: # Job name
   docker: # Job executor
     - image: 
       auth:
         username: mydockerhub-user
         password: $DOCKERHUB_PASSWORD  # context / project UI env-var reference
   steps:
     - checkout # Special step to checkout your source code
     - run: # Run step to execute commands, see
         # circleci.com/docs/configuration-reference/#run
         name: Running tests
         command: make test # executable command run in
         # non-login shell with /bin/bash -eo pipefail option
         # by default.
#... 
```

作业和步骤示例

### 工作流程

工作流位于层次结构的顶端。它们由作业组成，每个作业都有一个步骤列表。

为了减少长执行时间，作业以不同的方式执行——顺序、并行或手动。稍后，我们将更详细地了解优化技术。

工作流示例

```
 #...
workflows:
 build_and_test: # name of your workflow
   jobs:
     - build1 # predefined job name from the jobs section
     - build2:
         requires:
           - build1 # wait for build1 job to complete successfully before starting
           # see circleci.com/docs/workflows/ for more examples.
     - build3:
         requires:
           - build1 # wait for build1 job to complete successfully before starting 
```

球

### 工作流是从头开始实施的。为了避免重复的配置，球体开始发挥作用。orb 提供了可重用的代码片段，涵盖了像创建 Docker 映像并上传到 Docker Hub 这样的常见用例。CircleCI 提供了一个易于查找和使用的 orbs 存储库，任何人都可以浏览。

资料来源:circleci.com

语境

![config elements orbs](img/cd8773a31809ad83cb9db91ea388e87c.png)

众所周知，管道工具提供了创建配置选项的方法。CircleCI 通过包含一组环境变量的上下文来提供这一点。这些值可用于您组织中的所有项目。

假设您有一个所有服务都可以访问的安全 API。您可以构建一个上下文，并将 API 键配置为一个环境变量，每个团队都在他们的管道中引用该上下文。在运行时，CircleCI 将变量注入到工作流作业中。

### 管道优化

过一段时间后，构建应用程序的时间增加是很常见的。考虑到这一点， [CircleCI 提供了一系列功能来使流程更快](http://web.archive.org/web/20221220010244/https://www.netguru.com/blog/ci-cd-pipeline)和更简单。

并发

## 在 CircleCI 术语中，并发指的是作业执行。如果一个工作流中有许多作业，并且它们互不依赖，那么它们将并发运行。注意:这是一个软限制。

从上一节中，我们知道作业可以有各种执行器环境。除此之外，您可以根据 CPU 和内存需求微调执行器。通过提供像 small、medium 或 large 这样的资源类，可以影响这些因素。

#### 如果你使用了大量的 CPU 和内存，限制因素就会起作用。如果发生这种情况，CircleCI 会限制并发性，以确保平台保持稳定。但不用担心，CircleCI 的高级支持可以为您提高限额。

根据经验，如果可能的话，不要为工作流中的每个任务定义需求，这样它们可以并发运行。

平行

这在作业级别进行解释，表明完成特定作业需要多少执行者。我们使用这个特性来分散测试执行——它没有听起来那么复杂。

CircleCI 有一个命令行工具，帮助将测试用例分成更易管理的几堆。对于分割逻辑，使用三种方法之一:时间、名称或文件大小。

#### 数据持久性

在构建期间，会生成和处理许多文件。有依赖关系、测试报告、编译代码和图像等等。在某些时候，您可能需要访问和使用这些数据，因此您需要一个可以存储这些数据的地方。CircleCI 提供以下方式:

**缓存**–用密钥存储文件或目录。作业保存到缓存或通过键从缓存中检索数据。例如，使用它来加速依赖项管理器。

#### **工作区**–这些是工作流感知存储空间。使用它们在作业之间传递生成的版本号，或者在以后需要的编译代码之间传递。

**工件**–在工作流完成后持久保存数据的存储类。这些对于存储 jar 文件和测试报告非常有用。

1.  Docker 层缓存
2.  Docker 通常用于管道中。例如，我们对作业使用 Docker 执行器，或者有生成某种 Docker 映像的步骤。这个 CircleCI 特性缓存公共 Docker 层。它们在工作流运行过程中持续存在，并加速了映像的构建过程。
3.  CircleCI 的额外功能

#### 为了总结我们对 CircleCI 的介绍，我们还要提到几个特性。

**分支特定配置**–可以根据管道当前正在执行的分支来排除或包含作业。

## **步骤的条件执行**——你可以写条件，如果条件测试为真，则执行该步骤。如果您查看 orb 存储库，您可以看到示例，在那里您可以查看源代码。

**命令**–这些命令定义了一系列的步骤，是使代码可重用的有用特性。

1.  **参数**–在作业、命令和执行者下定义，参数最好与条件一起使用，以定义动态工作流。
2.  用于 Java 的 CircleCI 示例
3.  到目前为止，我们已经讨论了理论。现在，让我们概述代码示例。
4.  要创建 CircleCI 工作流，您需要定义以下内容:

## 您想要使用的球体列表

如果 orb 不支持某些东西，则列出已定义的作业

将执行作业的工作流列表

1.  将这些信息放入 yml 文件中。circleci/config.yml，circleci 会选择它。
2.  简单工作流程
3.  主要亮点:

以上是来自 Gradle orb 官方的一份工作。默认情况下，它将运行 build 命令。它还使用缓存来存储依赖关系。我们对默认设置做了一点小小的更改，因为基本作业当前配置为使用 Java 13。使用 tag 参数，我们将其更改为 CircleCI 图像报告中的新版本。

### 我们定义了作业的前置和后置步骤来添加新功能。这里，我们希望将 jar 文件带到下一个作业，这样我们就可以将它推送到弹性容器注册中心(ECR)。为了实现这一点，我们还使用了工作区特性。

```
 version: 2.1

orbs:
  gradle: circleci/gradle@3.0.0
  aws-ecr: circleci/aws-ecr@8.1.2

workflows:
  build-and-push:
    jobs:
      - gradle/run:                         # <--------------- 1.
          executor:
            name: gradle/default
            tag: 17.0.4
          post-steps:                       # <--------------- 2.
            - persist_to_workspace:
                root: build
                paths:
                  - libs
      - aws-ecr/build-and-push-image:       # <--------------- 3.
          repo: ng-java-circle-test-backend
          attach-workspace: true
          workspace-root: ./build
          tag: ${CIRCLE_BRANCH}.${CIRCLE_SHA1}
          requires:
            - gradle/run 
```

这是 AWS-ECR orb 的工作。我们只需要简单的参数来确保 jar 文件在之前的作业中可用。对于标记，我们使用 CircleCI 的内置环境变量。在表面之下，orb 使用额外的配置——一系列环境变量——我们需要与 AWS 进行通信:

1.  AWS_ACCESS_KEY_ID
2.  AWS_SECRET_ACCESS_KEY
3.  AWS _ ECR _ 注册表 _ID

1.  AWS_REGION
2.  梯度并行工作流
3.  我们从官方 Java 指南中复制了这个例子，它描述了大部分步骤。

### 我们有一个额外的注意事项:包含作业的 orb 有一个 executor 分配。如示例所示，如果您需要其他东西，可以更改它。在本例中，我们将其配置为使用 Docker 层缓存特性。

```
 version: 2.1

jobs:
  build-with-caches-and-parallel:
    parallelism: 2
    environment:
      GRADLE_OPTS: "-Dorg.gradle.daemon=false -Dorg.gradle.workers.max=2"
    docker:
      - image: cimg/openjdk:17.0.4
    steps:
      - checkout
      - restore_cache:
          key: v1-gradle-wrapper-
      - restore_cache:
          key: v1-gradle-cache-
      - run:
          name: Run tests in parallel
          command: |
            cd src/test/java
            # Get list of classnames of tests that should run on this node
            CLASSNAMES=$(circleci tests glob "**/*.java" \
              | cut -c 1- | sed 's@/@.@g' \
              | sed 's/.\{5\}$//' \
              | circleci tests split --split-by=timings --timings-type=classname)
            cd ../../..
            # Format the arguments to "./gradlew test"
            GRADLE_ARGS=$(echo $CLASSNAMES | awk '{for (i=1; i<=NF; i++) print "--tests",$i}')
            echo "Prepared arguments for Gradle: $GRADLE_ARGS"
            ./gradlew test $GRADLE_ARGS
      - save_cache:
          paths:
            - ~/.gradle/wrapper
          key: v1-gradle-wrapper-
      - save_cache:
          paths:
            - ~/.gradle/caches
          key: v1-gradle-cache-
      - run:
          name: Assemble JAR
          command: |
            if [ "$CIRCLE_NODE_INDEX" == 0 ]; then
              ./gradlew bootJar
            fi
      - run:
          name: Create build folder on all nodes
          command: |
            mkdir -p ./build/libs
      - persist_to_workspace:
          root: build
          paths:
            - libs

workflows:
  build-and-push-with-caches-and-parallel:
    jobs:
      - build-with-caches-and-parallel
      - aws-ecr/build-and-push-image:
          repo: ng-java-circle-test-backend
          attach-workspace: true
          workspace-root: ./build/libs
          tag: ${CIRCLE_BRANCH}.${CIRCLE_SHA1}
          executor:                                   # <------- Extra Note
            name: aws-ecr/default
            use-docker-layer-caching: true
          requires:
            - build-with-caches-and-parallel 
```

Terraform 简单工作流程

我们还应该自动化与基础设施相关的变更。使用 Terraform，我们可以通过代码来管理它。下面是一个管道示例:

主要亮点:

### YAML 代码重用示例。该截面被锚定，可以在以后引用。

```
 version: 2.1

orbs:
  terraform: circleci/terraform@3.1.0

common:                                                 # <------------ 1.
  terraform-common: &terraform-common
    path: terraform
    tag: 1.2.8

workflows:
  deploy_infrastructure:
    jobs:
      - terraform/validate:
          <<: *terraform-common
          checkout: true
      - terraform/plan:                                 # <------------ 2.
          <<: *terraform-common
          var_file: vars/test.tfvars
          persist-workspace: true
          requires:
            - terraform/validate
      - wait-for-apply-approval:
          type: approval
          filters:                                      # <------------ 3.
            branches:
              only: main
          requires:
            - terraform/plan
      - terraform/apply:
          <<: *terraform-common
          attach-workspace: true
          filters:
            branches:
              only: main
          requires:
            - wait-for-apply-approval 
```

这些是来自地球的工作。我们在配置文件中添加了通用步骤，并在需要的地方扩展了它们。这里，Terraform 使用提供的 var 文件来填充定义的变量。一旦它有了一个计划，它就持续到工作区，确保应用步骤执行我们批准的内容。

分支过滤在起作用。我们只从主分支部署基础设施；对于其他分支，最后两步是不可见的。

CircleCI 的 Netguru 最佳实践

1.  CircleCI 是我们鼓励开发人员在 Netguru 使用的 CI/CD 解决方案。许多工程团队将它用于内部和客户项目。
2.  我们努力遵守[以下准则](http://web.archive.org/web/20221220010244/https://www.netguru.com/blog/top-10-best-practises-to-benefit-more-form-circleci):
3.  使用大量较小的作业

## 确定并行性可以在哪些方面发挥作用

尽可能使代码可重用

如果合适，使用球体

*   集成滚动条以跟踪部署状态
*   集成测试报告的代码环境
*   切尔莱西:前进的道路
*   本指南将帮助您使用 CircleCI 持续集成工具更容易地开始 Java 项目。我们回答了“CircleCI 是什么？”并概述了它如何帮助[开发团队](http://web.archive.org/web/20221220010244/https://www.netguru.com/services/web-development/java-development)为他们的日常任务创建高效的工作流。如果您想了解更多信息，请随时联系我们。
*   Integrate Rollbar to track deployment status
*   Integrate CodeClimate for test reports

## CircleCI: the way forward

This guide will help you get started more easily with Java projects using the CircleCI Continuous Integration tool. We answered the question “what is CircleCI?” and outlined how it helps [development teams](http://web.archive.org/web/20221220010244/https://www.netguru.com/services/web-development/java-development) create an efficient workflow for their daily tasks. If you’d like more information, feel free to get in touch.