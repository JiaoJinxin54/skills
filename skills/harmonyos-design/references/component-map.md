# HarmonyOS Design 组件地图

以下内容基于 `HarmonyOSComponentUXExamples` 仓库整理，适合在 ArkUI 页面设计、组件封装和样式重构时快速选型。

## 1. 操作类

| 组件 | 适用场景 | ArkUI / HDS 能力 | 示例变体 | 设计链接 |
| --- | --- | --- | --- | --- |
| Button | 主操作、次操作、危险操作、单步提交 | `Button` | 尺寸、类型、禁用、图标、加载态 | https://developer.huawei.com/consumer/cn/doc/design-guides/button-0000001929683228 |
| ActionBar | 一组高频核心操作 | `HdsActionBar` | 横向、纵向、折叠式 | https://developer.huawei.com/consumer/cn/doc/design-guides/component_actionbar-0000002306891560 |
| Menu | 就地补充操作、长按浮层操作 | `Menu` `MenuItem` | 下拉、图标菜单、标题菜单、长按悬浮菜单 | https://developer.huawei.com/consumer/cn/doc/design-guides/menu-0000001957001877 |

使用建议：

- `Button` 先区分主次层级，再谈颜色和尺寸。
- `ActionBar` 适合承载 2 到 5 个高频动作，不要塞入低频设置项。
- `Menu` 用于收纳补充操作，不要隐藏用户最常用的主路径。

## 2. 容器类

| 组件 | 适用场景 | ArkUI / HDS 能力 | 示例变体 | 设计链接 |
| --- | --- | --- | --- | --- |
| BindSheet | 半模态展示、补充编辑、轻流程切换 | `bindSheet` | 模态/非模态、不同高度、多级面板、横屏场景 | https://developer.huawei.com/consumer/cn/doc/design-guides/bindsheet-0000001956852753 |
| Dialog | 确认、选择、输入、图文提示 | `CustomContentDialog` `SelectDialog` `TipsDialog` | 基础、图文、输入、确认、选择 | https://developer.huawei.com/consumer/cn/doc/design-guides/dialog-0000001957012569 |
| List | 信息列表、设置页、内容浏览、操作列表 | `List` `ListItem` `ListItemGroup` | 效率型、内容型、操作型 | https://developer.huawei.com/consumer/cn/doc/design-guides/list-0000001929853910 |

使用建议：

- `BindSheet` 适合不离开当前上下文的轻量任务；如果任务复杂或需要强确认，改用页面或 `Dialog`。
- `Dialog` 只承载一次性决策，不要把完整业务表单塞进弹窗。
- `List` 默认要有点击态、分组、空态和长内容策略。

## 3. 输入类

| 组件 | 适用场景 | ArkUI / HDS 能力 | 示例变体 | 设计链接 |
| --- | --- | --- | --- | --- |
| Counter | 数量调节、日期调节、列表内步进 | `Advance.Counter` | 紧凑型、内联型、日期型、列表型 | https://developer.huawei.com/consumer/cn/doc/design-guides/counter-0000001929853284 |
| Search | 搜索入口、筛选入口、搜索提交 | `Search` | 标准、推荐、混合、图标式、带搜索按钮 | https://developer.huawei.com/consumer/cn/doc/design-guides/search-0000001956852741 |
| TextBox | 文本录入、密码、说明、备注 | `TextInput` `TextArea` | 单行、多行、全宽、图标、错误、字数统计、密码 | https://developer.huawei.com/consumer/cn/doc/design-guides/textinput-0000001957012557 |

使用建议：

- 数值输入优先用 `Counter`，避免让用户在小范围数值里频繁键盘输入。
- 搜索框先确认是“即时筛选”还是“提交查询”，交互不同。
- 文本框必须定义错误态、长度限制和提交时机。

## 4. 导航类

| 组件 | 适用场景 | ArkUI / HDS 能力 | 示例变体 | 设计链接 |
| --- | --- | --- | --- | --- |
| BottomTabBar | 顶层信息架构切换 | `Tabs` `HdsTabs` 自定义 `tabBar` | 默认、舵式、背景模糊、渐变模糊、左右布局 | https://developer.huawei.com/consumer/cn/doc/design-guides/bottomtab-0000001956787789 |
| SubTab | 局部内容分组切换 | `ChipGroup` `Tabs` | 胶囊式、下划线式 | https://developer.huawei.com/consumer/cn/doc/design-guides/chipsgroup-0000001929788350 |
| TitleBar | 页面标题与顶部操作 | `HdsNavDestination` `EditableTitleBar` | 基础、强调型、抽屉型、二级页、可编辑 | https://developer.huawei.com/consumer/cn/doc/design-guides/titlebar-0000001929628982 |

使用建议：

- `BottomTabBar` 只放顶层目的地，不要拿来做页面内局部筛选。
- `SubTab` 适合平级内容段切换，不适合承载深层导航。
- `TitleBar` 负责页面身份识别和顶部动作，不要承担过重的信息展示。

## 5. 展示类

| 组件 | 适用场景 | ArkUI / HDS 能力 | 示例变体 | 设计链接 |
| --- | --- | --- | --- | --- |
| Badge | 未读数、新状态、轻提醒 | `Badge` | 基础、自定义颜色、列表型、推荐样式 | https://developer.huawei.com/consumer/cn/doc/design-guides/badge-0000001929816016 |
| Progress | 可见进度或加载状态 | `Progress` `LoadingProgress` | 线性、胶囊、环形、加载态 | https://developer.huawei.com/consumer/cn/doc/design-guides/progress-0000001929656644 |
| SnackBar | 即时操作反馈、带补救动作提醒 | `HdsSnackBar` | 定时关闭、常驻、横屏 | https://developer.huawei.com/consumer/cn/doc/design-guides/component_snackbar-0000002340726169 |
| Toast | 轻量反馈、无需用户决策 | `PromptAction.showToast` | 底部、指定位置、横竖屏不同宽度策略 | https://developer.huawei.com/consumer/cn/doc/design-guides/toast-0000001929656648 |

使用建议：

- `Badge` 只表达状态，不承载复杂文案。
- `Progress` 必须区分“有明确进度”与“无明确进度”。
- `SnackBar` 可带操作；`Toast` 只做轻反馈，不承载关键确认。

## 6. 选择类

| 组件 | 适用场景 | ArkUI / HDS 能力 | 示例变体 | 设计链接 |
| --- | --- | --- | --- | --- |
| Radio | 单选决策 | `Radio` | 对勾样式、圆点样式 | https://developer.huawei.com/consumer/cn/doc/design-guides/radio-0000001929853288 |
| Rating | 评分输入或评分展示 | `Rating` | 可交互、只展示 | https://developer.huawei.com/consumer/cn/doc/design-guides/rating-0000001929853906 |
| Slider | 连续值调节 | `Slider` | 基础、自定义、组合型 | https://developer.huawei.com/consumer/cn/doc/design-guides/slider-0000001957012565 |
| Toggle | 布尔开关 | `Toggle` | 默认、自定义颜色、尺寸、圆角 | https://developer.huawei.com/consumer/cn/doc/design-guides/toggleswitch-0000001956852745 |

使用建议：

- 互斥选项用 `Radio`，开关状态用 `Toggle`，不要混用。
- 评分既要考虑输入场景，也要考虑只读展示场景。
- `Slider` 适合连续值，离散值请补刻度或考虑其他控件。

## 7. 示例仓库的可复用目录模式

建议在你自己的 HarmonyOS 项目中参考以下拆分：

- `components/<category>/<component>/`：组件及示例页。
- `designtoken/`：颜色、尺寸、圆角、边距、间距。
- `model/`：断点、数据模型、枚举。
- `utils/`：窗口、日志、通用工具。
- `pages/`：页面入口与导航拼装。

当需求是“做一个更符合鸿蒙设计的页面”时，优先按组件类别拆文件，而不是按业务页面把所有 UI 代码堆在一个 `Index.ets`。
