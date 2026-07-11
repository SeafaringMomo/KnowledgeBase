# 包管理器

## npm / yarn / pnpm 对比

| 特性 | npm | yarn | pnpm |
|------|-----|------|------|
| 安装速度 | 中 | 快 | 很快（硬链接） |
| 磁盘占用 | 大 | 大 | 小（内容寻址存储） |
| 锁文件 | `package-lock.json` | `yarn.lock` | `pnpm-lock.yaml` |
| 工作区 | 支持 | 支持 | 支持 |
| 离线模式 | 有限 | 好 | 好 |

---

## 常用命令对照

| 操作 | npm | yarn | pnpm |
|------|-----|------|------|
| 初始化 | `npm init -y` | `yarn init -y` | `pnpm init` |
| 安装全部依赖 | `npm install` | `yarn install` | `pnpm install` |
| 添加生产依赖 | `npm install pkg` | `yarn add pkg` | `pnpm add pkg` |
| 添加开发依赖 | `npm install -D pkg` | `yarn add -D pkg` | `pnpm add -D pkg` |
| 全局安装 | `npm install -g pkg` | `yarn global add pkg` | `pnpm add -g pkg` |
| 移除依赖 | `npm uninstall pkg` | `yarn remove pkg` | `pnpm remove pkg` |
| 运行脚本 | `npm run dev` | `yarn dev` | `pnpm dev` |
| 清理缓存 | `npm cache clean --force` | `yarn cache clean` | `pnpm store prune` |

---

## 镜像源配置

```bash
# npm
npm config set registry https://registry.npmmirror.com
npm config get registry
npm config set registry https://registry.npmjs.org  # 恢复官方源

# yarn
yarn config set registry https://registry.npmmirror.com
yarn config get registry
yarn config set registry https://registry.yarnpkg.com  # 恢复官方源
yarn config delete registry                            # 删除自定义源

# pnpm
pnpm config set registry https://registry.npmmirror.com
pnpm config get registry
```

---

## package.json 关键字段

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "lint": "eslint .",
    "test": "vitest"
  },
  "dependencies": {
    "react": "^18.2.0"
  },
  "devDependencies": {
    "vite": "^5.0.0"
  },
  "engines": {
    "node": ">=18.0.0"
  }
}
```

### 版本号规则

| 符号 | 含义 |
|------|------|
| `1.2.3` | 固定版本 |
| `^1.2.3` | 兼容 1.x.x（不小于 1.2.3） |
| `~1.2.3` | 兼容 1.2.x |
| `*` | 最新版本 |
| `>`, `<`, `>=`, `<=` | 范围 |

---

## 工作区（Monorepo）

```json
// package.json
{
  "workspaces": ["packages/*"]
}
```

```yaml
# pnpm-workspace.yaml
packages:
  - 'packages/*'
```

---

*Related: [[06-engineering/00-Index|工程化]], [[06-engineering/Build_Tools|构建工具]]*
