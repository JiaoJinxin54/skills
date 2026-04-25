---
name: harmonyos-account-kit-atomicservice
description: 为 HarmonyOS 元服务接入 Account Kit 提供实现流程、配置检查、ArkTS 代码模式和样例迁移指导。用于实现或重构元服务中的华为账号静默登录、FunctionalButton 获取头像/手机号/收货地址/发票抬头、未成年人模式、公共事件订阅、取消授权、Client ID 与 bundleName 配置、Account Kit 错误处理，或参考 AccountKit-Samplecode-Clientdemo-For-Atomicservice-ArkTS 官方样例时触发。
---

# HarmonyOS Account Kit Atomic Service

基于 `HarmonyOS_Samples/account-kit-samplecode-clientdemo-for-atomicservice-arkts`
提炼 Account Kit 在元服务中的接入方式。优先帮助 Codex 把账号能力接入到真实 ArkTS 工程，而不是泛化讲解 HarmonyOS 页面设计。

## 与 harmonyos-design 的边界

- 使用本 skill 处理 Account Kit、元服务账号能力、FunctionalButton、未成年人模式和 AGC/Client ID 配置。
- 使用 `harmonyos-design` 处理页面信息架构、ArkUI/HDS 组件选型、设计 token、响应式布局和原生视觉一致性。
- 同时涉及账号能力和页面重构时，先用本 skill 定能力边界和数据流，再用 `harmonyos-design` 优化界面结构。

## 工作方式

1. 先确认工程是 `atomicService`，且目标设备、SDK、DevEco 版本满足约束。
2. 先做 AGC 与工程配置检查，再写 ArkTS 代码。
3. 把账号能力拆成静默登录、功能按钮、未成年人模式、授权取消、错误处理五块。
4. 需要具体代码模式时按需读取参考资料，不要一次加载全部。
5. 输出时明确哪些逻辑必须由服务端完成，尤其是手机号 code 换取 Access Token 和手机号。

## 快速决策

- **静默登录**：读取 [references/account-kit-atomicservice.md](references/account-kit-atomicservice.md)。
- **头像、手机号、地址、发票抬头**：读取 [references/functional-button.md](references/functional-button.md)。
- **未成年人模式和事件联动**：读取 [references/minors-protection.md](references/minors-protection.md)。
- **迁移官方样例到业务工程**：从配置检查开始，再按能力逐项迁移，不要直接复制整个示例工程。

## 实施流程

### 1. 配置检查

- 检查 `AppScope/app.json5`：`bundleType` 应为 `atomicService`，`bundleName` 应替换为 AGC 中配置的包名。
- 检查 `entry/src/main/module.json5`：需要配置 `metadata` 中的 `client_id`；涉及未成年人模式时配置 `minors_mode`。
- 检查 AGC：创建项目和元服务，申请获取手机号、获取收货地址等权限，配置签名、指纹和 Client ID。
- 记录约束：样例基线为 HarmonyOS 5.0.5 Release 及以上，DevEco Studio 5.0.5 Release 及以上，SDK 5.0.5 Release 及以上。

### 2. 静默登录

- 使用 `authentication.HuaweiIDProvider().createLoginWithHuaweiIDRequest()` 创建请求。
- 设置 `forceLogin = false`，避免未登录华为账号时主动拉起登录页。
- 用随机 `state` 防 CSRF，并校验响应 `state`。
- 从 `LoginWithHuaweiIDCredential` 取 `authorizationCode` 和 `unionID`。
- 将 `authorizationCode` 交给服务端处理，不要在客户端保存敏感令牌。

### 3. FunctionalButton 能力

- 使用 `@kit.ScenarioFusionKit` 的 `FunctionalButton` 和 `functionalButtonComponentManager`。
- 头像使用 `OpenType.CHOOSE_AVATAR`。
- 手机号使用 `OpenType.GET_PHONE_NUMBER`，回调只返回 code，手机号必须经服务端换取。
- 收货地址使用 `OpenType.CHOOSE_ADDRESS`。
- 发票抬头使用 `OpenType.CHOOSE_INVOICE_TITLE`。
- 用户取消不应提示错误；网络和系统错误用 Toast 或对话框做轻量反馈。

### 4. 未成年人模式

- 调用 `canIUse('SystemCapability.AuthenticationServices.HuaweiID.MinorsProtection')` 和 `supportMinorsMode()` 做能力判断。
- 用 `getMinorsProtectionInfoSync()` 读取系统状态和年龄段。
- 用 `leadToTurnOnMinorsMode(context)` 引导用户开启系统未成年人模式。
- 关闭或调整限制前用 `verifyMinorsProtectionCredential(context)` 校验健康使用设备密码。
- 在 `EntryAbility.onCreate` 订阅 `COMMON_EVENT_MINORSMODE_ON` 和 `COMMON_EVENT_MINORSMODE_OFF`，让元服务状态跟随系统状态。

## 输出要求

- 给出要改的文件路径和责任边界，例如配置文件、页面入口、组件、公共工具、服务端接口。
- 明确客户端仅拿授权 code，服务端换 token 和敏感信息。
- 对每个 Account Kit 能力列出成功、用户取消、网络异常、系统异常的处理策略。
- 遇到页面和视觉问题时引用 `harmonyos-design`，不要把设计 token 和布局规则塞进本 skill。

## 来源样例

- 仓库：`https://gitee.com/harmonyos_samples/account-kit-samplecode-clientdemo-for-atomicservice-arkts`
- 本 skill 提炼自临时拉取的提交：`b8f7564331d4d8957e23ecdfd032c59df6dc31c6`
