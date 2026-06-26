---
name: github-ops
description: >
  全面管理 GitHub 远程仓库。当用户要求操作 Issue、PR、
  Actions、Release，或提到"GitHub"、"远程仓库"、"Issue"、
  "Pull Request"、"合并"、"CI"等关键词时自动启用。
---

# GitHub 远程仓库操作技能

使用 GitHub CLI（gh）执行所有操作。
涉及写操作（创建、合并、删除）时，必须先向用户确认。

## Issue 管理

- 创建 Issue：`gh issue create --title "标题" --body "描述"`
- 列出未关闭的 Issue：`gh issue list`
- 查看某个 Issue 详情：`gh issue view <编号>`
- 关闭 Issue：`gh issue close <编号>`（⚠️ 需确认）
- 重新打开：`gh issue reopen <编号>`
- 添加标签：`gh issue edit <编号> --add-label "bug,urgent"`
- 分配负责人：`gh issue edit <编号> --add-assignee "@me"`
- 搜索 Issue：`gh issue list --search "关键词"`

## Pull Request 管理

- 创建 PR：`gh pr create --title "标题" --body "描述" --base main`
- 列出所有 PR：`gh pr list`
- 查看 PR 的代码变更：`gh pr diff <编号>`
- 查看 PR 详情：`gh pr view <编号>`
- 合并 PR：`gh pr merge <编号> --merge`（⚠️ 需二次确认）
- 审查并批准：`gh pr review <编号> --approve`（⚠️ 需确认）
- 请求修改：`gh pr review <编号> --request-changes --body "原因"`
- 在 PR 上留言：`gh pr comment <编号> --body "评论内容"`

## GitHub Actions（CI/CD）

- 查看最近的工作流运行：`gh run list`
- 查看某次运行的详情：`gh run view <run-id>`
- 查看运行日志：`gh run view <run-id> --log`
- 手动触发工作流：`gh workflow run <workflow-name>`
- 重新运行失败的任务：`gh run rerun <run-id> --failed`
- 列出所有工作流：`gh workflow list`

## Release 管理

- 创建 Release：`gh release create <tag> --title "标题" --notes "说明"`
- 列出所有 Release：`gh release list`
- 下载 Release 附件：`gh release download <tag>`
- 删除 Release：`gh release delete <tag>`（⚠️ 需确认）

## 仓库信息

- 查看仓库概览：`gh repo view`
- 在浏览器中打开仓库：`gh repo view --web`
- 克隆仓库：`gh repo clone <owner/repo>`
- Fork 仓库：`gh repo fork <owner/repo>`

## 安全规则（必须严格遵守）

1. 所有**写操作**执行前必须向用户确认，明确说出即将执行的命令
2. **绝不**自动合并到 main/master 分支，必须等用户明确同意
3. **绝不**执行 force push（`--force`）
4. 发现 .env、密钥、token 等敏感文件时**立即停止**并警告
5. 操作完成后报告结果，附上可点击的 GitHub 链接
6. 遇到权限不足时，提示用户运行 `gh auth status` 检查认证状态