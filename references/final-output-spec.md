# Final Output Spec — 根页面与预览规范

本文件只定义根目录 HTML 页面应该如何展示与预览。目录结构、文件命名、相对路径引用以 [artifact-structure.md](./artifact-structure.md) 为唯一权威。

---

## 一、共享要求

- 所有根目录 HTML 必须引用主样式文件，引用路径以 [artifact-structure.md](./artifact-structure.md) 为准。
- 不依赖外部 CDN、在线字体或远程资源。
- 不使用内联 `style` 属性。
- 所有视觉值必须来自主样式文件中的 CSS Variables。
- 文件标题必须包含产品名与平台，例如：`健康管理 App UI Design`。
- 根页面用于总览或入口，不替代 `icon/`、`components/`、`pages/` 中的资产文件。

---

## 二、design-system.html

必须展示 Design System 的可视化结果：

- 色彩系统：色盘关系、主色阶、语义色、背景层级、文字层级；若使用渐变，必须展示渐变色彩路径、用途、端点/中段预览、实际组件渲染和可读性结果。
- 字体系统：字号、行高、字重、文字颜色/对比、用途和真实渲染效果；必须展示三轴组合样例，不能只展示从 H1 到 Caption 的单列阶梯。
- 间距系统：spacing scale 的可视化条块。
- 圆角系统：不同 radius token 的真实圆角效果。
- 阴影系统：不同 elevation token 的真实层级。
- 暗色模式：仅当用户要求时展示 light / dark 双套 token。

---

## 三、icon-library.html

必须遵守 [icon-library.md](./icon-library.md)。

展示要求：

- 展示最终产物中所有图标文件。
- 每个图标展示预览、名称、symbol id、文件名和用途。
- 图标预览必须引用真实图标文件，不得复制 symbol 内容。

---

## 四、components.html

必须遵守 [component-catalog.md](./component-catalog.md)。

展示要求：

- 按组件类别分组。
- 每个组件展示名称、用途和关键状态。
- 组件状态必须真实渲染，不得只用文字描述。
- 组件展示必须来自 `components/` 中的组件文件，推荐用 `iframe` 或明确链接到对应文件。
- 不得在 `components.html` 中重新发明一套与组件文件不同的样式。

---

## 五、preview.html

`preview.html` 是最终浏览入口。

Preview 的第一视觉重点必须是用户要的产品界面本身，而不是交付文档外壳。根页面导航可以存在，但不得显著压缩、遮挡或弱化真实 UI。

展示要求：

- 单页面设计：直接预览对应页面文件。
- 多页面 App / 站点：提供页面切换控件。
- 页面预览必须来自 `pages/` 中的页面文件，推荐用 `iframe` 或可切换的页面容器承载实际页面文件内容。
- mobile 平台使用固定手机容器展示，保留状态栏和安全区概念。
- web 平台使用桌面画布展示，内容区居中。
- web 平台的 `preview.html` 必须按以下优先级处理：真实业务界面完整性 > 页面切换控件 > 交付文档美观。若设计包含左侧导航、主内容和右侧辅助栏，预览容器不得因为交付页 header 或过窄 frame 导致关键栏位默认隐藏。
- Preview 画布顶部必须完整可见，不得被文档顶部 sticky header 遮挡。

### Web 预览容器约束

- `preview.html` 的产品界面容器应使用接近真实目标平台的宽度，而不是缩成装饰性小卡片。
- 外层说明、导航、标题不能压缩真实 UI；必要时让真实 UI 独占首屏主要空间。
- 页面文件本身若需要三栏布局，预览容器默认宽度必须足以展示三栏；不能依赖用户横向滚动才看到关键栏位。
- 如果浏览器视口不足以完整展示 Web 设计，应优先隐藏交付文档装饰，而不是隐藏业务界面关键栏位。

### 多页面切换约定

页面切换控件必须有稳定 DOM 约定，默认打开第一个页面：

```html
<button class="page-button" data-page="page-home" aria-selected="true">首页</button>
<iframe id="page-home" class="page-frame is-active" src="./pages/page-home.html"></iframe>
```

- `data-page` 必须与目标 `.page-frame` 或 `.page-preview` 的 `id` 完全一致。
- 同一时间只能有一个页面按钮处于 `aria-selected="true"`。
- 同一时间只能有一个页面容器处于 `is-active`。
- 禁止为每个产物写一套互不兼容的页面切换结构。

---

## 六、交互要求

- 多页面切换不能刷新整个文档。
- 所有按钮、输入框和控件至少有 hover 或 active 视觉反馈。
- 不需要实现真实业务逻辑，但视觉状态必须可信。

---

## 七、禁止事项

- 禁止只生成一个胖 HTML 代替目录化资产输出。
- 禁止只给代码不打开 `preview.html`。
- 禁止让交付文档导航成为首屏主角，导致真实产品界面像被嵌在小预览框里。
- 禁止 Preview 和 Components 使用两套不同样式。
- 禁止使用大段说明文字代替真实 UI。
- 禁止出现未替换的模板占位符，例如 `{{PRODUCT_NAME}}`。
- 禁止使用字母、数字、emoji 或几何色块代替图标。
- 禁止在根页面内联图标定义、组件定义或页面本体来绕过资产目录。
