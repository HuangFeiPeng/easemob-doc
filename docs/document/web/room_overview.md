# 聊天室概述

<Toc />

## 功能描述

聊天室是支持多人加入的类似 Twitch 的组织。聊天室中的成员没有固定关系，用户离线后，超过 2 分钟会自动退出聊天室。聊天室成员在离线后，不会收到推送消息。聊天室可以应用于直播、消息广播等。若需调整该时间，需联系环信商务经理。

聊天室的使用限制视不同套餐版本而定，请参见 使用限制。

本文以及接下来几篇主要介绍聊天室管理功能，如需查看消息相关内容，参见 消息管理。

### 群组与聊天室的区别

群组和聊天室均为支持多人沟通的即时通讯系统。两者的区别在于，群组中的成员会有固定的强的关系，成员加入后会长时间的在群组中。聊天室中的成员没有固定关系，类似与一个开放的空间，用户可以自由加入，离开即退出聊天室。

#### 群组与聊天室的功能对比

见 [群组概述](group_overview.html)。

### 功能列表

#### 聊天室角色介绍

| 聊天室成员角色类型<div style="width: 160px;"></div> | 描述 | 管理权限 |
| :------------- | :----------------------------------------------------- | :----------------------------------------------------------- |
| 普通成员       | 不具备管理权限的聊天室成员。                           | 普通成员可以修改自己的聊天室资料。                           |
| 聊天室管理员   | 由聊天室所有者授权，协助进行管理，拥有一定的管理权限。 | 管理员可以对普通成员进行管理。   |
| 聊天室所有者   | 一般是聊天室的创建者，在聊天室中拥有最高的权限。   | 聊天室所有者可以指定管理员，解散聊天室，更改聊天室名称描述等信息，以及对聊天室成员进行管理。 |

#### 聊天室操作

| 功能           | 描述                                                         |
| :------------- | :----------------------------------------------------------- |
| 创建聊天室     | 只有被赋予 [超级管理员](/document/server-side/chatroom.html#管理超级管理员) 权限的用户有权限创建聊天室。聊天室成员数会受到版本指定聊天室最大成员数的限制。 |
| 加入聊天室     | 没有被加入黑名单的所有 app 用户可自由加入聊天室。                                   |
| 离开聊天室     | 所有聊天室成员都可以自由退出聊天室；也可能被动离开聊天室，原因分为：被管理员移出聊天室、聊天室解散和用户账号离线。   |
| 销毁聊天室     | 需要聊天室所有者权限。                                       |
| 获取聊天室列表 | 所有 app 用户有权限获取聊天室列表。 |
| 获取聊天室详情 | 所有聊天室成员有权限获取聊天室详情。        |
| 修改聊天室名称 | 需要聊天室所有者权限。                   |
| 聊天室公告     | 仅聊天室所有者有权限编辑公告、删除公告。<br/>公告更新会通过监听同步给所有成员。 |

#### 聊天室成员管理

| 功能               | 描述                                                         |
| :----------------- | :----------------------------------------------------------- |
| 将聊天室成员加入禁言名单 | 需要聊天室所有者或管理员权限，可以对单个聊天室成员进行禁言。   |
| 聊天室全员禁言     | 需要聊天室所有者或管理员权限。全员禁言时，默认聊天室所有者和管理员不禁言。 |
| 白名单管理         | 需要聊天室所有者或管理员权限。全员禁言时，白名单的成员可以发消息。        |
| 聊天室管理员       | 仅聊天室所有者可以对成员指定或移除管理员权限。          |
| 黑名单管理         | 需要聊天室所有者或管理员权限，被加入黑名单的成员会被移出聊天室。黑名单中的成员需要聊天室所有者主动从黑名单移除后才能再次加入聊天室。    |