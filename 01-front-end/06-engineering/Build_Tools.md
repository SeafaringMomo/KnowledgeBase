# 构建工具

## 为什么需要构建工具

现代前端项目通常需要：

- 代码转换（TypeScript、JSX、Vue SFC）
- 资源打包与压缩
- 代码分割与懒加载
- 开发服务器与热更新（HMR）
- CSS 预处理与 PostCSS 后处理

---

## Webpack

Webpack 是功能最全面的打包器，生态丰富，适合大型复杂项目。

```js
// webpack.config.js
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].[contenthash:8].js',
    clean: true,
  },
  module: {
    rules: [
      { test: /\.js$/, use: 'babel-loader' },
      { test: /\.css$/, use: ['style-loader', 'css-loader', 'postcss-loader'] },
      { test: /\.(png|svg|jpg)$/, type: 'asset/resource' },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({ template: './public/index.html' }),
  ],
  devServer: {
    hot: true,
    open: true,
  },
};
```

---

## Vite

Vite 基于原生 ESM，开发时启动极快，生产构建使用 Rollup。

```js
// vite.config.js
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  server: {
    port: 3000,
    proxy: {
      '/api': 'http://localhost:8080',
    },
  },
  build: {
    outDir: 'dist',
    sourcemap: true,
  },
});
```

---

## 工具对比

| 特性 | Webpack | Vite | esbuild | Rollup |
|------|---------|------|---------|--------|
| 启动速度 | 较慢 | 极快 | 极快 | 快 |
| 配置复杂度 | 高 | 低 | 低 | 中 |
| 生态成熟度 | 非常成熟 | 快速增长 | 增长中 | 成熟 |
| 生产输出 | 优秀 | 优秀（Rollup） | 快但需调优 | 最佳（库） |
| 适用场景 | 大型项目 | Vue/React 现代项目 | 极速编译 | 库/SDK |

---

*Related: [[06-engineering/00-Index|工程化]], [[06-engineering/Package_Managers|包管理器]]*
