# 如何使用 Bin/Setup 自动化本地应用程序设置过程

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/automate-local-app-setup-process-bin-setup>

 最近，我做了一些关于创建交互式 bin/setup 脚本的研究，该脚本可以引导开发人员通过 app 的本地设置过程加入新项目。

新的 bin/setup 脚本应该从一个样本创建一个 sec_config.yml 文件，同时要求开发人员提供必要的缺失值并将它们保存到新文件中。

我创建的脚本是一个示例，需要时可以更改。我创建了对 yaml 格式的标准 sec_config/secrets 文件和标准的支持。环境文件格式。

剧本分成几个部分。

第一部分

### [代码]

在这里，您需要添加安装/更新 gems、安装所需的 ruby 版本、npm/bower 依赖项等命令。

第二部分

[代码]

### 这个部分负责所有配置文件的逻辑。

在第一个块中，您可以从示例文件创建 database.yml 。如果数据库. yml.sample 文件有两个密钥:用户名和密码，你可以如上图所示替换它们。

你也可以用 gsub_file 方法替换代码:

[代码]

[代码]

在这个块中，您可以从示例创建一个新的配置文件，或者编辑现有的配置文件(如果存在的话)。您需要在 CONFIG_FILE 常量中设置配置路径。脚本将要求用户提供所需的值(示例文件中有键，但它们没有值)。

[代码]

对于这个 sec_config.yml.sample 文件，脚本将这样工作:

[代码]

...这导致下面的 sec_config.yml :

[代码]

[代码]

上面的模块是用来创建标准的。此示例中的 env 文件:

[代码]

这将要求用户输入缺少的值。您需要获取样本文件中存在但没有值的每个键的值。

第三部分

[代码]

这个部分负责数据库的设置。你可以在这里使用 bin/rake db:setup 或者添加额外的命令，比如 bin/rake db:test:prepare 。当然，你可以像例子中所示的那样添加漂亮和丰富多彩的信息。

### 第四部分

[代码]

This section is taking care of the database setup. You can just use bin/rake db:setup here or add additional commands like bin/rake db:test:prepare. Of course, you can add beautiful and colourful messages as shown in the example.

第五部分

### [代码]

您可以在这里为开发人员添加消息。

### 请在您的项目中随意使用和定制这个脚本。希望这能帮助开发者更快更容易的设置项目。

[code]

Here you can add messages for developers.

Feel free to use and customise this script in your projects. I hope this will help developers set up projects faster and easier.