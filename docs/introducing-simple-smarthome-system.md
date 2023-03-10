# 简单智能家居系统简介

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/introducing-simple-smarthome-system>

 一些开发人员喜欢在空闲时间做一些兼职项目。我是其中之一。我总是有一些代码和电子产品躺在周围，等待完成。这一次我想测试我在后端开发方面的技能，并在这个过程中学到一点东西。我有使用 Kotlin 的经验，所以对我来说显而易见的选择是尝试 Ktor。

这种方法的好处是可以看到 Kotlin 在其他平台上的运行，因为 Kotlin 多平台可能是我工具箱中的一个有价值的工具。我选择 Vue.js 作为前端，因为我过去已经尝试过 React。好的，我已经有了工具，但是我应该做什么呢？当我对智能家居仪表板环境中的现有解决方案不满意时，我找到了答案。

我不喜欢现有解决方案的哪些方面？

*   它们很难配置
*   他们有太多的特点
*   它们是通过配置文件配置的，几乎没有文档

所以我决定自己造一个。能有多难？

时隔半年多，我向您展示简单的智能家居系统:

它也适用于移动设备:

![Home Page](img/2f4bc56d6634baa41650ecce7c399e85.png)

我能做什么

它可以显示设备的最新状态。

![Working Smarthome system](img/3a569c4d2f279e5f93d4f84cdbcfe570.png)

将设备按逻辑分组，如房间或车库。

将事件发送回 MQTT 代理。

![Full dashboard](img/a1b2e1cc206878ecc3f6c83a36feeb22.png)

少了什么？

可以根据事件更改传感器状态的规则引擎。

![Mobile views](img/616b961ecf035189e92ea9da7095256e.png)

在开发 SSS 时，我尝试遵循几个概念:

它基于 MQTT。MQTT 是物联网世界中的一个流行标准，已经有很多设备支持它。

*   它应该很容易与我使用的两个工具集成: [zigbee2mqtt](http://web.archive.org/web/20221007161754/https://www.zigbee2mqtt.io/) (网关软件，允许你使用专有的 zigbee 设备而无需专有网关)和 [Tasmota](http://web.archive.org/web/20221007161754/https://github.com/arendst/Sonoff-Tasmota) (基于 ESP8622 芯片的设备的开源固件)。
*   一切都应该可以通过用户界面进行配置。
*   请到 [Github](http://web.archive.org/web/20221007161754/https://github.com/netguru/smarthome) 查看安装细节和源代码。

在开发过程中，我学到了一些东西。我想我更喜欢 React 而不是 Vue。Vue 的 MVVM 方法很好，但是可观察变量并不总是像预期的那样工作。即使在后端，Kotlin 也是一项高超的技术，但是 Ktor 一点也不固执己见，所以我需要解决很多问题，比如使用哪个数据库，什么是数据库连接池:)

*   我需要提到我在开发这个工具时从 Netguru 那里得到的巨大支持。我不仅有机会在这里推广它并在 Netguru 的 Github 上托管它，而且来自 Vue 团队的优秀人员也在这个框架上帮助了我。在 Netguru 成长是一种享受:)

照片由 [罗宾·格劳泽](http://web.archive.org/web/20221007161754/https://unsplash.com/@nahakiole?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](http://web.archive.org/web/20221007161754/https://unsplash.com/search/photos/sensor?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

There are a couple of concepts I’ve tried to follow while developing SSS:

1.  It is based on MQTT. MQTT is a popular standard in the IoT world with a lot of devices already supporting it.
2.  It should easily integrate with two tools I use: [zigbee2mqtt](http://web.archive.org/web/20221007161754/https://www.zigbee2mqtt.io/) (gateway software that allows you to use proprietary Zigbee devices without proprietary gateways) and [Tasmota](http://web.archive.org/web/20221007161754/https://github.com/arendst/Sonoff-Tasmota) (open source firmware for devices based on ESP8622 chips).
3.  Everything should be configurable through a UI.

Please go to [Github](http://web.archive.org/web/20221007161754/https://github.com/netguru/smarthome) for installation details and source code.

I’ve learned a couple of things during development. I think I like React more than Vue. The MVVM approach of Vue is nice, but observable variables didn’t always work as expected. Kotlin is a superb technology even on the backend, but Ktor is not opinionated at all, so I needed to figure out a lot of stuff, like which database to use and what is the database connection pool :)

I need to mention the great support I received from Netguru in developing this tool. Not only do I have the opportunity to promote it here and host it on Netguru’s Github, but also great people from the Vue team helped me with this framework. Growing at Netguru is a pleasure :)

* * *

Photo by [Robin Glauser](http://web.archive.org/web/20221007161754/https://unsplash.com/@nahakiole?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](http://web.archive.org/web/20221007161754/https://unsplash.com/search/photos/sensor?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)