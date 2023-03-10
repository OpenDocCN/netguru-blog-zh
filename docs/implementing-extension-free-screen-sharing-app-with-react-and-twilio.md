# 使用 React 和 Twilio 实现免扩展的屏幕共享应用

> 原文：<http://web.archive.org/web/20230307163032/https://www.netguru.com/blog/implementing-extension-free-screen-sharing-app-with-react-and-twilio>

 实时通信和屏幕共享的当前状态

在过去的几年里，通过互联网的实时通信变得越来越流行。文本聊天和视频对话是现代网络应用的重要组成部分。对演示、团队会议或其他类似用例有帮助的最有趣、最有用的功能之一是屏幕共享。

话虽如此，但屏幕共享的实际实现并没有那么简单。由于在不同浏览器中实现的差异，没有通用的编程方法可以在所有浏览器中工作。更糟糕的是，出于安全考虑，屏幕共享功能的实现包括实现和发布最终用户必须下载和安装的浏览器扩展。这一过程大大降低了用户体验，并可能导致用户根本不使用该功能。

幸运的是，W3C [Web 实时通信工作组](http://web.archive.org/web/20221007183440/http://www.w3.org/2011/04/webrtc/)正在研究 API 的规范，应该可以解决上述问题。然而，并不是所有的浏览器供应商都发布了他们应用程序的更新版本。我们可以在以下地址查看实现状态: [Chrome](http://web.archive.org/web/20221007183440/https://bugs.chromium.org/p/chromium/issues/detail?id=326740) 、 [Firefox](http://web.archive.org/web/20221007183440/https://bugzilla.mozilla.org/show_bug.cgi?id=1321221) 、[微软 Edge](http://web.archive.org/web/20221007183440/https://blogs.windows.com/msedgedev/2018/05/02/bringing-screen-capture-to-microsoft-edge-media-capture-api/) 和 [Safari](http://web.archive.org/web/20221007183440/https://bugs.webkit.org/show_bug.cgi?id=186294) 。

## 应用程序

以下应用程序使用 [WebRTC 适配器库](http://web.archive.org/web/20221007183440/https://github.com/webrtchacks/adapter)来最小化不同浏览器中新 API 的当前实现中的现有差异。[这篇文章展示了如何在不同的用例中使用这个库，并解释了创建它的原因。](http://web.archive.org/web/20221007183440/https://blog.mozilla.org/webrtc/getdisplaymedia-now-available-in-adapter-js/)

为了让代码库尽可能小，我们使用了 Twilio 的可编程视频和它的 API。如果你没有 Twilio 账户，你可以在这里阅读如何创建一个免费试用账户[。](http://web.archive.org/web/20221007183440/https://www.twilio.com/docs/usage/tutorials/how-to-use-your-free-trial-account)

应用程序由 [node.js backend](/web/20221007183440/https://www.netguru.com/blog/node-js-backend) 和 React frontend 组成。您可以在存储库的 [readme](http://web.archive.org/web/20221007183440/https://github.com/ksocha/twilio-screensharing/blob/master/README.md) 文件中找到关于如何运行应用程序的更多细节。

## 它是如何工作的

要连接到视频室，我们需要执行某些操作:

1.  生成访问令牌以获得对 Twilio 服务的访问权限。
2.  创建音频和视频轨道，从麦克风和摄像头传输数据。
3.  使用生成的令牌和曲目连接到视频室。

我们将使用 /token 端点来获得一个[访问令牌](http://web.archive.org/web/20221007183440/https://www.twilio.com/docs/video/tutorials/user-identity-access-tokens)，它将允许我们进行实际的调用。

```
app.get("/token", (req, res) => {
  const token = new AccessToken(
    process.env.TWILIO_ACCOUNT_SID,
    process.env.TWILIO_API_KEY,
    process.env.TWILIO_API_SECRET
  );

  token.addGrant(new VideoGrant());

  token.identity = req.query.user;

  res.send({ token: token.toJwt() });
});
```

在实际场景中，用户的身份应该从应用程序的身份验证层获取，但是在我们的例子中，它将作为请求的参数传递。通过传递身份，我们确保每个用户只能有一个与给定视频房间的活动连接。

加入房间非常简单——我们使用 Twilio API 创建视频和音频轨道，并使用它们连接到房间。这些曲目将用于从我们的设备中捕获和发布视频和音频到 Twilio 的服务器上。

```
const token = await this.getToken();

const localVideoTrack = await TwilioVideo.createLocalVideoTrack();
this.setState({ localVideoTrack });

const localAudioTrack = await TwilioVideo.createLocalAudioTrack();
this.setState({ localAudioTrack });

const videoRoom = await TwilioVideo.connect(
  token,
  {
    name: roomName,
    tracks: [localVideoTrack, localAudioTrack],
    insights: false
  }
);
```

要加入房间，我们需要键入房间的用户名和名称。

![screensharing1](img/1bd5ae364eab46435943bc830b007c54.png)

点击“加入”按钮后，我们应该能够看到来自相机的图像。

共享屏幕

![screensharing2](img/2b9c9db4dfca14c0738670c4fb580067.png)

如果我们的浏览器支持屏幕共享 API，那么应该启用开始共享按钮。如果我们点击按钮，屏幕应该是共享的，我们应该看到它的预览，而不是来自相机的图像。

## Sharing the screen

If our browser supports screen sharing API, the Start sharing button should be enabled. If we click on the button, the screen should be shared and we should see its preview instead of the image from the camera.

共享屏幕比加入房间稍微复杂一点。Twilio javascript 包没有任何帮助方法来帮助我们创建将用于发布屏幕内容的视频轨道。相反，我们将使用 WebRTCnavigator . media devices . getdisplaymediaAPI。

![screensharing3](img/e32d05eee40036481f020207769ccc9c.png)

之后，我们需要发布新的轨迹，并取消发布从相机中捕捉图像的旧轨迹(在本例中，我们不需要同时发布它们)。

无扩展方法的优缺点

```
const stream = await navigator.mediaDevices.getDisplayMedia({
  video: true
});

const newScreenTrack = first(stream.getVideoTracks());

this.setState({
  screenTrack: new TwilioVideo.LocalVideoTrack(newScreenTrack)
});
```

赞成的意见

## 骗局

您可以在 [GitHub](http://web.archive.org/web/20221007183440/https://github.com/ksocha/twilio-screensharing) 上查看该项目

You can view the project on [GitHub](http://web.archive.org/web/20221007183440/https://github.com/ksocha/twilio-screensharing)