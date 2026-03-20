# @linglongos/vite-plugin-html

基于 [vite-plugin-html](https://www.npmjs.com/package/vite-plugin-html) fork，**支持 Vite 6/7/8**，持续更新维护。

## 功能特性

- HTML 压缩能力
- EJS 模板能力
- 多页应用支持
- 支持自定义 `entry`
- 支持自定义 `template`
- **Vite 6/7/8 支持** - 持续更新维护
- **Rolldown 兼容** - 基于 Vite 8 新架构构建

## 安装

```bash
# 使用 npm
npm install @linglongos/vite-plugin-html -D

# 使用 yarn
yarn add @linglongos/vite-plugin-html -D

# 使用 pnpm
pnpm add @linglongos/vite-plugin-html -D
```

## 环境要求

- **Node.js**: >=18.0.0
- **Vite**: >=2.0.0

## 快速开始

### 1. 在 `index.html` 中添加 EJS 标签

```html
<head>
  <meta charset="UTF-8" />
  <link rel="icon" href="/favicon.ico" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title><%- title %></title>
  <%- injectScript %>
</head>
```

### 2. 在 `vite.config.ts` 中配置

```ts
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import { createHtmlPlugin } from '@linglongos/vite-plugin-html'

export default defineConfig({
  plugins: [
    vue(),
    createHtmlPlugin({
      minify: true,
      entry: 'src/main.ts',
      template: 'public/index.html',
      inject: {
        data: {
          title: 'index',
          injectScript: `<script src="./inject.js"></script>`,
        },
      },
    }),
  ],
})
```

## 多页应用配置

```ts
import { defineConfig } from 'vite'
import { createHtmlPlugin } from '@linglongos/vite-plugin-html'

export default defineConfig({
  plugins: [
    createHtmlPlugin({
      minify: true,
      pages: [
        {
          entry: 'src/main.ts',
          filename: 'index.html',
          template: 'public/index.html',
          injectOptions: {
            data: {
              title: 'index',
            },
          },
        },
        {
          entry: 'src/other-main.ts',
          filename: 'other.html',
          template: 'public/other.html',
          injectOptions: {
            data: {
              title: 'other page',
            },
          },
        },
      ],
    }),
  ],
})
```

## API 参考

### UserOptions

| 参数     | 类型                       | 默认值        | 说明             |
| -------- | -------------------------- | ------------- | ---------------- |
| entry    | `string`                   | `src/main.ts` | 入口文件路径     |
| template | `string`                   | `index.html`  | 模板相对路径     |
| inject   | `InjectOptions`            | -             | 注入 HTML 的数据 |
| minify   | `boolean \| MinifyOptions` | -             | 是否压缩 html    |
| pages    | `PageOption`               | -             | 多页配置         |

### InjectOptions

| 参数       | 类型                  | 默认值 | 说明                           |
| ---------- | --------------------- | ------ | ------------------------------ |
| data       | `Record<string, any>` | -      | 注入的数据 (可用 EJS 语法访问) |
| ejsOptions | `EJSOptions`          | -      | EJS 配置项                     |
| tags       | `HtmlTagDescriptor`   | -      | 需要注入的标签列表             |

### PageOption

| 参数          | 类型            | 默认值        | 说明             |
| ------------- | --------------- | ------------- | ---------------- |
| filename      | `string`        | -             | HTML 文件名      |
| template      | `string`        | `index.html`  | 模板相对路径     |
| entry         | `string`        | `src/main.ts` | 入口文件路径     |
| injectOptions | `InjectOptions` | -             | 注入 HTML 的数据 |

## 环境变量注入

默认会向 index.html 注入 `.env` 文件的内容，类似 Vite 的 `loadEnv` 函数。

## License

MIT

---

基于 [vite-plugin-html](https://www.npmjs.com/package/vite-plugin-html) fork
