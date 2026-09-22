---
name: github-publish-project
description: Publish a local project to GitHub by creating a repository, preparing a README, pushing code, and creating a tagged release. Use when asked to upload or publish a project to GitHub, create a public or private repository, add an introductory README, or publish an initial release; exclude routine commits and pull requests unless publishing is included.
metadata:
  short-description: Publish a project to GitHub
---

# GitHub Project Publisher

Turn a local project into a verified GitHub repository with a useful README and an initial release.

## Required Inputs

Resolve or confirm these values before the first external mutation:

- Local project root and intended files.
- GitHub owner and repository name.
- Repository visibility: public or private.
- Release tag and title, when a release was requested.
- Optional release binaries. Default to GitHub-generated source archives only.

Confirm only unresolved choices. If the user already explicitly supplied a value, do not ask again. Creating a public repository or publishing a release is external and difficult to hide; verify the exact target and visibility.

## Workflow

1. Inspect the project with `git status`, `git log`, `git remote -v`, and the file tree. Check for secrets, credentials, local databases, generated outputs, and large binaries before staging anything.
2. Ensure the intended source files are tracked and committed. Preserve existing history and never use force push. Do not commit `.env` files, tokens, private keys, dependency folders, caches, or build output.
3. Add or improve a README when missing or incomplete. Keep it factual and include the project name, one-line purpose, key features, usage instructions, and important data or privacy caveats. Do not invent deployment commands, licenses, compatibility claims, or screenshots.
4. Prefer an authenticated GitHub CLI when available:
   - Verify with `gh auth status`.
   - For a new empty repository, use `gh repo create <owner>/<repo> --public|--private --source . --remote origin --push`.
   - If the repository exists and is empty, configure `origin` and push the intended branch with `git push -u origin <branch>`.
   - If it contains commits, fetch and inspect first. Stop on divergence and ask how to proceed; never overwrite it.
5. When `gh` is unavailable or unauthenticated, read [references/browser-fallback.md](references/browser-fallback.md) and use the Codex in-app browser workflow.
6. Create the release only after the repository and default branch are verified:
   - Prefer `gh release create <tag> --target <branch> --title <title> --notes-file <file>`.
   - Use semantic tags such as `v1.0.0`. Do not reuse or delete an existing tag without explicit approval.
   - Attach binaries only when the user supplied or requested them. GitHub-generated source archives are enabled automatically.
   - Mark the release as a pre-release when requested or when the version clearly indicates production readiness is not intended.
7. Verify the repository page, rendered README, branch contents, release page, tag, and listed assets. Report the repository URL, release URL, commit, tag, and asset types.

## Safety and Failure Handling

- Treat credentials as opaque. Never print, paste, commit, or ask the user to send a token or password in chat.
- Let the user complete authentication in the browser or credential manager themselves.
- Never force push, delete history, overwrite a non-empty repository, or replace an existing release without explicit confirmation.
- Stop when repository ownership, visibility, target branch, or release tag differs from the authorized values.
- Prefer a minimal README for simple projects. Avoid boilerplate sections with no real content.

## Validation Checklist

- Repository visibility, owner, and name are correct.
- `.gitignore` excludes local secrets and generated artifacts.
- README renders correctly and its commands are true.
- The default branch contains the expected files and latest intended commit.
- The release tag points to the intended commit.
- Source archives are present; requested binaries are attached and downloadable.
## 中文说明

`github-publish-project` 用于把一个本地项目安全、完整地发布到 GitHub。它会先检查项目状态和敏感文件，再整理 README、初始化或连接 Git 仓库、创建 GitHub 仓库、推送默认分支，并在明确要求时创建 Release。

### 适用场景

- 将本地项目首次上传到 GitHub。
- 为现有本地目录补充 README、`.gitignore` 和 Git 仓库配置。
- 创建公开或私有 GitHub 仓库，并推送已确认的代码。
- 在代码和默认分支验证完成后创建版本标签与 Release。
- 在 GitHub CLI 不可用时，改用浏览器完成仓库创建和文件上传。

### 使用前需要确认

- 本地项目根目录，以及需要发布的文件范围。
- GitHub 所有者、仓库名和仓库可见性（Public 或 Private）。
- 需要发布版本时，确认标签、标题和说明；默认使用语义化标签，如 `v1.0.0`。
- 需要附加安装包或其他二进制文件时，明确文件名和目标 Release。

如果用户已经提供上述信息，不重复询问；只确认仍不明确的选项。

### 主要流程

1. 检查 `git status`、提交历史、远程地址和完整文件树。
2. 排查密钥、令牌、私钥、`.env`、本地数据库、缓存、依赖目录、构建产物和大体积二进制文件。
3. 确保目标源码已纳入 Git，并提交到合适的本地分支；禁止强制推送。
4. 补充或完善 README，只写能够验证的名称、用途、功能、用法、数据和隐私说明。
5. 优先使用已登录的 GitHub CLI。创建空仓库并推送时，不得覆盖已有提交；发现远程历史分叉时先停止并征求处理方式。
6. GitHub CLI 不可用时，使用仓库内的浏览器回退说明完成创建、上传和验证。
7. 仅在用户明确要求时创建 Release，并验证标签指向的提交、Release 页面和源码归档。
8. 最后检查仓库可见性、默认分支内容、README 渲染、Release 标签和附件是否与授权目标一致。

### 安全边界

- 绝不输出、提交或索要密码、访问令牌和私钥。
- 不强制推送，不删除历史，不覆盖非空仓库，不擅自替换已有 Release。
- 创建公开仓库、推送代码或发布 Release 前，目标所有者和可见性必须明确。
- 只提交用户希望发布的项目文件，保留用户已有提交历史和远程配置。

### 完成标准

- GitHub 所有者、仓库名和可见性与确认值一致。
- `.gitignore` 能排除本地敏感信息和生成文件。
- README 内容真实且命令可执行。
- 默认分支包含预期文件，并指向最新目标提交。
- 如创建了 Release，标签、说明、源码归档和用户要求的附件均已验证。

