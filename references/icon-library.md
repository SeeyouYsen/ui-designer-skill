# Icon Library — 图标库生成规范

图标是独立设计资产，必须先生成图标组件文件目录，再由 UI 组件和页面统一导入使用。AI 不得在页面或组件中临时内联 SVG 路径。

---

## 一、产物要求

每次生成 UI 设计交付物时，必须同时生成独立图标组件目录 `src/components/icons/`。目录位置、文件命名规范以 [artifact-structure.md](./artifact-structure.md) 为准。

每个图标是一个独立 React 组件（TSX 文件），接受 `size` 和 `className` props，内联 SVG 路径。

禁止在页面或 UI 组件中直接写 SVG 路径。所有图标必须从 `src/components/icons/` 导入。

---

## 二、图标组件结构

每个图标文件都必须是一个独立 TSX 文件，结构如下：

```tsx
interface IconProps {
  size?: number
  className?: string
}

export function IconHome({ size = 24, className = '' }: IconProps) {
  return (
    <svg
      width={size}
      height={size}
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      strokeWidth={1.5}
      strokeLinecap="round"
      strokeLinejoin="round"
      className={className}
      aria-hidden="true"
    >
      <path d="..." />
    </svg>
  )
}
```

### 必须遵守

- 每个图标一个独立 TSX 文件，组件名使用 PascalCase + `Icon` 前缀
- 必须有 `size` 和 `className` props，均有默认值
- `viewBox` 固定为 `"0 0 24 24"`
- 使用 `currentColor` 继承父元素颜色，不硬编码颜色
- 禁止在组件内写内联 `style` 属性
- 禁止引用外部图片、字体、CDN 或远程资源
- 同一套图标必须保持线性/面性风格一致
- 禁止把文字、数字或业务缩写画成图标主体，即使它们是用 `<path>` 绘制也不允许；例如血氧图标不能画成 `O2`，血压图标不能画成 `BP`

---

## 三、图标范围与隐喻

AI 必须根据产品实际页面生成所需图标。不要为了凑完整图标库生成无关图标。若场景未明确，可从以下通用图标中选择实际会使用的部分：

| 组件名 | 用途 |
|--------|------|
| `IconHome` | 首页/概览 |
| `IconDevice` | 设备/硬件 |
| `IconReport` | 报告/文档 |
| `IconProfile` | 用户/我的 |
| `IconSearch` | 搜索 |
| `IconSettings` | 设置 |
| `IconAlert` | 提醒/异常 |
| `IconPlus` | 添加 |
| `IconCheck` | 成功/完成 |
| `IconClose` | 关闭/失败 |

业务图标的隐喻应来自用户如何理解这个概念，而不是来自领域模板。生成业务图标前先问：

1. 用户在日常语言里如何称呼这个对象或动作？
2. 这个概念有没有用户熟悉的实物、动作、状态或方向隐喻？
3. 24x24 下是否一眼可辨，还是必须依赖文字标签？
4. 图标是否会与同页其他图标、状态色或数据符号混淆？
5. 是否正在用领域刻板印象替代真实语义，例如把所有健康指标都画成波形或液滴？

业务缩写只能作为文字标签出现，不能作为图标主体。例如 `BP`、`O2`、`HR` 可以作为指标文字，不得放在图标容器中替代图标。

---

## 四、引用规范

在 UI 组件或页面中使用图标：

```tsx
import { IconHome } from '../components/icons/IconHome'

// 使用
<IconHome size={24} className="text-primary" />

// 激活态颜色通过 Tailwind 类名控制
<IconHome size={20} className={active ? 'text-primary' : 'text-text-secondary'} />
```

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
