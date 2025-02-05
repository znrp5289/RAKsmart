# 免费替代 Cursor AI！VS Code、DeepSeek R1、Ollama 打造免费 AI 编程神器

在付费 AI 编程助手领域，Cursor、Windsurf、GitHub Copilot 等工具确实表现出色。但每月持续支出的订阅费用对不少开发者来说仍是一笔负担。

本文将介绍如何通过 VS Code **Cline** 扩展 + **Ollama** + **DeepSeek R1** 组合实现免费的 AI 编程环境搭建。

## Cline：VS Code 中的开源 AI 编程助手

**Cline** 是一个可在 VS Code 中使用的 AI 编程辅助扩展，它是开源的解决方案，能够根据用户的指令或代码输入提供多种辅助功能，从而大幅提升开发效率。

与之类似的 VS Code 扩展还有 **Continue**、**Roo Code** 等，这些工具均经过一定程度的验证，并且拥有活跃的社区支持。

这些工具在基本设置方式上大同小异，大家可以根据个人习惯选择适合自己的工具。需要注意的是，Cline 本身没有 Tab 自动补全功能，因此可以考虑配合 Continue 一同使用。

根据 openrouter.ai 平台的 token 使用情况来看，**Cline** 和 **Roo Code** 的 token 使用量远远领先于其他工具。

![Vs code, Cline 设置](https://bbtdd.com/img/943947066679.webp)

### Cline 资源链接
- [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=Continue.continue)
- [Github (22.3k Stars)](https://github.com/continuedev/continue)

### Roo Code (原 Roo Cline) 资源链接
- [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=RooVeterinaryInc.roo-cline)
- [Github (4.9k Stars)](https://github.com/RooVetGit/Roo-Code)

## DeepSeek R1 本地部署

### 为什么选择本地部署？
- **免费及低成本运营:** 与需要每月订阅费用的 Cursor AI 不同，Cline 是开源提供的，DeepSeek R1 也可以在本地运行，因此无需额外费用即可享受强大的 AI 辅助功能。
- **数据安全:** 所有的 AI 计算都在本地环境中进行，代码和相关数据不会传输到外部服务器，从而在隐私和安全方面提供了显著的优势。
- **快速响应:** DeepSeek R1 直接在用户的硬件上运行，因此无需网络延迟即可提供快速响应，与 API 调用方式相比，可以期待更低的延迟。

当然，不能期望在个人 PC 上运行的轻量化蒸馏模型能够提供与 API 提供的完整参数模型相同的效果。对于大多数个人开发者来说，承担一定费用并使用平台提供的 API 更为现实。本地运行方式建议以实验性的心态去尝试。

### 安装 Ollama
Ollama 是一个可以帮助在本地轻松运行像 DeepSeek R1 这样的大型语言模型的工具。

![ollama 下载](https://bbtdd.com/img/28258577.webp)

### 下载所需的 DeepSeek R1 模型
下载完成后，模型会自动在本地启动，显示如下信息：本地运行的模型可以通过 `http://localhost:11434` 访问。

## VS Code 和 Cline 设置

### 安装 VS Code 扩展
在 VS Code 扩展市场中搜索 `Cline` 并安装。

![Vs code, 安装 Cline 扩展](https://bbtdd.com/img/6511294117252.webp)

### 在 Cline 中连接 Ollama（本地 DeepSeek R1）
- 打开 VS Code 中的 `Cline` 设置。
- 在 API Provider 列表中选择 `Ollama`。
- 在 `Base URL` 字段中输入 `http://localhost:11434`，然后在模型选择选项中选择正在运行的 DeepSeek R1 模型（例如：`deepseek-r1:14b`）。

> 当本地 DeepSeek 模型部署成功后，在输入 `Base URL` 后，Model ID 下方会自动显示可选模型列表。

![Vs code, 在 Cline 中连接 Ollama](https://bbtdd.com/img/1299972204.webp)

> 如果在输入信息后出现 `MCP hub not available` 错误，重启 VS Code 即可解决。
> 参考 Github Issue: [https://github.com/cline/cline/issues/969](https://github.com/cline/cline/issues/969)

设置完成后，使用 Cline 发送 prompt 测试响应情况。若发现输入 prompt 后 CPU 疯转而响应特别慢或无响应，则可能是由于电脑硬件不足以支撑该模型运行，此时建议尝试使用低版本的模型进行测试。

- 获取 DeepSeek API 密钥 ([DeepSeek 官方网站](https://platform.deepseek.com/api_keys))
- 在 VS Code 中打开 Cline 设置，在 API Provider 列表中选择 `DeepSeek`，输入 API Key，并选择模型 `deepseek-reasoner`。

![Vs code, Cline 设置 DeepSeek API](https://bbtdd.com/img/74317642.webp)

现在，你也可以以每月 $0 的成本体验 Cursor 级别的 AI 编程！如果在设置过程中遇到问题，可以参考 Ollama 官方文档或 Cline GitHub 问题页面获取帮助。

👉 [WildCard | 一分钟注册，轻松订阅海外线上服务](https://bbtdd.com/WildCard)