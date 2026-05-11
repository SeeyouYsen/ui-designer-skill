# Artifact Structure — 设计资产目录结构规范

最终交付物是一个可直接运行的 React + Tailwind CSS + React Router 项目，执行 `npm install && npm run dev` 即可在浏览器中预览。

浏览器打开后呈现**两个顶层入口**，通过 `App.tsx` 的顶栏切换：
- **资源**：展示 Design System tokens、图标、组件，内含 3 个子 tab
- **设计图**：完整的设计页面，通过 React Router 跳转，页面本体无任何外层装饰

---

## 一、标准目录结构

```text
/tmp/ui-design-{产品名}/
├── package.json
├── vite.config.ts
├── tailwind.config.ts
├── tsconfig.json
├── index.html                    ← Vite 入口 HTML，仅挂载 #root
└── src/
    ├── main.tsx                  ← ReactDOM.createRoot + BrowserRouter
    ├── App.tsx                   ← 顶层两入口切换（资源 / 设计图）
    ├── index.css                 ← @tailwind 指令 + CSS 变量（Design Tokens）
    ├── components/
    │   ├── icons/
    │   │   ├── home.svg
    │   │   └── ...               ← 每个图标一个纯 SVG 文件
    │   └── ui/
    │       ├── Button.tsx
    │       └── ...               ← 每个 UI 组件一个 TSX 文件
    ├── pages/
    │   ├── HomePage.tsx
    │   └── ...                   ← 每个设计页面一个 TSX 文件
    └── viewer/
        ├── AssetsViewer.tsx      ← 资源入口：3 个子 tab 的容器
        ├── DesignSystemTab.tsx   ← 展示所有 Design Token（色彩、字体、间距、圆角）
        ├── IconsTab.tsx          ← 展示所有图标组件
        └── ComponentsTab.tsx     ← 展示所有 UI 组件及变体
```

---

## 二、各文件职责

| 路径 | 职责 |
|------|------|
| `package.json` | 项目依赖：react、react-dom、react-router-dom、tailwindcss、@tailwindcss/vite、typescript、vite |
| `vite.config.ts` | Vite 配置，使用 `@vitejs/plugin-react` 和 `@tailwindcss/vite` |
| `tailwind.config.ts` | Tailwind 配置，`content` 指向 `./src/**/*.{tsx,ts}`，`theme.extend` 中用 `var()` 引用 Design Token CSS 变量 |
| `tsconfig.json` | TypeScript 配置，target ES2020，moduleResolution bundler |
| `index.html` | Vite 入口，只含 `<div id="root">` 和 `<script type="module" src="/src/main.tsx">` |
| `src/main.tsx` | `createRoot(document.getElementById('root')).render(<BrowserRouter><App/></BrowserRouter>)` |
| `src/App.tsx` | 顶层两入口切换：`activeEntry === 'assets'` 渲染 `<AssetsViewer>`，否则渲染 `<Routes>`（设计页面路由） |
| `src/index.css` | `@tailwind base/components/utilities`，`:root { }` 中定义所有 Design Token CSS 变量 |
| `src/components/icons/*.svg` | 每个图标一个纯 SVG 文件，通过 `vite-plugin-svgr` 作为 React 组件导入 |
| `src/components/ui/*.tsx` | UI 组件，使用 Tailwind 类名，通过 props 控制变体 |
| `src/pages/*.tsx` | 设计页面组件，使用 `<Link>` 或 `useNavigate` 做页面跳转，用 `useState` 控制弹窗；**不感知任何外层结构** |
| `src/viewer/AssetsViewer.tsx` | 资源入口容器，内含 Design System / Icons / Components 三个子 tab |
| `src/viewer/DesignSystemTab.tsx` | 展示所有 CSS Token 变量（色彩色板、字体阶梯、间距、圆角、阴影、渐变） |
| `src/viewer/IconsTab.tsx` | 展示所有 `src/components/icons/` 图标，显示名称和预览 |
| `src/viewer/ComponentsTab.tsx` | 展示所有 `src/components/ui/` 组件及其主要变体 |

---

## 三、命名规范

### 图标文件

- 格式：kebab-case `.svg` 文件
- 示例：`home.svg`、`heart-rate.svg`、`blood-pressure.svg`
- 引用时加 `?react` 后缀：`import HomeIcon from '../components/icons/home.svg?react'`

### UI 组件

- 格式：`{ComponentName}.tsx`，使用 PascalCase
- 示例：`Button.tsx`、`Card.tsx`、`TabBar.tsx`、`Modal.tsx`
- 变体通过 props 控制，例如 `variant="primary" | "secondary" | "ghost"`

### 页面组件

- 格式：`{PageName}Page.tsx`，使用 PascalCase
- 示例：`HomePage.tsx`、`DevicesPage.tsx`、`ReportDetailPage.tsx`
- 路由路径使用 kebab-case，例如 `/home`、`/devices`、`/report-detail`

### 样式

- Design Token 全部写在 `src/index.css` 的 `:root { }` 中，格式为 `--color-primary: #...`
- Tailwind 配置中通过 `theme.extend` 用 `var(--color-primary)` 引用，使 Tailwind 类名可直接使用 Token
- 禁止在组件中写内联 `style` 属性，所有视觉值必须来自 Tailwind 类名或 Token 变量

---

## 四、配置文件模板

### package.json
```json
{
  "name": "ui-design-{产品名}",
  "private": true,
  "version": "0.0.1",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build"
  },
  "dependencies": {
    "react": "^19.0.0",
    "react-dom": "^19.0.0",
    "react-router-dom": "^7.0.0"
  },
  "devDependencies": {
    "@tailwindcss/vite": "^4.0.0",
    "@types/react": "^19.0.0",
    "@types/react-dom": "^19.0.0",
    "tailwindcss": "^4.0.0",
    "typescript": "^5.0.0",
    "vite": "^6.0.0",
    "@vitejs/plugin-react": "^4.0.0",
    "vite-plugin-svgr": "^4.0.0"
  }
}
```

### vite.config.ts
```ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import tailwindcss from '@tailwindcss/vite'
import svgr from 'vite-plugin-svgr'

export default defineConfig({
  plugins: [react(), tailwindcss(), svgr()],
})
```

### tailwind.config.ts
```ts
import type { Config } from 'tailwindcss'

export default {
  content: ['./index.html', './src/**/*.{ts,tsx}'],
  theme: {
    extend: {
      colors: {
        primary: 'var(--color-primary)',
        'primary-light': 'var(--color-primary-light)',
        surface: 'var(--color-surface)',
        'text-primary': 'var(--color-text-primary)',
        'text-secondary': 'var(--color-text-secondary)',
      },
      borderRadius: {
        sm: 'var(--radius-sm)',
        md: 'var(--radius-md)',
        lg: 'var(--radius-lg)',
        xl: 'var(--radius-xl)',
      },
    },
  },
} satisfies Config
```

### src/index.css（Design Tokens 写在这里）
```css
@tailwind base;
@tailwind components;
@tailwind utilities;

:root {
  --color-primary: #...;
  --color-primary-light: #...;
  --color-surface: #...;
  --color-text-primary: #...;
  --color-text-secondary: #...;
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;
  --radius-xl: 20px;
}
```

### src/main.tsx
```tsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import { BrowserRouter } from 'react-router-dom'
import './index.css'
import App from './App'

createRoot(document.getElementById('root')!).render(
  <StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </StrictMode>
)
```

### src/App.tsx（顶层两入口切换）
```tsx
import { useState } from 'react'
import { Routes, Route, Navigate } from 'react-router-dom'
import AssetsViewer from './viewer/AssetsViewer'
import HomePage from './pages/HomePage'
// import 其他页面...

export default function App() {
  const [entry, setEntry] = useState<'assets' | 'design'>('design')

  return (
    <div className="min-h-screen bg-white">
      {/* 顶部两入口切换栏 */}
      <div className="flex border-b border-gray-200">
        <button
          onClick={() => setEntry('design')}
          className={entry === 'design' ? 'px-6 py-3 border-b-2 border-primary font-medium' : 'px-6 py-3 text-gray-500'}
        >
          设计图
        </button>
        <button
          onClick={() => setEntry('assets')}
          className={entry === 'assets' ? 'px-6 py-3 border-b-2 border-primary font-medium' : 'px-6 py-3 text-gray-500'}
        >
          资源
        </button>
      </div>

      {/* 入口内容区 */}
      {entry === 'assets' ? (
        <AssetsViewer />
      ) : (
        <Routes>
          <Route path="/" element={<Navigate to="/home" replace />} />
          <Route path="/home" element={<HomePage />} />
          {/* 其他设计页面路由... */}
        </Routes>
      )}
    </div>
  )
}
```

---

## 五、交互规范

### 页面跳转
```tsx
import { Link, useNavigate } from 'react-router-dom'

// 声明式跳转
<Link to="/devices">设备</Link>

// 命令式跳转
const navigate = useNavigate()
navigate('/report-detail')
```

### 弹窗（Modal / Sheet / Dialog）
```tsx
const [open, setOpen] = useState(false)

<button onClick={() => setOpen(true)}>打开弹窗</button>
{open && <Modal onClose={() => setOpen(false)} />}
```

弹窗组件必须放在 `src/components/ui/Modal.tsx`（或对应名称），不得在页面文件中内联弹窗结构。

---

## 六、输出原则

1. 每个页面是一个独立 TSX 文件，在 `App.tsx` 的路由表中注册。
2. 每个图标是一个独立 TSX 组件，从 `src/components/icons/` 导入使用。
3. UI 组件放在 `src/components/ui/`，通过 props 控制变体，不在页面中临时写样式。
4. 所有颜色、字号、间距、圆角值必须来自 CSS Token 变量，通过 Tailwind 类名使用，禁止魔法数字。
5. 弹窗必须是独立组件，状态由父页面的 `useState` 控制。

## 七、范围控制

- 单页设计：`pages/` 只有一个页面文件，`App.tsx` 路由只有一条。
- 简单需求：`components/ui/` 只生成页面实际用到的组件。
- 图标：`components/icons/` 只生成页面和组件实际引用的图标。
- 配置文件（package.json、vite.config.ts 等）必须完整生成，不可省略，因为用户需要能直接 `npm run dev`。
