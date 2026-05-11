# UI Designer Copilot Skill

根据描述生成完整的 React + Tailwind CSS + React Router 设计资产项目，执行 `npm install && npm run dev` 即可在浏览器中预览所有页面并完成页面跳转和弹窗交互。

## 使用场景

- 设计新的产品界面或功能页面
- 生成可直接运行的移动端或 Web 端原型
- 快速迭代设计方案

在 Copilot Chat 中输入：
```
帮我设计一个健康管理应用的 UI
```

## 工作流程

```
需求问询（一次性收集）
    ↓
生成项目配置 + Design Tokens（src/index.css CSS 变量 + tailwind.config.ts）
    ↓
生成图标 React 组件（src/components/icons/*.tsx）
    ↓
生成 UI 组件（src/components/ui/*.tsx，Tailwind 类名 + props 变体）
    ↓
生成页面蓝图 JSON
    ↓
生成页面组件 + 路由（src/pages/*.tsx + src/App.tsx）
    ↓
npm install && npm run dev
```

## 输出产物

```
/tmp/ui-design-{产品名}/
├── package.json
├── vite.config.ts
├── tailwind.config.ts
├── tsconfig.json
├── index.html
└── src/
    ├── main.tsx
    ├── App.tsx                    ← 路由表（Routes / Route）
    ├── index.css                  ← Design Tokens（CSS 变量）
    ├── components/
    │   ├── icons/                 ← 图标 React 组件
    │   └── ui/                    ← UI 组件（Button、Card、Modal 等）
    └── pages/                     ← 页面组件（Link/useNavigate 跳转）
```

## 核心原则

### 色彩系统

- 避免机械梯度：根据语义和功能生成真实色盘，而不是把主色拉明暗
- 渐变必须有语义：品牌、主操作、状态、空间或数据编码，不为"高级"而渐变
- 颜色路径有逻辑：跨冷暖渐变必须有明确过渡路径，禁止脏灰、泥色

### 字体系统

- 三轴独立调度：字号、字重、文字颜色是三个独立维度，不绑定成"H1 黑→Caption 灰"的单调梯度

### 注意力层级

- 按场景分配权重，而不是套 L1/L2/L3 固定规则
- 完成后通过眯眼测试、灰度测试验证

### 图标

- 每个图标是独立 TSX 组件，接受 `size` 和 `className` props，使用 `currentColor`
- 禁止在页面或组件中直接内联 SVG 路径

### 交互

- 页面跳转：`<Link>`、`<NavLink>` 或 `useNavigate`
- 弹窗：独立组件 + 父页面 `useState` 控制

## 文件结构

```
ui-designer/
├── SKILL.md                      # Skill 入口和流程定义
├── README.md                     # 本文件
├── references/
│   ├── design-principles.md      # 设计 Token 推导规则、色彩系统、渐变规范
│   ├── visual-hierarchy.md       # 注意力权重框架
│   ├── artifact-structure.md     # React 项目结构、配置模板
│   ├── icon-library.md           # 图标 TSX 组件生成规范
│   ├── component-catalog.md      # UI 组件范围与变体
│   ├── blueprint-schema.md       # 页面蓝图 JSON 架构
│   ├── final-output-spec.md      # 路由配置、页面跳转、弹窗规范
│   └── quality-checklist.md      # 输出前质量检查清单
└── assets/
    └── icon-library-template.svg # 图标参考模板
```

