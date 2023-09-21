## 创建产品及应用

本文档主要指导您如何在环信推送控制台创建产品和应用，以及如何配置应用。
创建推送前，需要先进行如下操作：

![img](@static/images/instantpush/push_createproduct_app.png)

### 1、创建环信应用

注册环信账号，并登录环信通讯云管理后台：[环信通讯云管理后台](/document/v2/privatization/uc_configure.html#创建应用)，点击【添加应用】，根据提示填写应用信息，创建您的第一个应用。

![img](@static/images/instantpush/push_create_app.jpg)

### 2、开通PUSH服务

APP创建成功后，将显示至【应用列表】中，选中创建的APP，点击其【管理】按钮，为您的APP开通相应业务。 

左侧菜单栏为环信支持的私有化业务，这里选中【即时推送】→【服务概览】，**私有化服务默认开通PUSH业务**。

![img](@static/images/instantpush/push_open_app.jpg)


### 3、集成环信PUSH服务

#### 3.1 SDK下载

环信推送与IM使用相同的SDK，SDK下载地址：[SDK下载页](/document/v2/privatization/uc_private.html) 。

![img](@static/images/instantpush/push_sdk_download.jpg)

:::notice
环信IM用户可直接使用，无需进行移动端集成。
:::

#### 3.2 推送集成

详细Android 推送集成参考文档:[Android 推送集成](push_integration_process_android.html)
详细iOS SDK集成参考文档:[iOS SDK集成](push_integration_process_ios.html)

### 4、配置推送证书

点击进入【配置证书】，点击【添加推送证书】进行厂商证书的添加。
环信PUSH支持全平台系统下发，覆盖谷歌、华为、小米、魅族、OPPO、vivo等主流手机厂商通道，iOS双证书支持。 PUSH与IM使用相同的SDK，证书配置可以通用。

![img](@static/images/instantpush/push_add_certificate.png)

### 5、绑定推送用户

未使用环信IM的用户，需要单独创建用户并进行用户体系集成。
点击【应用概览】进入【用户认证】界面，点击【创建IM用户】可以在页面中添加用户，也可使用REST API进行用户配置。
用户体系集成介绍参考文档：[用户体系集成](/document/v2/server-side/account_system.html) 

![img](@static/images/instantpush/push_bind_user.png)
