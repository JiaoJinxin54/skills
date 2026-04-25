# FunctionalButton 能力模式

## 适用范围

用于元服务中通过 `FunctionalButton` 获取用户头像、手机号、收货地址和发票抬头。依赖：

```ts
import { FunctionalButton, functionalButtonComponentManager } from '@kit.ScenarioFusionKit';
```

## 通用结构

```ts
FunctionalButton({
  params: {
    openType: functionalButtonComponentManager.OpenType.GET_PHONE_NUMBER,
    label: $r('app.string.get'),
    styleOption: {
      styleConfig: new functionalButtonComponentManager.ButtonConfig()
        .width(72)
        .height(28)
        .fontSize(14)
    }
  },
  controller: new functionalButtonComponentManager.FunctionalButtonController()
    .onGetPhoneNumber((err, data) => {
      if (err) {
        showErrorMessage(this.getUIContext(), err);
        return;
      }

      const code = data.code;
      // 将 code 发送到元服务服务端，由服务端换取 Access Token 和手机号。
    })
})
```

`styleOption` 可以和业务设计系统整合，但不要改变系统授权流程的含义和用户确认链路。

## 能力映射

| 能力 | openType | 回调 | 结果处理 |
| --- | --- | --- | --- |
| 选择头像 | `CHOOSE_AVATAR` | `onChooseAvatar` | 读取 `avatarUri`，可复制到应用沙箱或上传云端 |
| 获取手机号 | `GET_PHONE_NUMBER` | `onGetPhoneNumber` | 读取 `code`，服务端换手机号 |
| 选择收货地址 | `CHOOSE_ADDRESS` | `onChooseAddress` | 保存 `ChooseAddressResult` |
| 选择发票抬头 | `CHOOSE_INVOICE_TITLE` | `onChooseInvoiceTitle` | 保存 `ChooseInvoiceTitleResult` |

## 手机号注意事项

- FunctionalButton 返回的是授权 code，不是手机号。
- 客户端拿到 code 后应发送给元服务服务端。
- 服务端调用 Account Kit 服务端 API 换取 Access Token，再获取用户手机号。
- 不要在客户端硬编码展示手机号；样例中的 `180******00` 只是演示占位。
- 未申请手机号权限时会失败，应提示权限或配置问题。

## 头像注意事项

- `avatarUri` 可能是临时地址，生产环境优先上传云端以保证多设备一致。
- 如果只做本地保存，可复制到应用沙箱再展示。
- 图片加载失败时清理无效地址，避免 UI 持续展示损坏头像。

## 地址和发票抬头

- 收货地址通常需要在 AGC 中申请权限。
- 成功后保存完整结构，不要只拼接展示文本，后续提交订单可能需要结构化字段。
- 用户取消选择时不要提示错误；网络异常或系统错误再提示。

## 错误码分组

用户取消：

- 收货地址：`1008100006`
- 发票抬头：`1010060001`
- 头像：`1001600005`
- 授权取消：`1001502012`

网络异常：

- 收货地址：`1008100002`
- 授权：`1001502005`
- 发票抬头：`1010060005`
- 头像：`1001600001`

处理建议：

- 用户取消：静默返回。
- 网络异常：用系统弹窗或 Toast 提示网络不可用。
- 其他异常：用 `getPromptAction().showToast()` 展示错误信息，避免阻塞主流程。
