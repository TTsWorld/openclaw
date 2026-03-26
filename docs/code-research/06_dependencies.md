# 依赖与生态系统

## 核心运行时依赖

### 1. AI SDK 与模型集成

| 依赖 | 版本 | 用途 |
|------|------|------|
| `@anthropic-ai/vertex-sdk` | ^0.14.4 | Anthropic Claude API 访问 |
| `@aws-sdk/client-bedrock` | ^3.1014.0 | AWS Bedrock 模型访问 |
| `@modelcontextprotocol/sdk` | 1.27.1 | MCP (Model Context Protocol) 支持 |
| `@agentclientprotocol/sdk` | 0.16.1 | ACP (Agent Client Protocol) 支持 |

**为什么选择这些 SDK？**

- **多提供商策略**：OpenClaw 不绑定单一 AI 提供商，通过插件化架构支持 Anthropic、OpenAI、Google、AWS Bedrock、Groq、Mistral 等
- **MCP/ACP 协议支持**：采用新兴的 AI 协议标准，实现与 Claude Desktop、Cline 等工具的互操作性

### 2. Web 框架

**Hono** (4.12.8)
- **它是什么**：轻量级、高性能的 Web 框架，专为边缘计算和 Serverless 设计
- **为什么选择它而不是 Express？**
  - 更小的包体积（~20KB vs Express 的 ~200KB+）
  - 原生支持 TypeScript，无需额外类型定义
  - 更好的边缘运行时兼容性（Cloudflare Workers、Deno、Bun）
  - 更快的冷启动时间，适合 CLI 工具场景
- **项目中的角色**：HTTP 服务器、Webhook 处理、API 路由

**Express** (^5.2.1)
- **项目中的角色**：部分旧模块和兼容性代码仍在使用 Express
- **迁移策略**：项目正在从 Express 向 Hono 迁移

### 3. 通信与网络

| 依赖 | 版本 | 用途 |
|------|------|------|
| `ws` | ^8.20.0 | WebSocket 服务器与客户端 |
| `@lydell/node-pty` | 1.2.0-beta.3 | 终端伪终端（TUI 模式） |
| `undici` | ^7.24.5 | 高性能 HTTP 客户端（Node.js 官方） |

### 4. 数据验证与序列化

**Zod** (^4.3.6)
- **它是什么**：TypeScript 优先的模式验证库
- **为什么选择它而不是 Joi/Yup？**
  - 原生 TypeScript 支持，类型推断零配置
  - 更小的包体积（Joi 约 100KB+，Zod 约 20KB）
  - 更好的性能（编译时类型检查）
  - 与 TypeScript 类型系统完美集成
- **项目中的角色**：配置验证、API 请求验证、运行时类型安全

**@sinclair/typebox** (0.34.48)
- **项目中的角色**：JSON Schema 生成，与 Zod 互补使用

### 5. 配置与工具

| 依赖 | 版本 | 用途 |
|------|------|------|
| `yaml` | ^2.8.3 | YAML 配置文件解析 |
| `json5` | ^2.2.3 | 支持注释的 JSON 解析 |
| `dotenv` | ^17.3.1 | 环境变量加载 |
| `chalk` | ^5.6.2 | 终端颜色输出 |
| `commander` | ^14.0.3 | CLI 命令解析 |
| `@clack/prompts` | ^1.1.0 | 交互式 CLI 提示 |
| `jiti` | ^2.6.1 | 运行时 TypeScript 加载 |

---

## 消息平台 SDK

### 1. Telegram
- **依赖**：`grammy` (^1.41.1), `@grammyjs/runner`, `@grammyjs/transformer-throttler`
- **说明**：grammy 是现代 TypeScript 优先的 Telegram Bot 框架，相比 node-telegram-bot-api 有更好的类型支持

### 2. Discord
- **依赖**：`@buape/carbon`, `@discordjs/voice`, `discord-api-types`
- **说明**：使用 Carbon 框架简化 Discord Bot 开发，支持语音功能

### 3. WhatsApp
- **依赖**：`@whiskeysockets/baileys` (7.0.0-rc.9), `jimp`
- **说明**：Baileys 是 WhatsApp Web 的非官方库，支持 QR 码登录

### 4. Slack
- **依赖**：`@slack/bolt` (^4.6.0), `@slack/web-api`
- **说明**：Slack 官方 SDK，支持 Socket Mode

### 5. 其他平台

| 平台 | 依赖 |
|------|------|
| LINE | `@line/bot-sdk` |
| 飞书/Lark | `@larksuiteoapi/node-sdk` |
| Matrix | `matrix-js-sdk`, `@matrix-org/matrix-sdk-crypto-nodejs` |
| iMessage | 原生集成（macOS 专用） |
| Signal | signal-cli REST API |
| IRC | 原生实现 |
| Twitch | 原生 WebSocket 实现 |

---

## 开发工具链

### 1. 构建与编译

| 依赖 | 版本 | 用途 |
|------|------|------|
| `typescript` | ^5.9.3 | 类型检查与编译 |
| `tsdown` | 0.21.4 | 快速 TypeScript 打包（基于 Rolldown） |
| `tsx` | ^4.21.0 | TypeScript 执行器（开发时） |

### 2. 测试

**Vitest** (^4.1.0)
- **它是什么**：现代 Vite 原生测试框架
- **为什么选择它而不是 Jest？**
  - 原生 ESM 支持（Jest 需要复杂配置）
  - 与 Vite 生态无缝集成
  - 更快的测试执行速度（使用 esbuild）
  - 内置 TypeScript 支持，无需 ts-jest
  - 更好的 Watch 模式性能
- **项目中的角色**：单元测试、集成测试、E2E 测试

**@vitest/coverage-v8** (^4.1.0)
- 代码覆盖率收集

### 3. 代码质量

**Oxlint** (^1.56.0) + **Oxfmt** (0.41.0)
- **它是什么**：Rust 编写的高性能 JavaScript/TypeScript linter 和 formatter
- **为什么选择它而不是 ESLint/Prettier？**
  - 极快的执行速度（比 ESLint 快 50-100 倍）
  - 零配置即可使用
  - 内置 TypeScript 支持
  - 规则与 ESLint 兼容
- **项目中的角色**：代码检查、格式化

**jscpd** (4.0.8)
- 代码重复检测

---

## UI 层依赖

位于 `/Users/zhihu/code/m_code/ai/openclaw-my/ui/package.json`：

| 依赖 | 版本 | 用途 |
|------|------|------|
| `lit` | ^3.3.2 | Web Components 框架 |
| `marked` | ^17.0.5 | Markdown 渲染 |
| `dompurify` | ^3.3.3 | HTML 净化（XSS 防护） |
| `vite` | 8.0.1 | 构建工具 |

**为什么选择 Lit？**
- 轻量级（~5KB）
- 原生 Web Components，无框架锁定
- 与 Vite 配合良好
- 适合嵌入式 UI 场景

---

## 有意不使用的方案

| 方案 | 未使用原因 |
|------|-----------|
| **Jest** | 选择 Vitest 以获得更好的 ESM 支持和更快的速度 |
| **ESLint/Prettier** | 选择 Oxlint/Oxfmt 以获得更好的性能 |
| **Joi/Yup** | 选择 Zod 以获得更好的 TypeScript 集成 |
| **Fastify** | 选择 Hono 以获得更小的体积和边缘兼容性 |
| **NestJS** | 项目需要轻量级架构，避免过度抽象 |
| **Prisma** | 使用原生 SQLite + sqlite-vec 向量扩展 |

---

## 外部系统集成

### 1. 数据库与存储
- **sqlite-vec** (0.1.7)：SQLite 向量扩展，用于语义搜索和记忆功能

### 2. 媒体处理
- **sharp** (^0.34.5)：高性能图像处理（基于 libvips）
- **pdfjs-dist** (^5.5.207)：PDF 解析
- **music-metadata** (^11.12.3)：音频元数据提取

### 3. 网页抓取
- **@mozilla/readability** (^0.6.0)：Mozilla 的文章提取算法
- **playwright-core** (1.58.2)：浏览器自动化（无头模式）
- **linkedom** (^0.18.12)：轻量级 DOM 实现（服务器端）

### 4. 安全与认证
- **@noble/ed25519** (3.0.1)：Ed25519 加密签名
- **https-proxy-agent** (^8.0.0)：HTTPS 代理支持

---

## 项目定位

**在同类工具中的位置**：

| 特性 | OpenClaw | 其他工具（如 n8n、Flowise） |
|------|----------|---------------------------|
| 架构 | 插件化、模块化 | 通常更单体 |
| AI 集成 | 多提供商原生支持 | 通常需要额外配置 |
| 消息渠道 | 30+ 渠道内置 | 通常较少 |
| 部署 | CLI 优先，支持 Docker | 通常 GUI 优先 |
| 协议支持 | MCP/ACP 原生 | 通常不支持 |

**核心优势**：
- 极致的模块化设计（80+ 扩展）
- 统一的插件 SDK
- 多平台支持（Node.js、iOS、Android、macOS）
- 协议优先（MCP/ACP）

**有意不支持的功能**：
- 可视化工作流编辑器（保持 CLI 优先）
- 内置数据库 ORM（保持轻量）
- 内置用户认证系统（依赖外部身份提供商）

---

## 关键文件路径

- **主依赖定义**：`/Users/zhihu/code/m_code/ai/openclaw-my/package.json`
- **UI 依赖**：`/Users/zhihu/code/m_code/ai/openclaw-my/ui/package.json`
- **扩展依赖**：`/Users/zhihu/code/m_code/ai/openclaw-my/extensions/*/package.json`
- **包管理**：pnpm workspace（`pnpm@10.32.1`）
- **Node 版本要求**：`>=22.16.0`
