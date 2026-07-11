# 前端最佳实践

## 性能

- 压缩图片并使用现代格式（WebP/AVIF）
- 路由级代码分割
- 优先使用 transform/opacity 做动画
- 减少重排与重绘
- 使用缓存策略

## 安全

- 永远不要信任用户输入
- 输出时进行转义
- 配置 CSP
- 使用 HTTPS 与 Secure/HttpOnly/SameSite Cookie
- 及时更新依赖

## 可维护性

- 组件单一职责
- 类型安全（TypeScript）
- 写单元测试与 E2E 测试
- 统一代码风格（ESLint + Prettier）
- 文档与注释

## 可访问性

- 使用语义化 HTML
- 图片加 alt
- 表单关联 label
- 支持键盘导航
- 检查颜色对比度

---

*Related: [[15-project-practice/00-Index|项目实践]], [[00-Index|前端知识体系]]*
