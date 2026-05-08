# Artifact Structure — 设计资产目录结构规范

最终交付物不是单个 HTML，而是一个有清晰目录结构的设计资产目录。所有图标、组件、页面、样式都必须是命名明确的独立文件。

---

## 一、标准目录结构

```text
/tmp/ui-design-{产品名}/
├── preview.html
├── design-system.html
├── icon-library.html
├── components.html
├── styles/
│   └── design-system.css
├── icon/
│   ├── icon-home.svg
│   ├── icon-device.svg
│   └── ...
├── components/
│   ├── button-primary.html
│   ├── card-data.html
│   └── ...
└── pages/
    ├── page-home.html
    ├── page-devices.html
    └── ...
```

---

## 二、各文件职责

| 路径 | 职责 |
|------|------|
| `preview.html` | 最终预览入口，汇总并切换所有页面原型 |
| `design-system.html` | Design System 总览，展示 Token 可视化 |
| `icon-library.html` | 图标总览，展示所有图标文件的预览、名称、用途 |
| `components.html` | 组件总览，列出组件目录中的所有组件文件 |
| `styles/design-system.css` | 全部 Design Token 和共享样式，只允许一个主样式文件 |
| `icon/*.svg` | 每个图标一个独立 SVG 文件 |
| `components/*.html` | 每个组件或关键变体一个独立 HTML 文件 |
| `pages/*.html` | 每个页面原型一个独立 HTML 文件 |

---

## 三、命名规范

### 图标文件

- 格式：`icon-{name}.svg`
- 使用小写 kebab-case
- 示例：`icon-home.svg`、`icon-heart-rate.svg`、`icon-blood-pressure.svg`

### 组件文件

- 格式：`{component-name}-{variant}.html`
- 组件名和变体均使用 kebab-case
- 示例：`button-primary.html`、`input-search.html`、`card-data.html`、`tabbar-scrollable.html`

### 页面文件

- 格式：`page-{page-name}.html`
- 页面名使用 kebab-case
- 示例：`page-home.html`、`page-devices.html`、`page-report-detail.html`

### 样式文件

- 主样式文件固定为：`styles/design-system.css`
- 禁止生成多个平级主题样式文件分散 Token

---

## 四、引用规则

### 根目录页面引用图标

`preview.html`、`design-system.html`、`icon-library.html`、`components.html` 必须这样引用图标：

```html
<svg class="ui-icon" aria-hidden="true">
  <use href="./icon/icon-home.svg#icon-home"></use>
</svg>
```

### 子目录页面引用图标

`components/*.html` 和 `pages/*.html` 必须这样引用图标：

```html
<svg class="ui-icon" aria-hidden="true">
  <use href="../icon/icon-home.svg#icon-home"></use>
</svg>
```

### 根目录页面引用样式

```html
<link rel="stylesheet" href="./styles/design-system.css">
```

### 子目录页面引用样式

```html
<link rel="stylesheet" href="../styles/design-system.css">
```

---

## 五、输出原则

1. 所有根目录 HTML 都是总览或入口页面，不重复承载组件/页面本体。
2. 组件本体必须存在于 `components/` 目录中，页面本体必须存在于 `pages/` 目录中。
3. Preview 中预览的页面必须来自 `pages/` 目录，而不是在 `preview.html` 中重新临时写一份页面结构。
4. Components 总览必须从 `components/` 目录汇总组件，而不是在 `components.html` 中重新发明一套组件。
5. Icon Library 总览必须从 `icon/` 目录汇总图标，而不是在 HTML 中重新复制图标内容。

## 六、范围控制

目录结构是交付边界，不代表必须生成大量资产。简单需求也应保留 `styles/`、`icon/`、`components/`、`pages/` 的资产模型，但可以减少实际文件数量。

- 单页设计：`pages/` 可以只有一个页面文件。
- 简单组件：`components/` 只生成页面真实复用或需要解释的组件。
- 图标目录：`icon/` 只放最终页面、组件和图标总览会使用的图标。
- 根目录四个 HTML 仍保留，职责保持清晰；不要退回单个胖 HTML。
- 简化的目标是减少无关资产，不是破坏目录化、可编辑、可预览的交付模型。
