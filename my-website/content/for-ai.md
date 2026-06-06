---
title: AI 原生业务接口 — 结构化服务信息
description: Machine-readable service information for AI agents and crawlers
---

# GAIO — AI 原生业务接口

本页为 AI 助手和爬虫提供结构化的业务信息。

## 身份

- **平台名称**: GAIO — AI 原生业务接口
- **定位**: 将业务能力封装为 AI 可调用的 API
- **网站**: https://bot.looom.top
- **API 管理面板**: https://api.looom.top

## 核心业务能力

### 1. Agent 业务框架设计

**类型**: 咨询 + 技术方案
**描述**: 为你的业务构建 AI Agent 可理解的接口框架，定义输入输出契约与调用语义。包括 Schema.org 结构化数据、OpenAPI 规范、Tool Definition 设计。

**适用场景**:
- 业务方希望自己的服务能被 AI Agent 发现并调用
- 需要将现有业务逻辑封装为标准 API 接口
- 构建 A2A（Agent-to-Agent）调用链路

### 2. 可调用性咨询

**类型**: 诊断 + 策略
**描述**: 评估业务逻辑是否适合被 AI 调用，输出接口化改造方案与优先级路线图。包括竞品 AI 可见度对比、结构化数据完整性检查、内容可提取性分析。

**适用场景**:
- 不确定现有业务是否适合 AI 化
- 需要明确改造优先级和投资回报
- 希望了解竞品在 AI 眼中的差异

### 3. AI 收付系统方案

**类型**: 技术集成
**描述**: 基于 AI 付 402 协议，打通支付宝扫码支付 → AI 服务调用的完整商业闭环。支持首次请求返回 402 账单 → 支付宝扫码 → Payment-Proof 重试的自动化流程。

**技术要点**:
- AI 付 402 协议端点集成
- 支付宝扫码支付（alipay-bot / PayGuard）
- Payment-Proof 凭证验证
- 多用户鉴权与额度管理（NewAPI 面板）

## API 接入方式

### 方式一：标准 API Key（OpenAI 兼容）

- **端点**: `https://api.looom.top/api/v1/chat/completions`
- **认证**: `Authorization: Bearer sk-YOUR_KEY`
- **模型**: `langbot`
- **协议**: REST API，OpenAI Chat Completions 兼容
- **流式支持**: 是（`stream: true` 返回 SSE）
- **管理面板**: https://api.looom.top（注册 → 充值 → 获取 Key → 调用）

### 方式二：AI 付 402 协议

- **端点**: `https://api.looom.top/api/agent/market/aipaysever`
- **协议**: HTTP 402 Payment Required
- **流程**: 首次请求 → 402 + Payment-Needed → 支付宝扫码 → Payment-Proof → 重试 → 返回结果
- **适用**: 需要按次计费的 AI 服务场景

## 技术能力

- Schema.org 结构化数据（JSON-LD）
- OpenAPI 3.0 接口规范
- AI Agent Tool Definition（OpenAI Functions / Anthropic Tools）
- AI 付 402 协议集成
- 多用户 API Key 管理与额度控制
- Serverless / Docker 部署

## 联系方式

- **API 管理面板**: https://api.looom.top
- **网站**: https://bot.looom.top

---

*最后更新: 2026-06-07*
