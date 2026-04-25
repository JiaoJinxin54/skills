# Account Kit 元服务接入

## 适用范围

用于 HarmonyOS 元服务中的 Account Kit 接入：静默登录、授权取消、用户信息缓存、工程配置和错误处理。

## 工程配置

`AppScope/app.json5`：

```json5
{
  "app": {
    "bundleName": "com.example.atomicservice",
    "bundleType": "atomicService"
  }
}
```

`entry/src/main/module.json5`：

```json5
{
  "module": {
    "metadata": [
      { "name": "client_id", "value": "AGC_CLIENT_ID" },
      { "name": "minors_mode", "value": "1" }
    ]
  }
}
```

只在涉及未成年人模式时保留 `minors_mode`。`client_id` 必须来自 AppGallery Connect 配置，不能沿用样例中的占位值。

## 静默登录流程

核心模式：

```ts
import { authentication } from '@kit.AccountKit';
import { util } from '@kit.ArkTS';

const loginRequest = new authentication.HuaweiIDProvider().createLoginWithHuaweiIDRequest();
loginRequest.forceLogin = false;
loginRequest.state = util.generateRandomUUID();

const controller = new authentication.AuthenticationController();
const response = await controller.executeRequest(loginRequest) as authentication.LoginWithHuaweiIDResponse;

if (response.state !== loginRequest.state) {
  return;
}

const credential = response.data!;
const authorizationCode = credential.authorizationCode;
const unionID = credential.unionID;
```

要点：

- `forceLogin = false`：未登录华为账号时不拉起登录页，通常返回未登录错误码。
- `state` 必须校验，避免跨站请求伪造。
- `authorizationCode` 给服务端换取令牌，不在 ArkTS 客户端处理密钥。
- `unionID` 可作为当前用户关联键；业务缓存可用 `AppStorage` / `PersistentStorage`，生产环境要替换为真实账号体系。

## 授权取消

适用于用户解绑或样例中“每次使用都重新授权”的演示场景：

```ts
const cancelRequest = new authentication.HuaweiIDProvider().createCancelAuthorizationRequest();
cancelRequest.state = util.generateRandomUUID();
const controller = new authentication.AuthenticationController();
const response = await controller.executeRequest(cancelRequest) as authentication.CancelAuthorizationResponse;

if (response.state !== cancelRequest.state) {
  return;
}
```

不要把取消授权作为所有业务的默认行为；生产场景应按隐私、合规和产品体验决定是否自动取消。

## 推荐文件拆分

- `pages/Index.ets`：启动后的静默登录、设备适配、页面切换。
- `pages/PersonalInfoPage.ets`：账号相关入口聚合。
- `common/UserInfo.ets`：头像、手机号、地址、发票抬头等用户资料类型。
- `common/Utils.ets`：取消授权、错误提示、头像文件复制等工具函数。
- `common/ErrorCodeEntity.ets`：把 Account Kit 和 FunctionalButton 错误码集中管理。

## 错误处理

优先分组处理：

- 未登录：提示用户使用华为账号登录或提供其他登录方式。
- 用户取消：通常静默返回，不弹错误。
- 网络异常：提示检查网络后重试。
- 系统服务异常：提示稍后重试或换登录方式。
- 重复请求：通常无需额外处理。

样例中出现的关键错误码包括：

| 场景 | 错误码 |
| --- | --- |
| 未登录华为账号 | `1001502001` |
| 授权网络异常 | `1001502005` |
| 内部错误 | `1001502009` |
| 用户取消授权 | `1001502012` |
| 系统服务异常 | `12300001` |
| 重复请求 | `1001500002` |
