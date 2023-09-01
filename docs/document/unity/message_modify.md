# 修改消息

对于单聊或群聊会话中已经发送成功的消息，SDK 支持对这些消息进行修改，修改成功后会同步给会话中的接收方。

- 对于单聊会话，只有消息发送方才能对消息进行修改。
- 对于群聊会话，群主和群管理员除了可以修改自己发送的消息，还可以修改普通群成员发送的消息，而普通群成员只能修改自己发送的消息。

:::notice
若使用该功能，需将 SDK 升级至 1.2.0 或以上版本。
:::

你可以调用 `ChatManager#ModifyMessage` 方法修改已经发送成功的消息。一条消息默认最多可修改 10 次，若要提升修改次数，需联系商务。

示例代码如下：

```csharp
    TextBody tb = new TextBody("new content");
    SDKClient.Instance.ChatManager.ModifyMessage(msgId, tb, new ValueCallBack<Message>(
         onSuccess: (dmsg) =>
         {
         
         },
         onError: (code, desc) =>
         {
         
         }
    ));
```
消息修改后，消息的接收方会收到 `IChatManagerDelegate#OnMessageContentChanged` 事件，该事件中会携带修改后的消息对象、最新一次修改消息的用户以及消息的最新修改时间。对于群聊会话，除了修改消息的用户，群组内的其他成员均会收到该事件。

```csharp
// 继承并实现 `IChatManagerDelegate`。
public class ChatManagerDelegate : IChatManagerDelegate {

    public void OnMessageContentChanged(Message msg, string operatorId, long operationTime)
    {
        // operatorId、operationTime也可通过以下方式来获取,数据与上述行参保持一致
        // string id = msg.Body.OperatorId;
        // long time = msg.Body.OperationTime;
    }
  }

// 注册监听器。
ChatManagerDelegate adelegate = new ChatManagerDelegate();
SDKClient.Instance.ChatManager.AddChatManagerDelegate(adelegate);

// 不使用监听器时需要移除监听器。
SDKClient.Instance.ChatManager.RemoveChatManagerDelegate(adelegate);
```