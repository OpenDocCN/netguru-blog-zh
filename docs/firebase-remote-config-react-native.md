# 如何让你的应用程序优雅地响应地区变化

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/firebase-remote-config-react-native>

 在某些情况下，您会希望根据应用程序的使用位置来更改应用程序的外观。例如，您可能希望在某些地区显示不同的内容，或者根据用户的位置更改调色板。今天我将向你展示如何使用 Firebase 远程配置和我们伟大的粘性视差头文件库！

首先，我们来谈谈 [Firebase](/web/20220924154013/https://www.netguru.com/services/firebase-development) 远程配置。这是 Firebase“堆栈”的一个非常酷的功能，你可以在几个小时之内在你的应用程序中实现它。如果你早点使用它，它可以提高你的开发速度——在开发过程的后期，你可以用它从服务器自动配置应用程序，而不需要用户通过谷歌 Play 商店或应用商店更新它！此外，您可以根据地区或其他变量来个性化您的应用程序。所有这些都是完全免费的，所以让我们继续看 React Native 中的[实现吧！](http://web.archive.org/web/20220924154013/https://www.netguru.com/services/react-native-development)

正如我前面所说的，我将向您展示 Firebase Remote Config 使用粘性视差头文件库提供的可能性，以及它附带的示例应用程序，您可以在这里找到:[https://github.com/netguru/sticky-parallax-header](http://web.archive.org/web/20220924154013/https://github.com/netguru/sticky-parallax-header)。我们可以通过远程配置轻松扩展其功能，这将允许您根据位置更改应用程序的配色方案。在我们的例子中，让我们为来自波兰的用户显示一组颜色，为世界其他地方的用户显示其他颜色。你要做的第一件事是安装 *react-native-firebase* 包和*react-native-firebase/remote-config*包。

接下来您要做的是创建一个 RemoteConfigService，它将初始化 react-native-firebase/remote-config 库。方法如下:

```
import remoteConfig from '@react-native-firebase/remote-config';

const RemoteConfigService = {
  initialize: async () => {
    await remoteConfig().setConfigSettings({
      minimumFetchIntervalMillis: 300,
    });
    await remoteConfig().setDefaults({region: 'World'});
    await remoteConfig().fetchAndActivate();
  },
};

export default RemoteConfigService;
```

如您所见，我创建了一个 initialize 方法，将默认的远程配置获取间隔设置为 300 毫秒。这将使我能够测试是否一切正常。在生产版本中，您可能想减少获取远程配置的频率。接下来是设置默认的配置值。让我们为我们的默认区域指定“World”值——如果我们在波兰，在下一次方法调用之后，它应该会改变。FetchAndActivate 负责获取远程配置并在我们的应用程序中激活它。你必须尽快调用这个方法，所以我建议在 App.js/App.tsx 文件中调用它。

最后一步是在屏幕中获得一个远程配置，我们将在其中改变我们的粘性视差标题的样式。但是首先，我们必须在 RemoteConfigService 中为它创建另一个方法。

```
import remoteConfig from '@react-native-firebase/remote-config';

const RemoteConfigService = {
  …,
  getRemoteValue: (key) => remoteConfig().getValue(key),
};

export default RemoteConfigService;
```

现在让我们转到主屏幕，在那里我有我们的粘性视差标题。我们可以将负责获取远程配置值的代码放在 componentDidMount 方法中。在我们得到它之后，我们必须设置我们的状态，以便组件能够重新呈现。也可以在 App.js 中完成，并通过导航道具传递到屏幕上，不过我们换个方式来做:

```
componentDidMount() {
    …,
    const regionConfig = RemoteConfigService.getRemoteValue('region')?.asString()
    this.setState({region: regionConfig})
  }
```

现在我们只需要为我们的区域和方法创建样式，将颜色应用到我们的粘性视差头。就是这样！您刚刚创建了一个响应地区变化的应用程序。

```
getBackgroundColor = () => {
    const {region} = this.state;
    let style;
    if (region === 'Poland') {
      style = styles.homeScreenHeaderPoland;
    } else {
      style = styles.homeScreenHeader;
    }
    return style;
};

renderHeader = () => {
    const {region} = this.state;
    return (
      <View style={[styles.headerWrapper, this.getBackgroundColor()]}>
        <Image
          resizeMode="contain"
          source={require('../../assets/images/logo.png')}
          style={styles.logo}
        />
      </View>
    );
  };
```

**警告！在 react-native-firebase/remote-config 版本 8.1.1 中，远程配置的初始化当前被中断。您可以将软件包降级到版本 5 或等待更新。**