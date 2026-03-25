`git worktree` 是 Git 在 2.5 版本引入的一项强大功能。简单来说，它允许你在**同一个本地代码库中同时检出（Checkout）多个分支**，并将它们放在不同的文件夹中。

### 1. 核心矛盾：为什么需要 `git-worktree`？

在没有 `worktree` 之前，当你正在 `feature` 分支开发，突然需要去 `main` 分支修复一个紧急 Bug 时，你通常有两种选择：

1. **`git stash`**：把当前改动存起来。缺点是切换回来时需要重新应用，且容易忘记 stash 的内容。
    
2. **`git clone`**：克隆一个新副本。缺点是浪费磁盘空间，且两个副本之间的 Git 历史不共享（需要重新 fetch）。
    

**`git-worktree` 解决了“上下文切换”的成本问题**：它让你在物理上拥有多个工作目录，但在逻辑上共享同一个 `.git` 数据库。

---

### 2. 目标分解：掌握 `git-worktree` 的路径

为了让你快速上手并应用到生产中，我们将目标分解为以下三个阶段：

#### 阶段一：建立模型（理解原理）

- **主工作区 (Main Worktree)**：当你运行 `git clone` 或 `git init` 时创建的初始目录。
    
- **链接工作区 (Linked Worktree)**：通过 `git worktree add` 创建的额外目录。它们共享 `.git` 文件夹中的对象数据库。
    

#### 阶段二：核心指令（实操技能）

|**目标**|**命令**|
|---|---|
|**新增工作区**|`git worktree add <路径> <分支名>`|
|**查看所有工作区**|`git worktree list`|
|**移除工作区**|`git worktree remove <路径>`|
|**清理无效路径**|`git worktree prune`|

#### 阶段三：高级场景应用

- **长时任务并行**：在一个工作区跑耗时数小时的测试/编译，在另一个工作区继续写代码。
    
- **对比不同版本**：直接在编辑器里打开两个文件夹，对比不同分支的代码逻辑。
    

---

### 3. `git-worktree` vs 其他工具

下表展示了在处理多任务切换时，不同工具的优劣：

|**维度**|**git stash**|**git clone (多副本)**|**git worktree**|
|---|---|---|---|
|**磁盘占用**|极低|高|中（仅源码，不重复 .git）|
|**上下文保留**|差（需清空工作区）|好（独立目录）|**好（独立目录）**|
|**数据共享**|共享所有本地提交|不共享（需 push/pull）|**完全共享**|
|**适用场景**|几分钟的临时切换|彻底隔离的项目环境|**频繁的分支间切换/对比**|

---

### 4. 注意事项与限制

- **不能重复检出同一分支**：为了防止冲突，Git 不允许在两个不同的工作区同时检出同一个分支。
    
- **构建系统影响**：某些构建工具（如 C++ 的 CMake 或 Rust 的 Cargo）可能会在工作区产生大量中间文件。虽然 `.git` 共享了，但这些构建产物在不同路径下通常无法共享。
    

> “`git-worktree` 改变了开发者的工作流模型，它通过消除‘暂存再恢复’的繁琐步骤，让多任务并行变得非常自然。” —— _Sanyam Arya_

---

### 5. 参考文献与资源

1. **Git 官方文档**: [git-worktree Documentation](https://git-scm.com/docs/git-worktree)
2. **Sanyam Arya (2025)**: [Beyond Stashing: Why Git Worktree is Your New Workflow Superpower](https://www.sanyamarya.com/blog/git-worktree-vs-stash-better-workflow/)
3. **Maxence Poutord (2022)**: [Simpler context switching with Git worktrees](https://www.maxpou.fr/blog/git-worktrees/)
4. **Stack Overflow**: [What would I use git-worktree for?](https://stackoverflow.com/questions/31935776/what-would-i-use-git-worktree-for)
