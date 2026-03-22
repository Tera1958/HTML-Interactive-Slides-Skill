# Frontend Slides

一个用于创建精美、动效丰富的 HTML 演示文稿的 AI 技能 —— 支持从零构建，或将 PowerPoint 文件转为 Web 幻灯片。

## 功能概述

**Frontend Slides** 帮助非设计师无需掌握 CSS 或 JavaScript，就能制作出高质量的 Web 演示文稿。采用"展示而非描述"的方式：AI 生成可视化风格预览，让你直接挑选喜欢的样式，而非用文字描述偏好。

### 核心特性

- **零依赖** — 单个 HTML 文件，内联所有 CSS/JS，无需 npm、构建工具或框架。
- **视觉风格发现** — 无法用语言表达设计偏好？没关系。从生成的视觉预览中直接挑选。
- **PPT 转换** — 将现有 PowerPoint 文件转为 Web 幻灯片，保留所有图片和内容。
- **反 AI 模板化** — 精选独特风格，避免泛滥的 AI 美学（告别白底紫色渐变）。
- **视口适配（强制要求）** — 每张幻灯片必须完整呈现在 100vh 内，绝不出现内部滚动条。
- **浏览器内联编辑**（可选）— 生成后可在浏览器中直接修改文字，自动保存至 localStorage，支持导出文件。
- **交互式展示模式（Mode D）** — 支持带标签云、卡片点击弹出面板的单页交互式演示。
- **效果库复用** — AI 会主动检索已有效果（`EFFECT_LIBRARY.md`），优先复用，避免重复造轮子。

---

## 工作流程

### Phase 0：检测模式

| 模式 | 触发条件 |
|------|---------|
| **Mode A** 全新演示 | 从零创建 |
| **Mode B** PPT 转换 | 转换 .pptx 文件 |
| **Mode C** 增强改进 | 优化现有 HTML 演示 |
| **Mode D** 交互式展示 | 单页多面板点击交互 |

---

### Phase 1：内容收集（Mode A）

一次性询问 4 个问题：
1. **用途** — Pitch deck / 教学 / 会议演讲 / 内部汇报
2. **长度** — 5-10 / 10-20 / 20+ 页
3. **内容状态** — 内容完整 / 粗略笔记 / 仅有主题
4. **内联编辑** — 是否需要在浏览器中直接编辑文字

若用户提供图片，AI 会逐一查看并评估，将可用图片纳入幻灯片结构设计（不是"先定结构再填图"，而是图文协同设计）。

---

### Phase 2：风格发现

1. 用户选择偏好情绪（Impressed / Excited / Calm / Inspired）
2. AI 生成 3 个独立风格预览（style-a/b/c.html），在浏览器中自动打开
3. 用户从预览中选择或混搭风格
4. **文件命名** — 询问用户为演示文稿取名（如 `my-pitch`），自动用于后续文件名

---

### Phase 3：生成演示文稿

AI 读取以下支持文件后生成：
- `viewport-base.css` — 强制响应式 CSS（完整内嵌）
- `html-template.md` — HTML 架构与 JS 功能
- `animation-patterns.md` — 动画参考
- `EFFECT_LIBRARY.md` — 已有效果库（复用优先）

生成结果为单一自包含 HTML 文件，所有 CSS/JS 内联，使用 Fontshare/Google Fonts，带详细注释。

**Slide 编号规则：**
- 每页自动分配结构化编号（`01`、`02`、...），HTML 注释格式：`<!-- Slide 01: 标题 -->`
- 多文件模式下文件名格式：`[name]-01-[keyword].html`
- 关键词取自标题，最多 3 个单词，小写短横线连接（如 `my-pitch-03-solution.html`）
- 目录页为 `[name]-toc.html`

**布局密度原则：**
- **填满页面，避免大片留白** — 目标 ≥ 85% 视觉覆盖率
- 内容元素不孤立，使用多列网格/卡片网格/分层组合填满空间
- 边距尽量小（侧边 `clamp(1.5rem, 3vw, 3rem)`，上下 `clamp(1rem, 2vh, 2rem)`）
- 垂直方向使用 `flex: 1` 撑满剩余高度
- 背景使用网格/点阵/渐变等装饰消除空白感
- 元素间距紧凑（`gap: 0.5rem–1rem`），避免松散布局
- 禁止单文本块居中、大面积留白、固定宽度不撑满等反模式

---

### Phase 4：PPT 转换

1. 运行 `python scripts/extract-pptx.py <input.pptx> <output_dir>` 提取内容
2. 展示提取的幻灯片标题、内容和图片数量供确认
3. 进入 Phase 2 选择风格
4. 生成保留所有文字、图片和备注的 HTML 演示

---

### Phase 5：交付

- 删除临时预览文件（`.claude-design/slide-previews/`）
- 在浏览器中打开最终文件
- 告知：文件位置、导航方式（方向键/空格/滑动/圆点）、CSS 变量自定义方式

---

### Phase E：效果捕获

用户或 AI 主动将新颖效果保存到 `EFFECT_LIBRARY.md`，供后续演示复用。

---

## 内置风格（12 种）

### 深色主题
- **Bold Signal** — 高冲击力，鲜艳卡片配深色背景
- **Electric Studio** — 干净专业，分栏布局
- **Creative Voltage** — 充满活力，复古现代，电蓝 + 霓虹
- **Dark Botanical** — 优雅精致，暖色点缀

### 浅色主题
- **Notebook Tabs** — 编辑风，纸质感配彩色标签
- **Pastel Geometry** — 友好亲切，竖向色块
- **Split Pastel** — 活泼现代，双色纵向分割
- **Vintage Editorial** — 有个性，几何形状装饰

### 特色主题
- **Neon Cyber** — 未来感，粒子背景，霓虹发光
- **Terminal Green** — 开发者风格，黑客美学
- **Swiss Modern** — 极简主义，包豪斯风格
- **Paper & Ink** — 文学感，首字下沉，摘引块

---

## Vercel 静态部署

```json
{
  "outputDirectory": "tera-intro",
  "routes": [
    { "src": "/", "dest": "/slide-toc.html" }
  ]
}
```

- 无 `index.html` 时必须配置根路由重写，否则 Vercel 返回 404
- 重新部署：`npx vercel --prod --yes`
- 单文件上限：100MB（注意 `.mov` 等大文件）

---

## 支持文件

| 文件 | 用途 | 读取时机 |
|------|------|---------|
| `SKILL.md` | 核心工作流与规则 | 技能调用时（始终） |
| `STYLE_PRESETS.md` | 12 种视觉预设 | Phase 2（风格选择） |
| `viewport-base.css` | 强制响应式 CSS | Phase 3（生成） |
| `html-template.md` | HTML 结构与 JS 功能 | Phase 3（生成） |
| `animation-patterns.md` | CSS/JS 动画参考 | Phase 3（生成） |
| `EFFECT_LIBRARY.md` | 可复用效果库 | Phase 3（生成）、Phase E（捕获） |
| `scripts/extract-pptx.py` | PPT 内容提取脚本 | Phase 4（转换） |

---

## 设计原则

1. **不需要成为设计师才能做出美观的东西** — 只需要对看到的东西做出反应。
2. **依赖就是债务** — 单个 HTML 文件十年后仍然可用。2019 年的 React 项目？祝你好运。
3. **泛化即平庸** — 每个演示文稿都应该像为这个场景量身打造，而非套模板。
4. **注释是善意** — 代码应该向未来的你（或任何打开它的人）解释自己。

---

## 环境要求

- CodeBuddy / Claude Code CLI
- PPT 转换需要：Python + `python-pptx` 库（`pip install python-pptx`）

## License

MIT — 自由使用、修改、分享。
