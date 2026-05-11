---
name: smart-archivist
description: AI 干货资料整理官：把截图、网页摘录、Prompt、工具教程、资源清单整理成可检索、可复用、可归档的知识库文档
version: "1.9.0"
updated: "2026-05-10"
---

# AI 干货资料整理官

## 1. 使用定位

本 skill 面向“碎片化 AI 资料整理”场景，适合处理以下内容：

- AI 工具教程，例如 ChatGPT、Claude、Kimi、Midjourney、Cursor、Figma AI 等使用方法
- Prompt 模板、提示词技巧、Agent/工作流配置
- AI 产品案例、AI 应用灵感、竞品观察、功能拆解
- AI 资源合集、工具清单、免费额度、插件导航
- 小红书、公众号、网页、截图、课程笔记中的 AI 干货信息

默认目标不是“原文搬运”，而是完成：

```text
提取文字 → 去水分 → 分类 → 打标签 → 结构化整理 → 生成文档 → 归档索引
```

兼容普通资料整理，但优先服务 AI 知识库沉淀。

---

## 2. 触发规则

### 2.1 自动触发

用户出现以下意图时触发：

- “整理一下”
- “归纳一下”
- “保存到资料库”
- “帮我把这个教程整理成文档”
- “把这几张图的 AI 干货提取出来”
- “把这个 Prompt / 工具清单整理一下”

### 2.2 默认不触发

| 场景 | 处理方式 |
|---|---|
| 用户只要求翻译 | 只翻译，不归档 |
| 用户只要求润色文章 | 只润色，不分类 |
| 用户发送纯数据表并要求分析 | 转到数据分析流程 |
| 文件意图不明确 | 先询问用途 |

---

## 3. 输入支持与边界

| 输入类型 | 当前处理能力 | 说明 |
|---|---|---|
| txt / md | 稳定支持 | 直接读取 |
| docx | 稳定支持 | 依赖 python-docx |
| xlsx | 稳定支持 | 依赖 openpyxl，提取单元格文本 |
| pdf 文本型 | 支持 | 依赖 pdfplumber |
| 图片 jpg/png/webp | 可选支持 | 依赖 rapidocr-onnxruntime，未安装时给出提示 |
| doc / xls | 不稳定 | 建议先转为 docx / xlsx |
| 扫描版 PDF | 有限支持 | 建议先 OCR 成文本再输入 |

如运行环境未安装某项依赖，脚本会给出明确错误，不再声称“全部格式都已支持”。

---

## 4. 默认工作流

默认采用“先整理，后调整”的轻量流程，但保存格式必须按用户意图执行。

```text
用户输入资料
↓
自动识别资料类型、工具名、场景和关键词
↓
清洗口水话、引流话术、重复段落
↓
生成标题、摘要、核心步骤、注意事项、标签
↓
若用户未说明保存格式，先询问保存格式，可多选：① Markdown ② HTML ③ DOCX
↓
根据用户选择输出对应格式
↓
按分类归档，并更新 README 索引
```

只有在分类置信度低于 0.5、内容过短、资料来源明显不完整时，才建议用户确认分类或内容；但只要用户没有说明保存格式，必须先询问输出格式。

---

## 5. 强制清洗规则

### 5.1 默认删除

- 寒暄和口播：宝子们、家人们、姐妹们、朋友们、谁懂啊
- 引流话术：关注我、主页更多、评论区留言、私我、转发收藏
- 空泛夸张：真的绝了、太强了、哭死、救命、无敌了
- 无操作价值：赶紧码住、先收藏再说、不看后悔
- 博主自我介绍：我是某某、专注 AI 多年等
- 重复段落、重复标题、重复分隔符

### 5.2 必须保留

- 工具名称、模型名称、版本号
- 具体路径、菜单、按钮、参数、命令
- Prompt 原文、配置模板、代码块
- 价格、额度、账号限制，但需标记“可能过期”
- 有价值的评论问答和补充说明
- 原始来源 URL，如用户提供

---

## 6. 分类体系

```text
AI资料库/
├── 01_AI工具教程/
│   ├── ChatGPT/
│   ├── Claude/
│   ├── Gemini/
│   ├── Kimi/
│   ├── Midjourney/
│   ├── Cursor/
│   └── 其他工具/
│
├── 02_Prompt提示词/
│   ├── 写作提示词/
│   ├── 产品经理提示词/
│   ├── 论文提示词/
│   ├── 绘图提示词/
│   └── 代码提示词/
│
├── 03_AI工作流/
│   ├── 办公提效/
│   ├── 内容创作/
│   ├── 数据分析/
│   ├── 简历求职/
│   └── 学习研究/
│
├── 04_AI资源合集/
│   ├── 免费工具/
│   ├── 网站导航/
│   ├── 插件扩展/
│   └── 模型资源/
│
├── 05_AI案例灵感/
│   ├── 产品案例/
│   ├── 商业案例/
│   ├── 设计案例/
│   └── 爆款案例/
│
└── 99_待整理/
```

分类优先级：

1. 具体工具教程优先进入 `01_AI工具教程`
2. Prompt 模板优先进入 `02_Prompt提示词`
3. 多工具串联、自动化流程进入 `03_AI工作流`
4. 工具合集、导航、资源包进入 `04_AI资源合集`
5. 案例、灵感、作品拆解进入 `05_AI案例灵感`
6. 无法判断进入 `99_待整理`

---

## 7. 标签体系

每篇文档建议生成 4 到 8 个标签，分为五类：

| 标签类型 | 示例 |
|---|---|
| 工具标签 | #ChatGPT #Claude #Kimi #Midjourney #Cursor |
| 资料类型 | #教程 #Prompt #案例 #工具清单 #避坑 |
| 使用场景 | #论文 #简历 #PPT #编程 #数据分析 #产品经理 |
| 难度等级 | #入门 #进阶 #高级 |
| 状态标签 | #已验证 #待验证 #可能过期 |

默认给所有来自社媒或资料合集的内容加 `#待验证`；涉及价格、免费额度、版本更新的内容加 `#可能过期`。

---

## 8. 输出格式与模板

### 8.1 输出格式确认规则

当用户明确说明输出格式时，按用户指定格式保存，例如“保存成 markdown”“导出 html 和 docx”。

当用户只说“整理一下”“保存到资料库”“归档一下”，但没有说明保存格式时，不要默认保存为 Markdown，应先询问：

```text
你想用哪种格式保存？可多选：
① Markdown：适合长期维护和知识库检索
② HTML：适合阅读和分享
③ DOCX：适合提交、打印或正式转发
```

用户回复数字、格式名或多个选项时，都应识别为对应格式，例如：

- “1” → markdown
- “1和3” → markdown + docx
- “markdown、html” → markdown + html
- “都要” → markdown + html + docx

确认格式后再执行生成与归档。

### 8.2 Markdown 模板

Markdown 输出结构如下：

```markdown
---
title: 标题
category: 一级分类/二级分类
tags: [#ChatGPT, #教程, #待验证]
source_type: text
collected_at: 2026-05-07
status: 待验证
---

# 标题

## 一句话总结

## 适用场景

## 核心方法 / 操作步骤

## Prompt / 参数 / 配置

## 注意事项

## 资料判断

## 原始资料摘录
```

可选输出：

- `.md`：便于长期维护和知识库检索
- `.html`：便于阅读和分享
- `.docx`：便于提交、打印或正式转发
- `.json`：保留结构化元数据，脚本可自动生成，但不作为用户三选项之一

### 8.3 HTML 网页版排版规范（强制）

所有通过本 skill 生成的 HTML 文件，**必须**遵循以下统一排版设计系统，确保每个网页输出风格一致。

#### 8.3.1 整体布局：左侧目录 + 右侧内容

页面采用双栏布局，左侧目录导航**可展开/收起**，正文始终在可用空间内居中：

**展开态（默认）：**
```
┌──────────┬──────────────────────────────────┐
│ 左侧目录  │         正文（居中）              │
│ (240px)  │    max-width: 900px              │
│          │    margin: 0 auto                │
│  ● 知识库1│  ┌ Hero ─────────────────────┐  │
│    知识库2│  │  标题 + 副标题 + 统计卡片  │  │
│    知识库3│  └───────────────────────────┘  │
│  ○ 知识库4│  ┌ kb-section ───────────────┐  │
│          │  │  h2 标题 + 正文内容       │  │
│  ◀ 收起  │  └───────────────────────────┘  │
└──────────┴──────────────────────────────────┘
```

**收起态：**
```
┌──┬─────────────────────────────────────────┐
│··│            正文（居中）                   │
│··│       max-width: 900px                  │
│··│       margin: 0 auto                    │
│··│  ┌ Hero ────────────────────────────┐  │
│··│  │  标题 + 副标题 + 统计卡片        │  │
│▶·│  └──────────────────────────────────┘  │
│··│  ┌ kb-section ──────────────────────┐  │
│··│  │  h2 标题 + 正文内容              │  │
│··│  └──────────────────────────────────┘  │
│··│                                         │
└──┴─────────────────────────────────────────┘
 44px  正文在剩余空间内居中
```

#### 8.3.2 设计令牌（CSS 变量）

```css
/* ── 浅色模式（默认） ── */
:root {
  --bg: #f8f9fb;
  --surface: #ffffff;
  --surface2: #f0f2f5;
  --border: #e2e6ed;
  --text: #1a1d2e;
  --text2: #6b7280;
  --accent: #4a6adf;
  --accent2: #3b56c4;
  --accent-soft: rgba(74,106,223,0.08);
  --green: #16a34a;
  --orange: #d97706;
  --red: #dc2626;
  --purple: #7c3aed;
  --cyan: #0891b2;
  --pink: #db2777;
  --shadow: 0 1px 3px rgba(0,0,0,0.06);
  --shadow-lg: 0 4px 16px rgba(74,106,223,0.12);
  --sidebar-bg: #ffffff;
  --sidebar-border: #e8ecf2;
}

/* ── 深色模式 ── */
[data-theme="dark"] {
  --bg: #0f1117;
  --surface: #1a1d27;
  --surface2: #232733;
  --border: #2e3345;
  --text: #e4e6ed;
  --text2: #9ea3b5;
  --accent: #6c8cff;
  --accent2: #4a6adf;
  --accent-soft: rgba(108,140,255,0.1);
  --green: #4ade80;
  --orange: #f59e0b;
  --red: #ef4444;
  --purple: #a78bfa;
  --cyan: #22d3ee;
  --pink: #f472b6;
  --shadow: 0 1px 3px rgba(0,0,0,0.2);
  --shadow-lg: 0 4px 16px rgba(108,140,255,0.15);
  --sidebar-bg: #141720;
  --sidebar-border: #2a2e3d;
}

/* ── 跟随系统偏好 ── */
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) {
    --bg: #0f1117;
    --surface: #1a1d27;
    --surface2: #232733;
    --border: #2e3345;
    --text: #e4e6ed;
    --text2: #9ea3b5;
    --accent: #6c8cff;
    --accent2: #4a6adf;
    --accent-soft: rgba(108,140,255,0.1);
    --green: #4ade80;
    --orange: #f59e0b;
    --red: #ef4444;
    --purple: #a78bfa;
    --cyan: #22d3ee;
    --pink: #f472b6;
    --shadow: 0 1px 3px rgba(0,0,0,0.2);
    --shadow-lg: 0 4px 16px rgba(108,140,255,0.15);
    --sidebar-bg: #141720;
    --sidebar-border: #2a2e3d;
  }
}
```

#### 8.3.3 字体与字号体系

| 元素 | 字体 | 字号 | 字重 | 颜色 |
|---|---|---|---|---|
| body | `-apple-system, BlinkMacSystemFont, 'Segoe UI', 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', sans-serif` | 15px | 400 | var(--text) |
| h1 (Hero 标题) | 同上 | 2rem | 700 | 渐变色 accent→cyan |
| h2 (区块标题) | 同上 | 1.35rem | 700 | var(--text) |
| h3 (子标题) | 同上 | 1.05rem | 600 | var(--accent) |
| h4 (细标题) | 同上 | 0.95rem | 600 | var(--cyan) |
| p, li | 同上 | 0.93rem | 400 | var(--text) |
| 代码 code | `'Fira Code', 'Cascadia Code', 'Consolas', monospace` | 0.85rem | 400 | var(--cyan) |
| 辅助文字 | 同上 | 0.8rem | 400 | var(--text2) |
| 标签 .tag | 同上 | 0.75rem | 400 | var(--text2) |
| 侧栏目录项 | 同上 | 0.85rem | 400 | var(--text2) |
| 侧栏当前项 | 同上 | 0.85rem | 600 | var(--accent) |

全局行高：`line-height: 1.7`

#### 8.3.4 左侧目录导航（可展开/收起）

左侧目录固定在页面左侧，展示到 h2 级别（即 `.kb-section` 的标题）。**核心交互：目录可点击展开/收起，正文始终在可用空间内居中。**

功能清单：
- 点击目录项平滑滚动到对应区块
- 滚动时自动高亮当前所在区块（IntersectionObserver）
- 当前项左侧有 accent 色竖线指示器
- **桌面端：点击 ◀ 收起为 44px 细条，点击 ▶ 展开为 240px 完整目录**
- **正文始终居中**：展开时在右侧剩余空间居中，收起时在全屏居中
- 移动端：底部浮动按钮呼出抽屉式目录
- 目录默认展开，每次打开页面均为展开态

**HTML 结构：**

```html
<!-- 移动端目录触发按钮 -->
<button class="toc-mobile-btn" onclick="toggleMobileSidebar()" aria-label="目录">
  <span>📑</span>
</button>

<!-- 左侧目录遮罩（移动端） -->
<div class="sidebar-overlay" onclick="toggleMobileSidebar()"></div>

<!-- 左侧目录 -->
<aside class="sidebar" id="sidebar">
  <!-- 展开/收起切换按钮（桌面端） -->
  <button class="sidebar-toggle" onclick="toggleCollapse()" title="展开/收起目录" aria-label="展开/收起目录">
    <span class="toggle-icon-collapsed">▶</span>
    <span class="toggle-icon-expanded">◀</span>
  </button>

  <div class="sidebar-inner">
    <div class="sidebar-header">
      <span class="sidebar-title">📑 目录</span>
      <button class="sidebar-close" onclick="toggleMobileSidebar()" aria-label="关闭目录">✕</button>
    </div>
    <nav class="sidebar-nav" id="sidebarNav">
      <!-- JS 动态生成 -->
    </nav>
  </div>

  <!-- 收起态：圆点指示器 -->
  <nav class="sidebar-dots" id="sidebarDots">
    <!-- JS 动态生成 -->
  </nav>
</aside>
```

**CSS 样式：**

```css
/* ── 侧栏容器 ── */
.sidebar {
  position: fixed;
  top: 0; left: 0;
  width: 240px; height: 100vh;
  background: var(--sidebar-bg);
  border-right: 1px solid var(--sidebar-border);
  z-index: 50;
  display: flex;
  transition: width 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

/* 收起态 */
.sidebar.collapsed { width: 44px; }

/* ── 展开/收起切换按钮 ── */
.sidebar-toggle {
  position: absolute;
  top: 16px; right: -14px;
  width: 28px; height: 28px;
  border-radius: 50%;
  background: var(--surface);
  border: 1px solid var(--border);
  color: var(--text2);
  font-size: 0.65rem;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: var(--shadow);
  z-index: 51;
  transition: all 0.2s;
}
.sidebar-toggle:hover {
  background: var(--accent);
  color: #fff;
  border-color: var(--accent);
}

/* 展开态显示 ◀，收起态显示 ▶ */
.sidebar.collapsed .toggle-icon-expanded,
.sidebar:not(.collapsed) .toggle-icon-collapsed { display: none; }

/* ── 侧栏内部容器 ── */
.sidebar-inner {
  width: 240px;
  min-width: 240px;
  overflow-y: auto;
  padding: 24px 0 80px;
  opacity: 1;
  transition: opacity 0.2s;
}
.sidebar.collapsed .sidebar-inner {
  opacity: 0;
  pointer-events: none;
}
.sidebar-inner::-webkit-scrollbar { width: 4px; }
.sidebar-inner::-webkit-scrollbar-thumb { background: var(--border); border-radius: 2px; }

/* 侧栏头部 */
.sidebar-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 20px 16px;
  border-bottom: 1px solid var(--sidebar-border);
  margin-bottom: 8px;
}
.sidebar-title {
  font-size: 0.9rem;
  font-weight: 600;
  color: var(--text);
}
.sidebar-close {
  display: none;
  background: none; border: none;
  color: var(--text2); font-size: 1rem;
  cursor: pointer; padding: 4px;
}

/* 导航列表 */
.sidebar-nav { padding: 0 12px; }
.sidebar-nav a {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 8px 12px;
  margin: 2px 0;
  border-radius: 8px;
  color: var(--text2);
  text-decoration: none;
  font-size: 0.85rem;
  line-height: 1.4;
  transition: all 0.2s;
  position: relative;
  border-left: 2px solid transparent;
}
.sidebar-nav a:hover {
  background: var(--accent-soft);
  color: var(--text);
}

/* 当前激活项 */
.sidebar-nav a.active {
  color: var(--accent);
  font-weight: 600;
  background: var(--accent-soft);
  border-left-color: var(--accent);
}

/* 目录项序号 */
.sidebar-nav a .toc-num {
  font-size: 0.7rem;
  font-weight: 700;
  color: var(--accent);
  min-width: 20px;
  opacity: 0.7;
}
.sidebar-nav a.active .toc-num { opacity: 1; }

/* ── 收起态：圆点指示器（始终垂直居中，滚动越过 Hero 后才显示） ── */
.sidebar-dots {
  display: none;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 10px;
  padding: 0;
  width: 44px;
  min-width: 44px;
  height: 100%;
  overflow-y: auto;
  opacity: 0;
  transition: opacity 0.35s ease;
}
.sidebar-dots.visible { opacity: 1; }
.sidebar.collapsed .sidebar-dots { display: flex; }
.sidebar-dots::-webkit-scrollbar { display: none; }

.sidebar-dots a {
  width: 8px; height: 8px;
  border-radius: 50%;
  background: var(--border);
  transition: all 0.2s;
  flex-shrink: 0;
  position: relative;
  text-decoration: none;
}
.sidebar-dots a:hover,
.sidebar-dots a.active {
  background: var(--accent);
  transform: scale(1.4);
}

/* 圆点 hover 提示 */
.sidebar-dots a::after {
  content: attr(data-title);
  position: absolute;
  left: 20px; top: 50%;
  transform: translateY(-50%);
  background: var(--surface);
  color: var(--text);
  border: 1px solid var(--border);
  padding: 4px 10px;
  border-radius: 6px;
  font-size: 0.75rem;
  white-space: nowrap;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.15s;
  box-shadow: var(--shadow);
}
.sidebar-dots a:hover::after { opacity: 1; }

/* ── 主内容区偏移（过渡动画） ── */
.main-wrapper {
  margin-left: 240px;
  transition: margin-left 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}
.sidebar.collapsed ~ .main-wrapper { margin-left: 44px; }

/* ── 移动端目录按钮 ── */
.toc-mobile-btn {
  display: none;
  position: fixed;
  bottom: 24px; left: 24px;
  z-index: 60;
  width: 48px; height: 48px;
  border-radius: 50%;
  background: var(--accent);
  color: #fff;
  border: none;
  font-size: 1.2rem;
  cursor: pointer;
  box-shadow: var(--shadow-lg);
  transition: transform 0.2s;
}
.toc-mobile-btn:hover { transform: scale(1.08); }

/* 遮罩 */
.sidebar-overlay {
  display: none;
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.4);
  z-index: 49;
  opacity: 0;
  transition: opacity 0.3s;
  pointer-events: none;
}

/* ── 响应式：移动端 ── */
@media (max-width: 860px) {
  .sidebar {
    width: 260px;
    transform: translateX(-100%);
    box-shadow: 4px 0 24px rgba(0,0,0,0.15);
  }
  .sidebar.collapsed { width: 260px; }
  .sidebar.open { transform: translateX(0); }
  .sidebar-toggle { display: none; }
  .sidebar-inner { opacity: 1 !important; pointer-events: auto !important; }
  .sidebar-dots { display: none !important; }
  .sidebar-close { display: block; }
  .sidebar-overlay { display: block; }
  .sidebar-overlay.show { opacity: 1; pointer-events: auto; }
  .main-wrapper { margin-left: 0 !important; }
  .toc-mobile-btn { display: flex; align-items: center; justify-content: center; }
}
```

**JavaScript（目录生成 + 滚动监听 + 展开/收起 + 主题切换）：**

```html
<script>
// ========== 主题切换 ==========
(function() {
  const saved = localStorage.getItem('doc-theme');
  if (saved === 'dark') document.documentElement.setAttribute('data-theme', 'dark');
  else if (saved === 'light') document.documentElement.setAttribute('data-theme', 'light');
})();

function toggleTheme() {
  const el = document.documentElement;
  const current = el.getAttribute('data-theme');
  const isDark = current === 'dark' ||
    (!current && window.matchMedia('(prefers-color-scheme: dark)').matches);
  const next = isDark ? 'light' : 'dark';
  el.setAttribute('data-theme', next);
  localStorage.setItem('doc-theme', next);
}

// ========== 桌面端：展开/收起目录 ==========
function toggleCollapse() {
  const sidebar = document.getElementById('sidebar');
  sidebar.classList.toggle('collapsed');
}

// ========== 移动端目录切换 ==========
function toggleMobileSidebar() {
  document.getElementById('sidebar').classList.toggle('open');
  document.querySelector('.sidebar-overlay').classList.toggle('show');
}

// ========== 自动生成目录 + 滚动监听 ==========
document.addEventListener('DOMContentLoaded', function() {
  const nav = document.getElementById('sidebarNav');
  const dots = document.getElementById('sidebarDots');
  const sections = document.querySelectorAll('.kb-section');
  const links = [];

  sections.forEach(function(section, i) {
    const h2 = section.querySelector('h2');
    if (!h2) return;

    if (!section.id) section.id = 'kb' + (i + 1);

    const badge = h2.querySelector('.badge');
    const badgeText = badge ? badge.textContent.trim() : '';
    const titleText = h2.textContent.replace(badgeText, '').trim();

    // ── 完整目录项 ──
    const a = document.createElement('a');
    a.href = '#' + section.id;
    a.innerHTML = '<span class="toc-num">' + String(i + 1).padStart(2, '0') + '</span>' + titleText;
    a.addEventListener('click', function(e) {
      e.preventDefault();
      section.scrollIntoView({ behavior: 'smooth', block: 'start' });
      if (window.innerWidth <= 860) toggleMobileSidebar();
    });
    nav.appendChild(a);

    // ── 收起态圆点 ──
    var dot = document.createElement('a');
    dot.href = '#' + section.id;
    dot.setAttribute('data-title', titleText);
    dot.addEventListener('click', function(e) {
      e.preventDefault();
      section.scrollIntoView({ behavior: 'smooth', block: 'start' });
    });
    dots.appendChild(dot);

    links.push({ el: a, dot: dot, section: section });
  });

  // IntersectionObserver 滚动监听
  var observer = new IntersectionObserver(function(entries) {
    entries.forEach(function(entry) {
      if (entry.isIntersecting) {
        links.forEach(function(l) {
          l.el.classList.remove('active');
          l.dot.classList.remove('active');
        });
        var idx = Array.from(sections).indexOf(entry.target);
        if (links[idx]) {
          links[idx].el.classList.add('active');
          links[idx].dot.classList.add('active');
        }
      }
    });
  }, { rootMargin: '-20% 0px -70% 0px', threshold: 0 });

  sections.forEach(function(s) { observer.observe(s); });

  // 滚动越过 Hero 后才显示圆点指示器
  var hero = document.querySelector('.hero');
  if (hero) {
    var heroObserver = new IntersectionObserver(function(entries) {
      dots.classList.toggle('visible', !entries[0].isIntersecting);
    }, { threshold: 0 });
    heroObserver.observe(hero);
  }
});
</script>
```

#### 8.3.5 页面结构（固定顺序）

```
<body>
  ├── 主题切换按钮（右上角固定）
  ├── 移动端目录触发按钮（左下角固定）
  ├── 目录遮罩层
  ├── 左侧目录导航 <aside class="sidebar">
  │   ├── 展开/收起切换按钮
  │   ├── 完整目录列表（JS 自动生成，到 h2 级别）
  │   └── 收起态圆点指示器（JS 自动生成，顶部对齐第一个正文区块）
  │
  └── 主内容区 <div class="main-wrapper">
      ├── 1. Hero 头部区
      │   ├── 标题（渐变色，h1）
      │   ├── 副标题/一句话描述
      │   ├── 元信息行：日期 + 归档路径 + 状态（如：📅 2026.5.7 · 📁 01_AI工具教程/Claude · ✅ 已验证）
      │   └── 标签行（≤6 个标签，从页脚移至此处）
      │
      ├── 2. 正文容器（max-width: 900px，居中）
      │   └── 多个 .kb-section 卡片依次排列
      │
      ├── 3. 页脚
      │   ├── 来源说明
      │   ├── 统计信息
      │   └── 质量评估（资料价值/可操作性/时效风险/收藏建议）
      │
      └── 4. 回到顶部按钮（固定右下角）
```

**Hero 元信息行规则：**
- 仅保留三要素：**日期** + **归档路径**（如 `01_AI工具教程 / Claude`）+ **状态**
- 不显示"面向人群"、"知识库数量"等冗余信息
- 不显示统计卡片栏（知识模块数、核心概念数、原始页数）

**标签行规则：**
- 标签从页脚移至 Hero 区（元信息行下方）
- **标签总数不超过 6 个**，超出部分截断
- 标签行居中显示

#### 8.3.6 组件样式规范

**Hero 头部：**
```css
.hero {
  background: linear-gradient(135deg, var(--surface) 0%, color-mix(in srgb, var(--accent) 6%, var(--surface)) 50%, var(--surface) 100%);
  border-bottom: 1px solid var(--border);
  padding: 48px 24px 40px;
  text-align: center;
}
.hero h1 {
  font-size: 2rem; font-weight: 700;
  background: linear-gradient(135deg, var(--accent), var(--cyan));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  margin-bottom: 12px;
}
.hero .subtitle { color: var(--text2); font-size: 0.95rem; }

/* 元信息行：日期 + 归档路径 + 状态 */
.hero .meta {
  margin-top: 16px;
  display: flex; gap: 12px;
  justify-content: center; flex-wrap: wrap;
}
.hero .meta span {
  background: var(--surface2); color: var(--text2);
  padding: 4px 12px; border-radius: 20px; font-size: 0.8rem;
}

/* 标签行（从页脚移至 Hero，≤6 个） */
.hero .hero-tags {
  display: flex; gap: 6px; flex-wrap: wrap;
  justify-content: center;
  margin-top: 14px;
}
```

> **注意：不再使用 `.stats-bar` 统计卡片栏。** 标签从页脚移至 Hero 区，页脚仅保留来源说明、统计信息和质量评估。

**知识区块卡片 .kb-section：**
```css
.kb-section {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 32px;
  margin-bottom: 28px;
  box-shadow: var(--shadow);
  scroll-margin-top: 24px;
}
.kb-section h2 {
  font-size: 1.35rem; font-weight: 700;
  color: var(--text);
  margin-bottom: 6px;
  display: flex; align-items: center; gap: 10px;
}
.kb-section h2 .badge {
  background: var(--accent2); color: #fff;
  font-size: 0.7rem; padding: 2px 10px;
  border-radius: 20px; font-weight: 500; white-space: nowrap;
}
.kb-section .lead {
  color: var(--text2); font-size: 0.92rem;
  margin-bottom: 24px; font-style: italic;
}
```

**Callout 提示框（四种变体）：**
- `.callout`（默认蓝边）：通用信息
- `.callout.tip`（绿边）：建议/最佳实践
- `.callout.warn`（橙边）：注意事项
- `.callout.danger`（红边）：警告/红线

```css
.callout {
  background: var(--surface2);
  border-left: 3px solid var(--accent);
  border-radius: 0 8px 8px 0;
  padding: 14px 18px; margin: 14px 0;
  font-size: 0.9rem;
}
.callout.warn  { border-left-color: var(--orange); }
.callout.tip   { border-left-color: var(--green); }
.callout.danger{ border-left-color: var(--red); }
.callout .callout-title { font-weight: 600; margin-bottom: 4px; font-size: 0.85rem; }
.callout.warn .callout-title   { color: var(--orange); }
.callout.tip .callout-title    { color: var(--green); }
.callout.danger .callout-title { color: var(--red); }
```

**通俗类比卡片 .analogy：**
```css
.analogy {
  background: linear-gradient(135deg, rgba(108,140,255,0.06), rgba(34,211,238,0.04));
  border: 1px solid rgba(108,140,255,0.15);
  border-radius: 10px;
  padding: 16px 20px; margin: 14px 0;
}
.analogy .analogy-title {
  font-size: 0.8rem; color: var(--purple);
  font-weight: 600; margin-bottom: 6px;
}
```

**表格：**
```css
table { width: 100%; border-collapse: collapse; margin: 14px 0; font-size: 0.88rem; }
th, td { border: 1px solid var(--border); padding: 10px 14px; text-align: left; }
th { background: var(--surface2); color: var(--accent); font-weight: 600; white-space: nowrap; }
tr:hover td { background: var(--accent-soft); }
```

**统计卡片 .stat-item：**
```css
.stats-bar { display: flex; gap: 16px; justify-content: center; margin-top: 20px; flex-wrap: wrap; }
.stat-item {
  background: var(--surface2); border-radius: 10px;
  padding: 12px 20px; text-align: center; min-width: 120px;
}
.stat-item .stat-num { font-size: 1.5rem; font-weight: 700; color: var(--accent); }
.stat-item .stat-label { font-size: 0.78rem; color: var(--text2); margin-top: 2px; }
```

**子序号标记 .sub-num：**
```css
.sub-num {
  display: inline-block;
  background: var(--accent2); color: #fff;
  width: 22px; height: 22px; border-radius: 50%;
  text-align: center; line-height: 22px;
  font-size: 0.75rem; font-weight: 600; margin-right: 6px;
}
```

**标签 .tag：**
```css
.tag-row { display: flex; gap: 6px; flex-wrap: wrap; margin-top: 12px; }
.tag {
  background: var(--surface2); color: var(--text2);
  padding: 3px 10px; border-radius: 14px; font-size: 0.75rem;
}
```

**主题切换按钮：**
```css
.theme-toggle {
  position: fixed; top: 16px; right: 16px; z-index: 100;
  background: var(--surface); border: 1px solid var(--border);
  border-radius: 50%; width: 40px; height: 40px;
  cursor: pointer; font-size: 1.1rem;
  display: flex; align-items: center; justify-content: center;
  box-shadow: var(--shadow); transition: all 0.2s;
}
.theme-toggle:hover { transform: scale(1.08); }
```

**回到顶部按钮：**
```css
.back-top {
  position: fixed; bottom: 32px; right: 32px;
  background: var(--accent); color: #fff;
  width: 44px; height: 44px; border-radius: 50%;
  display: flex; align-items: center; justify-content: center;
  text-decoration: none; font-size: 1.2rem;
  box-shadow: var(--shadow-lg);
  transition: transform 0.15s; z-index: 99;
}
.back-top:hover { transform: translateY(-2px); }
```

**页脚：**
```css
.footer {
  text-align: center; padding: 40px 24px;
  color: var(--text2); font-size: 0.8rem;
  border-top: 1px solid var(--border);
}
```

#### 8.3.7 响应式断点

```css
@media (max-width: 860px) {
  /* 左侧目录变为抽屉式 */
  .sidebar { transform: translateX(-100%); }
  .sidebar.open { transform: translateX(0); }
  .main-wrapper { margin-left: 0; }
  .toc-mobile-btn { display: flex; }
  .sidebar-close { display: block; }
}

@media (max-width: 640px) {
  .hero h1 { font-size: 1.5rem; }
  .kb-section { padding: 20px; }
  table { font-size: 0.8rem; }
  th, td { padding: 6px 8px; }
}
```

#### 8.3.8 生成检查清单

每次输出 HTML 文件前，确认以下项目：

- [ ] CSS 变量完整（`:root` 浅色 + `[data-theme="dark"]` 深色 + `prefers-color-scheme` 自动跟随）
- [ ] 左侧目录导航已放置（`<aside class="sidebar">`），JS 自动生成到 h2 级别
- [ ] 主题切换按钮已放置（右上角固定定位）
- [ ] 移动端目录触发按钮 + 遮罩层已放置
- [ ] 主内容区包裹在 `<div class="main-wrapper">` 中
- [ ] 字体栈正确（系统字体优先，代码用等宽字体）
- [ ] 页面结构完整（Sidebar → ThemeToggle → MainWrapper{Hero → Sections → Footer → BackTop}）
- [ ] 所有颜色使用 CSS 变量，无硬编码色值
- [ ] 表格、Callout、类比卡片使用规定 class
- [ ] 响应式断点已包含（860px + 640px）
- [ ] Hero 元信息行仅含三要素：日期 + 归档路径 + 状态（无"面向人群""知识库数量"）
- [ ] Hero 区无统计卡片栏（.stats-bar 已删除）
- [ ] 标签行在 Hero 区（元信息行下方），≤6 个标签，页脚不再重复标签
- [ ] 收起态圆点指示器始终垂直居中，Hero 滚出视口后才淡入显示（IntersectionObserver + opacity 过渡）
- [ ] 回到顶部按钮可用
- [ ] IntersectionObserver 滚动监听已初始化
- [ ] 移动端点击目录项后自动关闭侧栏

### 8.4 DOCX 排版规范（强制）

所有通过本 skill 生成的 DOCX 文件，**必须**遵循以下排版设计系统，确保每个 Word 文档输出风格一致。基于 `python-docx` 实现。

#### 8.4.1 页面设置

| 属性 | 值 |
|---|---|
| 纸张 | A4（210mm × 297mm） |
| 上/下边距 | 2.54cm（1 英寸） |
| 左/右边距 | 2.54cm（1 英寸） |
| 页眉 | 1.5cm |
| 页脚 | 1.75cm |

```python
from docx.shared import Cm, Pt, RGBColor
from docx.enum.text import WD_ALIGN_PARAGRAPH

section = document.sections[0]
section.top_margin = Cm(2.54)
section.bottom_margin = Cm(2.54)
section.left_margin = Cm(2.54)
section.right_margin = Cm(2.54)
```

#### 8.4.2 字体与字号体系

| 元素 | 字体 | 字号 | 字重 | 颜色 | 对齐 |
|---|---|---|---|---|---|
| 文档标题 | 微软雅黑 | 22pt（二号） | Bold | #1a1d2e | 居中 |
| 副标题/摘要 | 微软雅黑 | 11pt | Regular | #6b7280 | 居中 |
| h2 一级章节 | 微软雅黑 | 16pt（三号） | Bold | #1a1d2e | 左对齐 |
| h3 二级章节 | 微软雅黑 | 14pt（四号） | Bold | #4a6adf | 左对齐 |
| h4 三级章节 | 微软雅黑 | 12pt（小四） | Bold | #0891b2 | 左对齐 |
| 正文 | 微软雅黑 | 11pt | Regular | #1a1d2e | 两端对齐 |
| 列表项 | 微软雅黑 | 11pt | Regular | #1a1d2e | 两端对齐 |
| 代码块 | Consolas | 9pt | Regular | #0891b2 | 左对齐 |
| 表格正文 | 微软雅黑 | 10pt | Regular | #1a1d2e | 左对齐 |
| 表头 | 微软雅黑 | 10pt | Bold | #ffffff | 左对齐 |
| 页脚 | 微软雅黑 | 8pt | Regular | #9ea3b5 | 居中 |

行距：正文 1.5 倍，代码块 1.0 倍，表格 1.15 倍。

```python
from docx.shared import Pt, RGBColor
from docx.oxml.ns import qn

def set_font(run, name='微软雅黑', size=Pt(11), bold=False, color=RGBColor(0x1a, 0x1d, 0x2e)):
    run.font.name = name
    run._element.rPr.rFonts.set(qn('w:eastAsia'), name)
    run.font.size = size
    run.bold = bold
    run.font.color.rgb = color

def set_spacing(paragraph, line_spacing=1.5):
    pf = paragraph.paragraph_format
    pf.line_spacing = line_spacing
    pf.space_before = Pt(0)
    pf.space_after = Pt(6)
```

#### 8.4.3 文档结构（固定顺序）

```
1. 封面/标题区
   ├── 文档标题（22pt，居中，加粗，纯文字不加 emoji/图案）
   ├── 副标题/一句话总结（11pt，居中，灰色）
   └── 元信息表（6 行 × 2 列竖排：第一列加粗黑字标签，第二列黑字内容，无底纹，行列宽自适应）

2. 正文
   ├── h2 一级章节标题（16pt，加粗）
   │   ├── 段落正文（11pt，1.5 倍行距，两端对齐）
   │   ├── h3 二级章节（14pt，加粗，accent 色）
   │   ├── h4 三级章节（12pt，加粗，cyan 色）
   │   ├── 表格（10pt 正文，表头 accent 色底 + 白字）
   │   ├── 列表（11pt，bullet 缩进）
   │   ├── 代码块（Consolas 9pt，浅灰底，1.0 倍行距）
   │   └── Callout 提示框（左边框 accent 色，浅灰底）
   └── ...更多章节

3. 页脚（每页自动）
   ├── 左侧：文档标题（8pt，灰色）
   └── 右侧：页码

4. 末页附录
   ├── 质量评估表（4 行 × 2 列）
   └── 标签列表
```

#### 8.4.4 标题样式定义

```python
from docx.enum.style import WD_STYLE_TYPE

def create_heading_styles(document):
    """注册 h2/h3/h4 自定义样式"""
    # h2 — 一级章节
    h2 = document.styles.add_style('H2 Custom', WD_STYLE_TYPE.PARAGRAPH)
    h2.font.name = '微软雅黑'
    h2.font.size = Pt(16)
    h2.font.bold = True
    h2.font.color.rgb = RGBColor(0x1a, 0x1d, 0x2e)
    h2.paragraph_format.space_before = Pt(24)
    h2.paragraph_format.space_after = Pt(8)
    h2.paragraph_format.line_spacing = 1.5

    # h3 — 二级章节
    h3 = document.styles.add_style('H3 Custom', WD_STYLE_TYPE.PARAGRAPH)
    h3.font.name = '微软雅黑'
    h3.font.size = Pt(14)
    h3.font.bold = True
    h3.font.color.rgb = RGBColor(0x4a, 0x6a, 0xdf)
    h3.paragraph_format.space_before = Pt(16)
    h3.paragraph_format.space_after = Pt(6)
    h3.paragraph_format.line_spacing = 1.5

    # h4 — 三级章节
    h4 = document.styles.add_style('H4 Custom', WD_STYLE_TYPE.PARAGRAPH)
    h4.font.name = '微软雅黑'
    h4.font.size = Pt(12)
    h4.font.bold = True
    h4.font.color.rgb = RGBColor(0x08, 0x91, 0xb2)
    h4.paragraph_format.space_before = Pt(12)
    h4.paragraph_format.space_after = Pt(4)
    h4.paragraph_format.line_spacing = 1.5
```

#### 8.4.5 表格样式

```python
from docx.shared import Pt, RGBColor, Cm
from docx.oxml.ns import qn, nsdecls
from docx.oxml import parse_xml

def style_table(table):
    """统一表格样式：边框 + 表头 accent 底 + 斑马纹 + 垂直居中 + 段前段后为0 + 统一行高"""
    table.autofit = True

    # 设置表格边框
    tbl = table._tbl
    tblPr = tbl.tblPr if tbl.tblPr is not None else parse_xml(f'<w:tblPr {nsdecls("w")}></w:tblPr>')
    borders = parse_xml(
        f'<w:tblBorders {nsdecls("w")}>'
        '<w:top w:val="single" w:sz="4" w:space="0" w:color="e2e6ed"/>'
        '<w:left w:val="single" w:sz="4" w:space="0" w:color="e2e6ed"/>'
        '<w:bottom w:val="single" w:sz="4" w:space="0" w:color="e2e6ed"/>'
        '<w:right w:val="single" w:sz="4" w:space="0" w:color="e2e6ed"/>'
        '<w:insideH w:val="single" w:sz="4" w:space="0" w:color="e2e6ed"/>'
        '<w:insideV w:val="single" w:sz="4" w:space="0" w:color="e2e6ed"/>'
        '</w:tblBorders>'
    )
    tblPr.append(borders)

    # 所有单元格垂直居中
    for row in table.rows:
        for cell in row.cells:
            tcPr = cell._tc.get_or_add_tcPr()
            vAlign = parse_xml(f'<w:vAlign {nsdecls("w")} w:val="center"/>')
            tcPr.append(vAlign)

    # 表头行
    for cell in table.rows[0].cells:
        shading = parse_xml(f'<w:shd {nsdecls("w")} w:fill="4a6adf" w:val="clear"/>')
        cell._tc.get_or_add_tcPr().append(shading)
        for p in cell.paragraphs:
            pf = p.paragraph_format
            pf.space_before = Pt(0)
            pf.space_after = Pt(0)
            for run in p.runs:
                run.font.color.rgb = RGBColor(0xff, 0xff, 0xff)
                run.font.bold = True
                run.font.size = Pt(10)

    # 数据行：斑马纹
    for i, row in enumerate(table.rows[1:], 1):
        if i % 2 == 0:
            for cell in row.cells:
                shading = parse_xml(f'<w:shd {nsdecls("w")} w:fill="f0f2f5" w:val="clear"/>')
                cell._tc.get_or_add_tcPr().append(shading)
        for cell in row.cells:
            for p in cell.paragraphs:
                pf = p.paragraph_format
                pf.space_before = Pt(0)
                pf.space_after = Pt(0)

    # 统一行高：根据表格内最大文字行数设定
    max_lines = 1
    for row in table.rows:
        for cell in row.cells:
            lines = cell.text.count('\n') + 1
            if lines > max_lines:
                max_lines = lines

    if max_lines <= 1:
        height = Cm(0.8)
    elif max_lines == 2:
        height = Cm(1.2)
    else:
        height = Cm(1.6)

    for row in table.rows:
        row.height = height
        row.height_rule = WD_ROW_HEIGHT_RULE.AT_LEAST
```

> **行高规则**：根据表格内最大文字行数统一设定——1 行 0.8cm，2 行 1.2cm，3 行及以上 1.6cm。使用 `AT_LEAST` 规则，内容超出时自动扩展。

#### 8.4.6 Callout 提示框（Word 中的实现）

Word 不支持 CSS 那样的 border-left，用**缩进 + 左边框模拟的段落**替代：

```python
def add_callout(document, text, callout_type='info'):
    """
    添加 Callout 段落。
    callout_type: 'info'(蓝) / 'tip'(绿) / 'warn'(橙) / 'danger'(红)
    """
    colors = {
        'info':   RGBColor(0x4a, 0x6a, 0xdf),
        'tip':    RGBColor(0x16, 0xa3, 0x4a),
        'warn':   RGBColor(0xd9, 0x77, 0x06),
        'danger': RGBColor(0xdc, 0x26, 0x26),
    }
    p = document.add_paragraph()
    p.paragraph_format.left_indent = Cm(0.5)
    p.paragraph_format.line_spacing = 1.3

    # 左边框通过段落底纹模拟
    pPr = p._p.get_or_add_pPr()
    shd = parse_xml(f'<w:shd {nsdecls("w")} w:fill="f0f2f5" w:val="clear"/>')
    pPr.append(shd)

    run = p.add_run(text)
    run.font.name = '微软雅黑'
    run.font.size = Pt(10)
    run.font.color.rgb = colors.get(callout_type, colors['info'])
    return p
```

#### 8.4.7 代码块

```python
def add_code_block(document, code_text):
    """添加代码块：Consolas 9pt，浅灰底，1.0 行距"""
    p = document.add_paragraph()
    p.paragraph_format.line_spacing = 1.0
    p.paragraph_format.left_indent = Cm(0.5)

    # 浅灰底
    pPr = p._p.get_or_add_pPr()
    shd = parse_xml(f'<w:shd {nsdecls("w")} w:fill="f0f2f5" w:val="clear"/>')
    pPr.append(shd)

    run = p.add_run(code_text)
    run.font.name = 'Consolas'
    run.font.size = Pt(9)
    run.font.color.rgb = RGBColor(0x08, 0x91, 0xb2)
    return p
```

#### 8.4.8 页眉页脚

```python
def add_header_footer(document, title):
    """添加页眉（文档标题）和页脚（页码）"""
    for section in document.sections:
        # 页眉
        header = section.header
        header.is_linked_to_previous = False
        hp = header.paragraphs[0]
        hp.alignment = WD_ALIGN_PARAGRAPH.LEFT
        hr = hp.add_run(title)
        hr.font.name = '微软雅黑'
        hr.font.size = Pt(8)
        hr.font.color.rgb = RGBColor(0x9e, 0xa3, 0xb5)

        # 页脚 — 页码
        footer = section.footer
        footer.is_linked_to_previous = False
        fp = footer.paragraphs[0]
        fp.alignment = WD_ALIGN_PARAGRAPH.RIGHT
        # 插入页码域
        run = fp.add_run()
        fldChar1 = parse_xml(f'<w:fldChar {nsdecls("w")} w:fldCharType="begin"/>')
        run._r.append(fldChar1)
        run2 = fp.add_run()
        instrText = parse_xml(f'<w:instrText {nsdecls("w")} xml:space="preserve"> PAGE </w:instrText>')
        run2._r.append(instrText)
        run3 = fp.add_run()
        fldChar2 = parse_xml(f'<w:fldChar {nsdecls("w")} w:fldCharType="end"/>')
        run3._r.append(fldChar2)
```

#### 8.4.9 元信息表（封面区，6×2 竖排，简洁风格）

元信息表采用 6 行 × 2 列竖排布局。第一列加粗黑字标签，第二列黑字内容，无底纹，行列宽自适应内容，所有单元格垂直居中。

```python
def add_meta_table(document, meta):
    """
    封面元信息表，6×2 竖排，简洁风格。meta 示例：
    {
        'category': '01_AI工具教程 / ChatGPT',
        'date': '2026-05-10',
        'status': '已验证',
        'tags': '#ChatGPT #教程 #入门',
        'source': '公众号：AI前线',
        'version': 'v1.0'
    }
    """
    fields = [
        ('归档路径', meta.get('category', '')),
        ('日期',     meta.get('date', '')),
        ('状态',     meta.get('status', '')),
        ('标签',     meta.get('tags', '')),
        ('来源',     meta.get('source', '')),
        ('版本',     meta.get('version', '')),
    ]

    table = document.add_table(rows=len(fields), cols=2)
    table.alignment = WD_TABLE_ALIGNMENT.CENTER
    table.autofit = True

    # 边框
    tbl = table._tbl
    tblPr = tbl.tblPr if tbl.tblPr is not None else parse_xml(f'<w:tblPr {nsdecls("w")}></w:tblPr>')
    borders = parse_xml(
        f'<w:tblBorders {nsdecls("w")}>'
        '<w:top w:val="single" w:sz="4" w:space="0" w:color="e2e6ed"/>'
        '<w:left w:val="single" w:sz="4" w:space="0" w:color="e2e6ed"/>'
        '<w:bottom w:val="single" w:sz="4" w:space="0" w:color="e2e6ed"/>'
        '<w:right w:val="single" w:sz="4" w:space="0" w:color="e2e6ed"/>'
        '<w:insideH w:val="single" w:sz="4" w:space="0" w:color="e2e6ed"/>'
        '<w:insideV w:val="single" w:sz="4" w:space="0" w:color="e2e6ed"/>'
        '</w:tblBorders>'
    )
    tblPr.append(borders)

    for i, (label, value) in enumerate(fields):
        # 第一列：加粗黑字标签
        cell_label = table.rows[i].cells[0]
        cell_label.text = ''
        p = cell_label.paragraphs[0]
        run = p.add_run(label)
        run.font.name = '微软雅黑'
        run.font.size = Pt(10)
        run.bold = True
        run.font.color.rgb = RGBColor(0x1a, 0x1d, 0x2e)

        # 第二列：黑字内容
        cell_value = table.rows[i].cells[1]
        cell_value.text = ''
        p = cell_value.paragraphs[0]
        run = p.add_run(str(value))
        run.font.name = '微软雅黑'
        run.font.size = Pt(10)
        run.font.color.rgb = RGBColor(0x1a, 0x1d, 0x2e)

        # 垂直居中
        for cell in table.rows[i].cells:
            tcPr = cell._tc.get_or_add_tcPr()
            vAlign = parse_xml(f'<w:vAlign {nsdecls("w")} w:val="center"/>')
            tcPr.append(vAlign)

    return table
```

#### 8.4.10 生成检查清单

每次输出 DOCX 文件前，确认以下项目：

- [ ] 页面 A4，四边距 2.54cm
- [ ] 字体统一：中文 微软雅黑，英文/代码 Consolas
- [ ] 标题层级正确：h2(16pt) → h3(14pt accent) → h4(12pt cyan)
- [ ] 正文 11pt，1.5 倍行距，两端对齐
- [ ] 表格有边框 + 表头 accent 色底白字 + 斑马纹
- [ ] 代码块 Consolas 9pt，浅灰底，1.0 倍行距
- [ ] Callout 有缩进 + 浅灰底 + 对应颜色文字
- [ ] 标题纯文字，无 emoji/图案装饰
- [ ] 表格行高按最大文字行数统一：1行0.8cm / 2行1.2cm / 3行+1.6cm（AT_LEAST规则）
- [ ] 封面含元信息表（6×2 竖排：第一列加粗黑字，第二列黑字，无底纹，行列宽自适应，垂直居中）
- [ ] 页眉含文档标题（8pt 灰色），页脚含页码
- [ ] 末页含质量评估表 + 标签列表
- [ ] 无硬编码色值，所有颜色与 HTML 设计令牌对应

---

## 9. 质量判断

每篇资料都应给出四项判断：

| 维度 | 取值 |
|---|---|
| 资料价值 | 高 / 中 / 低 |
| 可操作性 | 高 / 中 / 低 |
| 时效风险 | 高 / 中 / 低 |
| 收藏建议 | 建议收藏 / 待验证后收藏 / 不建议收藏 |

判断依据：

- 有工具名、步骤、Prompt、配置细节 → 可操作性更高
- 只有营销话术、没有可复现步骤 → 价值较低
- 涉及“免费、最新、限时、价格、额度、版本” → 时效风险更高
- 内容来源不明或截图不完整 → 标记为待验证

---

## 10. 查重与合并建议

归档时应做基础查重：

- 文件名相似
- 标题相似
- 工具名相同且核心关键词相近
- Prompt 模板高度相似

发现疑似重复时，不直接覆盖，采用：

```text
原文件名_2.md
原文件名_3.md
```

同时在元数据中记录 `duplicate_hint`，建议后续人工合并。

---

## 11. 脚本入口

推荐入口。用户已明确格式时：

```bash
python scripts/run_pipeline.py input.txt --base-path ./AI资料库 --formats markdown html docx
```

用户未明确格式时，命令行会进入交互式选择：

```bash
python scripts/run_pipeline.py input.txt --base-path ./AI资料库
```

单独使用：

```bash
python scripts/ingest_file.py input.docx
python scripts/clean_content.py input.txt
python scripts/classify_content.py clean.json
python scripts/generate_document.py data.json --out-dir ./output --formats markdown html docx
python scripts/organize_files.py data.json --base-path ./AI资料库
```

---

## 12. 维护建议

- `references/filter_rules.json`：维护口水话、引流词、保留规则
- `references/tool_aliases.json`：维护 AI 工具别名
- `references/category_guide.md`：维护分类体系和判断规则
- `assets/markdown_template.md`：维护 Markdown 输出结构
- `scripts/run_pipeline.py`：主入口，不建议拆开给普通用户使用
