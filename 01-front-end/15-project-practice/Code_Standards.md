# 项目代码规范

## 命名规范

| 类型 | 规范 | 示例 |
|------|------|------|
| 变量/函数 | camelCase | `getUserInfo` |
| 类/组件 | PascalCase | `UserProfile` |
| 常量 | SCREAMING_SNAKE_CASE | `MAX_SIZE` |
| 文件/目录 | kebab-case | `user-profile` |
| CSS 类名 | BEM / kebab-case | `.user-profile__avatar` |

---

## 注释规范

```javascript
// 单行注释：说明意图
const count = 0; // 当前计数

/**
 * 计算两数之和
 * @param {number} a
 * @param {number} b
 * @returns {number}
 */
function add(a, b) {
  return a + b;
}
```

---

## 文件组织

- 一个文件只负责一种职责
- 组件文件不超过 300 行，必要时拆分
- 公共逻辑提取到 hooks/utils

---

## Git 提交规范

```
feat: 新功能
fix: 修复
refactor: 重构
docs: 文档
style: 格式（不影响代码逻辑）
test: 测试
chore: 构建/工具
perf: 性能优化
```

---

*Related: [[15-project-practice/00-Index|项目实践]], [[12-tools/Git|Git]]*
