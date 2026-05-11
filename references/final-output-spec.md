# Final Output Spec — 路由配置与交互规范

本文件定义 React 项目的路由结构、页面跳转和弹窗交互规范。目录结构、文件命名以 [artifact-structure.md](./artifact-structure.md) 为唯一权威。

---

## 一、顶层结构：两入口设计

`src/App.tsx` 渲染顶部切换栏，分为两个入口：

- **设计图**：嵌套的 `<Routes>`，包含所有设计页面，页面本身完全干净，无任何外层装饰
- **资源**：`<AssetsViewer>` 组件，内含 Design System / Icons / Components 三个子 tab

页面组件（`src/pages/*.tsx`）不感知外层入口切换，**不得引入或依赖 `App.tsx` 中的入口 tab**。

---

## 二、路由配置（App.tsx）

`App.tsx` 完整模板见 [artifact-structure.md §四](./artifact-structure.md)，本节只列行为约束：

### 路由规范
- 根路径 `/` 必须重定向到第一个主要设计页面
- 路由路径使用 kebab-case（如 `/report-detail`）
- 每个页面组件一条 `<Route>`，不使用动态路由（除非需求明确要求）

---

## 三、AssetsViewer 结构

`src/viewer/AssetsViewer.tsx` 是资源入口，内含三个子 tab：

```tsx
import { useState } from 'react'
import DesignSystemTab from './DesignSystemTab'
import IconsTab from './IconsTab'
import ComponentsTab from './ComponentsTab'

type Tab = 'ds' | 'icons' | 'components'

export default function AssetsViewer() {
  const [tab, setTab] = useState<Tab>('ds')
  const tabs: { id: Tab; label: string }[] = [
    { id: 'ds', label: 'Design System' },
    { id: 'icons', label: 'Icons' },
    { id: 'components', label: 'Components' },
  ]

  return (
    <div className="p-6">
      <div className="flex gap-4 border-b border-gray-200 mb-6">
        {tabs.map(t => (
          <button
            key={t.id}
            onClick={() => setTab(t.id)}
            className={tab === t.id ? 'pb-2 border-b-2 border-primary font-medium' : 'pb-2 text-gray-500'}
          >
            {t.label}
          </button>
        ))}
      </div>
      {tab === 'ds' && <DesignSystemTab />}
      {tab === 'icons' && <IconsTab />}
      {tab === 'components' && <ComponentsTab />}
    </div>
  )
}
```

### 各子 tab 内容

| Tab | 展示内容 |
|-----|----------|
| Design System | 所有 CSS Token：色彩色板（含名称和 hex）、字体阶梯、间距比例、圆角、阴影、渐变 |
| Icons | 所有 `src/components/icons/` 图标网格：图标预览 + 组件名称 |
| Components | 所有 `src/components/ui/` 组件及其主要变体展示 |

---

## 四、页面跳转

### 导航栏 / TabBar 跳转
```tsx
import { NavLink } from 'react-router-dom'

<NavLink
  to="/home"
  className={({ isActive }) =>
    isActive ? 'text-primary' : 'text-text-secondary'
  }
>
  <IconHome className="w-5 h-5" />
  <span>首页</span>
</NavLink>
```

### 命令式跳转（按钮点击后跳转）
```tsx
import { useNavigate } from 'react-router-dom'

const navigate = useNavigate()

<button onClick={() => navigate('/report-detail')}>
  查看报告
</button>
```

### 返回上一页
```tsx
<button onClick={() => navigate(-1)}>返回</button>
```

---

## 五、弹窗 / 底部抽屉 / Dialog

弹窗必须是独立组件，放在 `src/components/ui/` 中（如 `Modal.tsx`、`Sheet.tsx`、`Dialog.tsx`）。

### 状态控制（在页面组件中）
```tsx
const [modalOpen, setModalOpen] = useState(false)

<button onClick={() => setModalOpen(true)}>打开</button>

{modalOpen && (
  <Modal
    title="确认操作"
    onClose={() => setModalOpen(false)}
    onConfirm={() => {
      // 操作逻辑
      setModalOpen(false)
    }}
  >
    <p>弹窗内容</p>
  </Modal>
)}
```

### 弹窗组件结构
```tsx
interface ModalProps {
  title: string
  onClose: () => void
  onConfirm?: () => void
  children: React.ReactNode
}

export function Modal({ title, onClose, onConfirm, children }: ModalProps) {
  return (
    // 遮罩层
    <div className="fixed inset-0 bg-black/50 flex items-end justify-center z-50"
         onClick={onClose}>
      // 弹窗容器（阻止冒泡）
      <div className="bg-surface rounded-t-xl p-6 w-full max-w-md"
           onClick={e => e.stopPropagation()}>
        <h2 className="text-text-primary font-semibold">{title}</h2>
        {children}
        {/* 操作按钮 */}
      </div>
    </div>
  )
}
```

---

## 六、输出原则

1. `App.tsx` 顶层切换栏只有两个入口：**设计图**和**资源**，两者严格隔离。
2. 设计页面（`src/pages/`）完全干净，不含任何外层 tab 引用或 viewer 引用。
3. 资源 viewer（`src/viewer/`）不参与设计页面路由，不影响设计稿视觉。
4. 页面跳转必须使用 React Router 的 `<Link>`、`<NavLink>` 或 `useNavigate`，不使用 `window.location`。
5. 弹窗必须是独立组件，不在页面文件中内联弹窗结构。
6. 所有颜色、圆角、阴影值必须来自 `tailwind.config.ts` 中扩展的 Token，不使用魔法数字。
7. 第一个路由页面必须是产品核心界面（如首页），而不是"欢迎"或"加载"页。
8. 不使用外部 CDN，所有依赖通过 npm 安装。
- 禁止出现未替换的模板占位符，例如 `{{PRODUCT_NAME}}`。
- 禁止使用字母、数字、emoji 或几何色块代替图标。
- 禁止在根页面内联图标定义、组件定义或页面本体来绕过资产目录。
