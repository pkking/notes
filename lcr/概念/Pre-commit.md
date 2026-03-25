---
aliases: [pre-commit]
tags: [Git, 代码规范, 自动化]
---
# Pre-commit

Pre-commit 是一种代码提交前的自动化钩子机制，通常在执行 `git commit` 操作时触发。它被广泛用于在代码正式进入版本控制库之前进行静态代码分析（Linting）、代码格式化、类型检查（如 Mypy）以及安全性扫描等，从而确保提交的代码符合项目规范，减少后期集成过程中的质量风险。
