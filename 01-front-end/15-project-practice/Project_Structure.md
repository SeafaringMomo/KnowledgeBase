# 项目结构

## React 项目结构

```
src/
├── components/       # 通用组件
│   ├── Button/
│   │   ├── Button.tsx
│   │   ├── Button.module.css
│   │   └── index.ts
│   └── ...
├── features/         # 业务模块
│   ├── auth/
│   │   ├── components/
│   │   ├── hooks/
│   │   ├── api/
│   │   └── index.ts
│   └── ...
├── layouts/          # 布局组件
├── pages/            # 页面（路由）
├── hooks/            # 通用 Hooks
├── utils/            # 工具函数
├── constants/        # 常量
├── types/            # 类型定义
├── api/              # API 封装
├── stores/           # 状态管理
└── styles/           # 全局样式
```

---

## Vue 项目结构

```
src/
├── components/       # 通用组件
├── views/            # 页面
├── router/           # 路由配置
├── stores/           # Pinia 状态管理
├── composables/      # 组合式函数
├── utils/            # 工具函数
├── api/              # API 封装
└── assets/           # 静态资源
```

---

## 目录命名规范

- 小写 + 连字符：`user-profile`
- 组件目录大驼峰：`UserProfile`
- 索引文件：`index.ts` / `index.vue`
- 样式文件与组件同名

---

*Related: [[15-project-practice/00-Index|项目实践]], [[15-project-practice/Best_Practices|最佳实践]]*
