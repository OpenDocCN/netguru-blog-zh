# AR 快速查看和 USDZ

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/ar-quick-look-and-usdz>

 2018 年 6 月，在 WWDC 期间，苹果推出了一种新的内置浏览器，用于显示和分享使用皮克斯 USDZ 文件格式的高质量 3D 内容。它扩展了快速查看功能，这是一个用于预览文档、图像和 iOS 12.0 中的 3D 模型文件的框架。

UZDZ 文件不仅可以在应用程序中显示，当用 Safari 打开时，还可以在网站上显示。如果设备与 ARKit 不兼容，则只有对象模式可用。我想起了神秘的 USDZ 文件——但它究竟是什么呢？

![Image from Apple’s Human Interface Guidelines](img/32dc14e26097c02b551cbebb03a3672a.png)

# **通用场景描述 Zip 压缩–3x“w”:**

我们中的许多人可能会想“为什么这个名字这么长又复杂”和“好吧，现在我更喜欢用简称来称呼它”。然而，我们可以问三个问题，我称之为 3x“w”——谁？，什么？为什么呢？：

谁啊。

什么？

*   用于 3D 模型移动分发的 USDZ 文件格式，它是几个对象的组合，但仍然可以作为一个对象进行渲染。它是一个未加密和未压缩的 zip 存档，仅包含其数据可由 USD 运行时通过 mmap 使用的文件类型:
    *   USD: USDA，USDC，USD(苹果目前的实现只允许一个 USDC 文件，但这一限制将在未来的操作系统更新中取消)

    *   图像: PNG，JPEG 用于图像/纹理

    *   音频:嵌入式音频的 M4A、MP3、WAV

*   USDZ 归档的第一个文件是 USD 文件(这里:USDC；它包含模型、动画和材质定义)。如果需要，剩下的文件是图像和音频。

![Diagram example structure of USDZ file](img/33938242247ac5c061fbe0f786ac9fcb.png)Example structure of USDZ file

为什么？

*   苹果和皮克斯一起开发了一套新的模式来扩展 ar 用例的格式。根据 [苹果文档](http://web.archive.org/web/20221207131721/https://developer.apple.com/documentation/arkit/usdz_schemas_for_ar) ，其赋予 USDZ 文件 3D 资产 AR 能力如:
    *   将 3D 内容锚定在现实世界中的特定位置。

    *   对真实世界的情况做出反应。

    *   参与物理模拟。

    *   将音频效果连接到一个位置。

    *   通过显示文本来注释环境。

*   USDZ 将所有模型和纹理捆绑到一个文件中，以便高效地交付 3D 内容，而不必使用参考文件。

值得一提的是，USDZ 文件只能在苹果设备上使用——Android 不支持它们，并且有它们的等价物:gLTF 和 gLB。

# 我们能实现什么？

在 AR 快速查看中，我们可以使用以下功能:

# 它内部是如何工作的？

让我向您介绍 ViewController 和 QLPreviewController(用于预览项目的专用视图控制器)之间的应用程序流，在本例中，它用于呈现 3D 模型:

1.  **view controller呈现**QL preview controller。****

2.  **QL preview controller**向 **ViewController** 询问预览项数。

3.  **ViewController** 回答**QL preview controller**说是一个型号。**永远是一个型号。**

4.  **QL preview controller**向 **ViewController** 询问预览项的 URL。

5.  **ViewController** 为 USDZ 文件提供**QL preview controller**的 URL。

6.  **QL preview controller**向 **ViewController** 请求源视图进行过渡。

7.  **ViewController** 提供**QL previewcontroller**用于过渡的源视图(如上图:点击紫色单元格)。

# **成为 3D 文件创建者**

要创建好的 3D 模型，你需要做 6 件简单的事情:

*   **位置–**物体在该区域的位置

*   **物理尺寸**–物体的尺寸、比例、大小或范围。

*   **动画**——在三维空间中移动的幻觉

*   **接触阴影**–一个黑暗的区域，来自光源的光线被不透明的物体阻挡。它是高清晰度渲染管道(HDRP)光线在深度缓冲区内的屏幕空间中行进的阴影。

*   **外观**–一种心理表征，3D 物体的外观。

*   **透明性**–看穿物体的能力。

但是，如何正确地做到这一点，从而在我们的世界中正确地出现一个 3D 模型呢？让我们来看看苹果公司提供的一些有用的提示:

1.安置

*   对象应该面向正 z(相机)。
*   对象的底部应放置在地平面上，其中 y = 0。

*   对象的自然轴点应该放在原点。

2.物理尺度

*   应该参照现实生活中的尺寸来创建对象。

*   如果物体在现实生活中不存在，那么它的大小应该以模型完全适合设备屏幕的方式来创建。

3.动画

*   动画应该是循环的。

*   动画不应该改变对象的位置。

*   如果动画改变了物体的位置，你应该准备一些环境或者地方，位置是不变的(比如一圈草地)。多亏了它，当用户想把物体放在其他地方时，他或她将移动环境，而不是物体本身。

4.接触阴影

*   AR 快速查看总是提供接触阴影，所以你不应该添加另一个，因为你会有两个阴影。

*   AR Quick Look 可以打开和关闭阴影，具体取决于模式。此外，它可以在光源改变时将环境照明条件应用于阴影。

*   第一个框架很重要——选择一个能产生最佳阴影的框架。

5.出现

*   AR 快速查看使用了 **PBR** (基于物理渲染)**着色器**，通过它我们可以设置:
    *   反照率–模型的基色

    *   金属色——具有金属光泽的油漆、织物或颜色；物体的类型:导体或绝缘体

    *   粗糙度——物体的表面纹理(表面光洁度的组成部分):粗糙还是有光泽

    *   正常-表面细节

    *   环境遮挡——将场景中的每个点暴露在环境照明下；内部阴影

    *   发射-物体发出的光的级别

*   每种纹理都有其格式:
    *   阿尔伯托:RGB/RGBA

    *   金属色:灰度

    *   粗糙度:灰度

    *   正常:RGB

    *   环境光遮挡:灰度

    *   发射:RGB

    *   -以及 iOS 支持的其他格式-

    *   纹理应该是 2 的平方幂(2048，1024，512…)。

6.透明度

# **成为创造者容易，成为大师难……**

有了这些知识，你就可以创建自己的 3D 对象了——但要成为大师，你需要了解如何优化它们。有许多因素会影响模型的内存需求:

理论上，只有天空才是极限——但不是在这种情况下。具有单一 PBR 材料的模型存在限制:

*   最多实现 100k 个多边形(直边形状——3 个或更多边——由称为顶点的三维点和连接它们的称为边的直线定义)。

*   有一套 2048 × 2048 的 PBR 纹理。

*   创作不超过 10 秒的动画。

为了获得最佳性能，Apple 建议:

*   不包括没有(也不会)被使用的纹理。

*   对整个模型使用单一材质/纹理集(如果可能)。

*   把纹理预算花在最有价值和真实感的地方。

*   根据下载大小平衡纹理大小和质量。

*   冻结变换并合并相邻顶点。

**而作为开发者，我们也要记住像素在 AR 中是有物理大小的。**

# 获得自己的 USDZ 文件比以往任何时候都容易！

在 Xcode 10.0 中，Apple 引入了命令行工具 USDZ Converter，它允许开发人员将 3D 模型转换为。usdz 格式。它将 PBR 纹理映射到网格和子网格。有三种类型的输入文件:

您可以打开终端并:

```
xcrun usdz_converter input_model.obj output_model.usdz
```

```
xcrun usdz_converter Model.obj Model.usdz
  -g modelMesh
  -color_map Model_Albedo.png
  -metallic_map Model_Metallic.png
  -roughness_map Model_Roughness.png
  -normal_map Model_Normal.png
  -ao_map Model_AmbientOcclusion.png
  -emissive_map Model_Emissive.png
```

```
xcrun usdz_converter Model.obj Model.usdz -v
```

# 拒绝 3D 建模软件，通过 iPhone 编辑

USDZ 文件是只读的。编辑其内容需要打开包并使用适当的工具编辑其组成部分。然而，由于有了`SceneKit.ModelIO.`,有可能稍微修改我们的 3D 对象

首先，我们需要导入 `ARKit` 和`SceneKit.ModelIO`。然后我们可以创造出`sceneView: ARSCNView`。可以在界面构建器中添加或以编程方式创建。其次，我们必须获得 URL，即我们的 3D 对象在本地存储中的位置:

```
guard let url = Bundle.main.url(forResource: "ModelName", withExtension: "usdz") else { 
  return 
}
```

第一步完成了，太好了！现在我们必须初始化 MDLAsset，根据苹果文档的说法，它是一个 3D 对象和相关信息的索引容器，比如变换层次、网格、相机和灯光。我们将使用上面生成的 URL 来定位我们的 3D 模型。模型将在没有纹理的情况下加载，所以你会看到一个白色的形状——这就是为什么我们要调用`loadTextures()`方法。

```
let asset = MDLAsset(url: url)
asset.loadTextures()
```

如果我们不加载纹理，我们将在控制台中显示一个错误:

```
[SceneKit] Error: Failed to load <C3DImage 0x600000f4cc80 src:<url_to_our_usdz_file> [0.000000x0.000000]>
```

然而，让它保持原样并且不调用`loadTextures()`也是可以的——这不会影响任何其他事情。

现在我们需要将对象插入到场景中，这样用户就可以看到它了。我们创建场景实例，并将其分配给我们的场景视图。要设置我们的场景，我们必须将这些行添加到代码中:

```
let scene = SCNScene(mdlAsset: asset)
sceneView.scene = scene
```

根据需要，也可以设置:

左侧为折叠统计数据，右侧为展开统计数据

*   它决定了用户是否可以操作用来渲染场景的视角。操纵这个视点不会修改场景图和现有的摄像机。根据[苹果文档](http://web.archive.org/web/20221207131721/https://developer.apple.com/documentation/scenekit/scnview/1523171-allowscameracontrol)，在默认配置下，SceneKit 提供了以下控件:

    *   用一个手指平移以围绕场景旋转相机。

    *   用两个手指平移以在其局部 X，Y 平面上平移相机。

    *   用三个手指垂直平移以向前和向后移动相机。

    *   双击切换到场景中的下一个摄像机。

    *   用两个手指旋转以滚动摄影机(在摄影机节点的 Z 轴上旋转)。

    *   捏合以放大或缩小(改变相机的 FOV - `fieldOfView)`)。

    如果不将该属性设置为 true ，用户就不能修改视点。

*   `sceneView.autoenablesDefaultLighting` -指定接收器是否应该自动照亮没有光源的场景。当设置为 true 时，会自动添加一个漫射光，并放置到没有灯光或只有环境光的渲染场景中。没有它，在大多数情况下，模型将是黑色的，只有发射纹理将被应用。

设置完成了，干得好！现在我们可以开始有趣的部分，在这里我们可以编辑我们的 3D 模型。

我们将致力于苹果的复古电视模型，你可以在[快速查看图库](http://web.archive.org/web/20221207131721/https://developer.apple.com/augmented-reality/quick-look/)中找到。

我在做复古电视模型，但你可以选择其他任何东西！然而，它并不复杂，我不能修改它的内容太多。剧透:模型越复杂，我们能做的就越多。

当我完成这篇文章时，我发现椅子模型更适合修改内容，比如改变纹理、删除元素等。

然而，编辑 USDA 文件给了我们更好的编辑可能性——最大的问题是我们需要导入它们；没有第三方框架，我们无法使用代码从 USDZ 文件中获取美国农业部/USDC。

先总结一下第一部分，我一直写在上面。我下载了 USDZ 文件，并将其导入到项目中。此后，我得到了它的 URL 并创建了资产，它已经在场景初始化中使用。然后我分配了声明的场景并配置了 sceneView。

```
// Configure scene
sceneView.delegate = self // << The ViewController must conform to ARSCNViewDelegate
sceneView.autoenablesDefaultLighting = true
sceneView.allowsCameraControl = true
sceneView.showsStatistics = true

// Get URL to our 3D model 
guard let url = Bundle.main.url(forResource: "tv_retro", withExtension: "usdz") else {
  return 
}

// Create asset from above URL
let asset = MDLAsset(url: url)
asset.loadTextures()

// Create scene using above asset
let scene = SCNScene(mdlAsset: asset)
sceneView.scene = scene
```

我们已经准备好了我们需要的一切。现在我们可以跳到第二部分，研究我们的对象是如何工作的。感谢`object(at: Int)` 方法，我们可以得到我们的顶级对象。根据[苹果文档](http://web.archive.org/web/20221207131721/https://developer.apple.com/documentation/modelio/mdlobject)，MDLObject 是 3D 资产一部分的对象的基类，包括网格、相机和灯光。RetroTV MDLObject 结构如下所示:

```
- <<MDLObject: 0x7ff7bf5adc60>, Name: tv_retro, Children: 3> #0
  - super: NSObject
```

如您所见，MDLObject 继承自 NSObject，这是它的超类。它包含资源和子资源的名称。所谓子组件，我们指的是管理对象子组件集合的组件。它符合`MDLObjectContainerComponent`，这是一个通用接口，用于在对象层次结构中充当容器的类。它包含了 `objects`，这是一个`MDLObjects`的数组。让我们检查一下在这种情况下孩子包含了什么:

```
▿ 3 elements
  - <<MDLObject: 0x7ff7ba4b7b30>, Name: Looks, Children: 0> #0
    - super: NSObject
  - <<MDLMesh: 0x7ff7ba4b7b28>, Name: RetroTVBody, VertexCount: 69184, VertexBufferCount: 4> #1
    - super: MDLObject
      - super: NSObject
  - <<MDLMesh: 0x7ff7ba4b7b28>, Name: RetroTVScreen, VertexCount: 4096, VertexBufferCount: 4> #2
    - super: MDLObject
      - super: NSObject
```

我们的 children 数组包含三个元素:一个 MDLObject 和两个 MDLMeshes。

`MDLMesh`是用于渲染 3D 对象的顶点缓冲数据的容器。一个网格包含至少一个放置在名为`submeshes`的子网格对象数组中的 `MDLSubmesh`对象。子网格描述了应该如何组合网格的顶点以进行绘制，并引用描述子网格的预期表面外观的材料信息。

`VertexCount`是网格中顶点的数量，`VertexBufferCount`是网格顶点信息的来源的数量。

让我们声明对象和子对象——我们稍后会用到它们:

```
let object = asset.object(at: 0)
let childObjects = object.children.objects
```

我们得到了这个物体，我们知道它包含了什么。假设我们的用户想要一个塑料屏幕，而不是玻璃屏幕。让我们试着快速改变它。

首先，我们需要为屏幕创建一个节点。根据[苹果文档](http://web.archive.org/web/20221207131721/https://developer.apple.com/documentation/scenekit/scnnode),`SCNNode`是场景图的结构元素，代表 3D 坐标空间中的位置和变换，我们可以将几何图形、灯光、摄像机或其他可显示的内容附加到其上。

你还记得那个屏幕网吗？就是这个:

```
- <<MDLMesh: 0x7ff7ba4b7b28>, Name: RetroTVScreen, VertexCount: 4096, VertexBufferCount: 4> #2
    - super: MDLObject
      - super: NSObject
```

对象的子数组包含这个网格，我们将使用它来修改屏幕纹理。我们还将获得节点的几何图形，我们将为其分配一个新的纹理。这两个值都是可选的，所以我们将使用保护符打开它们。

```
guard
  let tvNode = scene.rootNode.childNode(withName: "RetroTVScreen", recursively: true),
  let geometry = tvNode.geometry,
  let texture = UIImage(named: "texture")
else { return }
```

太棒了，现在我们可以创建一个材质，我们将分配给我们的模型。`SCNMaterial`(和`MDLMaterial`，我们将在这一部分后以更精确的方式使用)是一组定义几何体表面渲染时外观的着色属性。

让我们改变电视的屏幕纹理。我们可以通过这三行简单的代码来实现:

```
let material = SCNMaterial()
material.normal.contents = texture
geometry.materials = [material]
```

我们创建一个材质的实例。然后，我们将它分配给该类型的内容——例如，它可以是正常的、发射的、透明的等等。最后，我们将一组材质分配给`geometry.materials`。就是这样——就这么简单！

我们也可以用更精确的方法来做——我们可以改变子网格的纹理。不幸的是，在 RetroTV 模型中，我们没有它们。不过，我给你看这个简单的方法，以后可能会有用。首先，我们创建一个扩展，这将有助于我们的工作。

```
extension MDLMaterial {
  func setTextureProperties(_ textures: [MDLMaterialSemantic: String]) -> Void {
    for (key, value) in textures {
      guard let url = Bundle.main.url(forResource: value, withExtension: "") else {
        fatalError("Failed to find URL for resource \(value).")
      }

      let property = MDLMaterialProperty(name: value, semantic: key, url: url)
      self.setProperty(property)
    }
  }
}
```

我们得到了纹理资源的 URL。然后我们创建一个`MDLMaterialProperty`实例，它被添加到我们的`MDLMaterial`中。你可能也想过:“但是在我们的字典里，`MDLMaterialSemantic` 到底是什么？”

嗯，它们是在渲染特定表面外观时材质属性值的语义使用的选项；由语义属性使用。你可以在苹果文档中找到一个材质语义列表——这个列表很大，多亏了它，我们可以用很多方式修改我们物体的纹理！

我们已经创建了扩展，但是现在我们想使用它。我们需要声明`MDLScatteringFunction`实例。它是一组材质属性，描述了材质的基本着色模型，以及更复杂的着色模型的超类。然后我们创建一个材质，我们提供它的名字和散射函数。现在我们可以使用上面的扩展来设置纹理。

```
let scatteringFunction = MDLScatteringFunction()
let material = MDLMaterial(name: "NewScreen", scatteringFunction: scatteringFunction)
material.setTextureProperties([.baseColor: "texture.jpg"])
```

我们创建了一个材质，如果用在我们的子网格上会很棒。我们从之前实现的 tvNode 中声明一个`MDLMesh` 实例。此后，我们迭代子网格，并给它们分配新的、漂亮的材质。

```
let mesh = MDLMesh(scnNode: tvNode)
mesh.submeshes?.forEach {
  if let submesh = $0 as? MDLSubmesh {
    submesh.material = material
  }
}
```

我们完成了我们的第一部分，这是改变纹理-多好的旅程！现在我们可以改变大小、位置和旋转。很简单很短的一部分。我们将对之前声明的`tvNode` 和 SCNVector3 进行操作，SCN vector 3 是一个三分量向量的表示。它有三个分量:x，y，z。

假设我们的用户想把电视盒子做得小一点，因为他或她在房间里没有空间放这么大的电视。我们可以使用以下方法缩小规模:

```
tvNode.scale = SCNVector3(0.3, 0.2, 0.5) // << Decrease size (scale down)
tvNode.scale = SCNVector3(2.2, 2.5, 1.1) // << Increase size (scale up)
```

我们还可以设置位置，这样我们的对象就会呈现在最佳位置。

```
tvNode.position = SCNVector3(0.0, 4, 0.0)
```

我喜欢将这两者结合起来，这样我们就可以修改对象的大小和位置，这样用户就可以看到尽可能好的效果。我个人认为，当一个对象的 y 缩小时，增加位置的 y 值是有好处的。我最喜欢的乘数是 20，但是我鼓励你尝试一些其他的值。我个人的最终结果是这样的:

```
let scaleY = 0.5
tvNode.scale = SCNVector3(1, scaleY, 1)
tvNode.position = SCNVector3(0, scaleY < 1 ? scaleY * 20 : 0, 0)
```

很好——现在我们可以得到模型的最佳视角了——我们有了尺寸，有了位置，所以现在我们可以试着旋转我们的 3D 物体了。您还将学习如何为上述更改设置动画。

我们将使用`SCNVector4` -它与`SCNVector3`非常相似，但是它的最后一个参数是以弧度表示的旋转角度，或以牛顿米表示的扭矩大小。

```
tvNode.rotation = SCNVector4(0, 1, 0, Double.pi / 2)
SCNTransaction.begin()
SCNTransaction.animationDuration = 2.0
tvNode.rotation = SCNVector4(1, 0, 0, Double.pi / 2)
SCNTransaction.commit()
```

正如你在上面看到的，我们已经使用了 `SCNTransaction`，这是一种创建隐式动画并将场景图形变化组合成原子更新的机制。我们从一个`begin()`方法开始，它为当前线程开始一个新的事务。

此后，我们设置动画持续时间——它告诉我们我们的动画将执行多长时间。最后，我们提交事务期间所做的所有更改。如果没有当前事务，此方法无效。

现在我们可以享受我们流畅的动画了！

现在让我们直接跳进兔子洞。因为 USDZ 文件是只读的，而且由于有了`SceneKit`和`ModelIO`，我们可以对它进行轻微的操作，所以我们需要问:是否有可能在我们的对象中添加或删除内容？

是也不是。真正的问题是:我们的对象有多复杂和精确。内容数量越少，可能性就越小。

我们使用的 RetroTV 模型并不复杂——它由两个网格组成:主体(盒子)和屏幕(玻璃面板)。例如，如果我们想要删除某个东西，我们必须从这两个中删除一个，或者两个都删除。

假设用户不喜欢玻璃面板，并且想要检查没有它的电视盒子会是什么样子。我们可以通过插入这段代码来解决这个问题，这样用户就可以看到一个更新的模型，没有了玻璃面板，有了以前的视角。

```
// Get screen object which we want to remove
guard
  let screenNode = scene.rootNode.childNode(withName: "RetroTVScreen", recursively: true),
  let nameOfNodeToRemove = screenNode.name,
  let objectToRemove = childObjects.first(where: { $0.name == nameOfNodeToRemove })
else { return }

// Remove screen from model
object.children.remove(objectToRemove)

// Reload sceneView
let newScene = SCNScene(mdlAsset: asset) // << Create new scene and assign our asset
let currentPOV = self.sceneView.pointOfView // << Get current point of view
self.sceneView.scene = newScene // << Assign new scene with updated object
self.sceneView.pointOfView = currentPOV // << Assign previous POV, so it's not reset to default one
```

首先，我们打开电视屏幕的一个节点，得到它的名字，并找到一个我们想要删除的对象的子对象。此后，我们使用了一个`remove(_ object: MDLObject)`方法，从对象中移除我们的屏幕。但是，更改已经应用，但是用户看不到它们。这就是为什么我们需要重新加载我们的场景视图。

为了重新加载 sceneView，我们创建了一个新场景，并分配了修改后的资源。我还得到了视点值，因为没有它加载一个新的场景到我们的场景视图会重置用户在改变前留下的视角——它发生时没有动画，在我看来很难看。最后，我们为 sceneView 分配一个带有更新对象和先前视点的新场景。

没那么糟糕——我们只是依赖于模型的复杂性。我们移除了电视屏幕——但是现在我们的模型是如此的空。如果我们给它加点东西呢？我认为我们的 RetroTV 模型缺少绿色立方体。是的，一个绿色的立方体。我们称它为盒子。

```
// Create green box
let box = SCNBox(width: 50, height: 50, length: 50, chamferRadius: 0)
let material = SCNMaterial()
material.diffuse.contents = UIColor.green
box.materials = [material]

// Add box node to our object
let boxNode = SCNNode(geometry: box)
boxNode.position = SCNVector3(10, 0, 0.3)
let mdlBox = MDLObject(scnNode: boxNode)
object.children.add(mdlBox)

// Reload sceneView to show user our greatest improvement
let newScene = SCNScene(mdlAsset: asset)
let currentPOV = self.sceneView.pointOfView
self.sceneView.scene = newScene
self.sceneView.pointOfView = currentPOV
```

一切都像以前一样——我们创建一个对象(这次我们使用了`SCNBox`)并给它分配一个材质。接下来的部分是本例中最重要的部分，因为我们从盒子中创建了一个节点，我们设置了一个位置并声明了绿色立方体的`MDLObject`实例。此后，我们将它插入到对象的子对象中。最后，我们像以前一样重新加载视图，这样用户就可以看到最终的结果。

绿色盒子工作正常。

我们希望保存这些更改，并向全世界分享我们改进的模型。我们如何实现这一目标？很简单！

```
// Save file in documents directory
let manager = FileManager.default
let documentsUrl = manager.urls(for: .documentDirectory, in: .userDomainMask)
guard let url = documentsUrl.first else { return }
let finalURL = url.appendingPathComponent("improvedTV.usdz")
self.sceneView.scene.write(to: finalURL, delegate: nil)

// Save to Files app
if manager.fileExists(atPath: finalURL.path) {
  let activityController = UIActivityViewController(activityItems: [finalURL], applicationActivities: nil)
  activityController.modalPresentationStyle = .popover
  present(activityController, animated: true)
}
```

恭喜你！我们已经成功保存了改进后的电视型号！

**奖励提示:**

有可能在物理设备上，你将有一个相机设置在对象内部，即使它在模拟器中显示正确。下面，你可以找到一个简单的代码，当用户启动应用程序时，它将指导你完美地设置你的相机，以获得你美丽的 3D 模型的最佳视角。首次创建场景并将其指定给 sceneView 后，可以设置视点。

It’s possible that on the physical device, you will have a camera set inside the object, even though it is shown correctly in the simulator. Below, you can find a simple code, which will give you direction for perfectly setting up your camera to get the best perspective on your beautiful 3D model when the user launches the application. After creating a scene and assigning it to the sceneView for the first time, you can set up the point of view.

```
// Create scene using asset
let scene = SCNScene(mdlAsset: asset)
sceneView.scene = scene

// Setup default point of view, so we can see whole object
sceneView.pointOfView?.position.y = 50
sceneView.pointOfView?.position.z = 300 
```