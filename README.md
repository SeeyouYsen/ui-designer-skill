# UI Designer Copilot Skill

一个完整的 UI 设计工作流 Copilot Skill，帮助快速生成设计系统、组件库、图标库和完整的原型交付文档。

## 🎯 使用场景

- 📐 设计新的产品界面或功能页面
- 🎨 生成统一的设计系统和 Design Token
- 🏗️ 创建可复用的组件库和模式库
- 📱 生成移动端或 Web 端完整原型
- 🔄 快速迭代设计方案

## 📋 工作流程

```
需求描述
    ↓
生成 Design Tokens（色彩、字体、间距、圆角等）
    ↓
生成独立 SVG 图标库
    ↓
生成组件库（Button、Card、Input 等）
    ↓
生成页面蓝图 JSON
    ↓
拼装页面原型 HTML
    ↓
合并成单一交付 HTML（Design System + Icon Library + Components + Preview）
    ↓
浏览器预览和验证
```

## 📁 文件结构

```
ui-designer/
├── SKILL.md                           # Skill 入口和流程定义
├── README.md                          # 本文件
└── references/
    ├── design-principles.md           # 设计令牌规则、色彩系统、字体系统、渐变规范
    ├── visual-hierarchy.md            # 注意力权重框架、注意力分配方法
    ├── component-catalog.md           # 组件选择框架、常见组件参考
    ├── icon-library.md                # SVG 图标库生成规范
    ├── quality-checklist.md           # 完整质量检查清单
    ├── final-output-spec.md           # 最终交付规范、HTML 结构、Tab 要求
    ├── blueprint-schema.md            # 页面蓝图 JSON 架构
    └── artifact-structure.md          # 文件目录结构规范
└── assets/
    ├── prototype-base.html            # 原型 HTML 基础模板
    ├── final-output-template.html     # 最终交付 HTML 模板
    └── icon-library-template.svg      # SVG 图标库模板
```

## 🚀 快速开始

### 1. 触发 Skill

在 Copilot Chat 中输入：
```
帮我设计一个健康管理应用的 UI
```

或提供更详细的需求：
```
设计一个家庭健康监测 App
- 用户角色：患者、家属、医生
- 主要功能：心率、血氧、血压、体温监测和趋势
- 平台：iOS
- 风格：专业、可信任、亲和力
```

### 2. Skill 输出物

Skill 会在 `/tmp/ui-design-{产品名}/` 下生成：

- **index.html** — 单一交付文件，包含四个 Tab：
  - Design System：色彩、字体、间距、圆角、阴影、渐变
  - Icon Library：所有 SVG 图标库
  - Components：组件库和用法示例
  - Preview：完整的页面原型预览

- **ui-icons.svg** — 独立的 SVG Sprite 文件

- **styles/design-system.css** — 完整的 CSS 变量定义

### 3. 验收标准

交付前必须通过 [quality-checklist.md](./references/quality-checklist.md) 的完整检查：

- ✅ 色彩系统完整，无机械梯度
- ✅ 字体三轴独立调度，无单调阶梯
- ✅ 注意力权重分配合理
- ✅ 图标库完整，无字母/数字/emoji 替代
- ✅ 所有图标通过 `./ui-icons.svg#icon-xxx` 引用
- ✅ 渐变无脏灰、泥色、破坏可读性
- ✅ 所有文字满足 WCAG AA 对比度

## 🎨 核心设计原则

### 色彩系统

- **避免机械梯度**：不要把主色拉成明暗梯度，应该根据语义和功能生成真实的色盘。
- **渐变必须有语义**：渐变用于品牌、主操作、状态、空间或数据编码，不是为了"高级"。
- **颜色路径有逻辑**：跨冷暖、跨互补的渐变必须有明确过渡路径，禁止脏灰、泥色、污渍感。

参考：[design-principles.md — 渐变色彩路径](./references/design-principles.md#渐变色彩路径)

### 字体系统

- **三轴独立调度**：字号、字重、文字颜色是三个独立维度，不能绑定成"H1 黑→Caption 灰"的单调梯度。
- **组合展示**：Design System 必须展示三轴组合样例，证明它们是独立的。
- **语义分离**：文字颜色单独参考色彩 token，不写进字体 token。

参考：[design-principles.md — 字体注意力三轴](./references/design-principles.md#字体注意力三轴)

### 注意力层级

- **按场景分配**：不是固定 L1/L2/L3，而是根据用户任务、决策价值、操作紧迫度动态分配权重。
- **多通道协作**：位置、尺寸、对比、颜色、形状、动画都是注意力资源，应避免全部堆砌。
- **权重验证**：完成后必须通过"眯眼测试"、灰度测试、权重反读等方式验证。

参考：[visual-hierarchy.md](./references/visual-hierarchy.md)

### 图标库

- **独立 SVG 文件**：图标必须存储在独立的 `ui-icons.svg` 中，不得内联在 HTML。
- **统一引用**：Components 和 Preview 都通过 `<use href="./ui-icons.svg#icon-name">` 引用。
- **禁止替代**：不使用字母、数字、emoji 代替图标；所有图标都是设计的矢量。

参考：[icon-library.md](./references/icon-library.md)

## 📚 详细文档

每个模块有独立的详细文档，按需阅读：

| 文档 | 内容 | 何时阅读 |
|------|------|---------|
| [design-principles.md](./references/design-principles.md) | 色彩、字体、间距、渐变、阴影规则 | 生成 Design Tokens 时 |
| [visual-hierarchy.md](./references/visual-hierarchy.md) | 注意力权重分配框架 | 组织信息层级时 |
| [component-catalog.md](./references/component-catalog.md) | 组件选择框架、常见组件参考 | 确定需要哪些组件时 |
| [icon-library.md](./references/icon-library.md) | SVG 图标库规范 | 生成图标库时 |
| [quality-checklist.md](./references/quality-checklist.md) | 完整质量检查 | 交付前验收 |
| [final-output-spec.md](./references/final-output-spec.md) | 最终 HTML、CSS、Tab 结构 | 验证交付格式 |
| [blueprint-schema.md](./references/blueprint-schema.md) | 页面蓝图 JSON 架构 | 设计复杂页面时 |
| [artifact-structure.md](./references/artifact-structure.md) | 文件结构和路径规范 | 检查目录是否规范 |

## 🔍 常见问题

### Q: 为什么要分离色彩和字体？
**A:** 色彩和字体是两个独立的视觉通道。如果把颜色绑定到字体变量里（如 `--h1: 28px bold black`），就无法实现"大标题用小字号但用亮色突出"或"小字号用粗体强调"这样的组合。

### Q: 图标为什么必须用 SVG Sprite？
**A:** SVG Sprite 允许在一个文件中定义所有图标，通过 `<use>` 标签引用，这样：
- 减少 HTTP 请求
- 统一管理所有图标的行为（hover、color 等）
- 避免重复代码
- 易于维护和替换

### Q: 为什么 Preview 不能直接内联 SVG path？
**A:** 内联 SVG 会导致：
- HTML 文件巨大
- 图标无法复用（每个地方都复制一份）
- 修改图标时需要改多个地方
- 无法统一管理图标的视觉特性

### Q: 渐变什么时候可以用？
**A:** 只在以下情况使用：
1. 品牌识别（Logo、Hero 背景）
2. 主要操作按钮（表示可交互和层次）
3. 数据编码（热力图、趋势）
4. 空间光感（深度、材质）

禁止：装饰按钮、小卡片、"看起来高级"但无语义的渐变。

### Q: 字体三轴怎么展示？
**A:** 在 Design System Tab 中展示组合样例，例如：

```
H1 黑色（大 + 粗 + 深）
H2 深灰（大 + 中粗 + 较深）
小字标签 亮色（小 + 细 + 亮）← 证明小字也可以亮色
引导文字 中灰（中 + 中粗 + 中灰）← 证明中字可以中灰
```

这样能证明字号、字重、颜色是独立调度的。

## 💡 最佳实践

1. **先问再做**：生成前问清楚用户的真实需求、目标用户、使用场景、参考风格。
2. **原则优先于模板**：不要套"医疗蓝"、"消费暖"、"B2B 黑白"这样的标签。
3. **色彩来自观察**：如果用户提供参考，分析参考图真正呈现的视觉关系，不直接复制颜色值。
4. **渐变必须自检**：生成前回答"为什么"，检查中间色，验证可读性。
5. **图标必须矢量**：所有图标都应该是用 SVG `<path>` 绘制的设计，不是位图或字体。
6. **测试交付**：在浏览器中预览最终 HTML，检查所有图标加载、所有文字可读、所有颜色正确。

## 📝 提交反馈

如果在使用过程中发现规则不清、输出不符预期、或有新的设计问题，欢迎在 GitHub Issues 中提交反馈。

---

**Made with ❤️ for Copilot UI Designers**

Last Updated: 2026-05-08
