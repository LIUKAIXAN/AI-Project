# AI Native Engineering Workspace — 产品需求文档（PRD v1.0）

> 文档状态：**已确认** | 版本：v1.0 | 日期：2026-07-14

---

## 一、项目概述

### 1.1 产品定位

一套面向 **6 人小团队** 的 AI Native 研发管理平台（MVP 阶段），替代目前飞书文档管理方式，解决 Issue 生命周期管理、团队协作通知、文档分散三大核心痛点。

### 1.2 产品目标（北极星指标）

> **MVP 第一周目标**：每个角色都觉得实用而不是麻烦，愿意用起来。

### 1.3 目标用户

| 角色 | 人数 | 核心场景 | 频率 |
|------|:---:|---------|------|
| Boss | 1 | 查看项目进展，了解谁在做什么 | 每天 1-2 次 |
| PM | 1 | 创建/管理 Issue，写需求文档，跟踪进度 | 每小时 |
| 前端 | 1 | 认领任务，更新状态，写修复说明 | 每小时 |
| 后端 | 2 | 认领任务，更新状态，写修复说明 | 每小时 |
| QA | 1 | 收到通知，执行测试，反馈结果 | 需要时 |

### 1.4 当前痛点（Top 3）

1. **任务更新不及时 + 状态滞后** — 全靠人工维护飞书表格
2. **测试人员无法及时收到通知** — 不知道什么时候有新任务要测
3. **文档分散，缺乏关联** — 补充内容需新建独立文档，来回切换

---

## 二、MVP 功能范围

### ✅ 包含（Phase 1）

| 模块 | 功能 |
|------|------|
| **用户系统** | 注册/登录/退出，角色权限管理 |
| **项目管理** | 创建/编辑多项目，按项目分类 |
| **Issue 管理** | CRUD，类型（Bug/Feature/Task/Improvement） |
| **Markdown 编辑** | 富文本描述，支持截图粘贴 + 附件上传 |
| **工作流** | 7 步状态流转（New → Suggested → Accepted → In Progress → Ready for Test → Testing → Done） |
| **评论** | Issue 内评论讨论 |
| **Dashboard** | 简单统计卡片 + 任务列表 + 状态分布 |
| **通知** | 浏览器桌面通知（状态变更、评论、指派） |
| **Issue 列表** | 筛选/搜索/列表+看板双视图 |

### ❌ 不包含（后续 Phase）

- Git 集成（Phase 4）
- AI 功能（Phase 3）
- 知识库/RAG（Phase 3/6）
- Sprint/Cycle 管理（Phase 2）
- 高级统计报表（Phase 2）
- 邮件/IM 通知（Phase 2）
- @Mention（Phase 2）

---

## 三、权限模型

### 3.1 四角色体系

| 角色 | 说明 |
|------|------|
| **Admin** | 系统管理员，管理用户和项目 |
| **PM** | 创建/管理 Issue，查看所有项目 |
| **Developer** | 认领/处理 Issue，更新状态，评论 |
| **QA** | 执行测试，创建 Bug Issue，更新测试状态 |
| **Viewer** | 只读查看（Boss 使用） |

### 3.2 权限矩阵

| 操作 | Admin | PM | Developer | QA | Viewer |
|------|:---:|:---:|:---:|:---:|:---:|
| 创建项目 | ✅ | ❌ | ❌ | ❌ | ❌ |
| 创建 Issue | ✅ | ✅ | ✅ | ✅ | ❌ |
| 编辑 Issue | ✅ | ✅ | ✅ | ✅ | ❌ |
| 删除 Issue | ✅ | ❌ | ❌ | ❌ | ❌ |
| 认领 Issue | ✅ | ✅ | ✅ | ✅ | ❌ |
| 状态流转 | ✅ | ✅ | ✅ | ✅ | ❌ |
| 评论 | ✅ | ✅ | ✅ | ✅ | ❌ |
| 上传附件 | ✅ | ✅ | ✅ | ✅ | ❌ |
| 查看 Dashboard | ✅ | ✅ | ✅ | ✅ | ✅ |
| 管理用户 | ✅ | ❌ | ❌ | ❌ | ❌ |

---

## 四、Issue 工作流

### 4.1 状态定义

```
New → Suggested → Accepted → In Progress → Ready for Test → Testing → Done
  ↑       ↑           ↑                                                │
  └───────┴───────────┴──────────────── (可退回) ──────────────────────┘
```

| 状态 | 含义 | 触发者 |
|------|------|--------|
| **New** | 刚创建，未分配 | PM / QA |
| **Suggested** | 建议分配给某人 | PM / QA（可选填负责人） |
| **Accepted** | 被指派人确认接手 | Developer（认领） |
| **In Progress** | 开发中 | Developer |
| **Ready for Test** | 开发完成，待测试 | Developer |
| **Testing** | QA 测试中 | QA |
| **Done** | 已完成/关闭 | PM / QA |

### 4.2 状态流转规则

```
New → Suggested    — PM/QA 指派负责人（可选）
New → In Progress  — Developer 直接接手（跳过 Suggested）
Suggested → Accepted   — 被指派人确认
Suggested → New        — 被指派人拒绝
Accepted → In Progress — 开始开发
In Progress → Ready for Test — 开发完成
Ready for Test → Testing  — QA 开始测试
Testing → Done        — QA 测试通过
Testing → In Progress — QA 发现问题，退回
任意状态 → New        — PM 重新分配（非 Done 状态）
```

### 4.3 通知触发

| 事件 | 通知对象 | 内容 |
|------|---------|------|
| Issue → Suggested | 被指派人 | "XXX 建议分配给你：Issue 标题" |
| 被拒绝 | Issue 创建者 | "XXX 拒绝了这个任务" |
| Issue → Ready for Test | 所有 QA | "有新的待测试任务：#ID 标题" |
| 退回（Testing→In Progress） | 负责人 | "测试发现问题，请修复：#ID" |
| 新评论 | Issue 创建者+负责人 | "XXX 评论了 #ID" |

---

## 五、页面结构

### 5.1 信息架构

```
/login          — 登录页
/register       — 注册页 (MVP 可选：Admin 手动创建用户)

/ (Dashboard)   — 主 Dashboard
  ├─ 统计卡片（总 Issue/Bug/Feature/本周完成）
  ├─ 我的任务
  ├─ 状态分布
  ├─ 待处理 / 最近完成

/:projectId/issues          — Issue 列表（列表/看板双视图）
/:projectId/issues/new      — 创建 Issue
/:projectId/issues/:id      — Issue 详情 + 评论

/settings                   — 个人设置（通知偏好）
/admin                      — 管理后台（Admin 可见）
  ├─ 用户管理
  └─ 项目管理
```

### 5.2 页面布局（通用）

```
┌──────────────────────────────────────────────┐
│ Sidebar (232px)  │   Main Content            │
│                  │                          │
│  Logo            │   Header + Actions       │
│  Nav Items       │                          │
│  Projects        │   Content Area           │
│  Settings        │                          │
│  User Info       │                          │
└──────────────────────────────────────────────┘
```

---

## 六、数据模型（核心实体）

### User
```
id, name, email, password, role(Admin/PM/Developer/QA/Viewer), avatar, createdAt
```

### Project
```
id, name, description, createdAt, updatedAt
```

### Issue
```
id, projectId, type(Bug/Feature/Task/Improvement), title, description(Markdown),
status(New/Suggested/Accepted/InProgress/ReadyForTest/Testing/Done),
priority(Urgent/High/Medium/Low), assigneeId, reporterId, module, createdAt, updatedAt
```

### Comment
```
id, issueId, authorId, content(Markdown), createdAt
```

### Attachment
```
id, issueId, commentId(可选), fileName, fileUrl, fileSize, uploadedById, createdAt
```

### Notification
```
id, userId, type, content, issueId, read(boolean), createdAt
```

### Label (标签)
```
id, name, color, projectId
```

### IssueLabel (关联表)
```
issueId, labelId
```

---

## 七、验收标准

- [ ] 用户可以注册、登录、退出
- [ ] Admin 可创建项目、管理用户角色
- [ ] 所有角色可创建和查看 Issue
- [ ] Issue 支持 Markdown 描述 + 截图粘贴 + 附件上传
- [ ] Issue 完整 7 步工作流流转正常
- [ ] 认领机制（Suggested → Accepted/退回）正常
- [ ] 评论功能正常
- [ ] Dashboard 显示统计和个人任务
- [ ] 状态变更时触发浏览器通知
- [ ] Viewer 角色只读，无法编辑
- [ ] 列表筛选/搜索 + 看板视图可用

---

## 八、技术方案（概要）

| 层级 | 技术 | 理由 |
|------|------|------|
| 框架 | Next.js 15 App Router | 全栈一体，API First |
| UI | Tailwind CSS + shadcn/ui | 代码可控，风格现代 |
| 数据库 | SQLite + Prisma ORM | 零部署，类型安全 |
| 认证 | NextAuth.js | 开箱即用 |
| 文件存储 | 本地（MVP）→ 云存储（Phase 2） | 简单起步 |
| 部署 | Vercel / Docker | 灵活选择 |

---

## 九、非功能需求

- 页面加载 < 2 秒
- 支持 Chrome / Edge / Firefox 最新版
- 6 人并发无性能问题
- 移动端可基本浏览（不强求适配）

---

> 📌 **下一步**：系统架构设计 + MVP 开发迭代计划