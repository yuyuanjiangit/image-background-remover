# AI 图片背景移除工具

基于 Vue3 + Tailwind CSS 开发的 AI 图片背景移除工具，使用浏览器端 AI 模型实现本地处理，无需上传图片到服务器。

## 功能特性

- 🖼️ **拖拽上传** - 支持拖拽或点击选择图片
- 🤖 **AI 背景移除** - 使用浏览器端 AI 模型，本地处理保护隐私
- ⚡ **实时预览** - 原图与处理结果并排对比
- 💾 **一键下载** - 支持下载透明背景 PNG 图片
- 📱 **响应式设计** - 适配桌面和移动设备

## 技术栈

- **Vue 3** - 渐进式 JavaScript 框架
- **Vite** - 下一代前端构建工具
- **Tailwind CSS** - 实用优先的 CSS 框架
- **@imgly/background-removal** - 浏览器端 AI 背景移除库

## 快速开始

### 安装依赖

```bash
cd projects
npm install
```

### 开发模式

```bash
npm run dev
```

### 构建生产版本

```bash
npm run build
```

## 使用说明

1. 打开应用后，拖拽或点击上传图片
2. 点击"移除背景"按钮开始处理
3. 等待 AI 处理完成（进度条显示处理进度）
4. 对比原图和处理后的效果
5. 点击"下载图片"保存透明背景图片

## 注意事项

- 首次使用时会下载 AI 模型（约 4MB），请耐心等待
- 处理过程完全在浏览器本地完成，图片不会上传到服务器
- 支持 JPG、PNG、WebP 等常见图片格式

## 项目结构

```
projects/
├── src/
│   ├── components/      # Vue 组件
│   │   ├── AppHeader.vue
│   │   ├── AppFooter.vue
│   │   ├── ImageUploader.vue
│   │   └── ImageEditor.vue
│   ├── App.vue         # 根组件
│   ├── main.js         # 入口文件
│   └── style.css       # 全局样式
├── index.html          # HTML 模板
├── package.json        # 项目配置
├── vite.config.js      # Vite 配置
├── tailwind.config.js  # Tailwind 配置
└── postcss.config.js   # PostCSS 配置
```

## License

MIT
