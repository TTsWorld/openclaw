# 研究计划

> **研究完成日期**：2026-03-25

## 研究摘要

### 项目核心价值

OpenClaw 是一个**多渠道 AI 助手网关**，采用"内核+扩展"的插件化架构，支持 20+ 消息平台（WhatsApp、Telegram、Slack、Discord、iMessage 等）和 50+ AI 提供商。核心设计理念是：

1. **自托管优先** - 用户完全掌控数据和运行环境
2. **统一抽象** - 通过 ChannelPlugin 接口屏蔽平台差异
3. **渐进式扩展** - 从 CLI 到 Web UI 到移动端，层层递进

### 文档索引

| 文档 | 主题 | 阅读建议 |
|------|------|---------|
| [01_architecture.md](./01_architecture.md) | 架构全景 | 首次阅读 |
| [02_mechanism_channels.md](./02_mechanism_channels.md) | 渠道系统 | 想了解消息如何收发 |
| [03_mechanism_plugins.md](./03_mechanism_plugins.md) | 插件系统 | 想开发插件 |
| [04_data_flow.md](./04_data_flow.md) | 数据流与状态 | 想了解核心数据模型 |
| [05_gateway.md](./05_gateway.md) | 网关系统 | 想了解设备配对和通信 |
| [06_dependencies.md](./06_dependencies.md) | 依赖与生态 | 想了解技术栈 |
| [07_learning_path.md](./07_learning_path.md) | 学习路径 | 快速找到切入点 |

### 设计亮点

1. **插件边界强制执行**：通过 Lint 规则和运行时检查确保插件隔离
2. **会话键设计**：`agent:<agentId>:<scope>:<id>` 字符串格式，简洁且支持灵活路由
3. **配置集中管理**：单一 `openclaw.json` 文件，支持热重载和版本控制
4. **网关多层级认证**：Device Token + Ed25519 签名，安全且无需中心化 CA
5. **MsgContext 标准化**：180+ 字段的扁平结构，统一 20+ 平台的消息格式

---

## 项目概述

**它是什么**：OpenClaw 是一个个人 AI 助手网关，允许用户在自己设备上运行 AI 助手，并通过多种消息渠道（WhatsApp、Telegram、Slack、Discord、iMessage 等 20+ 平台）进行交互。

**解决什么问题**：
- 用户不想被锁定在单一聊天平台或单一 AI 提供商
- 需要一个本地运行、隐私优先的 AI 助手解决方案
- 希望在常用消息应用中无缝使用 AI 功能
- 没有它，用户需要在每个平台单独配置机器人，或依赖云端 SaaS 服务

**谁在使用**：
- 技术爱好者和个人用户，希望拥有私人的、自托管的 AI 助手
- 通过 npm 全局安装，支持 macOS、Linux、Windows (WSL2)
- 提供 CLI、Web UI、移动应用（iOS/Android）多种交互方式

---

## 研究专题

### 专题 A：架构全景
- **目标**：理解项目由哪些模块组成，每个模块是什么、为什么存在
- **范围**：
  - `src/` 核心源代码目录结构
  - `extensions/` 插件扩展目录
  - `ui/` 前端界面
  - `apps/` 移动端应用（Android/iOS/macOS）
  - 插件 SDK 体系
- **输出**：`docs/code-research/01_architecture.md`

### 专题 B：核心机制 - 渠道系统（Channels）
- **目标**：理解渠道系统是什么、为什么这么设计、完整调用链
- **范围**：
  - `src/channels/` 渠道核心实现
  - `extensions/` 中各渠道插件（telegram、whatsapp、slack、discord 等）
  - 消息收发流程
  - 渠道生命周期管理
- **输出**：`docs/code-research/02_mechanism_channels.md`

### 专题 C：核心机制 - 插件系统（Plugins）
- **目标**：理解插件架构、Plugin SDK 设计、插件加载机制
- **范围**：
  - `src/plugins/` 插件核心
  - `src/plugin-sdk/` 或相关 SDK 实现
  - 插件注册、加载、隔离机制
  - Provider 体系（AI 模型提供商）
- **输出**：`docs/code-research/03_mechanism_plugins.md`

### 专题 D：数据流与状态管理
- **目标**：核心数据结构是什么、为什么这样建模、数据如何流动
- **范围**：
  - 消息数据模型
  - 会话/线程状态管理
  - 配置存储（`src/config/`）
  - 持久化层
- **输出**：`docs/code-research/04_data_flow.md`

### 专题 E：网关与通信协议
- **目标**：理解 Gateway 架构、设备配对、通信协议
- **范围**：
  - `src/gateway/` 网关实现
  - `src/daemon/` 守护进程
  - 设备配对机制（`extensions/device-pair`）
  - 协议定义和序列化
- **输出**：`docs/code-research/05_gateway.md`

### 专题 F：依赖与生态
- **目标**：核心依赖是什么、为什么选择它们而不是替代品
- **范围**：
  - package.json 依赖分析
  - AI 模型 SDK（Anthropic、OpenAI、Google 等）
  - 消息平台 SDK
  - 基础设施依赖（Hono、Express、WebSocket 等）
- **输出**：`docs/code-research/06_dependencies.md`

### 专题 G：学习路径（最后执行，依赖以上专题）
- **目标**：总结推荐阅读顺序，为贡献者提供指引
- **输出**：`docs/code-research/07_learning_path.md`

---

## 研究执行策略

1. **Phase 1**：并行执行专题 A、B、C（独立主题）
2. **Phase 2**：并行执行专题 D、E、F
3. **Phase 3**：执行专题 G，汇总所有研究文档

---

## 待解决疑问

（研究过程中添加，不要跳过）

### 已解答疑问

**Q1: 插件系统的沙箱隔离机制是如何实现的？**
> **A**: 通过多层机制实现：1) Lint 规则限制导入（如 `lint:extensions:no-src-outside-plugin-sdk`）；2) `PluginRuntime` 运行时 API 隔离核心模块；3) `withPluginRuntimePluginIdScope` 追踪插件上下文；4) `openBoundaryFileSync` 安全文件操作防止路径遍历。详见 `03_mechanism_plugins.md` 安全考虑章节。

**Q2: 渠道插件的接口契约具体定义在哪里？**
> **A**: 核心契约定义在 `src/channels/plugins/types.plugin.ts` 的 `ChannelPlugin` 接口，包含 config/setup/gateway/outbound/status 等多个适配器。每个渠道插件实现此接口并通过 `defineChannelPluginEntry` 注册。详见 `02_mechanism_channels.md` 核心组件章节。

**Q3: Gateway 如何处理多设备同时连接和消息同步？**
> **A**: 1) WebSocket 协议支持请求/响应/事件三种帧类型；2) `StateVersion` 版本号检测状态变更；3) 连接时发送完整 Snapshot，后续发送增量 EventFrame；4) 序列号 `seq` 检测消息丢失。详见 `05_gateway.md` 消息路由与多设备同步章节。
