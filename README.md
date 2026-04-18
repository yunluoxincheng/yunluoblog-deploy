# YunluoBlog

YunluoBlog 全栈博客系统，由前端（Astro）、后端（Spring Boot）、数据库（PostgreSQL）和反向代理（Nginx）组成，通过 Docker Compose 一键部署。

## 系统架构

```
                    Internet
                       |
                  [Nginx :80]
                   /         \
                  /           \
         /               \
    web:3000          server:8080
  (Astro 前端)       (Spring Boot API)
                           |
                      db:5432
                   (PostgreSQL 16)
```

**数据流**：

- **博客文章**：构建时从后端 API 同步到 Markdown 文件 → Astro 静态生成 HTML
- **展示页面**（友链、追番、日记、项目等）：运行时从 API 获取数据
- **管理后台**（`/admin/`）：完整的 JWT 认证 + 全部内容 CRUD 管理

## 仓库结构

本仓库为部署编排层，前端和后端通过 git submodule 引用：

```
yunluoblog/
├── yunluoblog-web/        # 前端 — Astro 6 + Svelte 5（submodule）
├── yunluoblog-server/     # 后端 — Spring Boot 3.4 API（submodule）
├── nginx/
│   └── nginx.conf         # Nginx 反向代理配置
├── docker-compose.yml     # 容器编排（db + server + web + nginx）
├── .env.example           # 环境变量模板
├── uploads/               # 文件上传存储目录（bind mount）
└── README.md
```

## 功能概览

### 前端（yunluoblog-web）

- Astro 6 静态站点 + Svelte 5 交互组件
- 完整管理后台（19 个管理页面），JWT 认证，自动 Token 刷新
- 博客文章、分类、标签、归档
- 追番、日记、友链、项目展示、技能、时间线、设备展示
- 相册（本地扫描 + API 管理）
- Pagefind 全文搜索、RSS/Atom、站点地图
- 暗色模式、主题色自定义、Swup 页面过渡动画
- 评论系统（Twikoo / Giscus）
- 音乐播放器

### 后端（yunluoblog-server）

- Spring Boot 3.4 REST API，70+ 端点，16 个控制器
- 17 个实体类，20 张数据库表，Flyway 迁移管理
- JWT 无状态认证（HS256，访问令牌 15 分钟 + 刷新令牌 7 天）
- 全部内容的 CRUD 管理：文章、分类、标签、友链、追番、日记、项目、技能、时间线、设备、相册
- 文件上传服务（10MB 限制，类型白名单，UUID 文件名）
- 站点配置管理（带版本历史）
- 导航链接树管理
- AOP 审计日志（自动记录所有管理操作）
- 加密文章（密码保护）
- Swagger UI / OpenAPI 文档
- 18 个集成测试类

## 快速开始

### 前置条件

- Docker & Docker Compose
- Git

### 部署步骤

```bash
git clone --recurse-submodules https://github.com/yunluoxincheng/yunluoblog.git
cd yunluoblog
cp .env.example .env
# 编辑 .env 修改数据库密码、JWT 密钥等敏感信息
docker compose up -d --build
```

### 初始管理员账号

首次启动后 Flyway 自动创建：

- 用户名：`admin`
- 密码：`admin123`
- **请务必在首次登录后修改默认密码**

## 服务说明

| 服务 | 说明 | 端口 |
|------|------|------|
| nginx | 反向代理，统一入口 | `${NGINX_PORT}`（默认 80） |
| web | 前端 (Astro preview) | 3000（内部） |
| server | 后端 API (Spring Boot) | `${SERVER_PORT}`（默认 8080） |
| db | PostgreSQL 数据库 | 5432（内部） |

## Nginx 路由

| 路径 | 目标 | 说明 |
|------|------|------|
| `/` | `web:3000` | 前端页面 |
| `/api/` | `server:8080` | 后端 API（自动去除 `/api` 前缀） |
| `/api/swagger-ui/` | `server:8080` | Swagger API 文档 |
| `/api/v3/api-docs` | `server:8080` | OpenAPI 规范 |

## 环境变量

参见 `.env.example`：

| 变量 | 说明 | 默认值 |
|------|------|--------|
| `POSTGRES_DB` | 数据库名 | `yunluoblog` |
| `POSTGRES_USER` | 数据库用户 | `yunluo` |
| `POSTGRES_PASSWORD` | 数据库密码 | `changeme` |
| `SPRING_DATASOURCE_URL` | 数据库连接 URL | `jdbc:postgresql://db:5432/yunluoblog` |
| `JWT_SECRET` | JWT 签名密钥（至少 32 字节） | — |
| `CORS_ALLOWED_ORIGIN` | 允许跨域的前端域名 | `http://localhost:4321` |
| `SERVER_PORT` | 后端暴露端口 | `8080` |
| `NGINX_PORT` | Nginx 暴露端口 | `80` |

## 子项目

| 仓库 | 说明 |
|------|------|
| [yunluoblog-web](https://github.com/yunluoxincheng/yunluoblog-web) | 前端 — Astro 6 + Svelte 5 |
| [yunluoblog-server](https://github.com/yunluoxincheng/yunluoblog-server) | 后端 — Spring Boot 3.4 API |

## 许可证

MIT
