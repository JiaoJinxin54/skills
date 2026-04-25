# 未成年人模式

## 适用范围

用于元服务内跟随 HarmonyOS 系统未成年人模式，并提供开启入口、关闭校验、年龄段展示和公共事件联动。

## 能力判断

所有调用前先判断系统能力：

```ts
import { minorsProtection } from '@kit.AccountKit';

if (!canIUse('SystemCapability.AuthenticationServices.HuaweiID.MinorsProtection')) {
  return;
}

if (!minorsProtection.supportMinorsMode()) {
  return;
}
```

不支持时隐藏入口，不要展示不可用开关。

## 读取系统状态

```ts
const info = minorsProtection.getMinorsProtectionInfoSync();
const enabled = info.minorsProtectionMode;
const ageGroup = info.ageGroup;
```

当系统未成年人模式关闭时，元服务内单独关闭标记应重置为 `false`，让后续状态继续跟随系统。

## 开启和关闭

开启：

```ts
await minorsProtection.leadToTurnOnMinorsMode(this.getUIContext().getHostContext());
```

关闭或调整限制前先校验健康使用设备密码：

```ts
const verified = await minorsProtection.verifyMinorsProtectionCredential(
  this.getUIContext().getHostContext()
);
```

关闭元服务内未成年人模式时，通常不要关闭系统模式，而是记录业务内 `userTurnOffFlag = true`。

## 公共事件订阅

在 `EntryAbility.onCreate` 注册订阅，监听：

- `commonEventManager.Support.COMMON_EVENT_MINORSMODE_ON`
- `commonEventManager.Support.COMMON_EVENT_MINORSMODE_OFF`

订阅成功后，把状态写入 `AppStorage`：

```ts
AppStorage.setOrCreate('minorsProtectionMode', true);
AppStorage.setOrCreate('availableTimeMode', true);
```

收到开启事件时：

- 若 `userTurnOffFlag` 为 `true`，元服务保持关闭。
- 否则读取 `ageGroup`，更新 `lowerAge` 和 `upperAge`。

收到关闭事件时：

- `minorsProtectionMode = false`
- `availableTimeMode = true`
- `userTurnOffFlag = false`

## 推荐状态字段

- `minorsProtectionMode`：系统或元服务当前是否处于未成年人模式。
- `availableTimeMode`：元服务内可用时长限制是否开启。
- `userTurnOffFlag`：用户是否主动关闭元服务内未成年人模式。
- `lowerAge` / `upperAge`：适龄内容年龄段。
- `showMinorsProtectionItem`：设备支持时才展示入口。

## 交互建议

- 开启时可直接引导系统未成年人模式设置。
- 关闭时先展示确认对话框，再校验密码。
- 调整可用时长限制也应校验密码。
- 未成年人模式开启后，应结合业务内容过滤或限制策略；本样例只展示状态联动。
