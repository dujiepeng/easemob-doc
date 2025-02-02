# 消息表情回复 Reaction iOS

<Toc />

环信即时通讯 IM 提供消息表情回复（下文统称 “Reaction”）功能。用户可以在单聊和群聊中对消息添加、删除表情。表情可以直观地表达情绪，利用 Reaction 可以提升用户的使用体验。同时在群组中，利用 Reaction 可以发起投票，根据不同表情的追加数量来确认投票。

注意：目前 Reaction 仅适用于单聊和群组。聊天室暂不支持 Reaction 功能。

## 技术原理

SDK 支持你通过调用 API 在项目中实现如下功能：

- `addReaction` 在消息上添加 Reaction；
- `removeReaction` 删除消息的 Reaction；
- `getReactionList` 获取消息的 Reaction 列表；
- `getReactionDetail` 获取 Reaction 详情；
- `EMChatMessage.reactionList` 从 `EMChatMessage` 对象获取 Reaction 列表。

Reaction 场景示例如下：

![img](@static/images/ios/reactions.png)

分别展示如何添加 Reaction，群聊中 Reaction 的效果，以及查看 Reaction 列表。

## 前提条件

开始前，请确保满足以下条件：

1. 完成 `HyphenateChat 3.9.2.1 或以上版本` SDK 初始化，详见 [快速开始](quickstart.html)。
2. 了解环信即时通讯 IM API 的 [使用限制](/product/limitation.html)。
3. 已联系商务开通 Reaction 功能。

## 实现方法

### 在消息上添加 Reaction

调用 `addReaction` 在消息上添加 Reaction，在 `messageReactionDidChange` 监听事件中会收到这条消息的最新 Reaction 概览。

示例代码如下：

```objectivec
// 添加 Reaction。
[EMClient.sharedClient.chatManager addReaction:"reaction" toMessage:"messageId" completion:^(EMError * _Nullable error) {
	refreshBlock(error, changeSelectedStateHandle);
}];

// 监听 Reaction 更新。
- (void)messageReactionDidChange:(NSArray<EMMessageReactionChange *> *)changes
{

}
```

### 删除消息的 Reaction

调用 `removeReaction` 删除消息的 Reaction，在 `onReactionChange` 监听事件中会收到这条消息的最新 Reaction 概览。

示例代码如下：

```objectivec
// 删除 Reaction。
[EMClient.sharedClient.chatManager removeReaction:"reaction" fromMessage:"messageId" completion:^(EMError * _Nullable error) {
	refreshBlock(error, changeSelectedStateHandle);
}];

// 监听 Reaction 更新。
- (void)messageReactionDidChange:(NSArray<EMMessageReactionChange *> *)changes
{

}
```

### 获取消息的 Reaction 列表

调用 `getReactionList` 从服务器获取指定消息的 Reaction 概览列表，列表内容包含 Reaction 内容，用户数量，用户列表（概要数据，即前三个用户信息）。示例代码如下：

```objectivec
[EMClient.sharedClient.chatManager getReactionList:@["messageId"] groupId:@"groupId" chatType:EMChatTypeChat completion:^(NSDictionary<NSString *, EMMessageReaction *> * _Nonnull, EMError * _Nullable) {

}];
```

### 获取 Reaction 详情

调用 `getReactionDetail` 可以从服务器获取指定 Reaction 的详情，包括 Reaction 内容，用户数量和全部用户列表。示例代码如下：

```objectivec
[EMClient.sharedClient.chatManager getReactionDetail:@"messageId" reaction:@"reaction" cursor:nil pageSize:30 completion:^(EMMessageReaction * _Nonnull, NSString * _Nullable cursor, EMError * _Nullable) {

}];
```