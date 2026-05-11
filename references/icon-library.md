# Icon Library — 图标库生成规范

图标是独立设计资产，必须先生成图标 SVG 文件目录，再由 UI 组件和页面统一导入使用。AI 不得在页面或组件中临时内联 SVG 路径。

---

## 一、产物要求

每次生成 UI 设计交付物时，必须同时生成独立图标目录 `src/components/icons/`。目录位置、文件命名规范以 [artifact-structure.md](./artifact-structure.md) 为准。

每个图标是一个独立 **纯 SVG 文件**，通过 `vite-plugin-svgr` 在 React 中作为组件导入。

禁止在页面或 UI 组件中直接写 SVG 路径。所有图标必须从 `src/components/icons/` 导入。

---

## 二、SVG 文件结构

每个图标文件都必须是一个独立 `.svg` 文件，结构如下：

```svg
<svg
  xmlns="http://www.w3.org/2000/svg"
  viewBox="0 0 24 24"
  fill="none"
  stroke="currentColor"
  stroke-width="1.5"
  stroke-linecap="round"
  stroke-linejoin="round"
  aria-hidden="true"
>
  <path d="..." />
</svg>
```

### 必须遵守

- 每个图标一个独立 `.svg` 文件，文件名使用 kebab-case（如 `home.svg`、`heart-rate.svg`）
- **不写 `width` 和 `height` 属性**，尺寸由调用方通过 CSS 控制
- `viewBox` 固定为 `"0 0 24 24"`
- 使用 `currentColor` 继承父元素颜色，不硬编码颜色值
- 禁止引用外部图片、字体、CDN 或远程资源
- 同一套图标必须保持线性/面性风格一致
- 禁止把文字、数字或业务缩写画成图标主体，即使它们是用 `<path>` 绘制也不允许

---

## 三、图标范围与隐喻

AI 必须根据产品实际页面生成所需图标。不要为了凑完整图标库生成无关图标。若场景未明确，可从以下通用图标中选择实际会使用的部分：

| 文件名 | 用途 |
|--------|------|
| `home.svg` | 首页/概览 |
| `device.svg` | 设备/硬件 |
| `report.svg` | 报告/文档 |
| `profile.svg` | 用户/我的 |
| `search.svg` | 搜索 |
| `settings.svg` | 设置 |
| `alert.svg` | 提醒/异常 |
| `plus.svg` | 添加 |
| `check.svg` | 成功/完成 |
| `close.svg` | 关闭/失败 |

业务图标的隐喻应来自用户如何理解这个概念，而不是来自领域模板。生成业务图标前先问：

1. 用户在日常语言里如何称呼这个对象或动作？
2. 这个概念有没有用户熟悉的实物、动作、状态或方向隐喻？
3. 24x24 下是否一眼可辨，还是必须依赖文字标签？
4. 图标是否会与同页其他图标、状态色或数据符号混淆？
5. 是否正在用领域刻板印象替代真实语义，例如把所有健康指标都画成波形或液滴？

业务缩写只能作为文字标签出现，不能作为图标主体。例如 `BP`、`O2`、`HR` 可以作为指标文字，不得放在图标容器中替代图标。

---

## 四、引用规范

在 UI 组件或页面中使用图标（通过 `vite-plugin-svgr` 导入）：

```tsx
import HomeIcon from '../components/icons/home.svg?react'

// 使用，尺寸通过 className 控制
<HomeIcon className="w-6 h-6 text-primary" />

// 激活态颜色通过 Tailwind 类名控制
<HomeIcon className={`w-5 h-5 ${active ? 'text-primary' : 'text-text-secondary'}`} />
```

`?react` 后缀是 `vite-plugin-svgr` 的导入语法，告知 Vite 将 SVG 转换为 React 组件。`currentColor` 会继承 `text-*` 类名的颜色。

禁止：

- 在页面或 UI 组件中直接写 SVG 路径
- 使用字母、数字、emoji 代替图标
- 使用 `<path>` 绘制字母、数字或业务缩写来伪装成图标
- 在组件中临时内联图标内容
- 使用外部图标库链接（如 Font Awesome CDN）

---

## 五、质量标准

- 图标语义必须清晰，一眼能看出用途
- 图标复杂度适中，24x24 下仍可辨认
- 同一套图标线宽、端点、转角、视觉重心一致
- 图标不能比文字抢视觉层级
- `src/components/icons/` 只包含最终设计会用到的图标，避免无意义膨胀
