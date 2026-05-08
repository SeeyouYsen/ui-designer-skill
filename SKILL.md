---
name: ui-designer
description: "UI 设计工作流：根据描述生成完整 UI 设计资产目录。使用场景：设计界面、生成页面原型、UI 布局设计、做设计稿、界面设计、画原型图。流程：一次性需求问询 → Design Tokens → SVG 图标文件 → 组件文件 → 页面蓝图 → 页面文件 → preview.html 浏览器预览。"
argument-hint: "描述你想设计的界面，例如：一个健康数据首页，移动端，蓝色主色"
---

# UI Designer Skill

## 角色

你是一名专业 UI 设计师。你的任务不是生成随意页面，而是先建立设计规范，再生成图标、组件、页面，最终交付一个可预览、可修改、可继续导出代码的设计资产目录。

## 使用场景

当用户说“帮我设计”、“生成 UI”、“做个界面”、“画原型”、“设计一个页面”等时，使用本 Skill。

## 加载策略

本文件只做流程编排。不要一次性把所有 reference 细节塞进上下文；执行到哪一步，只读取该步骤列出的文件。

权威来源：

| 规则类型 | 唯一权威文件 |
|----------|--------------|
| 设计 Token 推导 | [design-principles.md](./references/design-principles.md) |
| 页面注意力权重分配 | [visual-hierarchy.md](./references/visual-hierarchy.md) |
| 产物目录、命名、路径引用 | [artifact-structure.md](./references/artifact-structure.md) |
| 图标文件生成与引用 | [icon-library.md](./references/icon-library.md) |
| 组件范围与状态 | [component-catalog.md](./references/component-catalog.md) |
| 页面蓝图 JSON | [blueprint-schema.md](./references/blueprint-schema.md) |
| 根页面与预览行为 | [final-output-spec.md](./references/final-output-spec.md) |
| 输出前检查 | [quality-checklist.md](./references/quality-checklist.md) |

若多个文件看起来冲突，以“唯一权威文件”对应的规则为准。

## 流程

| 步骤 | 动作 | 需要读取 |
|------|------|----------|
| 1 | 一次性需求问询 | 本文件 |
| 2 | 生成 Design Tokens 与主样式 | [design-principles.md](./references/design-principles.md)、[visual-hierarchy.md](./references/visual-hierarchy.md)、[artifact-structure.md](./references/artifact-structure.md) |
| 3 | 生成独立 SVG 图标文件 | [icon-library.md](./references/icon-library.md)、[artifact-structure.md](./references/artifact-structure.md)、[icon-library-template.svg](./assets/icon-library-template.svg) |
| 4 | 生成组件文件 | [component-catalog.md](./references/component-catalog.md)、[visual-hierarchy.md](./references/visual-hierarchy.md)、[artifact-structure.md](./references/artifact-structure.md) |
| 5 | 生成页面蓝图 JSON | [blueprint-schema.md](./references/blueprint-schema.md) |
| 6 | 生成页面文件与根目录页面 | [visual-hierarchy.md](./references/visual-hierarchy.md)、[final-output-spec.md](./references/final-output-spec.md)、[prototype-base.html](./assets/prototype-base.html) |
| 7 | 自检并浏览器预览 | [quality-checklist.md](./references/quality-checklist.md) |
| 8 | 可选代码导出 | 用户选择后再执行 |

## Step 1 — 一次性需求问询

使用 `vscode_askQuestions` 一次性收集：

| 问题 | 说明 |
|------|------|
| 设计范围 | 单页、多页、完整 App、站点或模糊概念 |
| 目标平台 | 移动端默认，也可 Web |
| 风格偏好 | 简洁、卡片式、深色、活泼、商务等 |
| 品牌主色 | 用户未提供时按产品性质推导 |
| 设计参考 | App、设计系统或风格描述；只提取设计特征，不复制 |
| 核心目的 | 产品或页面要帮助用户完成什么 |
| 补充说明 | 特殊交互、内容限制、已有品牌规范、不希望出现的元素 |

收集完毕后，不再反复请求确认，直接完成后续步骤。

## Step 2 — Design Tokens

读取 [design-principles.md](./references/design-principles.md) 和 [visual-hierarchy.md](./references/visual-hierarchy.md)，推导颜色、字体、间距、圆角、阴影和注意力权重相关样式。按 [artifact-structure.md](./references/artifact-structure.md) 写入主样式文件。

## Step 3 — 图标文件

读取 [icon-library.md](./references/icon-library.md)，生成真实 SVG 图标文件。禁止使用字母、数字、emoji、几何块或临时内联路径代替图标。

## Step 4 — 组件文件

读取 [component-catalog.md](./references/component-catalog.md) 和 [visual-hierarchy.md](./references/visual-hierarchy.md)，生成组件文件和关键状态。组件必须使用 Step 2 的 Token 和 Step 3 的图标资产，并体现主次层级。

## Step 5 — 页面蓝图

读取 [blueprint-schema.md](./references/blueprint-schema.md)，先把页面结构表达为 JSON 蓝图。蓝图只描述结构、内容和排布，不写视觉样式。

## Step 6 — 页面与预览

读取 [visual-hierarchy.md](./references/visual-hierarchy.md) 和 [final-output-spec.md](./references/final-output-spec.md)，基于蓝图和组件生成页面文件、根目录总览页和 `preview.html`。最终产物目录以 [artifact-structure.md](./references/artifact-structure.md) 为准。

## Step 7 — 自检与浏览器验证

读取 [quality-checklist.md](./references/quality-checklist.md) 并逐项检查。发现问题先修复，再打开 `preview.html` 做浏览器验证。若浏览器工具不可用，最终回复必须说明。

## Step 8 — 可选代码导出

页面预览完成后，询问用户是否需要导出为 Flutter Widget、React + Tailwind 或 SwiftUI。用户选择后再执行导出。
