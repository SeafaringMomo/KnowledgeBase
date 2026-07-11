---
title: 项目实践章节索引
tags: [index, 前端, 项目实践]
updated: "2026-06-19"
---

# 项目实践知识体系

## 📋 目录

- [[15-project-practice/Project_Structure|项目结构]] — React/Vue 目录组织
- [[15-project-practice/Code_Standards|代码规范]] — 命名、注释、提交规范
- [[15-project-practice/Best_Practices|最佳实践]] — 性能、安全、可维护性

---

## 组件设计原则

### 单一职责

```jsx
// ❌ 一个组件做太多事
function UserProfile({ user, posts, comments, onFollow, onUnfollow }) {
  return (
    <div>
      <UserInfo user={user} />
      <FollowButton 
        isFollowing={user.isFollowing} 
        onFollow={onFollow}
        onUnfollow={onUnfollow}
      />
      <PostList posts={posts} />
      <CommentList comments={comments} />
    </div>
  );
}

// ✅ 拆分为多个组件
function UserProfile({ userId }) {
  const { user } = useUser(userId);
  return (
    <div>
      <UserHeader user={user} />
      <UserContent userId={userId} />
    </div>
  );
}
```

### 开放封闭

```jsx
// 组件应该对扩展开放，对修改封闭
// 错误示例：每次加新类型都要改组件
function Badge({ type }) {
  if (type === "success") return <span className="success">✓</span>;
  if (type === "error") return <span className="error">✗</span>;
  // ...
}

// 正确示例：传入 render props 或 children
function Badge({ children }) {
  return <span className="badge">{children}</span>;
}

// 使用
<Badge type="success">✓ 成功</Badge>
<Badge type="error">✗ 失败</Badge>
```

### 接口隔离

```typescript
// ❌ 大而全的 props
interface ButtonProps {
  label: string;
  icon?: string;
  onClick: () => void;
  loading?: boolean;
  disabled?: boolean;
  tooltip?: string;
  // ... 几十个 props
}

// ✅ 按需传入
interface ButtonProps {
  children: React.ReactNode;
  onClick?: () => void;
  disabled?: boolean;
}
```

---

## 目录结构

### React 项目

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
├── pages/            # 页面
├── hooks/            # 通用 Hooks
├── utils/            # 工具函数
├── constants/        # 常量
├── types/            # 类型定义
├── api/              # API 封装
└── stores/           # 状态管理
```

### Vue 项目

```
src/
├── components/
├── views/
├── router/
├── stores/           # Pinia
├── composables/      # Composition API 逻辑复用
├── utils/
├── api/
└── assets/
```

---

## API 设计

### RESTful 规范

```
GET    /users        # 获取用户列表
GET    /users/:id    # 获取单个用户
POST   /users        # 创建用户
PUT    /users/:id    # 更新用户（整体）
PATCH  /users/:id    # 部分更新
DELETE /users/:id    # 删除用户
```

### 统一响应格式

```json
{
  "code": 0,
  "message": "success",
  "data": {}
}

{
  "code": 400,
  "message": "参数错误",
  "data": null
}
```

---

## 代码审查清单

- [ ] 代码是否可读、命名清晰
- [ ] 是否有多余的重复代码
- [ ] 是否有潜在的性能问题
- [ ] 是否处理了边界情况
- [ ] 是否有安全漏洞（XSS、注入等）
- [ ] 单元测试覆盖率
- [ ] 是否符合代码规范
- [ ] 日志是否完善

---

## Git Commit 规范

```
<type>(<scope>): <subject>

feat(auth): add login with OAuth
fix(api): handle network timeout
docs: update README
style(ui): adjust button padding
refactor(store): simplify reducer logic
test: add unit tests for UserProfile
chore(deps): upgrade react to 18.2
perf(list): use virtual scroll for large lists
```

---

*Last updated: 2026-06-19*

*Related: [[00-Index|前端知识体系]]*
