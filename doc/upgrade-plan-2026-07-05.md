# celadon-color-engine 升级方案

**制定日期**: 2026-07-05
**对比基准**: [tmp/diff-analysis.md](../tmp/diff-analysis.md)

---

## 一、升级总览

| 优先级 | 文件 | 动作 | 影响范围 |
|--------|------|------|----------|
| **P0** | `tokens/colors.css` | 新增 11 个 Flyout 语义令牌 | 全局令牌层 |
| **P0** | `themes/day-night.css` | 新增 3 个 Flyout 夜间重映射 | 主题层 |
| **P1** | `base/base.css` | 移除 Tailwind 注入（恢复纯粹性） | 全局基线层 |
| **P1** | `components/dropdown.css` | 全部改用 Flyout 语义令牌，移除硬编码夜间覆盖 | 下拉菜单组件 |
| **P2** | `components/form.css` | 新增 4 个标准控件宽度修饰类 | 表单组件 |
| -- | `utilities/timeline-layouts.css` | **不升级**（A 有夜间主题适配，优于 B） | -- |

---

## 二、逐项详解

### P0-1: `tokens/colors.css` -- 新增 Flyout 语义令牌

**当前问题**：A 缺少浮层（flyout）类组件的独立语义令牌体系，dropdown 等组件直接硬编码 `--c-white`、`--c-gray-50` 等物理色值和 `--shadow-4` 等具体阴影值。

**新增内容**（共 11 个令牌，插入在 `:root` 的"背景扩展"与"链接"之间）：

| 令牌名 | 默认值 | 语义 |
|--------|--------|------|
| `--c-bg-flyout` | `var(--c-bg-elev)` | 浮层背景 |
| `--c-border-flyout` | `var(--c-border-light)` | 浮层边框 |
| `--c-shadow-flyout` | `var(--shadow-3)` | 浮层阴影 |
| `--c-divider-flyout` | `var(--c-divider)` | 浮层内分隔线 |
| `--c-text-flyout-item` | `var(--c-text-body)` | 浮层菜单项文字 |
| `--c-text-flyout-item-strong` | `var(--c-text-title)` | 浮层菜单项强调文字 |
| `--c-bg-flyout-item-hover` | `var(--c-bg-card)` | 浮层菜单项 hover 背景 |
| `--c-bg-flyout-item-active` | `var(--c-bg-selected)` | 浮层菜单项 active 背景 |
| `--c-bg-flyout-item-selected` | `var(--c-bg-selected)` | 浮层菜单项选中背景 |
| `--c-text-flyout-danger` | `var(--c-text-error)` | 浮层危险操作文字 |
| `--c-bg-flyout-danger-hover` | `var(--c-bg-error)` | 浮层危险操作 hover 背景 |

**附加**：B 同时将 `--c-bg-selected` 的默认值从 `var(--c-gray-100)` 改为 `var(--c-gray-200)`（选中态对比度更高）。可酌情决定是否同步修改。

**升级收益**：
- 浮层组件（dropdown、popover、context-menu）统一使用语义令牌
- 日/夜间主题自动适配，无需在组件内写 `.theme-night` 覆盖

---

### P0-2: `themes/day-night.css` -- 新增 Flyout 夜间重映射

**当前问题**：A 缺少 Flyout 相关令牌的夜间重映射。

**新增内容**（在 `.theme-night` 的"背景/边框/文字"块中新增 3 行）：

```css
--c-bg-flyout: var(--c-gray-800);
--c-border-flyout: var(--c-gray-600);
```

**升级收益**：夜间模式下浮层自动呈现深灰背景 + 深色边框，与 P0-1 配合使用。

---

### P1-1: `base/base.css` -- 移除 Tailwind 注入

**当前问题**：A 存在以下两行，引入了对 Tailwind CSS 的强依赖：

```css
/* Inject Tailwind preflight in this module to satisfy Tailwind plugin */
@tailwind base;
```

celadon 作为独立的纯 CSS 引擎，此依赖破坏了系统的纯粹性和可移植性。B 正常运行且无此行，说明它并非必要。

**操作**：删除上述两行，剩余内容与 B 完全一致。

**升级收益**：celadon 回归纯粹，仅靠 `@layer base` 完成全局基线设置，无外部框架依赖。

---

### P1-2: `components/dropdown.css` -- 切换为 Flyout 语义令牌

**当前问题**：A 的 dropdown 硬编码了大量物理色值和主题覆盖，代码量大且不利于维护。

**改动对照**（A 2751B -> B 2315B，瘦身 436B）：

| 属性 | A（当前） | B（目标） |
|------|-----------|-----------|
| 背景 | `var(--c-white)` | `var(--c-bg-flyout)` |
| 边框 | `var(--c-border-medium)` | `var(--c-border-flyout)` |
| 阴影 | `var(--shadow-4)` | `var(--c-shadow-flyout)` |
| z-index | `50` | `var(--z-dropdown)` |
| 菜单项文字 | `var(--c-text-body)` | `var(--c-text-flyout-item)` |
| hover 背景 | `var(--c-gray-50)` | `var(--c-bg-flyout-item-hover)` |
| hover 文字 | `var(--c-text-title)` | `var(--c-text-flyout-item-strong)` |
| active 背景 | `var(--c-gray-100)` | `var(--c-bg-flyout-item-active)` |
| active 文字 | `var(--c-text-title)` | `var(--c-text-flyout-item-strong)` |
| 选中背景 | `var(--c-gray-100)` | `var(--c-bg-flyout-item-selected)` |
| 选中文字 | `var(--c-text-title)` | `var(--c-text-flyout-item-strong)` |
| `.theme-night` 整块 | **存在**（4 条覆盖规则） | **删除**（令牌层自动处理） |

**升级收益**：
- 代码量减少约 16%，取消 4 条夜间覆盖规则
- 跟随令牌层自动适配日/夜主题
- z-index 统一由 `tokens/z-index.css` 管理

---

### P2: `components/form.css` -- 新增宽度修饰类

**当前问题**：A 缺少标准控件的宽度控制能力，短字段场景缺乏合适的宽度预设。

**新增内容**（在标准控件打包规则之后新增约 25 行）：

| 修饰类 | 效果 |
|--------|------|
| `.celadon-control--inline` | `width: auto` |
| `.celadon-control--w-xs` | `width: 80px; min-width: 80px` |
| `.celadon-control--w-sm` | `width: 104px; min-width: 104px` |
| `.celadon-control--w-md` | `width: 120px; min-width: 120px` |

适用选择器：`input.celadon-input.celadon-control--standard`、`select`、`textarea`。

**升级收益**：短字段无需写额外 CSS，直接组合使用。

---

## 三、不上合的内容（A 当前已优于 B）

| 文件 | A 优势 | 说明 |
|------|--------|------|
| `utilities/timeline-layouts.css` | 夜间主题色适配 + 更大间距 | A 的 `.spacious` 用 `--space-20`（B 用 `--space-16`），tutorial 同理；A 有 `.theme-night` 下 timeline-item 标题/副标题/描述的夜间适配，B 没有 |
| `.gitignore` | -- | 项目工程文件，保留 |
| `LICENSE` | -- | 开源许可，保留 |
| `README.md` | -- | 项目文档，保留 |

---

## 四、可选的 B 独有文件

| 文件 | 说明 | 建议 |
|------|------|------|
| `token-list.md` | CSS 令牌完整清单文档（v1.1.0），约 425 行 | 如果项目团队有 AI 辅助开发需求，可同步此文件 |

---

## 五、建议的执行顺序

```
  1. tokens/colors.css       ← 新增 11 个 Flyout 令牌 (P0-1)
  2. themes/day-night.css    ← 新增 3 个 Flyout 夜间重映射 (P0-2)
  ─── 令牌层就绪 ───
  3. base/base.css           ← 移除 Tailwind 注入 (P1-1)  [可与 4 并行]
  4. components/dropdown.css ← 切换为 Flyout 令牌 (P1-2)  [依赖 1+2]
  5. components/form.css     ← 新增宽度修饰类 (P2)
  ─── 组件层就绪 ───
  6. (可选) 同步 token-list.md
```

**关键依赖**：步骤 4 依赖步骤 1 + 2 的令牌就绪，否则 `--c-bg-flyout` 等变量未定义会导致样式崩塌。务必先完成 P0 再执行 P1-2。
