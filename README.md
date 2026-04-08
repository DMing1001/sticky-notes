# 墨滴 Modī - 桌面便笺

基于 Electron 的轻量桌面便笺应用。随手记，随时看。

## 功能

- 🗒️ 创建多个便笺，拖拽移动 & 调整大小
- 🎨 8 种鲜明配色
- 🌗 深色 / 浅色主题切换
- 🔍 搜索便笺（标题 + 正文）
- 📐 一键网格自动排列
- 📌 置顶、折叠、复制
- ☀️ **暖心消息** — 不同时间段自动显示鼓励话语
- ⌨️ 快捷键支持
- 💾 自动保存
- 📤 导出 JSON 备份
- 🔔 **系统托盘常驻** — 关闭窗口不退出
- ⌨️ **全局快捷键** `Ctrl+Shift+N` 快速新建

## 开发运行

```bash
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
├── main.js          # Electron 主进程
├── preload.js       # 预加载脚本（IPC 桥接）
├── index.html       # 应用主体（CSS + JS + HTML）
├── icon.png         # 应用图标
├── icon.svg         # 图标源文件
└── package.json     # 项目配置
```
