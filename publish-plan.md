# Engineering Knowledge Base 公开发布历史方案

> 本文件保留首次公开发布前的历史方案，不是当前操作指南。远程仓库已建立，当前以实际 Git remote 和 GitHub 仓库状态为准。

## 建议设置

| 项目 | 建议 |
| --- | --- |
| 仓库名称 | `engineering-knowledge-base` |
| 仓库描述 | 面向工科学生的本地工程资料管理工具：从 PDF 导入、页面复核到检索、证据摘录与本地备份 |
| 可见性 | 复核通过后设为 **Public** |
| 默认分支 | `main` |
| License | 暂不添加；待明确源码公开范围和许可方式后再决定 |
| 计划公开地址 | `https://github.com/JZ-05T68/engineering-knowledge-base` |

## 历史发布前确认项

GitHub login 已确认为 `JZ-05T68`，数字 ID 已确认为 `271708767`。下列命令是首次公开前的历史记录，现在不应重复执行。

1. 发布前仍应使用 GitHub CLI 只读复核当前登录用户：

   ```powershell
   gh auth status
   gh api user --jq '{login: .login, id: .id}'
   ```

2. 核验仓库级身份使用已确认的 GitHub noreply 邮箱，且没有修改全局 Git 配置：

   ```powershell
   git config --local --get user.name
   git config --local --get user.email
   ```

3. 再次复核 `verification-report.md`、暂存清单和最终提交内容。

## 历史首次发布命令

以下命令只应在完成独立复核并明确授权后执行：

```powershell
git branch -M main
gh repo create JZ-05T68/engineering-knowledge-base --public --source . --remote origin
git push -u origin main
```

以上命令只能在用户完成独立复核并另行明确授权后执行。

## GitHub Pages 建议

建议在仓库公开并完成链接复核后启用 GitHub Pages：

- 来源分支：`main`
- 来源目录：`/docs`
- 入口文件：`docs/index.html`

可以在仓库 Settings → Pages 中选择 “Deploy from a branch”，再选择 `main` 和 `/docs`。本轮只制作 Pages-ready 页面，不启用 Pages。

## 公开后验证

1. 在未登录浏览器窗口打开仓库首页，确认 README 中文、标题层级和状态说明正常；
2. 逐一打开 7 张截图，确认无 404、无变形且没有私密内容；
3. 打开 `docs/project-introduction.pdf`，确认 5 页均可预览和下载；
4. 启用 Pages 后打开站点首页，检查桌面与手机宽度；
5. 点击页面内 PDF 链接，确认使用仓库内相对路径；
6. 在浏览器网络面板中确认静态页没有第三方请求；
7. 再次确认仓库未出现源码、数据库、备份、日志、真实资料或 Git 历史之外的内部文件。

## 发现错误时的处置

优先立即把仓库改为 Private，停止公开访问后再修复：

```powershell
gh repo edit JZ-05T68/engineering-knowledge-base --visibility private --accept-visibility-change-consequences
```

如果内容不应保留且已确认仓库目标无误，可在 GitHub 网页中删除仓库，或在再次核对完整仓库名后使用：

```powershell
gh repo delete JZ-05T68/engineering-knowledge-base --yes
```

删除不可恢复，应只在明确确认后执行。若只是单个文件有误，先改为 Private，再在本地修正、重新审计并提交，不要直接在公开页面临时编辑。
