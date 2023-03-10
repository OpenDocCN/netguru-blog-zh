# 使用 Android 上的 ObjectDetection API 进行手部检测

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/hand-detection-using-objectdetection-api>

在上次 Netguru 团队<g class="gr_ gr_103 gr-alert gr_gramm gr_inline_cards gr_run_anim Punctuation only-ins replaceWithoutSep" id="103" data-gr-id="103">晚宴</g>期间，一位朋友向我展示了他的花哨的三星设备的一个整洁的功能——[手势控制自拍](http://web.archive.org/web/20220924153003/https://videotron.tmtx.ca/en/topic/samsung_galaxygrandprime/using_gesture_control_to_take_a_selfie.html#step=1)(探测手自拍)。一段时间以来，我一直对机器学习感兴趣，我想自己实现它。为了解决这个问题，我使用了[对象检测 API](http://web.archive.org/web/20220924153003/https://github.com/tensorflow/models/tree/master/research/object_detection) SSD 多盒模型，使用 <g class="gr_ gr_60 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling" id="60" data-gr-id="60">mobilenet</g> 特征映射提取器<g class="gr_ gr_61 gr-alert gr_spell gr_inline_cards gr_run_anim ContextualSpelling" id="61" data-gr-id="61">对 COCO(上下文中的公共对象)数据集进行预训练</g>。按照以下步骤创建一个简单的手部检测应用程序，并查看我的实验结果: