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
