# 离线推送设置

<Toc />

本文展示如何调用环信即时通讯 RESTful API 实现离线推送，包括设置离线推送通知显示的昵称、推送通知方式及免打扰模式。

:::tip
若要使用离线推送的高级功能，即设置推送通知模式、免打扰模式和自定义推送模板，你需要在环信即时通讯云控制后台中点击你的应用后选择 **服务管理** > **服务概览** 开通离线推送高级功能。
:::

## 公共参数

以下表格列举了即时通讯 RESTful API 的公共请求参数和响应参数：

### 请求参数

| 参数       | 类型   | 是否必需 | 描述                                                                                                                                            |
| :--------- | :----- | :------- | :---------------------------------------------------------------------------------------------------------------------------------------------- |
| `host`     | String | 是       | 访问 RESTful API 的域名或服务器信息。<br/>-公有云集成为 环信即时通讯控制台的 `即时通讯->服务概览`页面下的 `域名配置- Rest Api`。 <br/> -私有化集成为部署后 `服务器地址:端口`。 |
| `org_name` | String | 是       | 每个公司（组织）分配的唯一标识。详见 环信即时通讯控制台的 `应用概览->应用详情`页面下的 `应用信息-Orgname`。  |
| `app_name` | String | 是       | 创建应用时填入的应用名称。详见 环信即时通讯控制台的 `应用概览->应用详情`页面下的 `应用信息-Appname`。   |
| `username` | String | 是       | 环信用户 ID。                                                                                                                                   |

### 响应参数

| 参数              | 类型   | 描述                                                                           |
| :---------------- | :----- | :----------------------------------------------------------------------------- |
| `action`          | String | 请求方式。                                                                     |
| `organization`    | String | 环信即时通讯 IM 为每个公司（组织）分配的唯一标识，与请求参数 `org_name` 相同。 |
| `application`     | String | 环信即时通讯服务分配给每个 app 的唯一内部标识，开发者无需关注。                |
| `uri`             | String | 请求 URL。                                                                     |
| `timestamp`       | Long   | HTTP 响应的 Unix 时间戳，单位为毫秒。                                          |
| `applicationName` | String | 你在环信即时通讯云控制台创建应用时填入的应用名称，与请求参数 `app_name` 相同。 |
| `duration`        | Int    | 从发送 HTTP 请求到响应的时长，单位为毫秒。                                     |

## 认证方式

环信即时通讯 REST API 要求 Bearer HTTP 认证。每次发送 HTTP 请求时，必须在请求头部填入如下 `Authorization` 字段：

`Authorization: Bearer YourAppToken`

为提高项目的安全性，环信使用 Token（动态密钥）对即将登录即时通讯系统的用户进行鉴权。即时通讯 REST API 仅支持使用 App Token 的鉴权方式，详见 [使用 App Token 鉴权](easemob_app_token.html)。


## 绑定和解绑推送信息 

推送消息时，设备与推送信息会进行绑定，包括设备 ID、推送证书和 device token。

- 设备 ID：SDK 为每个设备分配的唯一标识符。

- device token：第三方推送服务提供的推送 token，该 token 用于标识每台设备上的每个应用，第三方推送服务通过该 token 明确消息是发送给哪个设备的，然后将消息转发给设备，设备再通知应用程序。例如，对于 FCM 推送，device token 指初次启动你的应用时 FCM SDK 为客户端应用实例生成的注册令牌 (registration token)。

- 推送证书：推送证书由第三方推送服务提供，一个 app 对应一个推送证书。

用户从第三方推送服务器获取 device token 和证书名称，然后向环信即时通讯服务器上传，服务器根据 device token 向用户推送消息。

你可以调用该接口对设备与推送信息进行绑定或解绑。

### HTTP 请求

```
PUT https://{host}/{org_name}/{app_name}/users/wzy/push/binding
```

#### 路径参数

参数及说明详见 [公共参数](#公共参数)。

#### 请求 header

| 参数            | 类型   | 描述                                                         | 是否必需 |
| :-------------- | :----- | :----------------------------------------------------------- | :------- |
| `Content-Type`  | String | 内容类型。请填 `application/json`。                          | 是       |
| `Authorization` | String | `Bearer ${YourAppToken}` Bearer 是固定字符，后面加英文空格，再加上获取到的 app token 的值。 | 是       |

### 请求 body

| 参数            | 类型   | 描述                                                         | 是否必需 |
| :-------------- | :----- | :----------------------------------------------------------- | :------- |
| `device_id`     | String | 移动端设备标识，服务端用于识别设备，进行推送信息的绑定和解绑。 | 是       |
| `notifier_name` | String | 推送证书名称。<br/> - 传入的证书名称必需与你在环信控制台的**添加推送证书**页面上填写的证书名称一致，否则推送失败。<br/> - 若 `notifier_name` 为空，表示解除当前设备与所有推送信息的绑定。 | 是       |
| `device_token`  | String | 推送设备 token。错误的信息会推送失败，且服务端自动解除绑定。若 `device_token` 为空，则会解除当前用户当前设备 ID 和当前证书名的绑定。 | 是       |

### HTTP 响应

#### 响应 body

如果返回的 HTTP 状态码为 `200`，表示请求成功，响应包体中包含以下字段：

| 参数        | 类型   | 描述                                                         |
| :---------- | :----- | :----------------------------------------------------------- |
| `entities`  | Array  | 当前设备绑定的推送信息列表。如果发送该绑定请求后该设备无任何推送绑定信息，则返回空列表。 |

其他参数及说明详见 [公共参数](#公共参数)。

### 示例

#### 请求示例

**绑定请求**

```shell
curl --location --request PUT 'https://XXXX/XXXX/XXXX/users/wzy/push/binding' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer <YourAppToken>' \
-d '{    
    "device_id": "8ce08cad-XXXX-XXXX-86c8-695a0d247cda",    
    "device_token": "XXXX",    
    "notifier_name": "104410638"
}'
```

**解除绑定**

```shell
curl --location --request PUT 'https://XXXX/XXXX/XXXX/users/wzy/push/binding' \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer <YourAppToken>' \
-d '{    
    "device_id": "8ce08cad-XXXX-XXXX-86c8-695a0d247cda",    
    "device_token": "",    
    "notifier_name": "104410638"
}'
```

#### 响应示例

**绑定响应**

```json
{ 
  "timestamp": 1688030642443, 
  "entities": [ 
    {            
      "device_id": "8ce08cad-9369-4bdd-86c8-695a0d247cda",
      "device_token": "BAEAAAAAB.jkuDmf8hRUPDgOel-zX9exVlcjS1akCWQIUA3cBbB_DprnHMeFR11PV1of1sVNKPmKdKhMB22YuO8-Z_Ksoqxo8Y",
      "notifier_name": "104410638"       
    }   
  ],    
  "action": "put",   
  "duration": 28
}
```

**解除绑定响应**

```json
{    
  "timestamp": 1688030767345,    
  "entities": [],    
  "action": "put",    
  "duration": 24
}
```

## 查询推送绑定信息

查询当前用户的所有设备的推送绑定信息。

### HTTP 请求

```
GET https://{host}/{org_name}/{app_name}/users/wzy/push/binding
```

#### 路径参数

参数及说明详见 [公共参数](#公共参数)。

#### 请求 header

| 参数            | 类型   | 描述             | 是否必需 |
| :-------------- | :----- | :------------------------------ | :------- |
| `Authorization` | String | `Bearer ${YourAppToken}` Bearer 是固定字符，后面加英文空格，再加上获取到的 app token 的值。 | 是       |

### HTTP 响应

#### 响应 body

如果返回的 HTTP 状态码为 `200`，表示请求成功，响应包体中包含以下字段：

| 参数       | 类型  | 描述                                                         |
| :--------- | :---- | :----------------------------------------------------------- |
| `entities` | Array | 当前用户的所有设备的推送绑定信息列表。若当前用户的任何设备均无推送绑定信息，则返回空列表。 |

其他参数及说明详见 [公共参数](#公共参数)。

### 请求示例

```shell
curl --location --request GET 'https://a1-hsb.easemob.com/easemob-demo/testy/users/wzy/push/binding' \
-H 'Authorization: Bearer <YourAppToken>'
```

### 响应示例

```json
{    
  "timestamp": 1688031327535,   
  "entities": [       
    {            
      "device_id": "8ce08cad-9369-4bdd-86c8-695a0d247cda",      
      "device_token": "BAEAAAAAB.jkuDmf8hRUPDgOel-zX9exVlcjS1akCWQIUA3cBbB_DprnHMeFR11PV1of1sVNKPmKdKhMB22YuO8-Z_Ksoqxo8Y",  
      "notifier_name": "104410638"      
    }   
    {            
      "device_id": "8ce08cad-9369-4bdd-86c8-695a0d247cda",      
      "device_token": "BAEAAAAAB.jkuDmf8hRUPDgOel-zX9exVlcjS1akCWQIUA3cBbB_DprnHMeFR11PV1of1sVNKPmKdKhMB22YuO8-Z_Ksoqxo8Y",  
      "notifier_name": "104410638"      
    }  
  ],    
  "action": "get",    
  "duration": 6
}
```

## 设置离线推送时显示的昵称

设置离线推送时显示的昵称。

### HTTP 请求

```http
PUT https://{host}/{org_name}/{app_name}/users/{username}
```

#### 路径参数

参数及说明详见 [公共参数](#公共参数)。

#### 请求 header

| 参数            | 类型   | 描述                                                                                                                 | 是否必需 |
| :-------------- | :----- | :------------------------------------------------------------------------------------------------------------------- | :------- |
| `Content-Type`  | String | 内容类型。请填 `application/json`。                                                                                  | 是       |
| `Accept`        | String | 内容类型。请填 `application/json`。                                                                                  | 是       |
| `Authorization` | String | App 管理员的鉴权 token，格式为 `Bearer YourAppToken`，其中 `Bearer` 为固定字符，后面为英文空格和获取到的 app token。 | 是       |

#### 请求 body

请求包体为 JSON Object 类型，包含以下字段：

| 参数       | 类型   | 描述         | 是否必需 |
| :--------- | :----- | :------------------- | :------- |
| `nickname` | String | 离线推送时在接收方的客户端推送通知栏中显示的发送方的昵称。你可以自定义该昵称，长度不能超过 100 个字符。<br/>支持以下字符集：<br/> - 26 个小写英文字母 a-z；<br/> - 26 个大写英文字母 A-Z；<br/> - 10 个数字 0-9；<br/> - 中文；<br/> - 特殊字符。<br/>若不设置昵称，推送消息详情时，即[设置离线推送通知的展示方式](#设置离线推送通知的展示方式)时将 `notification_display_style` 设置为 `1`，会显示发送方的用户 ID，而非昵称。<br/>该昵称可与用户属性中的昵称设置不同，不过我们建议这两种昵称的设置保持一致。因此，修改其中一个昵称时，也需调用相应方法对另一个进行更新，确保设置一致。更新用户属性中的昵称的方法，详见 [设置用户属性](userprofile.html#设置用户属性)。  | 否       |

### HTTP 响应

#### 响应 body

如果返回的 HTTP 状态码为 200，表示请求成功，响应包体中包含以下字段：

| 参数                 | 类型    | 描述                                                                                                                                                                                                           |
| :------------------- | :------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `entities`           | JSON    | 用户在推送通知中显示的昵称以及用户相关信息。                                                                                                                                                                   |
| `entities.uuid`      | String  | 用户的 UUID。系统为该请求中的 app 或用户生成的唯一内部标识，用于生成用户权限 token。                                                                                                                           |
| `entities.type`      | String  | 用户类型，即 `user`。                                                                                                                                                                                          |
| `entities.created`   | Number  | 用户注册的 Unix 时间戳，单位为毫秒。                                                                                                                                                                           |
| `entities.modified`  | Number  | 最近一次修改用户信息的 Unix 时间戳，单位为毫秒。                                                                                                                                                               |
| `entities.username`  | String  | 用户 ID。用户登录的唯一账号。                                                                                                                                                                                  |
| `entities.activated` | Boolean | 用户是否为活跃状态：<ul><li>`true`：用户为活跃状态。</li><li>`false`：用户为封禁状态。如要使用已被封禁的用户账户，你需要调用[解禁用户](account_system.html#账号解禁)解除封禁。</li></ul> |
| `entities.nickname`  | String  | 推送通知中显示的昵称。                                                                                                                                                                                         |

其他参数及说明详见 [公共参数](#公共参数)。

如果返回的 HTTP 状态码非 200，表示请求失败。你可以参考 [错误码](error.html) 了解可能的原因。

### 示例

#### 请求示例

```bash
curl -X PUT -H 'Content-Type: application/json' -H 'Accept: application/json' -H 'Authorization: Bearer <YourAppToken>' -d '{    "nickname": "testuser"   }' 'https://XXXX/XXXX/XXXX/users/user1'
```

#### 响应示例

```json
{
  "action": "put",
  "application": "8be024f0-XXXX-XXXX-XXXX-5d598d5f8402",
  "path": "/users",
  "uri": "https://XXXX/XXXX/XXXX/users",
  "entities": [
    {
      "uuid": "4759aa70-XXXX-XXXX-XXXX-6fa0510823ba",
      "type": "user",
      "created": 1542595573399,
      "modified": 1542596083687,
      "username": "user1",
      "activated": true,
      "nickname": "testuser"
    }
  ],
  "timestamp": 1542596083685,
  "duration": 6,
  "organization": "agora-demo",
  "applicationName": "testapp"
}
```

## 设置离线推送通知的展示方式

设置离线推送通知在客户端的展示方式，设置即时生效。服务端据此向用户推送离线消息。

### HTTP 请求

```http
PUT https://{host}/{org_name}/{app_name}/users/{username}
```

#### 路径参数

参数及说明详见 [公共参数](#公共参数)。

#### 请求 header

| 参数            | 类型   | 描述                               | 是否必需 |
| :-------------- | :----- | :-------------------------------------- | :------- |
| `Content-Type`  | String | 内容类型。请填 `application/json`。                                                                                  | 是       |
| `Authorization` | String | App 管理员的鉴权 token，格式为 `Bearer YourAppToken`，其中 `Bearer` 为固定字符，后面为英文空格和获取到的 app token。 | 是       |

#### 请求 body

请求包体为 JSON Object 类型，包含以下字段：

| 参数                         | 类型 | 描述             | 是否必需 |
| :--------------------------- | :--- | :--------------------------------- | :------- |
| `notification_display_style` | Int  | 离线推送通知的展示方式：<ul><li>（默认）`0`：推送标题为“您有一条新消息”，推送内容为“请点击查看”；</li><li>`1`：推送标题为“您有一条新消息”，推送内容为发送人昵称和离线消息的内容。</li></ul> | 是       |

### HTTP 响应

#### 响应 body

| 参数      | 类型    | 描述   |
| :-------------------- | :------ | :------------------------------------------------ |
| `entities`                                  | JSON    | 用户的离线推送通知的展示方式以及相关信息。        |
| `entities.uuid`                             | String  | 用户的 UUID。系统为该请求中的 app 或用户生成的唯一内部标识，用于生成用户权限 token。                                                                                                                           |
| `entities.type`                             | String  | 用户类型，即 `user`。                                                                                                                                                                                          |
| `entities.created`                          | Long    | 用户创建的 Unix 时间戳，单位为毫秒。                                                                                                                                                                           |
| `entities.modified`                         | Long    | 最近一次修改用户信息的 Unix 时间戳，单位为毫秒。                                                                                                                                                               |
| `entities.username`                         | String  | 用户 ID。用户登录的唯一账号。                                                                                                                                                                                  |
| `entities.activated`                        | Boolean | 用户是否为活跃状态：<ul><li>`true`：用户为活跃状态。</li><li>`false`：用户为封禁状态。如要使用已被封禁的用户账户，你需要调用[解禁用户](account_system.html#账号封禁)解除封禁。</li></ul> |
| `entities.notification_no_disturbing`       | Boolean | 是否设置为免打扰模式：<ul><li>`true`：是；</li><li>`false`：否。</li></ul>                                                                                                                                     |
| `entities.notification_no_disturbing_start` | Int     | 免打扰时间段的开始时间。                                                                                                                                                                                       |
| `entities.notification_no_disturbing_end`   | Int     | 免打扰时间段的结束时间。                                                                                                                                                                                       |
| `entities.notification_display_style`       | Int     | 离线推送通知的展示方式。                                                                                                                                                                                       |
| `entities.nickname`                         | String  | 离线推送通知收到时显示的昵称。                                                                                                                                                                                 |
| `entities.notifier_name`                    | String  | 推送证书名称。                                                                                                                                                                                                 |

其他参数及说明详见 [公共参数](#公共参数)。

如果返回的 HTTP 状态码非 200，表示请求失败。你可以参考 [错误码](error.html) 了解可能的原因。

### 示例

#### 请求示例

```bash
curl -X PUT -H "Authorization: Bearer <YourAppToken>" -i  https://XXXX/XXXX/XXXX/users/a -d '{"notification_display_style": "1"}'
```

#### 响应示例

```json
{
  "action": "put",
  "application": "17d59e50-XXXX-XXXX-XXXX-0dc80c0f5e99",
  "path": "/users",
  "uri": "https://XXXX/XXXX/XXXX/users",
  "entities": [
    {
      "uuid": "3b8c9890-XXXX-XXXX-XXXX-f50bf55cafad",
      "type": "user",
      "created": 1530276298905,
      "modified": 1534407146060,
      "username": "user1",
      "activated": true,
      "notification_no_disturbing": false,
      "notification_no_disturbing_start": 1,
      "notification_no_disturbing_end": 3,
      "notification_display_style": 1,
      "nickname": "testuser",
      "notifier_name": "2882303761517426801"
    }
  ],
  "timestamp": 1534407146058,
  "duration": 3,
  "organization": "1112171214115068",
  "applicationName": "testapp"
}
```

## 设置免打扰模式

设置离线推送免打扰模式，在免打扰期间，用户将不会收到离线消息推送。

该 API 为旧版接口，推荐使用新版的[设置离线推送 API](#设置离线推送)。

### HTTP 请求

```http
PUT https://{host}/{org_name}/{app_name}/users/{username}
```

#### 路径参数

参数及说明详见 [公共参数](#公共参数)。

#### 请求 header

| 参数            | 类型   | 描述                                                                                                                 | 是否必需 |
| :-------------- | :----- | :------------------------------------------------------------------------------------------------------------------- | :------- |
| `Content-Type`  | String | 内容类型。请填 `application/json`。                                                                                  | 是       |
| `Authorization` | String | App 管理员的鉴权 token，格式为 `Bearer YourAppToken`，其中 `Bearer` 为固定字符，后面为英文空格和获取到的 app token。 | 是       |

#### 请求 body

请求包体为 JSON Object 类型，包含以下字段：

| 参数                               | 类型    | 描述                                                                                             | 是否必需<div style="width: 80px;"></div> |
| :--------------------------------- | :------ | :----------------------------------------------------------------------------------------------- | :--------------------------------------- |
| `notification_no_disturbing`       | Boolean | 是否设置为免打扰模式：<ul><li>`true`：是；</li><li>`false`：否。</li></ul>                       | 否                                       |
| `notification_no_disturbing_start` | String  | 免打扰时间段的开始时间，精确到小时，取值范围为 [0,23]。例如 “8” 表示每日 8:00 开启免打扰模式。   | 否                                       |
| `notification_no_disturbing_end`   | String  | 免打扰时间段的结束时间，精确到小时，取值范围为 [0,23]。例如 “18” 表示每日 18:00 关闭免打扰模式。 | 否                                       |

:::tip
免打扰时间段的设置，应注意以下几项：

1. 开始时间和结束时间的设置立即生效，免打扰模式每天定时触发。例如，开始时间为 `8`，结束时间为 `10`，免打扰模式在每天的 8:00-10:00 内生效。若你在 11:00 设置开始时间为 `8`，结束时间为 `12`，则免打扰模式在当天的 11:00-12:00 生效，以后每天均在 8:00-12:00 生效。
2. 若开始时间和结束时间相同，则全天免打扰。
3. 若结束时间早于开始时间，则免打扰模式在每天的开始时间到次日的结束时间内生效。例如，开始时间为 `10`，结束时间为 `8`，则免打扰模式的在当天的 10:00 到次日的 8:00 生效。
   :::

### HTTP 响应

#### 响应 body

如果返回的 HTTP 状态码为 200，表示请求成功，响应包体中包含以下字段：

| 参数                                        | 类型    | 描述                                                                                                                                                                                                           |
| :------------------------------------------ | :------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `entities`                                  | JSON    | 用户的免打扰模式的相关信息。                                                                                                                                                                                   |
| `entities.uuid`                             | String  | 用户的 UUID。系统为该请求中的 app 或用户生成的唯一内部标识，用于生成用户权限 token。                                                                                                                           |
| `entities.type`                             | String  | 用户类型，即 `user`。                                                                                                                                                                                          |
| `entities.created`                          | Long    | 用户创建的 Unix 时间戳，单位为毫秒。                                                                                                                                                                           |
| `entities.modified`                         | Long    | 最近一次修改用户信息的 Unix 时间戳，单位为毫秒。                                                                                                                                                               |
| `entities.username`                         | String  | 用户 ID。用户登录的唯一账号。                                                                                                                                                                                  |
| `entities.activated`                        | Boolean | 用户是否为活跃状态：<ul><li>`true`：用户为活跃状态。</li><li>`false`：用户为封禁状态。如要使用已被封禁的用户账户，你需要调用[解禁用户](account_system.html#账号封禁)解除封禁。</li></ul> |
| `entities.notification_no_disturbing`       | Boolean | 是否设置为免打扰模式：<ul><li>`true`：是；</li><li>`false`：否。</li></ul>                                                                                                                                     |
| `entities.notification_no_disturbing_start` | Int     | 免打扰时间段的开始时间。                                                                                                                                                                                       |
| `entities.notification_no_disturbing_end`   | Int     | 免打扰时间段的结束时间。                                                                                                                                                                                       |
| `entities.notification_display_style`       | Int     | 离线推送通知的展示方式。                                                                                                                                                                                       |
| `entities.nickname`                         | String  | 离线推送通知收到时显示的昵称。                                                                                                                                                                                 |
| `entities.notifier_name`                    | String  | 推送证书名称。                                                                                                                                                                                                 |

其他参数及说明详见 [公共参数](#公共参数)。

如果返回的 HTTP 状态码非 200，表示请求失败。你可以参考 [错误码](error.html) 了解可能的原因。

### 示例

#### 请求示例

**设置免打扰时间段**

```bash
curl -X PUT -H "Authorization: Bearer <YourAppToken>" -i  "https://XXXX/XXXX/XXXX/users/a " -d '{"notification_no_disturbing": true,"notification_no_disturbing_start": "1","notification_no_disturbing_end": "3"}'
```

**关闭免打扰模式**

```bash
curl -X PUT -H "Authorization: Bearer <YourAppToken>" -i  "https://XXXX/XXXX/XXXX/users/a " -d '{"notification_no_disturbing": false}'
```

#### 响应示例

```json
{
  "action": "put",
  "application": "17d59e50-XXXX-XXXX-XXXX-0dc80c0f5e99",
  "path": "/users",
  "uri": "https://XXXX/XXXX/XXXX/users",
  "entities": [
    {
      "uuid": "3b8c9890-XXXX-XXXX-XXXX-f50bf55cafad",
      "type": "user",
      "created": 1530276298905,
      "modified": 1534405429835,
      "username": "User1",
      "activated": true,
      "notification_no_disturbing": true,
      "notification_no_disturbing_start": 1,
      "notification_no_disturbing_end": 3,
      "notification_display_style": 0,
      "nickname": "testuser",
      "notifier_name": "2882303761517426801"
    }
  ],
  "timestamp": 1534405429833,
  "duration": 4,
  "organization": "1112171214115068",
  "applicationName": "testapp"
}
```

## 设置离线推送

你可以设置全局离线推送的通知方式和免打扰模式以及单个单聊或群聊会话的离线推送设置。

### HTTP 请求

```http
PUT https://{host}/{org}/{app}/users/{username}/notification/{chattype}/{key}
```

#### 路径参数

| 参数       | 类型   | 描述                                                                                        | 是否必需 |
| :--------- | :----- | :------------------------------------------------------------------------------------------ | :------- |
| `chattype` | String | 对象类型，即会话类型：<br/> - `user`：用户，表示单聊；<br/> - `chatgroup`：群组，表示群聊。 | 是       |
| `key`      | String | 对象名称：<br/> - 单聊时为对端用户的用户 ID；<br/> - 群聊时为群组 ID。                      | 是       |

:::tip
如需设置 app 全局离线推送，`chattype` 需传 `user`，`key` 为当前用户 ID。
:::

其他参数及说明详见 [公共参数](#公共参数)。

#### 请求 header

| 参数            | 类型   | 描述                                                                                                                 | 是否必需 |
| :-------------- | :----- | :------------------------------------------------------------------------------------------------------------------- | :------- |
| `Content-Type`  | String | 内容类型。请填 `application/json`。                                                                                  | 是       |
| `Authorization` | String | App 管理员的鉴权 token，格式为 `Bearer YourAppToken`，其中 `Bearer` 为固定字符，后面为英文空格和获取到的 app token。 | 是       |

#### 请求 body

| 参数             | 类型   | 描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | 是否必需<div style="width: 80px;"></div> |
| :--------------- | :----- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :--------------------------------------- |
| `type`           | String | 离线推送通知方式：<br/> - `DEFAULT`：默认值，采用全局配置；<br/> - `ALL`：接收全部离线消息的推送通知；<br/> - `AT`：只接收提及当前用户的离线消息的推送通知。该字段推荐在群聊中使用。若提及一个或多个用户，需在创建消息时对 `ext` 字段传 "em_at_list":["user1", "user2" ...]；若提及所有人，对该字段传 "em_at_list":"all"。<br/> - `NONE`：不接收离线消息的推送通知。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | 否                                       |
| `ignoreInterval` | String | 离线推送免打扰时间段，精确到分钟，格式为 HH:MM-HH:MM，例如 08:30-10:00。该时间为 24 小时制，免打扰时间段的开始时间和结束时间中的小时数和分钟数的取值范围分别为 [00,23] 和 [00,59]。免打扰时段的设置仅针对 app 生效，对单聊或群聊不生效。<br/> 该参数的设置说明如下：<br/> - 开始时间和结束时间的设置立即生效，免打扰模式每天定时触发。例如，开始时间为 `08:00`，结束时间为 `10:00`，免打扰模式在每天的 8:00-10:00 内生效。若你在 11:00 设置开始时间为 `08:00`，结束时间为 `12:00`，则免打扰模式在当天的 11:00-12:00 生效，以后每天均在 8:00-12:00 生效。<br/> - 若开始时间和结束时间相同，则全天免打扰。<br/> - 若结束时间早于开始时间，则免打扰模式在每天的开始时间到次日的结束时间内生效。例如，开始时间为 `10:00`，结束时间为 `08:00`，则免打扰模式的在当天的 10:00 到次日的 8:00 生效。<br/> - 目前仅支持在每天的一个指定时间段内开启免打扰模式，不支持多个免打扰时间段，新的设置会覆盖之前的设置。<br/> - 若不设置该参数，传空字符串。 | 否                                       |
| `ignoreDuration` | Long   | 离线推送免打扰时长，单位为毫秒。该参数的取值范围为 [0,604800000]，`0` 表示该参数无效，`604800000` 表示免打扰模式持续 7 天。<br/> 与`ignoreInterval` 的设置长久有效不同，该参数为一次有效。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | 否                                       |

### HTTP 响应

#### 响应 body

如果返回的 HTTP 状态码为 200，表示请求成功，响应包体中包含以下字段：

| 参数                  | 类型   | 描述                   |
| :-------------------- | :----- | :--------------------- |
| `data`                | JSON   | 离线推送的设置。       |
| `data.type`           | String | 离线推送通知方式。     |
| `data.ignoreInterval` | String | 离线推送免打扰时间段。 |
| `data.ignoreDuration` | Long   | 离线推送免打扰时长。   |

如果返回的 HTTP 状态码非 200，表示请求失败。你可以参考 [错误码](error.html) 了解可能的原因。

### 示例

#### 请求示例

```bash
curl -L -X PUT '{url}/{org_name}/{app_name}/users/{username}/notification/user/{key}' \
-H 'Authorization: Bearer <YourAppToken>' \
-H 'Content-Type: application/json' \
--data-raw '{
    "type":"NONE",
    "ignoreInterval":"21:30-08:00",
    "ignoreDuration":86400000
}'
```

#### 响应示例

```json
{
  "path": "/users",
  "uri": "https://XXXX/XXXX/XXXX/users/notification/user/hxtest",
  "timestamp": 1647503749918,
  "organization": "hx",
  "application": "17fe201b-XXXX-XXXX-XXXX-1ed1ebd7b227",
  "action": "put",
  "data": {
    "type": "NONE",
    "ignoreDuration": 1647590149924,
    "ignoreInterval": "21:30-08:00"
  },
  "duration": 20,
  "applicationName": "hxdemo"
}
```

## 查询离线推送设置

查询指定单聊、指定群聊或全局的离线推送设置。

### HTTP 请求

```http
GET https://{host}/{org}/{app}/users/{username}/notification/{chattype}/{key}
```

#### 路径参数

| 参数       | 类型   | 描述                                                                                        | 是否必需 |
| :--------- | :----- | :------------------------------------------------------------------------------------------ | :------- |
| `chattype` | String | 对象类型，即会话类型：<br/> - `user`：用户，表示单聊；<br/> - `chatgroup`：群组，表示群聊。 | 是       |
| `key`      | String | 对象名称：<br/> - 单聊时为对端用户的用户 ID；<br/> - 群聊时为群组 ID。                      | 是       |

其他参数及说明详见 [公共参数](#公共参数)。

#### 请求 header

| 参数            | 类型   | 描述                                                                                                                 | 是否必需 |
| :-------------- | :----- | :------------------------------------------------------------------------------------------------------------------- | :------- |
| `Authorization` | String | App 管理员的鉴权 token，格式为 `Bearer YourAppToken`，其中 `Bearer` 为固定字符，后面为英文空格和获取到的 app token。 | 是       |

### HTTP 响应

#### 响应 body

如果返回的 HTTP 状态码为 200，表示请求成功，响应包体中包含以下字段：

| 参数                  | 类型   | 描述                   |
| :-------------------- | :----- | :--------------------- |
| `data`                | JSON   | 离线推送通知的设置。   |
| `data.type`           | String | 离线推送通知方式。     |
| `data.ignoreInterval` | String | 离线推送免打扰时间段。 |
| `data.ignoreDuration` | Long   | 离线推送免打扰时长。   |

如果返回的 HTTP 状态码非 200，表示请求失败。你可以参考 [错误码](error.html) 了解可能的原因。

### 示例

#### 请求示例

```bash
curl -L -X GET '{url}/{org}/{app}/users/{username}/notification/chatgroup/{key}' \
-H 'Authorization: Bearer <YourAppToken>'
```

#### 响应示例

```json
{
  "path": "/users",
  "uri": "https://XXXX/XXXX/XXXX/users/notification/chatgroup/12312312321",
  "timestamp": 1647503749918,
  "organization": "hx",
  "application": "17fe201b-XXXX-XXXX-XXXX-1ed1ebd7b227",
  "action": "get",
  "data": {
    "type": "NONE",
    "ignoreDuration": 1647590149924,
    "ignoreInterval": "21:30-08:00"
  },
  "duration": 20,
  "applicationName": "hxdemo"
}
```

## 设置推送通知的首选语言

设置离线推送消息的首选语言。

### HTTP 请求

```http
PUT https://{host}/{org}/{app}/users/{username}/notification/language
```

#### 路径参数

参数及说明详见 [公共参数](#公共参数)。

#### 请求 header

| 参数            | 类型   | 描述                                                                                                                 | 是否必需 |
| :-------------- | :----- | :------------------------------------------------------------------------------------------------------------------- | :------- |
| `Content-Type`  | String | 内容类型。请填 `application/json`。                                                                                  | 是       |
| `Authorization` | String | App 管理员的鉴权 token，格式为 `Bearer YourAppToken`，其中 `Bearer` 为固定字符，后面为英文空格和获取到的 app token。 | 是       |

#### 请求 body

| 参数                  | 类型   | 描述                                                                                               | 是否必需 |
| :-------------------- | :----- | :------------------------------------------------------------------------------------------------- | :------- |
| `translationLanguage` | String | 用户接收的推送通知的首选语言的代码。如果设置为空字符串，表示无需翻译，服务器直接推送原语言的通知。 | 是       |

### HTTP 响应

#### 响应 body

如果返回的 HTTP 状态码为 200，表示请求成功，响应包体中包含以下字段：

| 参数            | 类型   | 描述                               |
| :-------------- | :----- | :--------------------------------- |
| `data`          | JSON   | 用户接收推送通知的首选语言。       |
| `data.language` | String | 用户接收推送通知的首选语言的代码。 |

如果返回的 HTTP 状态码非 200，表示请求失败。你可以参考 [错误码](error.html) 了解可能的原因。

### 示例

#### 请求示例

```bash
curl -L -X PUT '{url}/{org}/{app}/users/{username}/notification/language' \
-H 'Authorization: Bearer <YourAppToken>' \
-H 'Content-Type: application/json' \
--data-raw '{
    "translationLanguage":"EU"
}'
```

#### 响应示例

```json
{
  "path": "/users",
  "uri": "https://XXXX/XXXX/XXXX/users/notification/language",
  "timestamp": 1648089630244,
  "organization": "hx",
  "application": "17fe201b-XXXX-XXXX-XXXX-1ed1ebd7b227",
  "action": "put",
  "data": {
    "language": "EU"
  },
  "duration": 66,
  "applicationName": "hxdemo"
}
```

## 获取推送通知的首选语言

获取推送通知的首选语言。

### HTTP 请求

```http
GET https://{host}/{org_name}/{app_name}/users/{username}/notification/language
```

#### 路径参数

参数及说明详见 [公共参数](#公共参数)。

#### 请求 header

| 参数            | 类型   | 描述                                                                                                                 | 是否必需 |
| :-------------- | :----- | :------------------------------------------------------------------------------------------------------------------- | :------- |
| `Content-Type`  | String | 内容类型。请填 `application/json`。                                                                                  | 是       |
| `Authorization` | String | App 管理员的鉴权 token，格式为 `Bearer YourAppToken`，其中 `Bearer` 为固定字符，后面为英文空格和获取到的 app token。 | 是       |

### HTTP 响应

#### 响应 body

如果返回的 HTTP 状态码为 200，表示请求成功，响应包体中包含以下字段：

| 参数            | 类型   | 描述                               |
| :-------------- | :----- | :--------------------------------- |
| `data`          | JSON   | 用户接收推送通知的首选语言。       |
| `data.language` | String | 用户接收推送通知的首选语言的代码。 |

如果返回的 HTTP 状态码非 200，表示请求失败。你可以参考 [错误码](error.html) 了解可能的原因。

### 示例

#### 请求示例

```bash
curl -L -X GET '{url}/{org}/{app}/users/{username}/notification/language' \
-H 'Authorization: Bearer <YourAppToken>'
```

#### 响应示例

```json
{
  "path": "/users",
  "uri": "https://XXXX/XXXX/XXXX/users/notification/language",
  "timestamp": 1648089630244,
  "organization": "hx",
  "application": "17fe201b-XXXX-XXXX-XXXX-1ed1ebd7b227",
  "action": "put",
  "data": {
    "language": "EU"
  },
  "duration": 66,
  "applicationName": "hxdemo"
}
```

## 创建离线推送模板

创建离线推送消息模板。

### HTTP 请求

```http
POST https://{host}/{org_name}/{app_name}/notification/template
```

#### 路径参数

参数及说明详见 [公共参数](#公共参数)。

#### 请求 header

| 参数            | 类型   | 描述                                                                                                                 | 是否必需 |
| :-------------- | :----- | :------------------------------------------------------------------------------------------------------------------- | :------- |
| `Content-Type`  | String | 内容类型。请填 `application/json`。                                                                                  | 是       |
| `Authorization` | String | App 管理员的鉴权 token，格式为 `Bearer YourAppToken`，其中 `Bearer` 为固定字符，后面为英文空格和获取到的 app token。 | 是       |

#### 请求 body

| 参数              | 类型   | 描述                                  | 是否必需 |
| :---------------- | :----- | :------------------------------------ | :------- |
| `name`            | String | 要添加的推送模板的名称。              | 是       |
| `title_pattern`   | String | 自定义推送标题，例如： 标题 {0}。     | 是       |
| `content_pattern` | String | 自定义推送内容，例如：内容 {0}, {1}。 | 是       |

### HTTP 响应

#### 响应 body

如果返回的 HTTP 状态码为 200，表示请求成功，响应包体中包含以下字段：

| 参数                   | 类型   | 描述                                         |
| :--------------------- | :----- | :------------------------------------------- |
| `data`                 | JSON   | 推送模板的相关信息。                         |
| `data.name`            | String | 推送模板的名称。                             |
| `data.createAt`        | Number | 创建模板的 Unix 时间戳，单位为毫秒。         |
| `data.updateAt`        | Number | 最近一次修改模板的 Unix 时间戳，单位为毫秒。 |
| `data.title_pattern`   | String | 推送模板的自定义标题。                       |
| `data.content_pattern` | String | 推送模板的自定义内容。                       |

如果返回的 HTTP 状态码非 200，表示请求失败。你可以参考 [错误码](error.html) 了解可能的原因。

### 示例

#### 请求示例

```bash
curl -X POST '{url}/{org}/{app}/notification/template' \
-H 'Authorization: Bearer <YourAppToken>' \
-H 'Content-Type: application/json' \
--data-raw '{
    "name": "test7",
    "title_pattern": "你好,{0}",
    "content_pattern": "推送测试,{0}"
}'
```

#### 响应示例

```json
{
  "uri": "https://XXXX/XXXX/XXXX/notification/template",
  "timestamp": 1646989584108,
  "organization": "hx",
  "application": "17fe201b-XXXX-XXXX-XXXX-1ed1ebd7b227",
  "action": "post",
  "data": {
    "name": "test7",
    "createAt": 1646989584124,
    "updateAt": 1646989584124,
    "title_pattern": "你好,{0}",
    "content_pattern": "推送测试,{0}"
  },
  "duration": 26,
  "applicationName": "hxdemo"
}
```

## 查询离线推送模板

查询离线推送消息使用的模板。

### HTTP 请求

```http
GET https://{host}/{org_name}/{app_name}/notification/template/{name}
```

#### 路径参数

| 参数   | 类型   | 描述                     | 是否必需 |
| :----- | :----- | :----------------------- | :------- |
| `name` | String | 要查询的推送模板的名称。 | 是       |

其他参数及说明详见 [公共参数](#公共参数)。

#### 请求 header

| 参数            | 类型   | 描述                                                                                                                 | 是否必需 |
| :-------------- | :----- | :------------------------------------------------------------------------------------------------------------------- | :------- |
| `Content-Type`  | String | 内容类型。请填 `application/json`。                                                                                  | 是       |
| `Authorization` | String | App 管理员的鉴权 token，格式为 `Bearer YourAppToken`，其中 `Bearer` 为固定字符，后面为英文空格和获取到的 app token。 | 是       |

### HTTP 响应

#### 响应 body

如果返回的 HTTP 状态码为 200，表示请求成功，响应包体中包含以下字段：

| 参数                   | 类型   | 描述                                           |
| :--------------------- | :----- | :--------------------------------------------- |
| `data`                 | JSON   | 推送模板的相关信息。                           |
| `data.name`            | String | 推送模板的名称。                               |
| `data.createAt`        | Number | 创建模板时的 Unix 时间戳，单位为毫秒。         |
| `data.updateAt`        | Number | 最近一次修改模板时的 Unix 时间戳，单位为毫秒。 |
| `data.title_pattern`   | String | 推送模板的自定义标题。                         |
| `data.content_pattern` | String | 推送模板的自定义内容。                         |

如果返回的 HTTP 状态码非 200，表示请求失败。你可以参考 [错误码](error.html) 了解可能的原因。

### 示例

#### 请求示例

```bash
curl -X GET '{url}/{org}/{app}/notification/template/{name}' \
-H 'Authorization: Bearer <YourAppToken>'
```

#### 响应示例

```json
{
  "uri": "https://XXXX/XXXX/XXXX/notification/template/test7",
  "timestamp": 1646989686393,
  "organization": "hx",
  "application": "17fe201b-XXXX-XXXX-XXXX-1ed1ebd7b227",
  "action": "get",
  "data": {
    "name": "test7",
    "createAt": 1646989584124,
    "updateAt": 1646989584124,
    "title_pattern": "你好,{0}",
    "content_pattern": "推送测试,{0}"
  },
  "duration": 11,
  "applicationName": "hxdemo"
}
```

## 删除离线推送模板

删除离线消息推送模板。

### HTTP 请求

```http
DELETE https://{host}/{org_name}/{app_name}/notification/template/{name}
```

#### 路径参数

| 参数   | 类型   | 描述                     | 是否必需 |
| :----- | :----- | :----------------------- | :------- |
| `name` | String | 要删除的推送模板的名称。 | 是       |

其他参数及说明详见 [公共参数](#公共参数)。

#### 请求 header

| 参数            | 类型   | 描述                                                                                                                 | 是否必需 |
| :-------------- | :----- | :------------------------------------------------------------------------------------------------------------------- | :------- |
| `Content-Type`  | String | 内容类型。请填 `application/json`。                                                                                  | 是       |
| `Authorization` | String | App 管理员的鉴权 token，格式为 `Bearer YourAppToken`，其中 `Bearer` 为固定字符，后面为英文空格和获取到的 app token。 | 是       |

### HTTP 响应

#### 响应 body

如果返回的 HTTP 状态码为 200，表示请求成功，响应包体中包含以下字段：

| 参数                   | 类型   | 描述                                           |
| :--------------------- | :----- | :--------------------------------------------- |
| `data`                 | JSON   | 删除的推送模板的相关信息。                     |
| `data.name`            | String | 推送模板的名称。                               |
| `data.createAt`        | Number | 推送模板的创建时间戳，单位为毫秒。             |
| `data.updateAt`        | Number | 最近一次修改模板时的 Unix 时间戳，单位为毫秒。 |
| `data.title_pattern`   | String | 推送模板的自定义标题。                         |
| `data.content_pattern` | String | 推送模板的自定义内容。                         |

如果返回的 HTTP 状态码非 200，表示请求失败。你可以参考 [错误码](error.html) 了解可能的原因。

### 示例

#### 请求示例

```bash
curl -X DELETE '{url}/{org}/{app}/notification/template/{name}' \
-H 'Authorization: Bearer {YourAppToken}'
```

#### 响应示例

```json
{
  "uri": "https://XXXX/XXXX/XXXX/notification/template/test7",
  "timestamp": 1646989686393,
  "organization": "hx",
  "application": "17fe201b-XXXX-XXXX-XXXX-1ed1ebd7b227",
  "action": "delete",
  "data": {
    "name": "test7",
    "createAt": 1646989584124,
    "updateAt": 1646989584124,
    "title_pattern": "你好,{0}",
    "content_pattern": "推送测试,{0}"
  },
  "duration": 11,
  "applicationName": "hxdemo"
}
```
