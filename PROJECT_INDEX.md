# API Gateway Worker - 项目文档索引

## 📋 项目概述

**API Hub** 是一个强大的 API 代理服务集合，通过 Cloudflare Workers 或本地服务器部署，为多个 AI 和 Web 服务提供统一访问入口。

## 🏗️ 架构设计

### 核心组件

| 组件 | 文件 | 用途 |
|-----|------|------|
| **Worker 运行时** | `src/worker.js` | 使用 fetch API 的 Cloudflare Workers 部署 |
| **服务器运行时** | `src/server.js` | 用于本地部署的 Node.js HTTP 服务器 |
| **配置文件** | `wrangler.toml` | Cloudflare Workers 配置 |
| **包配置** | `package.json` | Node.js 依赖和脚本 |

## 🌐 支持的服务

### AI API 代理
- **OpenAI** (`/openai/*`) → `api.openai.com` 🤖
- **Google Gemini** (`/gemini/*`) → `generativelanguage.googleapis.com` 🌟  
- **Claude** (`/claude/*`) → `api.anthropic.com` 🧠
- **Claude Web** (`/claude2api/*`) → `claude.ai` 🎯
- **Grok** (`/grok/*`) → `api.x.ai` ⚡

### 其他服务
- **Docker Registry** (`/docker/*`) → `docker.ixu.cc` 🐳
- **GitHub API** (`/github/*`) → `github.com` 📦
- **Telegram Bot** (`/telegram/*`) → `api.telegram.org` 📱

## 📁 项目结构

```
api_gateway_worker/
├── src/
│   ├── worker.js      # Cloudflare Workers 运行时
│   └── server.js      # Node.js HTTP 服务器
├── package.json       # 依赖和脚本
├── wrangler.toml      # CF Workers 配置
└── README.md          # 中文文档
```

## 🔧 核心功能

### 请求流程
1. **路由检测** → 解析传入请求路径
2. **服务映射** → 匹配到配置的 API 服务
3. **请求头处理** → 转发相关请求头
4. **代理请求** → 转发到目标 API
5. **响应处理** → 返回代理响应

### 技术能力
- ✅ **CORS 支持** - 跨域请求处理
- ✅ **请求头转发** - 维护 API 认证
- ✅ **多运行时** - Workers 和 Node.js 支持
- ✅ **Web 界面** - 优雅的服务仪表板
- ✅ **错误处理** - 优雅的故障管理

## 📋 API 配置模式

```javascript
const API_CONFIGS = {
  "service_name": {
    host: "api.example.com",           // 目标主机名
    paths: ["/v1/"],                   // 允许的路径前缀
    description: "服务描述",             // UI 显示文本
    logo: "🔥",                        // 服务表情符号
    directUrl: false                   // 可选：直接 URL 模式
  }
}
```

## 🚀 部署选项

### 1. Cloudflare Workers (推荐)
```bash
npm run deploy
```

### 2. 本地开发
```bash
npm run dev
```

### 3. Node.js 服务器
```bash
node src/server.js
```

## 🔍 代码分析

### Worker.js 特性
- **事件驱动架构** 使用 `addEventListener('fetch')`
- **现代 fetch API** 用于 HTTP 代理
- **嵌入式 HTML 模板** 用于 Web 界面
- **错误边界处理** 使用 try-catch 块

### Server.js 特性  
- **Node.js HTTP/HTTPS 模块** 用于代理
- **基于流的请求转发** 使用 `pipe()`
- **CORS 预检处理** 使用 OPTIONS 方法
- **进程环境配置** 通过 PORT

### 共享架构
- **相同的 API_CONFIGS** 跨两个运行时
- **相同的 HTML_TEMPLATE** 保持 UI 一致性
- **统一的错误处理模式**
- **通用的请求路由逻辑**

## 🛡️ 安全考虑

### 当前实现
- ✅ 已配置 CORS 请求头
- ✅ 请求头清理（移除 host, connection）
- ✅ 针对允许服务的路径验证
- ⚠️ **无需认证**（开放代理）
- ⚠️ **未实现速率限制**
- ⚠️ **无请求日志**用于监控

### 建议
- 实现 API 密钥认证
- 为每个服务添加速率限制
- 启用请求/响应日志记录
- 考虑基于 IP 的访问控制

## 📊 文件统计

| 文件 | 行数 | 类型 | 功能 |
|-----|------|------|------|
| `src/worker.js` | 419 | JavaScript | CF Workers 运行时 |
| `src/server.js` | 506 | JavaScript | Node.js 服务器 |
| `README.md` | 120 | Markdown | 中文文档 |
| `wrangler.toml` | 8 | TOML | CF 配置 |
| `package.json` | 12 | JSON | Node.js 配置 |

## 🔗 外部依赖

### 运行时依赖
- **无** - 纯 JavaScript 实现

### 开发依赖
- `wrangler` ^3.0.0 - Cloudflare Workers CLI

## 🌍 演示和资源

- **演示站点**: https://api.ixu.cc
- **GitHub 仓库**: https://github.com/Ten-o/api_gateway_worker
- **部署按钮**: 一键 Cloudflare Workers 部署

## 📈 性能特征

### Cloudflare Workers
- **冷启动**: 全球 ~10-50ms
- **内存**: 最小占用
- **并发**: 高度可扩展
- **地理分布**: 边缘分发

### Node.js 服务器
- **启动**: 本地 ~100-500ms
- **内存**: Node.js 开销
- **并发**: 基于事件循环
- **地理分布**: 单一位置

---

*最后更新: 2025-07-25*  
*由 Claude Code SuperClaude Framework 生成*