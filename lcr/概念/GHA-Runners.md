---
aliases: [GHA Runners, GitHub Actions Runners, 自托管运行器]
tags: [DevOps, GitHub, 持续集成, 基础设施]
---
# GHA-Runners

**相关笔记**: [[镜像同步]]

## 概念定义
**GHA-Runners**（GitHub Actions Runners）是执行 GitHub Actions 工作流（Workflows）中的作业（Jobs）的应用。除了 GitHub 托管的公共运行器，企业常通过**自托管运行器（Self-hosted Runners）**来控制硬件资源、安全网络环境。

## Ascend GHA Runners 场景
在昇腾（Ascend）硬件的软件工程实践中，由于需要特定的 NPU（昇腾 AI 处理器）驱动和硬件环境，必须通过 GHA-Runners 的形式将昇腾服务器（如 Atlas 系列）接入 GitHub Actions，以便对开源项目（如 vLLM, MindIE）进行国产算力适配测试。

## 关键配套设施
1. **Sync-Tools**：用于同步必要的容器基础镜像到昇腾本地仓库，确保 CI 环境快速就绪。
2. **Sccache**：用于跨构建阶段共享编译缓存，显著缩短 C++/Python 编译时间。
3. **监控系统**：对运行器的 NPU 占用、健康状态进行实时监控。

## 项目参考
- [ascend-gha-runners](https://github.com/ascend-gha-runners)
