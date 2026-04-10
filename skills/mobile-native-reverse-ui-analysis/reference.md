# 移动原生逆向分析 — 12 维度（iOS + Android）

执行时按序号 **1→12** 完整输出；每维建议用 **iOS** / **Android** 并列小标题。最后输出 **iOS 最终生成提示词** 与 **Android 最终生成提示词**（或单平台其一）。

---

## 1. 屏幕与产品上下文（Screen Metadata）

**iOS**

- 屏幕名称、所属流程（Onboarding / 主 Tab / 模态等）
- 推荐入口：`NavigationStack`、`sheet`、`fullScreenCover`
- 目标：iOS 15+ / 16+ / 17+（若未知则写「待确认」）

**Android**

- `Activity` / 单 `Activity`+`NavHost` / `Dialog` 等承载方式
- Material 3 是否强制、Edge-to-edge 与状态栏处理
- `minSdk` / `compileSdk`（若未知则写「待确认」）

**共用**

- 页面类型（列表、表单、详情、仪表盘等）
- 若来自 Web：对应路由或页面用途一句话

---

## 2. 视图层级结构（View Hierarchy）

**iOS**

- 顶层：`NavigationStack` + root、`TabView`、`ScrollView` + `LazyVStack` 等
- 树状或缩进描述子视图嵌套

**Android**

- 顶层：`Scaffold`、`Column`+`verticalScroll`、`LazyColumn` 等
- 与 `TopAppBar`、`BottomBar`、`FAB` 的相对关系

**共用**

- 与 Web「Header/Main/Footer」对应的原生区域命名

---

## 3. 关键 UI 组件列表（Native Components）

逐条列出，格式示例：

```
组件名称：顶部应用栏
iOS：NavigationStack + toolbar / large title 与否
Android：TopAppBar / CenterAlignedTopAppBar（M3）
位置：顶部固定
功能：标题、返回、右侧菜单
样式：背景色、标题字体层级
交互：按钮点击、overflow 菜单
```

列表须覆盖：导航、列表/网格、卡片、输入框、按钮、Chip/Badge、底部栏、空状态、加载与错误态（若可见）。

---

## 4. 视觉元素（Visual Elements）

**iOS**

- `SF Symbols` 命名建议或「自定义 PDF/矢量」
- `AsyncImage` / `Image`、分隔线 `Divider`、背景 `Material`

**Android**

- Material Icons（Outlined/Filled）、`Icon`/`ImageVector` 或 Coil 加载网络图
- `HorizontalDivider`、`Brush` 渐变背景

**共用**

- 装饰性图形、插图区域、品牌 Logo 位

---

## 5. 色彩系统（Color & Theming）

**iOS**

- `Color` / Asset Catalog 命名建议；`light`/`dark` 语义色
- `accentColor`、列表背景、分组背景、分隔线颜色

**Android**

- `ColorScheme`（`primary`、`surface`、`onSurface` 等 M3 roles）
- `dynamicColor`（Android 12+）是否启用

**共用**

- 主色/辅色/中性色（HEX 或语义名）；对比度与可读性一句点评

---

## 6. 字体与排版（Typography）

**iOS**

- 文本样式：`largeTitle`、`headline`、`body`、`caption` 等
- Dynamic Type：是否支持多档缩放、最大字号下截断/换行策略

**Android**

- `MaterialTheme.typography`（`displayLarge`…`labelSmall`）
- `sp` 与行高；大屏/折叠屏是否调整

**共用**

- 标题层级与正文层级对应关系

---

## 7. 视觉风格总结（Design Style）

2～3 个关键词 + 依据（扁平、极简、Material 3、玻璃态、商务、游戏化等），并说明 **iOS 与 Material 对齐方式**（例如「iOS 侧偏系统标准控件；Android 侧 M3 filled button」）。

---

## 8. 动效与交互（Interaction & Motion）

**iOS**

- 转场、`withAnimation`、列表插入删除动画、`swipeActions`
- 触觉 `UIImpactFeedbackGenerator` 是否建议

**Android**

- `AnimatedVisibility`、`animateItem`、`SwipeToDismiss`
- Ripple、Predictive back（如相关）

**共用**

- 下拉刷新、分页加载、骨架屏、BottomSheet/半屏、SnackBar/Toast 文案位

---

## 9. 适配与无障碍（Layout, Safe Area & A11y）

**iOS**

- Safe Area、横竖屏、`sizeClass`；键盘避让
- VoiceOver：`accessibilityLabel`、`accessibilityHint`、焦点顺序

**Android**

- Window insets、`enableEdgeToEdge`、键盘 IME padding
- TalkBack：可点击区域、`contentDescription`、合并语义

**共用**

- RTL、大字号、最小点击区域（约 44pt / 48dp）

---

## 10. 依赖与工程约束（Dependencies）

**iOS**

- 纯 SwiftUI / UIKit 混编；SPM 第三方（如 Kingfisher、SnapKit）
- 是否需 `@Observable` / TCA 等（仅当用户提及）

**Android**

- Compose BOM、Navigation-Compose、Coil；或 View 体系 + ConstraintLayout
- 主题与 `Theme.Goalgo` 类项目封装（若从仓库可知则对齐命名）

---

## 11. 架构与可维护性（Code Quality）

**iOS**

- 建议拆分：`View` + `ViewModel`（`Observable`）或等价模式；预览 `Preview`
- 魔法数下沉为常量或 Design tokens

**Android**

- `Composable` 文件拆分、`remember`/`derivedStateOf` 使用原则
- 字符串资源、维度资源

**共用**

- 命名规范、单文件行数控制、状态上提边界

---

## 12. 数据、状态与复用（State & Reusability）

**iOS**

- `@State` / `@Bindable` / 列表 `Identifiable` 模型字段
- 可复用子视图：参数列表（等价 HTML 技能中的 Card 参数化）

**Android**

- `State`、`rememberSaveable`、`LazyList` item key
- 可复用 Composable 签名建议

**共用**

- 加载中/空/错三态由谁持有；是否需要分页与下拉刷新状态机

---

## iOS 最终生成提示词（模板）

输出时替换 `{{}}`，形成**一段可复制**的完整文字：

```
请用 SwiftUI（若需 UIKit 请写明）实现以下 iOS 界面，最低系统 {{版本}}。
整体风格：{{关键词}}；主色 {{颜色说明}}；遵循系统动态字体与深色模式。
导航结构：{{NavigationStack/Tab 等}}。
页面结构：
- {{区域1：组件与布局}}
- {{区域2：…}}
交互：{{点击、滑动、刷新、弹层等}}。
无障碍：为图标与按钮补充 accessibilityLabel。
请给出完整可编译的示例代码（含预览），状态用 @Observable / @State 等合理拆分。
```

---

## Android 最终生成提示词（模板）

```
请用 Jetpack Compose（若需 XML+View 请写明）与 Material 3 实现以下 Android 界面，minSdk {{版本}}。
整体风格：{{关键词}}；ColorScheme：{{primary/surface 等说明}}；支持浅色/深色（及动态取色若需要）。
顶层：{{Scaffold + TopAppBar/BottomBar}}。
页面结构：
- {{区域1}}
- {{区域2}}
交互：{{Ripple、Swipe、ModalBottomSheet 等}}。
无障碍：contentDescription 与合并语义说明。
请给出完整可编译的示例代码（含 Theme 占位），列表用 LazyColumn 并处理空/加载态。
```

---

## 输入示例

**自 HTML 分析承接**

```
以下为网页逆向结论摘要：…（或粘贴 html-page-reverse-ui-analysis 输出）
请生成 iOS + Android 原生一键提示词。
```

**自截图**

```
附件为截图，双端原生还原，支持深色模式。
```
