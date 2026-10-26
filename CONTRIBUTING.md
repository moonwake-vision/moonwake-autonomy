# 参与 Moonwake Autonomy 开发

本文规定 `moonwake-autonomy` 的最低协作要求。目标不是增加流程，而是让每次修改都能被理解、复现和安全地合入整船上位机系统。

## Fork 与远程仓库

成员 Clone 自己的 Fork，并保持以下含义不变：

```text
origin    = 个人 GitHub Fork
upstream  = moonwake-vision/moonwake-autonomy
```

普通成员把任务分支 Push 到 `origin`，再向 `upstream/main` 提交 Pull Request。不要把下载的 ZIP 文件当作协作仓库。

## 开始任务

从最新 `upstream/main` 创建短期任务分支：

```bash
git switch main
git fetch upstream
git merge --ff-only upstream/main
git push origin main
git switch -c <type>/<scope>-<description>
```

分支名称使用小写英文和连字符，例如：

```text
feat/camera-usb-capture
fix/camera-empty-frame
docs/camera-setup
test/perception-red-buoy
refactor/fusion-measurement-queue
chore/ros2-dependencies
```

不要建立长期个人 `dev`、`debug`、`final` 或笼统的 `camera` 分支。任务合并后删除任务分支。

## Commit

Commit 信息遵循 Conventional Commits：

```text
<type>(可选 scope): 简短描述
```

示例：

```text
feat(camera): publish USB camera frames
fix(camera): handle empty captured frames
docs(camera): add device setup guide
chore(repo): bootstrap ROS 2 workspace
```

提交前至少执行：

```bash
git status
git diff
git add <明确的文件路径>
git diff --staged
```

只提交与当前任务相关的修改。不要提交秘密、生成物、数据集、模型、rosbag、原始视频或无关格式化结果。

## Pull Request

普通任务 PR 的目标是 `moonwake-vision/moonwake-autonomy` 的 `main`。只有 Maintainer 明确安排多模块联调或版本冻结时，才使用 `integration/*` 或 `release/*`。

一个可评审的 PR 至少写明：

1. 修改背景与目标；
2. 主要修改；
3. 验证环境、方法与实际结果；
4. 影响范围；
5. 已知风险、限制和未完成事项。

工作尚未完成但需要讨论接口或尽早暴露风险时，可以创建 Draft PR。回应 Review 时继续向原任务分支 Push，不要重新创建 PR。

普通任务默认使用 Squash and merge。PR 标题应符合 Conventional Commits，并成为合入 `main` 后的主提交标题。

## 合入 `main` 的最低条件

- 修改范围与 PR 目标一致；
- 至少一名非作者成员完成 Review；
- 阻塞性评论已经处理，讨论已经解决；
- 完成适合该修改的构建、测试或岸上验证；
- 行为发生变化时，README、参数和接口说明同步更新；
- 不包含秘密、生成物、大文件或无关修改。

作者不得因为自己拥有 Write 权限而绕过 PR。下水验证通过后，由 Maintainer 在实际部署的 Commit 上创建 annotated `onboard-*` Tag。
