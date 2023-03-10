# 如何在撰写多平台中创建自定义图表

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/compose-multiplatform-custom-charts>

 Compose Multiplatform 是一个现代的工具包，它提供了广泛的有用且有趣的 API。在这个技术演练中，我将尝试解释 Canvas 的基本用法以及如何绘制简单的自定义图表。

随着您对 Canvas 的进一步研究，您会发现，就像 Compose 中的几乎所有东西一样，您可以用几行代码创建许多元素。如你所知， [Compose Multiplatform 基于 Google 的 Jetpack Compose](/web/20221006005454/https://www.netguru.com/blog/starting-compose-multiplatform-project) 。它与 Jetpack 有很多重叠的功能，但 Compose 可以在多个平台上运行。您将在下面看到的图表在 Android 和桌面应用程序上都可以正常工作。

## 帆布

如果平面设计师有 Photoshop，开发者有 Canvas。在创建项目所需的自定义图形和图表时，您可以尽情发挥想象力。

Canvas 是 Jetpack [Compose UI 框架](/web/20221006005454/https://www.netguru.com/blog/multiplatform-adaptive-ui)中的一个可组合函数。您可以在布局中定位画布，就像任何其他合成 UI 元素一样。它允许您精确控制元素的位置和样式来绘制元素。以类似的方式组合多平台 Canvas 函数，但是使用 Skia 图形引擎。

### 坐标系

当涉及到在画布上绘图时，您需要以像素而不是与密度无关的像素进行思考。请记住，画布绘图不会自动缩放。

![coordinate_system](img/5e0052a3285032b715360dc43076b3e6.png)

在上图中，您可以看到像素是如何基于从(0，0)到(242，242)开始的坐标进行组织的。X 轴从左到右，而 Y 轴从上到下。每个像素代表坐标，坐标是在屏幕上排列 UI 元素的基础。使用此信息在画布上放置形状或自定义元素。Canvas 允许您将可视元素准确地放置在您想要的位置。

### 画布的基本用法

使用 canvas 有两种主要方式。

*   您可以像插入任何其他可组合文件一样插入它:

```
@Composable
fun SimpleComposable1() {
    Box {
        Canvas(modifier = Modifier.size(200.dp)) {
            // your drawing code goes here
        }
    }
} 
```

*   可以用`drawBehind`的方法:

```
@Composable
fun SimpleComposable2() {
    Box(modifier = Modifier.size(200.dp).drawBehind {
        // your drawing code goes here
    })
} 
```

绘制简单的形状开发人员通常使用 Android 中的 Canvas 来创建几种形式的视觉元素，其中最常见的是不同类型的形状。请考虑以下情况:

```
// draw rectangle
 drawRect(
        color = Color.Yellow,
        topLeft = Offset(50f, 50f), // values in pixels
        size = Size(100f, 100f) // values in pixels
    )

// draw circle
 drawCircle(
        color = Color.Red,
        radius = 100f, // values in pixels
        center = center
    )

// draw line
drawLine(
        color = Color.Green,
        start = Offset(0f, 0f), // values in pixels
        end = Offset(size.width, size.height),
        strokeWidth = 5f // values in pixels
    ) 
```

*   您可以使用扩展函数来拆分绘图代码:

```
fun DrawScope.drawRectangleExample() {
    drawRect(
        color = Color.Yellow,
        topLeft = Offset(50f, 50f),
        size = Size(100f, 100f)
    )
}

fun DrawScope.drawCircleExample() {
    drawCircle(
        color = Color.Red,
        radius = 100f,
        center = center
    )
}

fun DrawScope.drawLineExample() {
    drawLine(
        color = Color.Green,
        start = Offset(0f, 0f),
        end = Offset(size.width, size.height),
        strokeWidth = 5f
    )
} 
```

*   然后，在画布上使用它:

```
@Composable
fun SimpleComposable1() {
    Box {
        Canvas(modifier = Modifier.size(200.dp)) {
            drawRectangleExample()
            drawCircleExample()
            drawLineExample()
        }
    }
} 
```

如何绘制简单的图表

![canva](img/2abf174344192bd1956950764c906add.png)

对于那些希望构建自定义图表的人，在开始旅程时，可以先从绘制基本图表开始。在接下来的几节中，我将简要介绍如何绘制折线图、饼图和条形图。

## 折线图

折线图的构建相对简单。下面的例子只显示了基础知识。它没有经过任何优化，以便于阅读。在现实世界中，你应该使用`remember`来保存状态，避免重新计算和重绘。

Line charts are relatively simple to construct. The example below shows only the basics. It is stripped out from any optimizations to make it easier to read. In the real world, you should use `remember` to save states and avoid recalculation and redrawing.

```
//point representation
data class Point(val x: Float, val y: Float)

@Composable
fun SuperSimpleLineChart(modifier: Modifier = Modifier.size(300.dp, 200.dp)) {
    // our values to draw
    val values = listOf(
        Point(0f, 1f),
        Point(1.5f, 1.2f),
        Point(2f, 0.9f),
        Point(2.5f, 2f),
        Point(3f, 1.3f),
        Point(3.5f, 3.2f),
        Point(4f, 0.8f),
    )
    // find max and min value of X, we will need that later
    val minXValue = values.minOf { it.x }
    val maxXValue = values.maxOf { it.x }

    // find max and min value of Y, we will need that later
    val minYValue = values.minOf { it.y }
    val maxYValue = values.maxOf { it.y }

    // create Box with canvas
    Box(modifier = modifier
        .drawBehind { // we use drawBehind() method to create canvas

            // map data points to pixel values, in canvas we think in pixels
            val pixelPoints = values.map {
                // we use extension function to convert and scale initial values to pixels
                val x = it.x.mapValueToDifferentRange(
                    inMin = minXValue,
                    inMax = maxXValue,
                    outMin = 0f,
                    outMax = size.width
                )

                // same with y axis
                val y = it.y.mapValueToDifferentRange(
                    inMin = minYValue,
                    inMax = maxYValue,
                    outMin = size.height,
                    outMax = 0f
                )

                Point(x, y)
            }

            val path = Path() // prepare path to draw

            // in the loop below we fill our path
            pixelPoints.forEachIndexed { index, point ->
                if (index == 0) { // for the first point we just move drawing cursor to the position
                    path.moveTo(point.x, point.y)
                } else {
                    path.lineTo(point.x, point.y) // for rest of points we draw the line
                }
            }

            // and finally we draw the path
            drawPath(
                path,
                color = Color.Blue,
                style = Stroke(width = 3f)
            )
        })
}

// simple extension function that allows conversion between ranges
fun Float.mapValueToDifferentRange(
    inMin: Float,
    inMax: Float,
    outMin: Float,
    outMax: Float
) = (this - inMin) * (outMax - outMin) / (inMax - inMin) + outMin 
```

因为在 Compose Multiplatform 中没有直接的方法将文本放到画布上(这是可以做到的，但它需要在 oldschool view canvas 和 Skia engine 的桌面上分别实现 Android)，所以您可以通过将其他可组合视图包围起来，将轴描述插入到图表中。

![chart](img/d32a7406db7e9c9ae9aa2d82fd3bc6a8.png)

为了使过程更简单，代码更通用，您可以在视图中包装图表。

圆形分格统计图表

饼图是由弧线构成的。使用百分比作为输入日期。要画一个圆弧，你需要起始角度和扫描角度，你可以通过已知一个圆有 360 度的百分比来计算。

```
@Composable
fun SuperSimpleLineChartWithLabels() {
    Column(
        Modifier
            .padding(10.dp)
            .border(width = 1.dp, color = Color.Black)
            .padding(5.dp)
            .width(IntrinsicSize.Min)
    ) {
        Row(Modifier.height(IntrinsicSize.Min)) {
            Column(
                modifier = Modifier
                    .fillMaxHeight(),
                verticalArrangement = Arrangement.SpaceBetween
            ) {
                Text(text = “Max Y”)
                Text(text = “Min Y”)
            }
            SuperSimpleLineChart()
        }
        Row(Modifier.fillMaxWidth(), horizontalArrangement = Arrangement.SpaceBetween) {
            Text(“Min X”)
            Text(“Max Y”)
        }
    }
} 
```

![chart_completed](img/7d4f1b6e324041e336f753666be32945.png)

### 为了给图表增添趣味，您可以通过添加一些额外的线条并将`sweepAngle`限制从硬编码的 360 度更改为动画来添加简单而有效的动画，如下所示:

条形图

绘制条形图是最简单的一种。我认为代码是不言自明的:

```
// pie chart data representation
data class PieChartItem(val percentage: Float, val color: Color)

@Preview
@Composable
fun SuperSimplePieChart() {

    // our values to be displayed in percentage.
    // As we assume that values are in percent, sum can’t be bigger than 100
    val values = listOf(
        PieChartItem(10f, Color.Red),
        PieChartItem(20f, Color.Green),
        PieChartItem(40f, Color.Yellow),
        PieChartItem(30f, Color.Blue)
    )

    // box with canvas
    Box(
        Modifier
            .size(200.dp) // we give the box some size
            .background(Color.White) // white background
            .padding(10.dp) // padding for nice look
            .border(width = 1.dp, color = Color.Black) // border for aesthetic
            .drawBehind { // create canvas inside box

                var startAngle: Float = START_ANGLE // we use the variable to track start angle of each arc

                values.forEach { // for each value
                    val sweepAngle = it.percentage.mapValueToDifferentRange( // we transform it to degrees from 0 to 360
                        inMin = 0f, // 0%
                        inMax = 100f, // 100%
                        outMin = 0f, // 0 degrees
                        outMax = FULL_CIRCLE_DEGREES // 360 degrees
                    )

                    // using extension function we draw the arc
                    drawArc(
                        color = it.color,
                        startAngle = startAngle,
                        sweepAngle = sweepAngle
                    )

                    startAngle += sweepAngle // increase sweep angle
                }
            })
}

// extension function that facilitates arc drawing
private fun DrawScope.drawArc(
    color: Color,
    startAngle: Float, // angle from which arc will be started
    sweepAngle: Float // angle that arc will cover
) {
    val padding = 48.dp.toPx() // some padding to avoid arc touching the border
    val sizeMin = min(size.width, size.height)
    drawArc(
        color = color,
        startAngle = startAngle,
        sweepAngle = sweepAngle,
        useCenter = false, // draw arc without infill
        size = Size(sizeMin - padding, sizeMin - padding), // size of the arc/circle in pixels
        style = Stroke( // width of the ark
            width = sizeMin / 10
        ),
        topLeft = Offset(padding / 2f, padding / 2f) // move the arc to center
    )
} 
```

![pie_chart](img/71b0f219adc95026840352f0ec004f5d.png)

To spice up the chart, you can add simple but effective animation by adding a few extra lines and changing the `sweepAngle` limit from hard coded 360 degrees to an animated one, as such:

您还可以添加一些额外的修饰:

```
fun SuperSimplePieChart() {

    var animationPlayed by remember() { // to play animation only once
        mutableStateOf(false)
    }

    val maxAngle by animateFloatAsState( // animate from 0 to 360 degree for 1000ms
        targetValue = if (animationPlayed) FULL_CIRCLE_DEGREES else 0f,
        animationSpec = tween(durationMillis = 1000)
    )

    LaunchedEffect(key1 = true) { // fired on view creation, state change triggers the animation
        animationPlayed = true
    }

    .
    .
    .
     values.forEach { // for each value
                    val sweepAngle =
                        it.percentage.mapValueToDifferentRange( // we transform it to degrees from 0 to 360
                            inMin = 0f, // 0%
                            inMax = 100f, // 100%
                            outMin = 0f, // 0 degrees
                            outMax = maxAngle // <--- chagne this to maxAngle
                        )
    ... 
```

### 结论

Compose [中的 Canvas 多平台](/web/20221006005454/https://www.netguru.com/services/kotlin-multiplatform)允许绘制任何可视组件。然而，这需要精确的计划，因为我们操作的是实际的像素，对象必须缩放到窗口或屏幕的大小。它在桌面和 Android 上运行良好，只需要很少的平台依赖实现。不幸的是，我们还不能在网络或 IOS 上使用它...

```
data class Bar(val value: Float, val color: Color)

const val BAR_WIDTH = 50f //bar width in pixels

@Preview
@Composable
fun SuperSimpleBarChart(modifier: Modifier = Modifier.size(300.dp, 200.dp)) {
    // our values to draw
    val bars = listOf(
        Bar(10f, Color.Blue),
        Bar(20f, Color.Red),
        Bar(30f, Color.Green),
        Bar(40f, Color.Yellow),
        Bar(10f, Color.Cyan)
    )
    val maxValue = bars.maxOf { it.value } // find max value

    // create Box with canvas
    Box(
        modifier = modifier
            .drawBehind { // we use drawBehind() method to create canvas

                bars.forEachIndexed { index, bar ->
                    // calculate left and top coordinates in pixels
                    val left = index
                        .toFloat()
                        .mapValueToDifferentRange(
                            inMin = 0f,
                            inMax = bars.size.toFloat(),
                            outMin = 0f,
                            outMax = size.width
                        )
                    val top = bar.value
                        .mapValueToDifferentRange(
                            inMin = 0f,
                            inMax = maxValue,
                            outMin = size.height,
                            outMax = 0f
                        )

                    // draw the bars
                    drawRect(
                        color = bar.color,
                        topLeft = Offset(left, top),
                        size = Size(BAR_WIDTH, size.height - top)
                    )
                }
            })
} 
```

![graph_chart](img/508f334952d540f8eabb045df9e716f4.png)

You can also add some extra touches:

```
@Composable
fun SuperSimpleBarGraphWithLabels() {
    Column(
        Modifier
            .padding(10.dp)
            .border(width = 1.dp, color = Color.Black)
            .padding(5.dp)
            .width(IntrinsicSize.Min)
    ) {
        Row(Modifier.height(IntrinsicSize.Min)) {
            Column(
                modifier = Modifier
                    .fillMaxHeight(),
                verticalArrangement = Arrangement.SpaceBetween
            ) {
                Text(text = “Max”)
                Text(text = “Min”)
            }
            SuperSimpleBarChart()
        }
    }
} 
```

![graph_chart_completed](img/cc9324047d928fb71f69165a40d74995.png)

## Conclusion

Canvas in Compose [Multiplatform](/web/20221006005454/https://www.netguru.com/services/kotlin-multiplatform) allows drawing any visual component. Still, it requires precise planning because we operate on actual pixels, and objects have to scale to window or screen size. It operates nicely on Desktop and Android, requiring very little platform dependent implementation. Unfortunately we can not use it on the Web or IOS, yet...