# @linglongos/vite-plugin-html

[**English**](./README.md) | [**中文**](./README.zh_CN.md)

Forked from [vite-plugin-html](https://www.npmjs.com/package/vite-plugin-html) with **Vite 6/7/8 support** and modernized dependencies.

## Features

- HTML compression capability
- EJS template capability
- Multi-page application support
- Support custom `entry`
- Support custom `template`
- **Vite 6/7/8 support** - Active maintenance and compatibility updates
- **Rolldown compatible** - Built for Vite 8's new architecture

## Install

```bash
# using npm
npm install @linglongos/vite-plugin-html -D

# using yarn
yarn add @linglongos/vite-plugin-html -D

# using pnpm
pnpm add @linglongos/vite-plugin-html -D
```

## Requirements

- **Node.js**: >=18.0.0
- **Vite**: >=2.0.0

## Quick Start

### 1. Add EJS tags to `index.html`

```html
<head>
  <meta charset="UTF-8" />
  <link rel="icon" href="/favicon.ico" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title><%- title %></title>
  <%- injectScript %>
</head>
```

### 2. Configure in `vite.config.ts`

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

## Multi-Page Application

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

## API Reference

### UserOptions

| Parameter | Type                       | Default       | Description                   |
| --------- | -------------------------- | ------------- | ----------------------------- |
| entry     | `string`                   | `src/main.ts` | Entry file path               |
| template  | `string`                   | `index.html`  | Relative path to the template |
| inject    | `InjectOptions`            | -             | Data injected into HTML       |
| minify    | `boolean \| MinifyOptions` | -             | Whether to compress html      |
| pages     | `PageOption`               | -             | Multi-page configuration      |

### InjectOptions

| Parameter  | Type                  | Default | Description                               |
| ---------- | --------------------- | ------- | ----------------------------------------- |
| data       | `Record<string, any>` | -       | Injected data (accessible via EJS syntax) |
| ejsOptions | `EJSOptions`          | -       | EJS configuration                         |
| tags       | `HtmlTagDescriptor`   | -       | List of tags to inject                    |

### PageOption

| Parameter     | Type            | Default       | Description                   |
| ------------- | --------------- | ------------- | ----------------------------- |
| filename      | `string`        | -             | HTML file name                |
| template      | `string`        | `index.html`  | Relative path to the template |
| entry         | `string`        | `src/main.ts` | Entry file path               |
| injectOptions | `InjectOptions` | -             | Data injected into HTML       |

## Environment Variables

By default, the contents of the `.env` file will be injected into index.html, similar to Vite's `loadEnv` function.

## License

MIT

---

Forked from [vite-plugin-html](https://www.npmjs.com/package/vite-plugin-html)
