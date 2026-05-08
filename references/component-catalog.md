# Component Catalog — 组件库生成规范

AI 生成 `components/` 目录与 `components.html` 时必须根据产品和页面真实需要选择组件范围。组件视觉必须来自 Design Tokens，不得为单个组件临时发明颜色、字号、间距、圆角或阴影。

---

## 一、组件生成原则

1. 组件库不是页面截图，而是可复用的 UI 零件集合。
2. 每个被选中的组件必须展示默认态和至少一种关键状态。
3. 组件名称必须能映射到 [blueprint-schema.md](./blueprint-schema.md) 中的 `type` 或 `control.variant`。
4. 页面原型只能从组件库取组件拼装，不得在 Preview 中创建组件库不存在的新样式。
5. 若用户需求出现组件目录未覆盖的场景，优先用 `control + variant` 扩展，并在 `components/` 与 `components.html` 中补充该 variant 的展示。
6. 组件必须支持注意力权重变化，遵守 [visual-hierarchy.md](./visual-hierarchy.md)：同一类组件至少区分高权重、常规权重、低权重或禁用状态，不能全部像主操作或重点卡片。
7. 不要为了“完整”生成与当前产品无关的组件。未被页面、状态或设计系统说明使用的组件应跳过。

---

## 二、组件范围决策

生成组件前先回答：

1. 当前页面和后续页面真实会用到哪些交互和展示模式？
2. 哪些组件是产品核心体验的一部分，哪些只是可选辅助？
3. 哪些状态必须被用户理解，例如选中、禁用、加载、错误、空状态？
4. 是否存在领域特有组件，例如日程项、设备状态、指标趋势、订单行、编辑工具栏？
5. 哪些组件如果生成出来也不会被使用，应避免加入目录。

组件范围分三层：

- 基础组件：文字、按钮、图标、卡片、列表、输入或导航中实际会用到的部分。
- 核心业务组件：直接承载产品任务的组件，例如日程项、指标卡、设备行、流程节点、内容卡。
- 支撑组件：空状态、加载态、标签、徽标、筛选、分页、图表等，仅在页面需要时生成。

下面清单是常见组件参考，不是全量必生成清单。AI 应先根据实际页面和交互需求列出会用到的组件类型，再从下表参考对应变体；不得为了表格完整性逐项生成无关组件。

### 文字与媒体

| 组件 | 对应蓝图 | 常见状态/变体 |
|------|---------|--------------|
| Typography | `text` | 页面标题、区块标题、卡片标题、正文、说明文字、标签文字 |
| Icon | `icon` | 默认、激活、禁用 |
| Image | `image` | 缩略图、横幅图、空占位 |
| Avatar | `control.variant=avatar` | 默认、带状态点 |

### 图标规则

- Icon 组件必须从 `icon/` 目录中的独立 SVG 图标文件引用，具体生成规则见 [icon-library.md](./icon-library.md)。
- `components.html` 只展示图标在组件中的使用方式，不承担图标总览职责。
- 业务缩写只能作为文字标签出现，不能作为图标本体。例如 `BP` 可以作为指标标签，不能放在图标容器里替代血压图标。
- Preview 中出现的每个图标都必须能在 `icon-library.html` 中找到。

图标引用路径以 [artifact-structure.md](./artifact-structure.md) 为准，组件文件不得复制图标定义。

### 操作与输入

| 组件 | 对应蓝图 | 常见状态/变体 |
|------|---------|--------------|
| Button | `button` | primary、secondary、ghost、destructive；default、pressed、disabled、loading |
| Icon Button | `button + icon role` | default、active、disabled |
| Input | `input` | default、focus、filled、error、disabled |
| Search Input | `input` | empty、filled、focused |

### 容器与列表

| 组件 | 对应蓝图 | 常见状态/变体 |
|------|---------|--------------|
| Card | `card` | basic、data、media、action、selected、muted、highlight |
| Data Card | `card` + 数据展示 role | 主指标、次指标、指标+趋势、指标+状态标签 |
| Chart Card | `card` + 图表 role | line、bar、ring；含标题、图例、数值摘要；主图表/辅助图表 |
| List Item | `list > card` 或 `row` | one-line、two-line、leading icon、trailing action、selected、muted |
| Section Header | `text + row` | 普通标题、带操作入口 |
| Empty State | `card + image/text/button` | 无数据、无搜索结果 |
| Loading Skeleton | `card` 或 `list` | 卡片骨架、列表骨架 |

### 导航与页面结构

| 组件 | 对应蓝图 | 常见状态/变体 |
|------|---------|--------------|
| App Bar | `appbar` | 标题型、标题+操作、logo+导航（Web） |
| Bottom Bar | `bottombar` | 3 项、4 项、5 项；active、inactive |
| Tab Bar | `tabbar` | equal width、scrollable；active、inactive |
| Page Switcher | Preview 内部控件 | 多页面切换 |

### 控件

| 组件 | 对应蓝图 | 常见状态/变体 |
|------|---------|--------------|
| Switch | `control.variant=switch` | on、off、disabled |
| Slider | `control.variant=slider` | normal、disabled、带数值 |
| Progress | `control.variant=progress` | linear、ring、不同进度 |
| Checkbox | `control.variant=checkbox` | checked、unchecked、disabled |
| Radio | `control.variant=radio` | selected、unselected、disabled |
| Stepper | `control.variant=stepper` | normal、min、max |
| Chip | `control.variant=chip` | default、selected、disabled |
| Badge | `control.variant=badge` | number、dot、status |
| Tag | `control.variant=tag` | neutral、success、warning、error、info |
| Divider | `control.variant=divider` | horizontal、with label |

---

## 三、组件展示规则

- `components.html` 必须按实际组件类别分组，例如基础、业务、导航、反馈、数据展示或控件。
- 每个组件展示区必须包含组件名称、用途说明、状态示例。
- 状态示例必须并排展示，方便用户比较。
- 组件库应覆盖本次设计需要复用和解释的组件；不要生成不会被页面或设计系统使用的组件。
- Preview 中使用的所有视觉样式，都必须能在 `components/` 或 `components.html` 找到来源。
- 若组件使用渐变背景，必须说明渐变服务的语义，并在 Design Tokens 中有对应渐变 token；按钮渐变必须通过可读性和可点击性检查。
- 按钮渐变自检：端点颜色必须来自同一色盘逻辑；中段不得发灰、发脏或像污渍；文字和图标在渐变最亮、最暗和中间区域都必须可读；hover、pressed、disabled 状态仍需清楚区分。
- 图表、指标、趋势等数据展示若出现在页面中，不得临时自造样式，必须作为业务组件或数据展示组件在 `components/` 与 `components.html` 中展示。
- 组件展示必须体现注意力权重差异：高权重卡片、常规卡片、低权重列表项、主按钮、次按钮、标签和元信息不能拥有同等视觉强度。
- Preview 不得呈现明显“占位稿”气质：避免随机字母图标、数字图标、emoji 图标、无意义灰块、未命名图片框、重复假内容。
