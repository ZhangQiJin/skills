---
name: mobile-native-reverse-ui-analysis
description: Reverse-engineers UI inputs (HTML from web analysis, screenshots, design notes, or layout snippets) into 12-dimension structured specs for native iOS (SwiftUI/UIKit) and Android (Jetpack Compose/XML), then outputs consolidated one-shot AI prompts to generate Swift/Kotlin UI. Use when converting web or designs to native mobile, asking for iOS+Android UI逆向, 原生 UI 提示词, SwiftUI/Compose 还原, or pairing with html-page-reverse-ui-analysis after HTML analysis.
---

# 移动原生 UI 逆向分析（全维度专业版 → iOS / Android 一键原生提示词）

## 何时使用

- 在已用 **html-page-reverse-ui-analysis** 技能完成网页 12 维分析后，需要**同屏双端原生**实现说明与提示词
- 用户提供截图、Figma/设计说明、或 HTML/网页分析结论，要求**生成 Swift + Kotlin 原生 UI 描述**
- 需要**全维度专业拆解**（结构、组件、主题、交互、无障碍）并输出**可复制的一键生成提示词**

## 角色设定

以**移动端 UI 架构师**视角工作：将输入映射为 iOS Human Interface Guidelines 与 Android Material Design 3 下的视图层级、组件选型、主题 token 与状态管理建议，避免把 Web 概念原样搬运到原生（如不用 `div` 描述布局，改用 `VStack`/`Column` 等）。

## 工作流程

1. **确认输入**：至少一项——(a) HTML/网页逆向分析结果；(b) 截图 + 简要说明；(c) 设计稿文字描述；(d) 现有 `xml`/`storyboard` 片段。缺失则请用户补充目标平台（仅 iOS / 仅 Android / 双端）与最低系统版本（若已知）。
2. **统一抽象**：先把界面理解为「屏幕 → 区域 → 可复用组件 → 原子控件」，再分别落到 iOS 与 Android 的惯用 API（见 [reference.md](reference.md)）。
3. **按维度输出**：严格按 [reference.md](reference.md) **12 个维度**顺序输出（序号 1→12），每维同时覆盖 **iOS** 与 **Android** 两行或并列小节，信息尽量具体到组件名与布局语义。
4. **双端一键提示词**：在第 12 维之后，单独两节：
   - **「iOS 最终生成提示词」**：一段完整中文（或用户偏好语言），可直接用于生成 SwiftUI 或 UIKit 代码
   - **「Android 最终生成提示词」**：一段完整描述，用于生成 Jetpack Compose 或 XML+View 代码  
   若用户指定单平台，只输出对应一节。
5. **缺失信息**：无法从输入推断的项标注「需设计稿确认」或「推断为…」，并在一键提示词中写明假设（如深色模式、动态字体）。

## 输出语言

- 与用户一致；默认**中文**正文，保留必要英文技术名（SwiftUI、Compose、NavigationStack 等）。
- 代码块内语言标签：`swift`、`kotlin`、`xml` 等按实际片段使用。

## 与 HTML 技能的衔接

若用户已提供 HTML 逆向分析的 12 维结果：  
**不要重复冗长网页分析**，可简要摘要第 1～2 维，从第 3 维起 focus 在**原生映射**；12 维仍须逐条覆盖（可与网页结论对照标注「由 HTML 第 X 维映射」）。

## 附加资源

- 12 维移动版定义、组件书写格式、一键提示词模板：见 [reference.md](reference.md)
