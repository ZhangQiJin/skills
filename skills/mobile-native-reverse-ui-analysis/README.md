# mobile-native-reverse-ui-analysis 使用说明

`mobile-native-reverse-ui-analysis` 是一个面向移动端原生开发的 UI 逆向分析 skill。它可以把网页分析结果、截图、设计说明、HTML 片段、Storyboard/XML 片段等输入，转换成 iOS 与 Android 原生实现所需的结构化规格说明，并最终输出可直接交给 AI 生成 SwiftUI/UIKit 或 Jetpack Compose/XML 代码的一键提示词。

演示录屏：[assets/demo.mov](assets/demo.mov)

## 适用场景

- 已经使用 `html-page-reverse-ui-analysis` 完成网页 12 维分析，需要继续生成 iOS 与 Android 原生 UI 还原提示词。
- 用户提供移动端截图、Figma 设计说明、产品原型说明，希望拆解成 SwiftUI/UIKit 与 Jetpack Compose/XML 的实现方案。
- 需要把 Web 页面结构转换为原生移动端视图层级，避免继续使用 `div`、`section`、CSS 盒模型等 Web 语义。
- 希望输出完整的 12 维原生 UI 分析，包括组件、视觉、色彩、排版、交互、无障碍、工程依赖、架构拆分与状态管理。
- 需要最终得到两段可复制的一键生成提示词：`iOS 最终生成提示词` 与 `Android 最终生成提示词`。

## 文件结构

```text
mobile-native-reverse-ui-analysis/
├── SKILL.md
├── reference.md
├── README.md
└── assets/
    └── demo.mov
```

- `SKILL.md`：skill 入口说明，定义触发场景、角色、工作流与输出要求。
- `reference.md`：12 维移动原生 UI 分析标准与最终提示词模板。
- `README.md`：当前使用说明文档。
- `assets/demo.mov`：录屏演示文件。

## 安装方式

### Cursor

把本目录放入 Cursor 可识别的 skills 目录，例如：

```bash
mkdir -p ~/.cursor/minimax-skills/skills
cp -R mobile-native-reverse-ui-analysis ~/.cursor/minimax-skills/skills/
```

重启 Cursor 或刷新 agent 配置后，即可在对话中通过 skill 名称触发。

### Codex

把本目录放入 Codex/Agents 的 skills 目录，例如：

```bash
mkdir -p ~/.agents/skills
cp -R mobile-native-reverse-ui-analysis ~/.agents/skills/
```

也可以通过符号链接接入：

```bash
ln -s /path/to/mobile-native-reverse-ui-analysis ~/.agents/skills/mobile-native-reverse-ui-analysis
```

重启 Codex 后，skill 会出现在可用技能列表中。

## 如何触发

在对话中直接说出 skill 名称，或者描述与移动原生 UI 逆向相关的任务即可。

示例：

```text
请使用 mobile-native-reverse-ui-analysis，把这张截图逆向成 iOS + Android 原生 UI 提示词。
```

```text
以下是 html-page-reverse-ui-analysis 的 12 维网页分析结果，请继续生成 SwiftUI 和 Jetpack Compose 的原生实现提示词。
```

```text
帮我把这个 H5 页面转换成 iOS UIKit 和 Android XML 原生页面说明。
```

## 输入要求

至少提供以下任意一种输入：

- 网页逆向分析结果，尤其是 `html-page-reverse-ui-analysis` 输出的 12 维结论。
- 页面截图或录屏，加上页面用途说明。
- Figma、设计稿、产品原型或 PRD 的文字描述。
- HTML、XML、Storyboard、SwiftUI、Compose 等已有布局片段。

如果输入信息不足，skill 会优先补问：

- 目标平台：仅 iOS、仅 Android，还是双端。
- iOS 最低版本，例如 iOS 15+、iOS 16+、iOS 17+。
- Android minSdk、是否使用 Material 3、是否使用 Compose。
- 是否需要深色模式、动态字体、无障碍、横屏或平板适配。

## 输出结构

skill 会严格按照 12 个维度输出，每个维度同时覆盖 iOS 与 Android：

1. 屏幕与产品上下文
2. 视图层级结构
3. 关键 UI 组件列表
4. 视觉元素
5. 色彩系统
6. 字体与排版
7. 视觉风格总结
8. 动效与交互
9. 适配与无障碍
10. 依赖与工程约束
11. 架构与可维护性
12. 数据、状态与复用

第 12 维之后会额外输出：

- `iOS 最终生成提示词`
- `Android 最终生成提示词`

如果用户指定单平台，只输出对应平台的最终提示词。

## 与 html-page-reverse-ui-analysis 配合

推荐工作流：

1. 先使用 `html-page-reverse-ui-analysis` 分析网页，得到网页侧的 12 维 UI 结构。
2. 再使用 `mobile-native-reverse-ui-analysis`，把网页结论映射到移动原生组件。
3. 最后把生成的 iOS / Android 一键提示词交给 AI 编码工具生成代码。

示例输入：

```text
以下是网页 12 维逆向分析结果：
【粘贴 html-page-reverse-ui-analysis 输出】

请使用 mobile-native-reverse-ui-analysis，生成 iOS SwiftUI 与 Android Jetpack Compose 的原生 UI 实现提示词。
```

## 示例 Prompt

### 截图转双端原生提示词

```text
请使用 mobile-native-reverse-ui-analysis 分析附件截图。
目标：双端原生还原。
iOS：SwiftUI，最低 iOS 16。
Android：Jetpack Compose + Material 3，minSdk 26。
要求：输出 12 维分析，并在最后给出 iOS 和 Android 最终生成提示词。
```

### 网页分析结果转原生提示词

```text
请基于下面的网页逆向分析结果，继续使用 mobile-native-reverse-ui-analysis 输出移动端原生方案。
要求：
1. 不重复冗长网页分析。
2. 每个维度都写出 iOS 与 Android 的原生组件映射。
3. 最后生成 SwiftUI 和 Jetpack Compose 可直接使用的一键提示词。

【粘贴网页 12 维分析结果】
```

### 单平台 iOS

```text
请使用 mobile-native-reverse-ui-analysis 只输出 iOS 方案。
目标技术栈：SwiftUI。
最低版本：iOS 16。
输入：下面是页面设计说明……
```

### 单平台 Android

```text
请使用 mobile-native-reverse-ui-analysis 只输出 Android 方案。
目标技术栈：Jetpack Compose + Material 3。
minSdk：26。
输入：下面是页面设计说明……
```

## 输出质量标准

- 不把 Web 结构直接照搬到原生，不使用 `div`、`flex` 等作为最终实现描述。
- iOS 使用 `NavigationStack`、`ScrollView`、`LazyVStack`、`List`、`TabView`、`sheet` 等原生语义。
- Android 使用 `Scaffold`、`TopAppBar`、`LazyColumn`、`Column`、`Row`、`Card`、`ModalBottomSheet` 等 Material/Compose 语义。
- 色彩、字体、间距、圆角、阴影、状态、动效和无障碍都要明确说明。
- 对无法确认的信息标注“需设计稿确认”或“推断为...”，避免编造。
- 最终提示词应是一段完整、可复制、可直接用于生成代码的文字。

## 常见问题

### 只给一张截图可以用吗？

可以。skill 会基于截图做视觉和结构推断，但无法确认的数据状态、接口字段、深色模式等会标注为“需确认”。

### 可以同时输出 UIKit 和 SwiftUI 吗？

可以。默认优先 SwiftUI；如果用户要求 UIKit，会映射到 `UIViewController`、`UICollectionViewCompositionalLayout`、`UIStackView`、SnapKit 等 UIKit 方案。

### Android 必须使用 Compose 吗？

默认优先 Jetpack Compose + Material 3。如果用户要求 XML + View 体系，会改为 `Activity/Fragment`、`RecyclerView`、`ConstraintLayout`、`MaterialToolbar` 等实现建议。

### 需要输出代码吗？

这个 skill 的核心产物是“分析规格 + 一键生成提示词”。如果用户继续要求实现代码，可以把最终提示词交给编码模型或后续开发 skill 使用。

## 维护建议

- 修改 12 维定义时，优先更新 `reference.md`。
- 修改触发条件或工作流程时，更新 `SKILL.md`。
- 修改使用方式、示例和安装说明时，更新当前 `README.md`。
