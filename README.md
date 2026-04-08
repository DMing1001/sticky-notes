# 便笺 StickyNotes - 桌面版

基于 [DMing1001/sticky-notes](https://github.com/DMing1001/sticky-notes) 的 Electron 桌面封装。

## 功能

- 🗒️ 创建多个便笺，拖拽移动 & 调整大小
- 🎨 8 种鲜明配色
- 🌗 深色 / 浅色主题切换
- 🔍 搜索便笺（标题 + 正文）
- 📐 一键网格自动排列
- 📌 置顶、折叠、复制
- ⌨️ 快捷键支持
- 💾 自动保存
- 📤 导出 JSON 备份
- 🔔 **系统托盘常驻** — 关闭窗口不退出
- ⌨️ **全局快捷键** `Ctrl+Shift+N` 快速新建

## 开发运行

```bash
cd sticky-notes-app
npm install
npm start
```

## 打包为 exe 安装包

```bash
npm run build:win
```

打包产物在 `dist/` 目录下。

## 文件结构

```
sticky-notes-app/
├── main.js          # Electron 主进程
├── preload.js       # 预加载脚本（IPC 桥接）
├── index.html       # 便笺应用（来自原项目）
├── icon.png         # 应用图标
├── icon.svg         # 图标源文件
└── package.json     # 项目配置
```
