---
name: 42edu:db
description: Database design skill for online education platforms
---

# 数据库设计技能 - 42edu:db

## 技能概述

专为在线教育平台设计的数据库架构技能，涵盖学员、教师、课程、作业、证书等核心业务实体的数据建模。

---

## 适用场景

- 在线教育平台数据库初始化
- 学习管理系统（LMS）数据建模
- 教育SaaS平台数据架构设计
- MOOC平台数据库优化

---

## 核心能力

### 1. 教育实体建模
- 用户体系：学员、教师、管理员
- 课程体系：课程、课时、章节
- 学习体系：报名、进度、证书
- 互动体系：作业、评论、讨论

### 2. 数据关系设计
- 一对多关系：教师-课程、课程-课时
- 多对多关系：学员-课程（通过报名表）
- 自关联关系：评论回复、课程系列

### 3. 性能优化
- 学习进度查询优化
- 课程列表分页优化
- 证书验证查询优化
- 直播数据实时写入优化

### 4. 数据安全
- 学员隐私数据加密
- 学习记录访问控制
- 敏感信息脱敏
- 数据备份策略

---

## 输入要求

### 必需文件
- `.42cog/real/real.md` - 现实约束
- `.42cog/cog/cog.md` - 认知模型

### Real约束示例
```markdown
<real>
- 学员个人信息必须加密存储
- 学习记录仅学员本人和授权教师可见
- 证书编号必须唯一且可验证
- 视频观看进度需实时保存
</real>
```

### Cog模型示例
```markdown
<cog>
- user：用户
  - student：学员
  - teacher：教师
- course：课程
- lesson：课时
- enrollment：报名记录
- homework：作业
- certificate：证书
</cog>
```

---

## 输出内容

### 1. 数据库表结构

**核心表清单**：
- `users` - 用户基础表
- `students` - 学员扩展表
- `teachers` - 教师扩展表
- `courses` - 课程表
- `lessons` - 课时表
- `enrollments` - 报名记录表
- `learning_progress` - 学习进度表
- `homeworks` - 作业表
- `homework_submissions` - 作业提交表
- `certificates` - 证书表
- `livestreams` - 直播表

### 2. 索引设计

**性能索引**：
```sql
-- 课程查询优化
CREATE INDEX idx_courses_status ON courses(status);
CREATE INDEX idx_courses_teacher ON courses(teacher_id);

-- 学习进度查询优化
CREATE INDEX idx_enrollment_student ON enrollments(student_id);
CREATE INDEX idx_progress_student_course ON learning_progress(student_id, course_id);

-- 证书验证优化
CREATE UNIQUE INDEX idx_certificate_number ON certificates(certificate_number);
```

### 3. 数据库关系图

ERD（实体关系图）包含：
- 实体定义
- 主键/外键关系
- 基数约束（1:1, 1:N, M:N）

### 4. 迁移脚本

```sql
-- 初始化脚本 init.sql
-- 迁移脚本 migrations/001_create_users.sql
-- 种子数据 seeds/dev_data.sql
```

---

## 技术规范

### 数据库选型
- **推荐**: PostgreSQL 14+
- **备选**: MySQL 8.0+
- **缓存**: Redis 6.0+

### 命名规范
- 表名：小写复数形式（`users`, `courses`）
- 字段名：小写下划线分隔（`created_at`, `user_id`）
- 索引名：`idx_表名_字段名`（`idx_users_email`）
- 外键名：`fk_表名_字段名`（`fk_enrollments_student_id`）

### 数据类型
- **ID**: `UUID` 或 `BIGINT`
- **时间**: `TIMESTAMP WITH TIME ZONE`
- **文本**: `VARCHAR(255)` 或 `TEXT`
- **枚举**: `ENUM` 或 `VARCHAR(50)`
- **JSON**: `JSONB`（PostgreSQL）

### 字段约束
- 所有表必须有 `id` 主键
- 所有表必须有 `created_at` 和 `updated_at`
- 软删除使用 `deleted_at`
- 外键必须设置级联规则

---

## 教育领域特性

### 学习进度追踪
```sql
CREATE TABLE learning_progress (
  id UUID PRIMARY KEY,
  student_id UUID NOT NULL,
  lesson_id UUID NOT NULL,
  watch_duration INT DEFAULT 0, -- 观看时长（秒）
  completion_rate DECIMAL(5,2) DEFAULT 0, -- 完成度（百分比）
  completed_at TIMESTAMP,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW(),
  FOREIGN KEY (student_id) REFERENCES students(id),
  FOREIGN KEY (lesson_id) REFERENCES lessons(id),
  UNIQUE(student_id, lesson_id)
);
```

### 证书验证系统
```sql
CREATE TABLE certificates (
  id UUID PRIMARY KEY,
  certificate_number VARCHAR(50) UNIQUE NOT NULL, -- 证书编号
  student_id UUID NOT NULL,
  course_id UUID NOT NULL,
  issue_date DATE NOT NULL,
  verify_code VARCHAR(20) UNIQUE, -- 验证码
  certificate_url TEXT, -- 证书图片URL
  created_at TIMESTAMP DEFAULT NOW(),
  FOREIGN KEY (student_id) REFERENCES students(id),
  FOREIGN KEY (course_id) REFERENCES courses(id)
);

-- 证书验证索引
CREATE INDEX idx_certificate_verify ON certificates(verify_code);
```

### 作业评分系统
```sql
CREATE TABLE homework_submissions (
  id UUID PRIMARY KEY,
  homework_id UUID NOT NULL,
  student_id UUID NOT NULL,
  content TEXT, -- 作业内容
  attachment_url TEXT, -- 附件URL
  score DECIMAL(5,2), -- 得分
  feedback TEXT, -- 教师评语
  submitted_at TIMESTAMP,
  reviewed_at TIMESTAMP,
  status VARCHAR(20) DEFAULT 'pending', -- pending/submitted/reviewed
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW(),
  FOREIGN KEY (homework_id) REFERENCES homeworks(id),
  FOREIGN KEY (student_id) REFERENCES students(id)
);
```

---

## 性能优化建议

### 分页查询优化
```sql
-- 使用游标分页代替 OFFSET
SELECT * FROM courses 
WHERE id > :last_id 
ORDER BY id 
LIMIT 20;
```

### 学习进度统计优化
```sql
-- 使用物化视图缓存统计结果
CREATE MATERIALIZED VIEW student_learning_stats AS
SELECT 
  student_id,
  COUNT(DISTINCT course_id) as enrolled_courses,
  COUNT(CASE WHEN completion_rate = 100 THEN 1 END) as completed_lessons,
  AVG(completion_rate) as avg_completion_rate
FROM enrollments e
JOIN learning_progress lp ON e.student_id = lp.student_id
GROUP BY student_id;

-- 定期刷新
REFRESH MATERIALIZED VIEW student_learning_stats;
```

### 热点数据缓存
- 热门课程信息 → Redis（TTL: 5分钟）
- 用户学习进度 → Redis（实时更新）
- 证书验证结果 → Redis（TTL: 1小时）

---

## 数据安全措施

### 敏感数据加密
```sql
-- 使用 pgcrypto 扩展
CREATE EXTENSION IF NOT EXISTS pgcrypto;

-- 加密存储手机号
INSERT INTO users (phone_encrypted) 
VALUES (pgp_sym_encrypt('13800138000', 'encryption_key'));

-- 查询时解密
SELECT pgp_sym_decrypt(phone_encrypted, 'encryption_key') as phone 
FROM users WHERE id = :user_id;
```

### 行级安全策略（RLS）
```sql
-- 启用行级安全
ALTER TABLE learning_progress ENABLE ROW LEVEL SECURITY;

-- 学员只能看自己的学习记录
CREATE POLICY student_own_progress ON learning_progress
  FOR SELECT
  USING (student_id = current_setting('app.current_user_id')::UUID);
```

---

## 使用方法

```bash
# 生成数据库设计规约
42cog g spec --role tech --skill db --plugin 42edu

# 输出文件：.42cog/spec/dev/db.spec.md
```

---

## 相关技能

- `42edu:devops` - 数据库部署和运维
- `42cog:tech:backend` - 后端API开发
- `42cog:tech:qa` - 数据库测试

---

**42edu:db** - 专业的教育平台数据库设计
