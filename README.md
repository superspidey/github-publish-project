# GitHub Publish Project Skill

一个面向 Codex 的 GitHub 项目发布 Skill，用于将本地项目安全地创建、检查、README 化并发布到 GitHub。

## 简介

`github-publish-project` 会优先使用已登录的 GitHub CLI；当 CLI 不可用时，则按 `references/browser-fallback.md` 使用浏览器完成操作。整个流程强调发布前检查、明确目标仓库、保留已有提交，并避免意外公开敏感文件。

## 主要能力

- 检查 Git 状态、文件树、敏感信息、生成文件和大体积二进制文件。
- 为项目补充或生成简洁、真实、可执行的 README。
- 创建公开或私有 GitHub 仓库，并推送已确认的默认分支。
- 安全处理已有仓库：发现远程分歧时停止，不覆盖已有历史。
- 在用户明确要求时创建语义化版本标签和 GitHub Release。
- 在 GitHub CLI 不可用时，提供浏览器回退流程。
- 验证仓库、README、分支、标签和 Release 附件。

## 使用方式

将此目录放到 Codex Skills 目录下，例如：

```bash
git clone https://github.com/superspidey/github-publish-project.git "$HOME/.codex/skills/github-publish-project"
```

然后在 Codex 中调用：

```text
使用 $github-publish-project，把这个项目发布到我的 GitHub 仓库。
```

Skill 在开始外部修改前，会确认或要求提供：

- 本地项目根目录和待发布文件范围。
- GitHub 所有者与仓库名。
- Public 或 Private 可见性。
- 可选：Release 标签、标题、说明和附件。

## 目录结构

```text
.
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    └── browser-fallback.md
```

`SKILL.md` 包含英文工作流与中文说明；`references/browser-fallback.md` 说明 GitHub CLI 不可用时如何通过浏览器完成创建、上传和发布。

## 安全边界

- 不提交 `.env`、令牌、私钥、依赖目录、缓存或构建产物。
- 不输出或索要密码、访问令牌和私钥。
- 不强制推送，不删除 Git 历史，不覆盖非空仓库。
- 不擅自替换已有 Release，不使用未经确认的仓库所有者和可见性。

