# 让你的 iOS PWA 感觉像原生应用的 6 个技巧

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/pwa-ios>

 3 月 30 日，期待已久的 iOS 11.3 更新发布，支持 iPhones 和 ipad 上的基本 PWA [功能，如服务人员和应用清单文件。虽然这些最终得到支持是很棒的，但 iOS 上的](/web/20221004130823/https://www.netguru.com/services/ios-mobile-app-development)[渐进式网络应用](/web/20221004130823/https://www.netguru.com/services/progressive-web-app-development)的用户体验仍然不完美。

这意味着许多现有的 pwa 在苹果设备上仍然存在一些严重的问题，同时在 Android 上提供了近乎原生的体验。在前端社区，失望的评论已经出现，问题和缺陷的列表很长。但是，不要失去希望！下面你会找到一些关于如何解决这些问题的提示，让你的 PWA 尽可能接近[本地移动应用](http://web.archive.org/web/20221004130823/https://www.netguru.com/services/mobile-development)。

## iOS 上的 PWA 支持有什么问题？

在最新的 iOS 更新中，苹果增加了对服务人员和应用清单文件的支持。现在，您可以利用服务人员的缓存，让您的 PWA 在没有互联网连接的情况下工作。请记住，这是 PWA 定义中的一项基本要求。不幸的是，苹果的实现有一些缺点。

相比 Android，服务人员的支持非常有限。您只能保存应用数据和缓存文件，但不能保存后台任务。存储也有 50MB 和“几周”的限制。

iOS 11.3 还引入了对清单文件的支持。但是我们的测试表明它远非完美。图标不能很好地工作(或者根本不能),也不支持启动屏幕——当应用程序加载时，你只会看到一个空白的白色屏幕。该应用程序每次从后台返回时都会重新加载，并且不支持推送通知和其他许多对移动应用程序至关重要的功能。总而言之，UX 的整体情况相当糟糕。

## 那又怎样？在 iOS 上提供 PWAs 的原生体验是不可能的吗？

谢天谢地，你可以做很多事情来改善你的应用程序的外观，并提供一个更好的 UX。通过一些简单的技巧，你可以构建一个在很多情况下与原生应用没有区别的应用。

我相信 pwa 是一个包容性、表演性和直观的网络的未来，这就是为什么我想分享一些克服 iOS 在 pwa 方面的局限性的技巧。

## #PROTIP 1:让应用程序图标再次变得伟大(在每台设备上)

我们注意到 iOS 不使用清单文件中的图标，这使得主屏幕上应用程序的快捷方式看起来很糟糕。有一个简单的解决方案可以解决这个问题——只需添加一个带有正确图像的 *apple-touch-icon* meta 标签。避免透明的图标——那些不会起作用。

```
<!-- place this in a head section -->
<link rel="apple-touch-icon" href="touch-icon-iphone.png">
<link rel="apple-touch-icon" sizes="152x152" href="touch-icon-ipad.png">
<link rel="apple-touch-icon" sizes="180x180" href="touch-icon-iphone-retina.png">
<link rel="apple-touch-icon" sizes="167x167" href="touch-icon-ipad-retina.png"> 
```

现在，您的应用程序将从一开始就看起来非常完美！

## #PROTIP 2:修复启动屏幕

在应用程序完全加载并准备好使用之前，通常会显示启动屏幕。不幸的是，iOS 不支持从清单生成的启动屏幕，这是它在 Android 上的工作方式。相反，它显示的是一个白色的空白屏幕，这绝对不是我们希望提供给用户的体验。

谢天谢地，我们已经找到了在苹果开发者页面中描述的解决方案。苹果支持自定义 meta 标签来指定预先生成的闪屏- ***苹果-触摸-启动-图像*** 。所以你只需要生成适当大小的闪屏图像，如下所示:

当你准备好令人惊叹的启动屏幕后，剩下唯一要做的事情就是像这样在头部链接它们:

```
<!-- place this in a head section -->
<meta name="apple-mobile-web-app-capable" content="yes" />
<link href="/apple_splash_2048.png" sizes="2048x2732" rel="apple-touch-startup-image" />
<link href="/apple_splash_1668.png" sizes="1668x2224" rel="apple-touch-startup-image" />
<link href="/apple_splash_1536.png" sizes="1536x2048" rel="apple-touch-startup-image" />
<link href="/apple_splash_1125.png" sizes="1125x2436" rel="apple-touch-startup-image" />
<link href="/apple_splash_1242.png" sizes="1242x2208" rel="apple-touch-startup-image" />
<link href="/apple_splash_750.png" sizes="750x1334" rel="apple-touch-startup-image" />
<link href="/apple_splash_640.png" sizes="640x1136" rel="apple-touch-startup-image" /> 
```

现在你可以看到启动时难看的白色屏幕不见了:

我们的 PWA 看起来好多了，不是吗？

## #PROTIP 3:自己创建一个“添加到主屏幕”弹出窗口！

在 Android 上，有一个本地弹出窗口，鼓励用户在主屏幕上添加应用程序，并通知他们我们的页面是 PWA。不幸的是，iPhone 上没有这种功能，所以我们的访问者甚至不知道我们的应用程序的功能。此外，在 iOS 上需要点击多达 3 次才能将应用程序添加到主屏幕。

但是不要担心，我们已经解决了这个问题！我们可以添加一个自定义弹出窗口，这将表明我们的应用程序可以添加到主屏幕。

你可以随意设计弹出窗口，我们例子如下所示。难的是只能在 Safari 中显示，而不能在独立模式下显示(当应用程序已经添加到主屏幕时)。您可以使用***window . navigator . standalone***来检查您的应用程序是否处于独立模式。

看一下这个片段:

```
// Detects if device is on iOS 
const isIos = () => {
  const userAgent = window.navigator.userAgent.toLowerCase();
  return /iphone|ipad|ipod/.test( userAgent );
}
// Detects if device is in standalone mode
const isInStandaloneMode = () => ('standalone' in window.navigator) && (window.navigator.standalone);

// Checks if should display install popup notification:
if (isIos() && !isInStandaloneMode()) {
  this.setState({ showInstallMessage: true });
} 
```

创建适当的弹出组件并将检测代码粘贴到适当的生命周期方法中后，最终产品将如下所示:

请记住，在 iPad 上，共享按钮位于屏幕顶部，地址栏旁边，因此您应该相应地更改弹出窗口的位置。

## #PROTIP 4:坚持一切

由于 iOS 会在每次启动/转到后台时重启你的应用程序，你应该自己保持它的状态。如果你使用 React 和 Redux，有一些很棒的软件包可以帮助你- [redux-persist](http://web.archive.org/web/20221004130823/https://github.com/rt2zz/redux-persist) 和 [react-router-redux](http://web.archive.org/web/20221004130823/https://github.com/reactjs/react-router-redux) 。对于 Vue，您可以使用类似的 vuex-persist 和 vuex-router-sync。当然，您可以创建自己的解决方案，这将是最适合您的情况。

在 React 中，react-router-redux 将在默认情况下在装载时重定向到 URL 中指定的路由。虽然它可能可以正常使用，但我们更喜欢存储中的路线。这是一个如何实现这一目标的示例:

```
import { ConnectedRouter, push } from 'react-router-redux';

class PersistedConnectedRouter extends ConnectedRouter {
  componentWillMount() {
    const { store: propsStore, history, isSSR } = this.props;
    this.store = propsStore || this.context.store;

    if (!isSSR) {
      this.unsubscribeFromHistory = history.listen(this.handleLocationChange);
    }

    //this is the tweak which will prefer persisted route instead of that in url:
    const location = this.store.getState().router.location || {};
    if (location.pathname !== history.location.pathname) {
      this.store.dispatch(push(location.pathname));
    }
    this.handleLocationChange(history.location);
    // --
  }
}

export default PersistedConnectedRouter; 
```

## #PROTIP 5:关注导航

为了确保你的应用程序可以在独立模式下使用，你必须检查你是否正确地实现了你的导航。在苹果设备上没有后退按钮，所以你必须确保用户能够使用你的应用程序内置的导航从任何屏幕返回。

您可以通过显示后退按钮或为 iOS 设备添加额外的菜单栏来实现这一点。

## #PROTIP 6:准备离线

为了提供真正的本土体验，你应该为弱连接或根本没有连接做好准备。在一些简单的情况下，持久存储数据就足够了，但这是我们可以利用我们全新的(在 iOS 上)服务工作者 API 的地方——我们能够在那里缓存网络请求(例如 API 调用)。

为了利用服务人员的缓存，您可以使用 Google 提供的[示例代码。请确保您已经阅读了链接的文章，以了解该模板的利弊。](http://web.archive.org/web/20221004130823/https://googlechrome.github.io/samples/service-worker/basic/)

```
// source: https://googlechrome.github.io/samples/service-worker/basic/
/*
 Copyright 2016 Google Inc. All Rights Reserved.
 Licensed under the Apache License, Version 2.0 (the "License");
 you may not use this file except in compliance with the License.
 You may obtain a copy of the License at
     http://www.apache.org/licenses/LICENSE-2.0
 Unless required by applicable law or agreed to in writing, software
 distributed under the License is distributed on an "AS IS" BASIS,
 WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 See the License for the specific language governing permissions and
 limitations under the License.
*/

// Names of the two caches used in this version of the service worker.
// Change to v2, etc. when you update any of the local resources, which will
// in turn trigger the install event again.
const PRECACHE = 'precache-v1';
const RUNTIME = 'runtime';

// A list of local resources we always want to be cached.
const PRECACHE_URLS = [
  'index.html',
  './', // Alias for index.html
  'styles.css',
  '../../styles/main.css',
  'demo.js'
];

// The install handler takes care of precaching the resources we always need.
self.addEventListener('install', event => {
  event.waitUntil(
    caches.open(PRECACHE)
      .then(cache => cache.addAll(PRECACHE_URLS))
      .then(self.skipWaiting())
  );
});

// The activate handler takes care of cleaning up old caches.
self.addEventListener('activate', event => {
  const currentCaches = [PRECACHE, RUNTIME];
  event.waitUntil(
    caches.keys().then(cacheNames => {
      return cacheNames.filter(cacheName => !currentCaches.includes(cacheName));
    }).then(cachesToDelete => {
      return Promise.all(cachesToDelete.map(cacheToDelete => {
        return caches.delete(cacheToDelete);
      }));
    }).then(() => self.clients.claim())
  );
});

// The fetch handler serves responses for same-origin resources from a cache.
// If no response is found, it populates the runtime cache with the response
// from the network before returning it to the page.
self.addEventListener('fetch', event => {
  // Skip cross-origin requests, like those for Google Analytics.
  if (event.request.url.startsWith(self.location.origin)) {
    event.respondWith(
      caches.match(event.request).then(cachedResponse => {
        if (cachedResponse) {
          return cachedResponse;
        }

        return caches.open(RUNTIME).then(cache => {
          return fetch(event.request).then(response => {
            // Put a copy of the response in the runtime cache.
            return cache.put(event.request, response.clone()).then(() => {
              return response;
            });
          });
        });
      })
    );
  }
}); 
```

在为您想要缓存的资产设置了正确的路径并注册了一个服务人员之后，您应该可以开始工作了(离线)。指示应用程序离线也是很好的——您可以添加在线/离线事件监听器来显示通知。

```
componentDidMount() {
    window.addEventListener('online', () => this.setOnlineStatus(true));
    window.addEventListener('offline', () => this.setOnlineStatus(false));
  }

  componentWillUnmount() {
    window.removeEventListener('online');
    window.removeEventListener('offline');
  }

  setOnlineStatus = isOnline => this.setState({ online: isOnline }) 
```

这就是我们的示例应用程序的最终结果:

有待解决的问题

## 这些提示将帮助您为 iOS 上的 PWA 提供接近原生的 UX。但是，当然，仍然有一些问题需要解决。我们仍然不支持:

后台同步，

*   推送通知，
*   一些 API 像 TouchID，ARKit，在 App Payments，或者 iPad 上的 Split View，
*   方向锁，
*   状态栏颜色变化，
*   任务管理器中正确的应用程序屏幕(将显示当前的应用程序屏幕，而不是启动图像)。
*   此外，您应该记住，缓存和任何本地存储的数据不会在 Safari、WebView 和 Web.App 之间共享。这意味着，例如，用户必须在将您的应用程序添加到主屏幕后再次登录，这在某些情况下可能会导致不良 UX。

有想法吗？分享给我们吧！