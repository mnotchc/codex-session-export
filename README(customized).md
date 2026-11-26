# 二次开发说明

[README.md](README.md) 随上游仓库更新。本文件记录本地定制，供以后合并上游更新时核对。定制尽量集中在对应功能处，每次增减或调整定制都同步更新这里。

## 当前定制

| 定制 | 修改位置与行为 |
| --- | --- |
| 分段历史导出 | [codex-md.py](codex-md.py) 的 `load_rollout_history()` 支持用日志文件名末尾的分段 ID 查找上一段历史，兼容已有的会话 ID 引用。 |
| 会话列表合并 | `scan_sessions()` 根据历史引用合并同一条会话的分段，只显示最新分段。旧分段 ID 仍可用于搜索。 |
| Markdown 去除图片 | `_strip_export_images()` 在导出时清理对话和工具输出中的图片内容，包括图片 Base64。完整和精简模式都应用这项规则。 |

[test_codex_md.py](test_codex_md.py) 覆盖分段历史串联、列表合并和图片清理。

## 同步上游

1. 在项目根目录运行 `git status --short --branch`。如果有正在修改的内容，先本地提交或用 `git stash` 临时保存。
2. 依次运行 `git switch main`、`git fetch upstream` 和 `git merge upstream/main`。
3. 合并完成后，如果第 1 步用了 `git stash`，运行 `git stash pop` 恢复本地改动。
4. 如果合并或恢复改动时发生冲突，按上表核对涉及的功能。`README.md` 如有冲突，采用上游版本。上游已经覆盖的定制可以移除，同时更新本文件。
5. 运行 `python -m unittest discover -q`。测试压缩日志可用 Python 3.14，较早的 Python 可安装 `backports.zstd`。
