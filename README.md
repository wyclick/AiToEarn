# AiToEarn — AI 内容营销智能体

[![GitHub stars](https://img.shields.io/github/stars/yikart/AiToEarn?color=fa6470)](https://github.com/yikart/AiToEarn/stargazers)
[![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

**Monetize · Publish · Engage · Create —— 一站式平台。**

AiToEarn 通过 AI Agent 自动化完成内容创作、多平台分发、互动运营和变现。覆盖抖音、小红书、快手、B站、TikTok、YouTube、Facebook、Instagram、Threads、X（Twitter）、Pinterest、LinkedIn 等 12+ 主流平台。

> 本项目 Fork 自 [yikart/AiToEarn](https://github.com/yikart/AiToEarn)。

---

## 本 Fork 新增贡献

### Claude Agent SDK 生产级集成

设计并实现了 `AgentRuntimeService`（~1045 行 NestJS 服务），将 Anthropic Claude Agent SDK 接入生产后端。

**架构**：
```
用户请求 (SSE) → AgentRuntimeService
  ├─ claudeQuery()          # Claude SDK 封装
  ├─ MCP Server Manager     # 8+ 工具服务（视频/图片/字幕/剪辑等）
  ├─ Message Stream (RxJS)  # 流式管道（init → chunk → keepAlive → done）
  ├─ Sub-Agent Dispatcher   # 主Agent + polling-task + skill-analyzer
  └─ Session Persistence    # S3 双向同步，支持断点续传
```

**技术亮点**：
- **MCP 多工具管理**：集成 8 类 MCP 工具服务器（媒体生成、视频编辑、字幕、图片编辑、风格迁移、短剧剪辑等），动态生成工具白名单
- **多 Agent 协作**：主 Agent（Sonnet/Opus）编排任务，子 Agent（Haiku）轮询异步视频状态
- **RxJS 流式管道**：firstMessage$ → restMessages$ → keepAlive$ → completion$，AbortController 中止 + takeUntil 优雅关闭
- **会话持久化**：本地文件系统 ↔ S3 双向同步，支持任务中断后恢复
- **PostToolUse Hook**：修复 MCP 返回图片 URL 被当作文本处理的问题，转换为 image content block

### 前端交互优化

- `LowBalanceAlertProvider`：修复余额弹窗，适配自托管部署场景
- 去弹窗优化：改善用户交互体验

### 部署适配

- `docker-compose.yml`：本地化部署配置调整

---

## 快速使用

| 方式 | 说明 |
|------|------|
| 浏览器直接使用 | [aitoearn.cn](https://aitoearn.cn/) 或 [aitoearn.ai](https://aitoearn.ai/) |
| Docker 部署 | `git clone && docker compose up -d` |
| MCP 接入 | Claude Desktop / Cursor 中配置 MCP Server 地址 |
| 源码开发 | `pnpm install && pnpm nx serve` |

详细使用说明见原项目 [yikart/AiToEarn](https://github.com/yikart/AiToEarn)。

---

## 技术栈

NestJS 11 / TypeScript / Next.js / React / MongoDB / Redis / RxJS / MCP / Claude Agent SDK / Docker

## License

MIT License
