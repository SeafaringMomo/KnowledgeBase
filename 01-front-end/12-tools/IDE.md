# IDE 与编辑器

## VS Code

最流行的前端编辑器，插件生态丰富。

### 推荐插件

| 插件 | 作用 |
|------|------|
| ESLint | 代码检查 |
| Prettier | 代码格式化 |
| Tailwind CSS IntelliSense | Tailwind 提示 |
| GitLens | Git 增强 |
| Thunder Client | API 测试 |
| Error Lens | 行内错误提示 |

### 常用快捷键

| 快捷键 | 作用 |
|--------|------|
| `Ctrl+P` | 快速打开文件 |
| `Ctrl+Shift+P` | 命令面板 |
| `Ctrl+Shift+F` | 全局搜索 |
| `F12` | 跳转到定义 |
| `Ctrl+\`` | 打开终端 |
| `Ctrl+D` | 选中下一个相同词 |

---

## 调试配置

```json
// .vscode/launch.json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "chrome",
      "request": "launch",
      "name": "Launch Chrome",
      "url": "http://localhost:3000",
      "webRoot": "${workspaceFolder}"
    }
  ]
}
```

---

*Related: [[12-tools/00-Index|开发工具]]*
