## 1. 认证页面与路由守卫

- [x] 1.1 创建登录页面 `src/pages/admin/login.astro` — 用户名/密码表单 + 调用 `POST /v1/auth/login`
- [x] 1.2 创建 Svelte AuthGuard 组件 `src/components/features/admin/AuthGuard.svelte` — Svelte `onMount` 中检测 localStorage Token，未登录跳转 `/admin/login/`。若项目使用 Swup，需监听 `content:replace` 事件重新检查
- [x] 1.3 创建管理后台布局 `src/layouts/AdminLayout.astro` — 侧边栏导航 + 顶栏 + AuthGuard 包裹（Svelte 组件通过 `client:load` 水合）。所有管理页面通过 `import AdminLayout from '@layouts/AdminLayout.astro'` 继承此布局和认证保护

## 2. 管理首页

- [x] 2.1 创建管理首页 `src/pages/admin/index.astro` — 使用 AdminLayout，展示站点统计概览（文章数、分类数、标签数等），数据从 API 获取

## 3. 公共 UI 组件

- [x] 3.1 创建 `DataTable` 通用表格组件（分页 + 排序 + 列配置）
- [x] 3.2 创建 `ConfirmDialog` 删除确认对话框
- [x] 3.3 创建 `FormBuilder` 通用表单组件（校验 + 提交 + 错误提示）。V1 支持基础字段类型（text、textarea、select、multiSelect、number、checkbox），嵌套子项（如 experience、links）和复杂场景由各模块单独实现表单
- [x] 3.4 创建 `Toast` 通知组件（操作成功/失败提示）
- [x] 3.5 创建 `PageHeader` 页面标题 + 操作按钮组件
- [x] 3.6 验证：`pnpm type-check` 通过、`pnpm build` 通过
