# Final Output Spec — 路由配置与交互规范

本文件定义 React 项目的路由结构、页面跳转和弹窗交互规范。目录结构、文件命名以 [artifact-structure.md](./artifact-structure.md) 为唯一权威。

---

## 一、路由配置（App.tsx）

`src/App.tsx` 是路由入口，必须包含所有页面的路由定义。

### 基本结构
```tsx
import { Routes, Route, Navigate } from 'react-router-dom'
import HomePage from './pages/HomePage'
import DevicesPage from './pages/DevicesPage'
// import 其他页面...

export default function App() {
  return (
    <Routes>
      <Route path="/" element={<Navigate to="/home" replace />} />
      <Route path="/home" element={<HomePage />} />
      <Route path="/devices" element={<DevicesPage />} />
      {/* 其他路由... */}
    </Routes>
  )
}
```

### 路由规范
- 根路径 `/` 必须重定向到第一个主要页面
- 路由路径使用 kebab-case（如 `/report-detail`）
- 每个页面组件一条 `<Route>`，不使用动态路由（除非需求明确要求）
- 不使用 hash 路由（`#`）

---

## 二、页面跳转

### 导航栏 / TabBar 跳转
```tsx
import { NavLink } from 'react-router-dom'

<NavLink
  to="/home"
  className={({ isActive }) =>
    isActive ? 'text-primary' : 'text-text-secondary'
  }
>
  <IconHome size={24} />
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

## 三、弹窗 / 底部抽屉 / Dialog

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

## 四、移动端容器

移动端设计使用固定手机容器展示：

```tsx
// src/main.tsx 或 App.tsx 外层
<div className="min-h-screen bg-gray-100 flex items-center justify-center">
  <div className="w-[390px] h-[844px] bg-surface overflow-hidden relative shadow-2xl rounded-[40px]">
    {/* 页面内容 */}
  </div>
</div>
```

- 移动端容器固定宽度 390px（iPhone 14 标准），高度 844px
- Web 平台不使用手机容器，页面内容区居中展示

---

## 五、输出原则

1. `App.tsx` 必须注册所有页面路由，用户打开项目即可通过导航浏览所有页面。
2. 页面跳转必须使用 React Router 的 `<Link>`、`<NavLink>` 或 `useNavigate`，不使用 `window.location`。
3. 弹窗必须是独立组件，不在页面文件中内联弹窗结构。
4. 所有颜色、圆角、阴影值必须来自 `tailwind.config.ts` 中扩展的 Token，不使用魔法数字。
5. 第一个页面必须是产品核心界面（如首页），而不是"欢迎"或"加载"页。
6. 不使用外部 CDN，所有依赖通过 npm 安装。
- 禁止出现未替换的模板占位符，例如 `{{PRODUCT_NAME}}`。
- 禁止使用字母、数字、emoji 或几何色块代替图标。
- 禁止在根页面内联图标定义、组件定义或页面本体来绕过资产目录。
