# Git

## 常用命令

```bash
# 基础操作
git clone <url>
git status
git add .
git commit -m "feat: add login"
git push origin main
git pull origin main

# 分支
git branch feature/login
git checkout -b feature/login
git merge feature/login
git rebase main

# 撤销
git checkout -- file.txt      # 撤销工作区修改
git reset HEAD file.txt       # 撤销暂存
git revert <commit>           # 生成新的撤销提交
```

---

## 分支策略

| 策略 | 说明 |
|------|------|
| Git Flow | main + develop + feature/release/hotfix |
| GitHub Flow | main + feature，PR 合并 |
| Trunk-Based | main + 短期分支，频繁集成 |

---

## Commit 规范

```
<type>(<scope>): <subject>

feat(auth): add OAuth login
fix(api): handle timeout
refactor(store): simplify reducer
docs: update README
style: format code
test: add unit tests
chore(deps): upgrade react
perf(list): virtual scroll
```

---

*Related: [[12-tools/00-Index|开发工具]], [[06-engineering/CI_CD|CI/CD]]*
