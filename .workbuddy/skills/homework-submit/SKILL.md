---
name: homework-submit
description: One-sentence homework submission for the bigdata-ai-course repo. Use this skill whenever the user says "提交作业"、"交作业"、"commit 并 push"、"推送作业"、"同步到 GitHub" — it stages all changes in the course repo, creates a commit with the course commit-message convention, pushes to origin main, and verifies the remote actually received it. Handles the local proxy quirk (git push to github.com may fail with SSL/502) with retry + fallback strategies. NOT for creating homework content (files must already exist) and NOT for force-push or history rewriting.
agent_created: true
---

# Homework Submit — 作业一键提交 Skill

## Overview

把「git add → commit → push → 验证远程」压缩成一句话。用户说"提交作业"即可触发。全部步骤在本仓库内完成，不需要用户打开任何图形界面。

## Repo Facts（每次执行前先核对，路径若变化以实际为准）

- **仓库根目录**：`C:\Users\Huawei\WorkBuddy\2026-09-03-11-20-24\bigdata-ai-course`
- **远程仓库**：`https://github.com/zhiji-bigdate/bigdata-ai-course.git`（分支 `main`）
- **Git 身份**：zhiji-bigdate / 3918673632@qq.com（已写入全局配置，无需重复设置）
- **gh CLI**：已登录 `zhiji-bigdate`（token 权限 repo），`gh api` 走 api.github.com，可用作验证与兜底
- **网络特性**：本机代理 `http://127.0.0.1:1674`。`gh api`（api.github.com）畅通；`git` 对 github.com 的 HTTPS 传输曾出现 `SSL unexpected eof` / `CONNECT tunnel failed, 502`，多为临时性，重试即可

## Steps（按顺序执行）

1. **检查状态**：`cd` 到仓库根目录，运行 `git status --short` 和 `git log --oneline -3`，向用户口头确认这次要提交哪些内容。
2. **暂存**：`git add -A`（用户只点名部分文件时只 add 点名的文件）。
3. **提交**：commit message 用课程约定格式：
   `完成 <日期> <作业/内容名>：<一句话说明>`
   例：`完成 0917 课堂作业：创建并运行 py 与 ipynb 文件`
4. **推送**：`git push origin main`。
   - 失败且报 SSL/502 → 等几秒重试，最多 3 次；
   - 仍失败 → 试 `git -c http.proxy=http://127.0.0.1:1674 -c https.proxy=http://127.0.0.1:1674 push origin main`；
   - 仍失败 → 检查本机是否有其他代理端口在监听（如 7890/7897/10808），换端口试；
   - 全部失败 → 停下来告诉用户"推送通道暂时不通，本地提交已保存"，不要硬来。
5. **验证**：`gh api "repos/zhiji-bigdate/bigdata-ai-course/commits?per_page=1" --jq '.[0].sha[0:7]'`，结果应等于本地 `git rev-parse --short HEAD`。一致才算提交成功。

## Rules

- **禁止 `--force`**：除非用户明确要求并理解后果。
- 本地与远程历史已于 2026-09-24 对齐；若推送报 non-fast-forward，先 `git pull --rebase origin main` 再推，不要 reset --hard。
- 提交前不要动 `.workbuddy/`、`notes/` 里与本次作业无关的文件。
- 遇到二进制大文件（视频/图片 >50MB）先提醒用户 GitHub 有 100MB 硬限制、50MB 起警告。
