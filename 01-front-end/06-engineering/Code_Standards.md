# 代码规范

## ESLint

ESLint 用于检查 JavaScript/TypeScript 代码中的潜在问题与风格。

```js
// .eslintrc.js
module.exports = {
  env: { browser: true, es2022: true, node: true },
  extends: ['eslint:recommended', 'plugin:react/recommended', 'plugin:@typescript-eslint/recommended'],
  parser: '@typescript-eslint/parser',
  plugins: ['@typescript-eslint', 'react', 'react-hooks'],
  rules: {
    'no-unused-vars': 'error',
    'no-console': 'warn',
    'react/prop-types': 'off',
  },
};
```

```bash
npx eslint .
npx eslint . --fix
```

---

## Prettier

Prettier 是代码格式化工具，统一团队代码风格。

```json
// .prettierrc
{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "es5",
  "printWidth": 100,
  "tabWidth": 2,
  "arrowParens": "avoid"
}
```

```bash
npx prettier --write .
npx prettier --check .
```

---

## Husky + lint-staged

在提交前自动运行 lint/format，保证代码质量。

```bash
npm install -D husky lint-staged
npx husky init
npx husky add .husky/pre-commit "npx lint-staged"
```

```json
// package.json
{
  "lint-staged": {
    "*.{js,ts,jsx,tsx}": ["eslint --fix", "prettier --write"],
    "*.{css,scss,md}": ["prettier --write"]
  }
}
```

---

## 代码审查清单

- [ ] 命名清晰、语义化
- [ ] 无 console.log / debugger 残留
- [ ] 无未使用变量
- [ ] 错误与边界情况已处理
- [ ] 复杂逻辑有注释说明

---

*Related: [[06-engineering/00-Index|工程化]], [[06-engineering/Build_Tools|构建工具]]*
