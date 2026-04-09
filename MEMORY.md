# MEMORY.md

## 用户信息
- GitHub 用户名: DMing1001

## 项目

### 墨滴 Modī (原便笺 Sticky Notes)
- 单文件 HTML 便笺应用，PC 端用，Electron 封装
- GitHub: https://github.com/DMing1001/sticky-notes
- Pages: https://dming1001.github.io/sticky-notes/
- 2026-04-08 重命名为「墨滴 Modī」，重新设计了 logo（紫色双层便笺 + 墨滴点缀）

### 已完成功能
- 深色/浅色主题、8 种配色、搜索、网格排列（Masonry 瀑布流）、置顶、折叠、复制
- 快捷键、自动保存、导出 JSON、系统托盘、全局快捷键 Ctrl+Shift+N
- Checklist 模式、字体大小调节、图片支持
- **暖心消息**（2026-04-08 新增）：8 个时段 × 10 句话，点击可切换随机话语
- **单实例锁定**（2026-04-08 新增）：防止多开导致数据丢失
- **工具栏瘦身**（2026-04-08）：56px→40px，纯图标按钮
- **导入 JSON**（2026-04-08 新增）：支持导入之前导出的便笺备份
- **搜索高亮**（2026-04-08 新增）：匹配关键词黄色高亮显示
- **Markdown 快捷**（2026-04-08 新增）：输入 `- `、`* `、`# `、`> ` 后 Enter 自动格式化
- **图片预览**（2026-04-08 优化）：点击图片弹出浮层预览，不再溢出容器

### CI/CD
- GitHub Actions 自动构建 workflow（.github/workflows/build.yml）
- 打 tag（如 `v1.0.0`）→ 自动构建 Electron exe → 生成 GitHub Release
- push 到 main → 自动部署 GitHub Pages
- 首个 Release: v1.0.0 → Modi.Setup.1.0.0.exe

### 开发流程
- 改代码 → commit → push → 自动部署（GitHub Pages）
- 想出新版本 → `git tag v1.x.x && git push --tags` → 自动打包 + Release
- 封装：npm run build:win（本地），CI 用 `npx electron-builder --win --publish never`

### 注意事项
- 用户分享过 GitHub token，已提醒每次用完撤销
- electron-builder 打包时不要自动发布（用 --publish never），Release 由 softprops/action-gh-release 处理

## Bug 修复记录 (2026-04-09)

### Bug 1: 清单模式回车不正常
**现象**: 在清单模式下按回车，新行出现了但无法在新行打字，光标无法正确跳到新行。在文字中间回车正常，但末尾回车有问题。
**根因**: 
- 原实现直接操作 DOM text node（Range 拆分 + 手动创建元素 + focus），浏览器在 contenteditable 环境下的内部选择状态管理与手动 DOM 操作冲突
- `findCheckItem` 函数在 `note-bd` 处提前返回 null，某些光标位置找不到 check-item
- 尝试加 `mousedown` handler 强制 focus，反而阻止了浏览器默认光标放置（mousedown 阶段的 focus 会干扰 contenteditable）
**修复方案**: 
- 完全重写清单 keydown handler
- Enter/Backspace 改为"修改数据模型 → renderChecklist 重新渲染 → setTimeout focus 新项"路线
- 删掉全局 `findCheckItem`，换成 keydown 内部的 `findCI`（遍历到 document 为止）
- 删掉所有 `mousedown` handler，让浏览器自然处理光标
- 删掉直接 DOM text node 操作，避免与浏览器 selection 管理冲突

### Bug 2: 排列后便笺宽度重置
**现象**: 点排列后所有便笺宽度变回默认 280px，用户自定义的宽度丢失。
**根因**: `arrange()` 里无条件执行 `n.w = Math.round(colW)` 覆盖宽度。
**修复方案**:
- 排列按钮改为弹出选择菜单，两种模式：
  - "保持当前宽度"（默认）— 按便笺各自宽度做瀑布流排列
  - "统一宽度排列" — 原来的等宽 Masonry 布局
- 记忆选择到 localStorage
- 保持宽度模式：相同宽度的便笺归入同一列，不同宽度各自成列
