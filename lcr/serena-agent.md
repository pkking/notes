---
aliases:
  - serena
---
**Serena** 是一个由 **Oraios AI** 开发的**开源编码智能体（Coding Agent）工具包**。

在当前的 AI 开发领域，虽然大语言模型（LLM）擅长生成代码片段，但在处理复杂的真实代码库（Codebase）时，往往会面临上下文缺失、编辑不精确等问题。Serena 的核心目标就是通过提供一套底层的语义工具，将通用的 LLM 转变为能够真正理解并修改大型工程代码的高效智能体。

---

### 1. Serena 的核心能力

Serena 的设计逻辑是“让 AI 拥有程序员的眼睛和手”。它的关键技术特性包括：

- **语义检索（Semantic Retrieval）：** 传统的 AI 往往只能看到你粘贴给它的代码。Serena 能够通过符号级别的检索，帮助 LLM 在海量代码中准确找到相关的函数、类或变量定义。
    
- **LSP 集成：** 它整合了**语言服务器协议（Language Server Protocol）**。这意味着它能像现代 IDE（如 VS Code）一样，理解代码的结构、引用关系和类型信息。
    
- **精确编辑（Symbolic Editing）：** 不同于简单的文本替换，Serena 支持符号级别的插入和替换操作。这大大降低了 AI 在修改代码时破坏原有语法结构的风险。
    
- **降低 Token 成本：** 通过精准定位和检索，Serena 能够减少发送给 LLM 的无关上下文，从而节省 API 开销。
    

### 2. 为什么需要 Serena？（核心矛盾点）

在 AI 辅助开发的演进中，**“通用 LLM 能力”与“复杂工程落地”**之间存在矛盾。

Serena 抓住了这个关键点：LLM 虽然“聪明”，但它没有“工程语境”。Serena 扮演了“桥梁”的角色，为 AI 提供了一个操作代码库的标准接口（Runtime/Toolkit），使其从一个“对话者”变成一个真正的“执行者”。

### 3. 典型使用场景

- **自动化重构：** 批量修改代码库中的旧接口调用。
    
- **故障自动化修复：** 配合测试工具，自动定位并修复代码库中的漏洞或错误。
    
- **构建自研 AI Agent：** 如果你正在基于 LangGraph 或其他框架开发自己的 SaaS 运维或开发助手，Serena 可以作为其底层的“代码操作引擎”。
    

---

### 4. 引用与参考

|**来源**|**描述**|**地址**|
|---|---|---|
|**Jimmy Song (Cloud Native)**|Serena 工具包简介与智能体运行时思考|[jimmysong.io/zh/ai/serena/](https://jimmysong.io/zh/ai/serena/)|
|**Oraios AI**|开源编码智能体项目主页|[github.com/oraios/serena](https://github.com/oraios/serena) (假设地址)|

---

Serena 的工作流体现了从“基于文本的代码补全”到“基于语义的代码智能体”的转变。它的核心逻辑是利用 **LSP (Language Server Protocol)** 为大模型提供一套类似于人类开发者使用 IDE（如点击跳转、查找引用）的“工具箱”。

以下是 Serena 的普通工作流及其与现有工具结合使用的详细分析：

### 5. Serena 的标准工作流 (Workflow)

Serena 的工作流程通常分为四个关键阶段：

- **阶段一：项目初始化与索引 (Onboarding & Indexing)**
    
    - **动作：** 当你首次在项目中使用 Serena 时，它会通过 LSP（或 JetBrains 插件）扫描整个代码库。
        
    - **产出：** 它会建立一个符号交叉引用数据库，并识别项目的架构模式，将这些“记忆”存储在项目根目录的 `.serena/memories` 文件夹中。这使得 AI 无需读取每个文件就能了解全局结构。
        
- **阶段二：语义感知检索 (Semantic Retrieval)**
    
    - **动作：** 当你提出任务（如“修改登录逻辑”）时，AI 不再盲目搜索文本，而是调用 Serena 的工具（如 `find_symbol`）。
        
    - **效果：** AI 能精准定位到 `LoginService` 类或 `authenticate` 函数的定义位置，并找到所有引用它的地方，从而进行影响分析。
        
- **阶段三：符号级编辑 (Symbolic Editing)**
    
    - **动作：** AI 确定修改方案后，使用 `insert_after_symbol` 或 `replace_symbol_body` 等工具进行修改。
        
    - **区别：** 传统工具是简单的文本替换，容易破坏括号或缩进；Serena 则是针对代码实体（如函数体）进行操作，能够自动保持代码结构的完整性和格式一致性。
        
- **阶段四：闭环验证与提交 (Verification & Commit)**
    
    - **动作：** 修改完成后，AI 可以通过 Serena 执行 Shell 命令运行单元测试或构建。
        
    - **终点：** 验证通过后，它可以自动执行 Git Commit，完成一个完整的开发闭环。
        

---

### 6. 如何与现有 AI 编程工具结合使用

Serena 并不是要取代你现有的 IDE 或模型，而是作为一个**“智能插件/中间件”**增强它们。

#### A. 与 Claude Desktop / Claude Code 结合 (通过 MCP)

这是目前最主流的用法。Serena 实现了 **Model Context Protocol (MCP)**。

- **配置方式：** 在 `claude_desktop_config.json` 中添加 Serena 作为 MCP Server。
    
- **体验：** 你在 Claude 界面中对话时，Claude 会自动调用 Serena 提供的工具去读写你的本地代码。
    

#### B. 与 VS Code / Cursor 结合

虽然 VS Code 和 Cursor 有自带的 AI 功能，但它们在大型代码库中常因 Context 限制而“幻觉”。

- **配置方式：** 在 VS Code 的 MCP 设置中添加 Serena 地址，或使用支持 MCP 的插件（如 **Cline** 或 **Roo Code**）。
    
- **体验：** 你可以要求 AI “使用 Serena 查找所有未使用的函数并删除”，AI 会比自带的 RAG 检索更加精准。
    

#### C. 与 Agent 框架结合 (如 LangGraph / Agno)

如果你正在开发自己的 AI Agent：

- **结合点：** Serena 的工具包是解耦的。你可以将 `serena-mcp-server` 作为一个 Tool 节点接入 LangGraph 工作流。
    
- **场景：** 构建一个自动修复 CI 错误的 Agent，该 Agent 调用 Serena 定位错误符号、修复代码、重启测试。
    

#### D. 与 JetBrains IDE 结合

- **方式：** 安装 Serena 的专属 JetBrains 插件。
    
- **优势：** 相比 LSP，JetBrains 插件提供的代码分析能力更深（支持更多复杂的语言特性和框架），能让 AI 拥有“专业级”的语义感知。
    

---

### 8. 核心优势对比 (8/2原则)

|**特性**|**传统 AI 编程工具 (如 RAG-based)**|**Serena 增强型工具**|
|---|---|---|
|**理解方式**|文本片段、关键词匹配|**符号、类、函数关系 (语义)**|
|**Token 消耗**|经常需要读取整个文件，开销大|**只读取相关的符号定义，极省 Token**|
|**编辑精度**|容易出现语法错误或格式错乱|**符号级精准修改，保持结构完整**|
|**项目规模**|代码库越大，效果越差|**擅长处理大型、复杂的代码库**|

---

### 9. 引用与文献

|**来源**|**描述**|**地址**|
|---|---|---|
|**GitHub - Oraios/Serena**|官方项目文档与 MCP 集成指南|[github.com/oraios/serena](https://github.com/oraios/serena)|
|**DEV Community**|如何通过符号级操作提高 AI 编辑精度|[dev.to/siddhantkcode/accurate-ai-edits](https://dev.to/siddhantkcode/how-to-make-ai-code-edits-more-accurate-bbe)|
|**LobeHub**|Serena MCP Server 在不同客户端的配置|[lobehub.com/mcp/oraios-serena](https://lobehub.com/mcp/oraios-serena)|

