---
name: harmonyos-design
description: 为 HarmonyOS ArkUI 应用、元服务与页面组件提供符合 HarmonyOS Design 的设计选型、交互约束、组件拆分、设计 token、响应式布局与实现落地指导。用于新建或重构 HarmonyOS 页面、封装通用组件、选择导航/弹层/输入/反馈模式、统一视觉语言，或需要参考官方 ArkUI 组件与 HDS 风格实现时触发。
---

# HarmonyOS Design

基于 `HarmonyOSComponentUXExamples` 提炼 HarmonyOS Design 的常用组件分类、设计 token、响应式断点和实现习惯。优先帮助 Codex 把业务需求落成“像鸿蒙原生”的 ArkUI 页面与组件，而不是只给出泛化 UI 建议。

## 工作方式

1. 先把需求映射到六类组件之一：操作、容器、输入、导航、展示、选择。
2. 优先复用系统组件与 HDS 组件，不要先写完全自定义控件。
3. 先抽设计 token，再写页面布局，不要把颜色、间距、圆角散落在业务代码。
4. 默认考虑断点、自适应、安全区、横竖屏和折叠态。
5. 同时给出设计理由和 ArkTS 落地方式，必要时补充页面结构或组件拆分建议。

## 输出要求

- 明确页面信息层级、主次操作、反馈方式和导航结构。
- 优先给出 ArkUI / HDS 原生能力映射，例如 `Tabs`、`Search`、`Progress`、`bindSheet`、`PromptAction.showToast`。
- 建议公共样式沉淀到 token 文件，如 `Color.ets`、`Padding.ets`、`Space.ets`、`CornerRadius.ets`、`Size.ets`。
- 对需要适配平板、折叠屏、2in1 或横屏的页面，必须给出断点与布局切换策略。
- 对弹窗、半模态、Toast、SnackBar、菜单等临时界面，说明触发条件、关闭方式和是否允许打断主流程。

## 快速决策

### 页面/组件分类

- 主操作触发：查 [references/component-map.md](references/component-map.md) 中操作类组件。
- 页面承载、确认、中断流程：查容器类组件。
- 搜索、文本录入、数量调节：查输入类组件。
- 顶部/底部/二级导航切换：查导航类组件。
- 提示、进度、轻反馈：查展示类组件。
- 单选、评分、开关、连续值：查选择类组件。

### 组件选型规则

- 有官方组件就不要从零造轮子。
- 有标准交互形态就不要自创状态机。
- 能用 token 驱动的尺寸和间距，不要直接硬编码到页面。
- 页面骨架先稳定，再做视觉强化，例如模糊、渐变、图标和过渡。

## 设计落地流程

### 1. 拆需求

- 明确这是列表页、详情页、表单页、导航页还是即时反馈场景。
- 标出主操作、次操作、危险操作、只读信息和可编辑信息。
- 判断是否需要跨设备自适应、分栏、横屏或折叠态。

### 2. 选组件

- 从 [references/component-map.md](references/component-map.md) 选择最接近的组件族和示例变体。
- 如果一个需求同时涉及多类组件，先定导航与容器，再定输入和反馈。

### 3. 建页面骨架

- 先用 `Navigation`、`List`、`Tabs`、`Row`、`Column` 建层级。
- 列表页默认考虑分组、吸顶、点击态、选中态和空白态。
- 表单页默认考虑键盘避让、错误提示、字符计数和提交反馈。

### 4. 抽 token

- 颜色、边距、间距、圆角、尺寸统一放进 token 文件。
- 参考 [references/design-tokens.md](references/design-tokens.md) 的拆分方式。
- 不要把同一语义的值在不同页面写成多个近似常量。

### 5. 做响应式

- 参考 [references/responsive-layout.md](references/responsive-layout.md) 处理宽度断点、纵横比和导航模式切换。
- 对大屏场景优先考虑主从结构、分栏、侧边导航与更宽的内容容器。

### 6. 校验原生感

- 操作区是否清晰，反馈是否即时，页面是否遵守安全区。
- 弹层是否过多，导航是否重复，视觉重点是否过散。
- 页面在手机、折叠屏、平板或 2in1 下是否仍保持层级稳定。

## 参考资料

- 组件总览与官方链接： [references/component-map.md](references/component-map.md)
- Token 与视觉约束： [references/design-tokens.md](references/design-tokens.md)
- 断点与适配方式： [references/responsive-layout.md](references/responsive-layout.md)
- 开发实施清单： [references/implementation-playbook.md](references/implementation-playbook.md)
