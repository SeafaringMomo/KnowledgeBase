# CI/CD

## 持续集成（CI）

每次提交代码时自动执行：

- 安装依赖
- 代码检查（Lint）
- 单元测试 / E2E 测试
- 构建产物

## 持续部署（CD）

将通过 CI 的代码自动部署到测试或生产环境。

---

## GitHub Actions 示例

```yaml
# .github/workflows/deploy.yml
name: Deploy

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'npm'

      - run: npm ci
      - run: npm run lint
      - run: npm run test
      - run: npm run build

      - name: Deploy
        if: github.ref == 'refs/heads/main'
        run: |
          # 部署到服务器或 CDN
          echo "Deploying..."
```

---

## GitLab CI 示例

```yaml
# .gitlab-ci.yml
stages:
  - install
  - lint
  - test
  - build
  - deploy

install:
  stage: install
  script:
    - npm ci
  cache:
    paths:
      - node_modules/

lint:
  stage: lint
  script:
    - npm run lint

test:
  stage: test
  script:
    - npm run test

build:
  stage: build
  script:
    - npm run build
  artifacts:
    paths:
      - dist/

deploy:
  stage: deploy
  script:
    - echo "Deploy to production"
  only:
    - main
```

---

## 前端部署策略

| 策略 | 说明 |
|------|------|
| 静态托管 | Vercel、Netlify、GitHub Pages、Cloudflare Pages |
| CDN 加速 | 将构建产物上传至 CDN，配置缓存策略 |
| Docker | 使用 Nginx 镜像部署 |
| 蓝绿部署 | 同时部署两套环境，流量无缝切换 |

---

*Related: [[06-engineering/00-Index|工程化]], [[06-engineering/Build_Tools|构建工具]]*
