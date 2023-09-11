# Demo（EaseIM App）介绍

<Toc />

环信即时通讯 IM iOS 端提供示例应用可供体验。

1. [下载 Demo](https://www.easemob.com/download/demo)。

2. 输入你的手机号，获取验证码，然后输入。

3. 选择同意《环信服务条款》与《环信隐私协议》，然后点击 **登录** 登录 Demo。

![img](@static/images/demo/ios_login.png)

## 代码下载

您可以通过以下两种方式获取到源代码：

- 下载代码压缩包：[IM SDK 及 Demo 下载](https://www.easemob.com/download/im)
- 下载源代码：[github 源码地址](https://github.com/easemob/chat-ios)

欢迎大家提交 PR 改进和修复 EaseIM 和 EaseIMKit 中的问题。

## 运行 EaseIM 工程

从 [IM SDK 及 Demo 下载](https://www.easemob.com/download/im) 下载 iOS SDK 压缩包，然后解压。解压后在 `EaseIM` 文件夹下，即为 EaseIM 的工程目录。

终端 cd 到 EaseIM 的 `podfile` 目录下，终端执行 `pod install` 命令，等待下载完所有的 pod 依赖库，即可打开 `EaseIM.xcworkspace`，运行 EaseIM demo 进行自定义再次开发。

## 使用到的 pod 库

- 环信 SDK pod 'HyphenateChat', '3.8.0'
- 环信 IM UI 库 pod 'EaseIMKit', '3.8.0.1'
- 环信音视频 UI 库 pod 'EaseCallKit', '3.8.0.3'
- 声网音视频 SDK pod 'AgoraRtcEngine_iOS', '3.3.1'

三方库包含

```
* pod 'MBProgressHUD'
* pod 'Masonry'
* pod 'MJRefresh'
* pod 'SDWebImage'
```

## 主要模块介绍

Demo 中有几大 UI 功能模块，在集成时将对应的模块添加到工程中即可。

- Helper——自定义库和页面，第三方库，全局通用模块
- Chat——聊天模块
- Conversation——会话列表模块
- Communicate——实时音视频模块（包含 1v1 实时通话以及多人实时通话的功能
- Contact——好友列表模块
- Group——群组模块
- Chatroom——聊天室模块

在集成时，必须要先向自己的工程中导入 `Helper` 模块，然后再根据自己的需求导入其他模块。

## 主要类介绍

- **EaseIMHelper**：环信（EaseIM）全局帮助类，Demo 的单例类，主要是全局监听接收消息，好友，群组，聊天室等相关事件的回调，从而进行对应的处理；
- **EMConversationsViewController**：Demo 的会话列表功能页面，展示了扩展条目侧滑事件及条目点击事件实现等；
- **EMChatViewController**：Demo 的聊天功能页面，展示了扩展条目长按事件，预置条目长按菜单及对重写部分长按事件功能，展示了如何重置及添加更多扩展功能，并展示了实现了头像点击事件及正在输入等事件的相应；
- **EMContactsViewController**：Demo 展示的联系人页面，展示了添加条目长按侧滑及实现条目点击事件等；
- **EMGroupInfoViewController**：实现了如下功能：添加群成员，修改群公告及群介绍，上传共享文件，进行群组管理，设置消息免打扰及解散或者退出群组等。

## 部分 UI 展示

![联系人列表](@static/images/android/app-demo-ui-2.jpeg)

![聊天页面](@static/images/android/app-demo-ui-3.jpeg)