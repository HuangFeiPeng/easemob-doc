# Demo（EaseIM App）介绍

<Toc />

环信即时通讯 IM Android 端提供示例应用可供体验。

1. [下载 Demo](https://downloadsdk.easemob.com/mp/downloads/sdk/imsdkdemo_android-4.1.2.apk)。

2. 输入你的登录账号和密码。

3. 选择同意《环信服务条款》与《环信隐私协议》，然后点击 **登录** 登录 Demo。

<!--![img](@static/images/demo/android_login.png)-->

## 代码下载

您可以通过以下两种方式获取到源代码：
- 下载代码压缩包：[IM SDK 及 Demo 下载](https://downloadsdk.easemob.com/downloads/easemob-sdk-4.1.2.zip)
欢迎大家提交 PR 改进和修复 EaseIM 和 EaseIMKit 中的问题。
## 导入 EaseIM

下载 Android SDK 压缩包，然后解压。解压后在 examples 文件夹下，即为 EaseIM 的工程目录。
### 导入到 Android Studio
打开 Android Studio，点击 File > Open，打开 EaseIm3.0 根目录即可。
:::notice
环信（EaseIM）中使用了百度地图，在开发你自己的应用时，请在 AndroidManifest.xml 中把相应的 key 改成你自己申请的。
:::
### 导入到 Eclipse
点击 File→Import→点击 Android 下的子目录→Next→选择输入 EaseIm3.0 的根路径→Finish。
## 用到的第三方库
- androidx.appcompat:appcompat：appcompat 库（对应以前的 appcompat-v7 库）必须添加；
- com.google.android.material:material：material 库（对应以前的 design 库），如果用到 TabLayout，CardView 等 material degign 控件时需要添加；
- androidx.constraintlayout:constraintlayout：constraintlayout 布局，如果没有用到可以不用添加；
- androidx.lifecycle:lifecycle-extensions：ViewModel 和 LiveData 库，如果没有用到可以不用添加；
- com.android.support:multidex：工程总方法数超过 64k 时，需要用这个库做分包处理，可以删除；
- play-services-base和firebase-messaging：谷歌推送所需要的库，如果不用该推送可以不用添加；
- com.scwang.smartrefresh:SmartRefreshLayout：下拉刷新及上拉加载更多库，可以不用添加；
- androidx.swiperefreshlayout:swiperefreshlayout：下拉刷新库，可以不用添加；
- androidx.room：android 数据库，可以不用添加；
- com.huawei.hms:push：华为推送所需要的库，如果不用该推送可以不用添加；
- com.meizu.flyme.internet:push-internal：魅族推送所需要的库，如果不用该推送可以不用添加；
- vivo_push_v2.3.1.jar： vivo 推送所需要的库，如果不用该推送可以不用添加；
- oppo_push_v2.1.0jar: oppo 推送所需要的库，如果不用该推送可以不用添加；
- mi_push_v3.6.12.jar: 小米推送所需要的库，如果不用该推送可以不用添加；
- bolts-android-1.2.0.jar、Parse-1.9.4.jar：Demo 中的用户信息存储在 Parse，这两个库是 Parse 所需要的库，开发者如果没用 Parse 存储，不要复制到自己项目中。
## 主要模块介绍
- **聊天模块** – 主要展示了如何依赖 EaseIMKit 实现聊天列表，如何扩展消息类型及如何增加扩展菜单等的逻辑。
- **会话列表模块** – 主要展示了如何依赖 EaseIMKit 实现会话列表的逻辑及实现系统消息的具体逻辑。
- **联系人模块** – 主要展示了如何依赖 EaseIMKit 实现联系人列表的逻辑。
- **聊天设置模块** – 主要展示了 IM SDK 提供的对于群聊及聊天室的对成员及群组属性的操作。
- **设置模块** – 主要展示了 IM SDK 对于新消息的设置及消息免打扰，群组等通用设置。
- **开发者模块** – 主要展示了 IM SDK 提供的一些常规的开发者可以设置的功能。
## 主要类介绍
- **DemoHelper**：环信（EaseIM）全局帮助类，主要功能为初始化 IM SDK，初始化 EaseIMKit 相关及注册对话类型等；
- **ConversationListFragment**：继承自 EaseIMKit 中的 EaseConversationListFragment，展示了扩展条目长按事件，在列表前端添加布局及条目点击事件实现等；
- **ChatActivity及ChatFragment**：ChatFragment 继承自 EaseIMKit 中的 EaseChatFragment，展示了扩展条目长按事件，预置条目长按菜单及对重写部分长按事件功能，展示了如何重置及添加更多扩展功能，并展示了实现了头像点击事件及正在输入等事件的相应；
- **ContactListFragment**：继承自 EaseIMKit 中的 EaseContactListFragment，展示了添加头布局，添加条目长按功能及实现条目点击事件等；
- **GroupDetailActivity**：实现了如下功能：添加群成员，修改群公告及群介绍，上传共享文件，进行群组管理，设置消息免打扰及解散或者退出群组等。
## 部分 UI 展示

<img src=@static/images/android/app-demo-ui-1.jpeg  title=“会话列表” width="200"/>&nbsp;&nbsp;
<img src=@static/images/android/app-demo-ui-2.jpeg  title=联系人列表 width="200"/>&nbsp;&nbsp;
<img src=@static/images/android/app-demo-ui-3.jpeg  title=聊天页面 width="200"/>
<!--
![会话列表](@static/images/android/app-demo-ui-1.jpeg?w=50)
<br>
![联系人列表](@static/images/android/app-demo-ui-2.jpeg?w=50)
<br>
![聊天页面](@static/images/android/app-demo-ui-3.jpeg?w=50)
-->