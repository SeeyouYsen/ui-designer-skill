# Blueprint Schema — 页面蓝图 JSON 规范

AI 生成 Page Blueprint JSON 时必须严格遵守本规范。

---

## 顶层结构

```json
{
  "page": "页面名称（必填）",
  "platform": "mobile | web（默认 mobile）",
  "appbar": { ... },
  "body": { ... },
  "bottombar": { ... },
  "tabbar": { ... },
  "tabpages": [ { ... }, { ... } ]
}
```

- `appbar`、`bottombar`、`tabbar`、`tabpages` 为可选的页面级固定区域
- `body` 为必填的主内容区域；若使用 `tabbar`，则 `body` 替换为 `tabpages` 数组，每项对应一个 tab 的内容
- `tabbar` 和 `tabpages` 必须同时出现，不可单独使用

### 平台差异

| 字段 | mobile | web |
|------|--------|-----|
| `bottombar` | 常用 | 不使用 |
| `appbar` | 顶部导航 | 顶部横向导航栏，children 可包含 logo、nav-item、button |
| 容器宽度 | 390px | 1280px，内容区 960px 居中 |

---

## Type 枚举（共 13 个，只能从此列表选择）

### 布局容器（必须有 `children`）

| type | 说明 |
|------|------|
| `row` | 横向排列子元素 |
| `column` | 纵向排列子元素 |
| `stack` | 层叠子元素（z 轴方向） |
| `list` | 可纵向滚动的列表容器 |

### 叶子元素（不得有 `children`）

| type | 说明 |
|------|------|
| `text` | 文字 |
| `image` | 图片 |
| `icon` | 图标（小尺寸，语义化） |
| `button` | 按钮 |
| `input` | 输入框 |
| `control` | 交互控件，需配合 `variant` 字段 |

### 卡片（可以有 `children`，也可以没有）

| type | 说明 |
|------|------|
| `card` | 卡片容器，内部可嵌套任意元素 |

### 页面级固定区域（只出现在顶层，不嵌套在 body 中）

| type | 说明 |
|------|------|
| `appbar` | 顶部导航栏 |
| `bottombar` | 底部导航栏 |
| `tabbar` | 标签页导航条 |
| `tabpage` | tabbar 对应的页面内容容器 |

---

## control 的 variant 枚举

`type: "control"` 时必须指定 `variant`，只能从以下值选择：

```
switch      开关
slider      滑块
progress    进度条
checkbox    复选框
radio       单选
stepper     数量增减器
chip        标签片
badge       角标
avatar      头像
divider     分割线
tag         状态标签
```

---

## 字段定义

| 字段 | 类型 | 必填条件 | 说明 |
|------|------|---------|------|
| `type` | string | 必填 | 从 type 枚举中选择 |
| `role` | string | 必填 | 自然语言描述这个元素的作用 |
| `children` | array | 布局容器必填 | 子元素数组 |
| `variant` | string | type=control 时必填 | 从 control variant 枚举选择 |
| `align` | string | 可选 | `left / center / right`，同行多元素时使用 |
| `repeat` | number | 可选 | 重复渲染 N 次，用于列表项 |
| `scroll` | string | 可选 | `horizontal / vertical`，容器可滚动时使用 |
| `value` | string | 可选 | 固定文本内容，如标题文字 |

### 数据展示与图表表达

为了保持 type 枚举稳定，图表和数据展示不新增 `chart` 类型。需要图表时使用：

```json
{ "type": "card", "role": "七日心率折线图" }
{ "type": "card", "role": "设备在线率环形图" }
{ "type": "card", "role": "本周检测次数柱状图" }
```

AI 在渲染时必须将这些 `card` 映射为组件资产中的 Data Card 或 Chart Card。

---

## 约束规则

1. `type` 必须从枚举中选择，禁止自造类型
2. 布局容器（row / column / stack / list）必须有 `children` 字段
3. 叶子元素（text / image / icon / button / input / control）不得有 `children` 字段
4. `card` 的 `children` 可选，有内容时嵌套布局元素
5. `control` 必须有 `variant` 字段
6. `appbar` / `bottombar` / `tabbar` 每种最多出现一次，且只在顶层
7. 使用 `tabbar` 时，顶层必须有 `tabpages` 数组替代 `body`，每个 tabpage 对应一个 tab 项
8. Web 平台不得使用 `bottombar`
9. `role` 字段必须填写，不可为空，用于指导 AI 渲染内容

### 扩展讨论原则

默认保持 type 枚举稳定，避免页面蓝图变成任意 HTML 描述。若某个设计确实无法用现有 type + role + variant 表达，AI 不得直接自造 type，应先判断：

1. 是否可以用 `card`、`control.variant` 或更具体的 `role` 表达。
2. 是否只是视觉样式问题，应该交给组件资产处理，而不是新增蓝图类型。
3. 是否是当前产品的核心业务组件，值得在组件库中沉淀。
4. 若仍必须扩展，应在最终回复中说明新增类型的必要性和替代方案。

---

## 完整示例

```json
{
  "page": "健康首页",
  "platform": "mobile",
  "appbar": {
    "type": "appbar",
    "children": [
      { "type": "text",  "role": "页面标题",    "align": "left",  "value": "健康数据" },
      { "type": "image", "role": "用户头像",     "align": "right" }
    ]
  },
  "body": {
    "type": "column",
    "children": [
      {
        "type": "row",
        "scroll": "horizontal",
        "children": [
          { "type": "card", "role": "心率数据展示卡片" },
          { "type": "card", "role": "血压数据展示卡片" },
          { "type": "card", "role": "血氧数据展示卡片" }
        ]
      },
      {
        "type": "text",
        "role": "区块标题",
        "value": "近期记录"
      },
      {
        "type": "list",
        "repeat": 3,
        "children": [
          {
            "type": "card",
            "role": "历史检测记录条目",
            "children": [
              { "type": "icon",  "role": "记录类型图标" },
              {
                "type": "column",
                "children": [
                  { "type": "text", "role": "记录名称" },
                  { "type": "text", "role": "记录时间" }
                ]
              },
              { "type": "text", "role": "数值结果", "align": "right" }
            ]
          }
        ]
      }
    ]
  },
  "bottombar": {
    "type": "bottombar",
    "children": [
      { "type": "icon", "role": "首页导航入口" },
      { "type": "icon", "role": "设备导航入口" },
      { "type": "icon", "role": "我的导航入口" }
    ]
  }
}
```

---

## tabbar 用法示例

```json
{
  "page": "数据看板",
  "platform": "mobile",
  "appbar": {
    "type": "appbar",
    "children": [
      { "type": "text", "role": "页面标题", "value": "数据看板" }
    ]
  },
  "tabbar": {
    "type": "tabbar",
    "children": [
      { "type": "text", "role": "日报 tab", "value": "日报" },
      { "type": "text", "role": "周报 tab", "value": "周报" },
      { "type": "text", "role": "月报 tab", "value": "月报" }
    ]
  },
  "tabpages": [
    {
      "tab": "日报",
      "type": "column",
      "children": [
        { "type": "card", "role": "今日数据汇总" },
        { "type": "card", "role": "折线趋势图" }
      ]
    },
    {
      "tab": "周报",
      "type": "column",
      "children": [
        { "type": "card", "role": "本周数据汇总" },
        { "type": "card", "role": "柱状对比图" }
      ]
    },
    {
      "tab": "月报",
      "type": "column",
      "children": [
        { "type": "card", "role": "本月数据汇总" }
      ]
    }
  ]
}
```
