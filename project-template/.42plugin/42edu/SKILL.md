---
name: 42edu
description: Educational domain plugin for 42COG with specialized skills for online learning platforms
---

# 42edu - 在线教育领域插件

## 插件概述

42edu 是专门为在线教育领域开发的 42COG 插件，提供教育平台开发所需的专业技能集。

---

## 技能列表

### 1. db - 数据库设计技能

**目录**: `42edu/db/`

**功能**: 为在线教育平台设计数据库架构

**适用场景**:
- 学员、教师、课程等教育实体建模
- 学习进度、作业、证书等数据存储
- 直播、报名等业务数据管理

**使用方法**:
```bash
42cog g spec --role tech --skill db --plugin 42edu
```

---

### 2. devops - 运维部署技能

**目录**: `42edu/devops/`

**功能**: 为在线教育平台提供运维和部署方案

**适用场景**:
- 视频流媒体服务部署
- CDN配置和加速
- 高并发直播架构
- 数据备份和恢复

**使用方法**:
```bash
42cog g spec --role tech --skill devops --plugin 42edu
```

---

### 3. ui - 界面设计技能

**目录**: `42edu/ui/`

**功能**: 为在线教育平台设计用户界面

**适用场景**:
- 课程列表和详情页面
- 视频播放器界面
- 学习进度展示
- 直播间交互界面
- 作业提交和批改界面

**使用方法**:
```bash
42cog g spec --role design --skill ui --plugin 42edu
```

---

## 插件特色

### 教育领域专业性
- 针对在线教育的特殊需求优化
- 符合教育行业规范和最佳实践
- 支持学习路径、证书系统等教育特性

### 开箱即用
- 预配置常见教育场景
- 内置教育行业数据模型
- 提供教育平台UI组件库

### 可扩展性
- 支持自定义技能扩展
- 兼容42COG标准工作流
- 可与其他插件组合使用

---

## 安装和使用

### 安装插件

```bash
# 方式1：从本地安装
42cog plugin install .42plugin/42edu

# 方式2：从远程仓库安装
42cog plugin install 42edu
```

### 使用插件技能

```bash
# 1. 初始化项目
42cog init

# 2. 生成认知模型（使用42edu模板）
42cog g cog --plugin 42edu

# 3. 生成数据库设计规约
42cog g spec --role tech --skill db --plugin 42edu

# 4. 生成UI设计规约
42cog g spec --role design --skill ui --plugin 42edu

# 5. 生成DevOps规约
42cog g spec --role tech --skill devops --plugin 42edu

# 6. 生成实际代码
42cog g work --plugin 42edu
```

---

## 技能详细说明

### db 技能详情

**输入**:
- `real.md` - 数据安全、隐私保护等约束
- `cog.md` - 学员、教师、课程等实体定义

**输出**:
- 数据库表结构设计
- 索引优化方案
- 数据关系图
- 迁移脚本

**特性**:
- 支持PostgreSQL/MySQL
- 学习进度追踪表设计
- 证书验证表设计
- 直播数据存储优化

---

### devops 技能详情

**输入**:
- `real.md` - 性能要求、可用性约束
- `spec/dev/sys.spec.md` - 系统架构规约

**输出**:
- Docker容器配置
- Kubernetes部署清单
- CI/CD流水线配置
- 监控告警方案

**特性**:
- 视频转码服务部署
- CDN接入配置
- 负载均衡方案
- 自动扩缩容策略

---

### ui 技能详情

**输入**:
- `real.md` - 用户体验要求
- `cog.md` - 业务流程定义

**输出**:
- UI组件设计规范
- 页面布局设计
- 交互流程图
- 响应式设计方案

**特性**:
- 视频播放器组件
- 课程卡片组件
- 学习进度组件
- 直播聊天组件
- 暗黑模式支持

---

## 配置文件

### plugin.config.json

```json
{
  "name": "42edu",
  "version": "1.0.0",
  "description": "Educational domain plugin for 42COG",
  "skills": {
    "db": {
      "enabled": true,
      "path": "42edu/db"
    },
    "devops": {
      "enabled": true,
      "path": "42edu/devops"
    },
    "ui": {
      "enabled": true,
      "path": "42edu/ui"
    }
  },
  "templates": {
    "cog": "templates/education-cog.md",
    "real": "templates/education-real.md"
  }
}
```

---

## 依赖和兼容性

### 系统要求
- 42COG >= 1.0.0
- Node.js >= 18.0.0 或 Bun >= 1.0.0

### 推荐技术栈
- 前端: Next.js + React + TailwindCSS
- 后端: Node.js + Express 或 Bun
- 数据库: PostgreSQL + Redis
- 存储: 阿里云OSS 或 AWS S3
- CDN: 阿里云CDN 或 CloudFlare

---

## 扩展开发

### 添加新技能

1. 在 `42edu/` 下创建新目录
2. 创建技能配置文件 `skill.json`
3. 编写技能提示词模板
4. 更新 `SKILL.md` 文档

示例：
```bash
mkdir -p .42plugin/42edu/qa
cat > .42plugin/42edu/qa/skill.json << EOF
{
  "name": "qa",
  "description": "Quality assurance for educational platforms",
  "role": "tech"
}
EOF
```

---

## 社区和支持

### 贡献指南
欢迎为42edu插件贡献代码和建议：
- 提交Issue反馈问题
- 提交PR贡献代码
- 分享最佳实践案例

### 许可证
MIT License

---

**42edu - 让在线教育开发更简单**
