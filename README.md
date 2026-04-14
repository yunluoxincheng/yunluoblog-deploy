# YunluoBlog 部署配置

YunluoBlog 全栈部署编排，包含前端、后端、数据库和反向代理。

## 仓库结构

本仓库根目录为部署编排层，子目录为独立的 git 仓库：

```
yunluoblog/
├── mizuki/              # 前端（独立仓库）
├── yunluoblog-server/   # 后端（独立仓库）
├── nginx/
│   └── nginx.conf       # Nginx 反向代理配置
├── docker-compose.yml   # 容器编排
├── .env                 # 环境变量（不提交）
└── .gitignore
```

## 快速开始

### 1. 克隆子项目

```bash
git clone https://github.com/yunluoxincheng/yunluoblog.git mizuki
git clone https://github.com/yunluoxincheng/yunluoblog-server.git
```

### 2. 配置环境变量

```bash
cp .env.example .env
# 编辑 .env 修改数据库密码等敏感信息
```

### 3. 启动全部服务

```bash
docker compose up -d --build
```

## 服务说明

| 服务 | 说明 | 端口 |
|------|------|------|
| nginx | 反向代理，统一入口 | 80 |
| web | 前端 (mizuki) | 3000（内部） |
| server | 后端 API (Spring Boot) | 8080 |
| db | PostgreSQL 数据库 | 5432 |

## Nginx 路由

- `/` → 前端页面
- `/api/` → 后端 API

## 许可证

MIT
