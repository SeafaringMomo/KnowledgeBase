# 微前端

## 为什么需要微前端

- 多个团队独立开发、部署
- 技术栈无关
- 渐进式迁移老项目
- 独立发布，降低风险

---

## 实现方案

| 方案 | 特点 | 代表库 |
|------|------|--------|
| iframe | 简单隔离，通信复杂 | 原生 |
| Web Components | 原生组件化 | Lit / Stencil |
| Module Federation | 共享依赖，运行时集成 | Webpack 5 |
| qiankun | 基于 single-spa，完善生态 | 蚂蚁金服 |
| single-spa | 路由级应用聚合 | single-spa |

---

## Module Federation 示例

```js
// host/webpack.config.js
const { ModuleFederationPlugin } = require('webpack').container;

module.exports = {
  plugins: [
    new ModuleFederationPlugin({
      name: 'host',
      remotes: {
        remote: 'remote@http://localhost:3001/remoteEntry.js',
      },
      shared: { react: { singleton: true }, 'react-dom': { singleton: true } },
    }),
  ],
};
```

---

## 注意事项

- 样式隔离（Shadow DOM / CSS Modules / BEM）
- 公共依赖共享
- 路由协调
- 全局状态管理

---

*Related: [[14-new-features/00-Index|新技术]], [[06-engineering/Modularization|模块化]]*
