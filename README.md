# YunluoBlog 部署配置

YunluoBlog 全栈部署编排，包含前端、后端、数据库和反向代理。

## 仓库结构

本仓库为部署编排层，前端和后端通过 git submodule 引用：

```
yunluoblog-deploy/
├── yunluoblog-web/        # 前端（submodule → YunLuoBlog）
├── yunluoblog-server/     # 后端（submodule）
├── nginx/
│   └── nginx.conf         # Nginx 反向代理配置
├── docker-compose.yml     # 容器编排
├── .env.example           # 环境变量模板
└── .gitignore
```

## 快速开始

```bash
git clone --recurse-submodules https://github.com/yunluoxincheng/yunluoblog-deploy.git
cd yunluoblog-deploy
cp .env.example .env
# 编辑 .env 修改数据库密码等敏感信息
docker compose up -d --build
```

## 服务说明

| 服务 | 说明 | 端口 |
|------|------|------|
| nginx | 反向代理，统一入口 | 80 |
| web | 前端 (yunluoblog-web) | 3000（内部） |
| server | 后端 API (Spring Boot) | 8080 |
| db | PostgreSQL 数据库 | 5432 |

## Nginx 路由

- `/` → 前端页面
- `/api/` → 后端 API

## 许可证

MIT
