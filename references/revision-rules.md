# Revision Rules — 修改模式规范

修改模式的核心约束：**从变更点追溯影响链，只动影响链上的文件，不在影响链以外新增任何资产。**

---

## 一、零新增原则

除非用户明确要求，修改时**禁止**：

- 新增 `src/components/ui/` 组件
- 新增 `src/components/icons/` 图标
- 新增 CSS Token / CSS 变量
- 新增页面路由

所有修改必须在现有资产范围内完成。遇到"现有组件不够用"时，**优先组合现有组件**，而不是造新的。

---

## 二、影响链追溯

收到修改请求后，按以下顺序判断影响范围：

1. **变更直接落在哪个文件？**（Token、组件、图标、页面、路由）
2. **是否依赖共享 Token？** → 若改了 Token，追踪所有引用该变量的组件和页面
3. **是否改变了组件 API（props、变体名）？** → 追踪所有使用该组件的页面
4. **是否新增/删除了文件？** → 新增页面需更新路由；新增/删除图标或组件需更新对应 Viewer Tab

不在影响链上的文件**一律不动**。

---

## 三、允许新增的场景与约束

用户明确要求新增时，**必须遵守对应原始规范**，不得走捷径：

| 新增类型 | 必须遵守的规范 | 同步更新 |
|----------|---------------|----------|
| 新增组件 | [component-catalog.md](./component-catalog.md)：独立 TSX、props 变体、使用现有 Token 和图标 | `src/viewer/ComponentsTab.tsx` 加入展示 |
| 新增图标 | [icon-library.md](./icon-library.md)：纯 SVG、kebab-case、currentColor、无 width/height | `src/viewer/IconsTab.tsx` 加入展示 |
| 新增 Token | [design-principles.md](./design-principles.md)：来自色彩学关系，非机械色阶 | `src/index.css`、`tailwind.config.ts`、`src/viewer/DesignSystemTab.tsx` 同步更新 |
| 新增页面 | [blueprint-schema.md](./blueprint-schema.md) + [final-output-spec.md](./final-output-spec.md)：先写蓝图再写页面 | `src/App.tsx` 路由表加入新路由 |

---

## 四、禁止行为

修改时无论什么情况都不得出现以下内容：

- 在页面或组件中临时内联 SVG 路径（应从 `src/components/icons/` 导入）
- `style=` 内联样式（应使用 Tailwind 类名）
- 硬编码颜色值（应使用 Token 类名）
- 在页面内直接写弹窗 JSX 结构（弹窗必须是独立组件）
- 在修改页面时顺手新建未被要求的组件

---

## 五、完成后说明

修改完成后，必须告知用户：

1. 改了哪些文件（列出文件路径）
2. 是否有连带更新（Token 传播、Viewer Tab 同步等）
3. 是否有未满足的需求（因零新增原则跳过了什么，需用户确认是否允许新增）
