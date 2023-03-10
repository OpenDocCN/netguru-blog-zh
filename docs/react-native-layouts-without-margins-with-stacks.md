# 使用堆栈构建无边距的 React 本机布局

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/react-native-layouts-without-margins-with-stacks>

 现在世界上每个人都有手机，每个手机的尺寸都不一样。这对开发人员和设计人员来说是个大问题，因为他们必须为所有屏幕尺寸提供完美的 UI。幸运的是，我们有很多工具来解决这个问题——其中之一就是 Stacks 库。

[Stacks](http://web.archive.org/web/20221209132440/https://github.com/mobily/stacks) helps us create a responsive layout easily without using margins. Margins are considered bad because they break component encapsulation and make reusability harder. If you want to learn more about margins, [check out this interesting article](http://web.archive.org/web/20221209132440/https://mxstbr.com/thoughts/margin/). The Stacks library is built in **ReScript**. ReScript compiles to JavaScript which means that Stacks can be used in projects written in TypeScript, Flow, and ReScript.

堆栈库基于间隔组件。间隔组件是将管理子组件位置的责任转移到父组件的组件。间隔组件的工作方式与**柔性盒**相似。我们把正确的道具传给家长，然后孩子会相应地改变位置。

该库包含许多布局组件，其中一些是**堆栈**、**图块**、**框**、**列**和**列**。为了使用这些组件，你必须将**主组件之一**放在组件树的**顶端**。 **Grid** 和**stack provider**是定义在创建应用布局时要使用哪些选项的主要组件。该库还包含一些有用的钩子，例如， **useCurrentBreakpoint** 返回安装该应用的设备类型， **useWindowDimensions** 返回一个具有屏幕宽度和高度属性的对象。

```
 const breakpoint = useCurrentBreakpoint();

 const isItemHorizontal = () => {
 return breakpoint === 'tablet';
 };
```

**使用当前断点示例**

### React 本机布局教程

这是一个简单的 React 本地应用 ，演示了 Stacks 库的基本用法。这是一个电影应用程序，显示当前最受欢迎的电影。

![](img/64a06c30a7212c2ac23a725b2675e9b1.png)![](img/de8b80c116ba3c6b1f7342fa388ddfa5.png)**HomeScreen and DetailsScreen on a mobile phone**![](img/d8a34f3f2c5473ea809097e4c160ed87.png)![](img/8a1fa1fa8722ccd9f545611ca9214049.png)

**平板电脑上的主屏幕和详细信息屏幕**

这是应用程序的布局。它由两个屏幕组成——第一个屏幕显示电影列表，而第二个屏幕显示 T2 电影的详细信息。请看一下**的 stack provider**组件。它被添加到我们的组件树的根中。它使用定义子组件默认间距的间距属性。

```
 <StacksProvider spacing={2}>
 <AppNavigator />
 </StacksProvider>
```

**App.js**

**AppNavigator**

```
 <NavigationContainer>
 <Stack.Navigator initialRouteName={SCREENS.home}>
 <Stack.Screen
 options=
 name={SCREENS.home}
 component={HomeScreen}
 />
 <Stack.Screen name={SCREENS.details} component={DetailsScreen} />
 </Stack.Navigator>
 </NavigationContainer>
```

主屏幕

### [**主屏**](http://web.archive.org/web/20221209132440/https://github.com/imariic/TestStacks/blob/main/src/screens/HomeScreen.js) 有两个组件——[**主电影**](http://web.archive.org/web/20221209132440/https://github.com/imariic/TestStacks/blob/main/src/components/MainMovie.js) 和 [**电影滚动**](http://web.archive.org/web/20221209132440/https://github.com/imariic/TestStacks/blob/main/src/components/MoviesScroll.js) 。MainMovie 中的根元素是 Stack。Stack 用于水平或垂直排列子组件。通过指定 **space** props，您可以计算子元素之间的距离。距离是这样计算的:**间距*空间**。在这种情况下，距离是 4*4 = 16。

**main movie . js
T2
**

```
 <Stack space={4} align="center" paddingTop={5}>
 <Image
 style={resolveStyle(styles.tabletImage, styles.mobileImage)}
 source={source}
 resizeMode="stretch"
 />
 <Text style={resolveStyle(styles.tabletTitle, styles.mobileTitle)}>
 {title}
 </Text>
 <Text style={resolveStyle(styles.tabletRating, styles.mobileRating)}>
 {vote_average}/10
 </Text>
 </Stack>
```

[**movie scroll**](http://web.archive.org/web/20221209132440/https://github.com/imariic/TestStacks/blob/main/src/components/MoviesScroll.js)是一个非常有趣的组件，因为它在每种设备类型上都有不同的表现。在手机上，组件将垂直呈现，但在平板电脑上是水平呈现。该组件包含行为与**视图**相同的**框**组件。值得一提的是，Stack 接受一组值来确定对齐方式。数组的第一个值将应用于手机，第二个值将应用于平板电脑。通过这种方式，您可以实现快速响应。

**电影角色。js**

```
 <Box padding={2}>
 <ScrollView
 horizontal={isItemHorizontal()}
 showsHorizontalScrollIndicator={false}
 showsVerticalScrollIndicator={false}>
 <Stack
 align={['center', 'stretch']}
 space={2}
 horizontal={isItemHorizontal()}>
 {constructMoviesList()}
 </Stack>
 </ScrollView>

 </Box>
```

详细信息屏幕

[**细节画面**](http://web.archive.org/web/20221209132440/https://github.com/imariic/TestStacks/blob/main/src/screens/DetailsScreen.js) 只有一个组件——[**movie details**](http://web.archive.org/web/20221209132440/https://github.com/imariic/TestStacks/blob/main/src/components/MovieDetails.js)。如果我们点击一部电影，细节屏幕就会打开。使用堆栈提供的**行**和**行**组件构建 MovieDetails 布局。行也是一个**间隔组件**。行和列用于垂直对齐项目**和**。堆栈和行非常相似，只是行不能水平对齐。

### `<ScrollView>`

`<Rows space={4} alignX={['center']} paddingTop={4}>`

`<Row width="content" padding={3}>`

`<Text style={titleStyle}>{title}</Text>`

`<Text style={descriptionStyle}>{overview}</Text>`

`</Row>`

`<Row width="content">`

`<Image`

`resizeMode="stretch"`

`style={imageStyle}`

`source={generateImageSource(poster_path)}`

`/>`

`</Row>`

`<Row width="content">`

`<Text style={releaseDateAndRatingStyle}>`

`Release date: {release_date}`

`</Text>`

`</Row>`

`<Row width="content">`

`<Text style={releaseDateAndRatingStyle}>Rating: {vote_average}</Text>`

`</Row>`

`</Rows>`

`</ScrollView>`

**MovieDetails.js**

结论

```
   <ScrollView>
     <Rows space={4} alignX={['center']} paddingTop={4}>
       <Row width="content" padding={3}>
         <Text style={titleStyle}>{title}</Text>
         <Text style={descriptionStyle}>{overview}</Text>
       </Row>
       <Row width="content">
         <Image
           resizeMode="stretch"
           style={imageStyle}
           source={generateImageSource(poster_path)}
         />
       </Row>
       <Row width="content">
         <Text style={releaseDateAndRatingStyle}>
           Release date: {release_date}
         </Text>
       </Row>
       <Row width="content">
         <Text style={releaseDateAndRatingStyle}>Rating: {vote_average}</Text>
       </Row>
     </Rows>
   </ScrollView>
```

Stacks 是一个非常有趣的库，是在 React Native 中构建布局的新方法。这使得开发更容易，因为你绝对不需要使用保证金。然而，库**并不完美**，因为 useCurrentBreakpoint 在 iOS 上无法工作。此外，该库没有使字体大小响应的选项，所以您必须手动完成

### Conclusion

Stacks is a very interesting library and a new way to build layouts in React Native. It makes the development easier because you absolutely do not need to use margins. However, the library **is not perfect** because useCurrentBreakpoint does not work on iOS. Also, the library does not have an option to make font sizes responsive so you have to do that manually