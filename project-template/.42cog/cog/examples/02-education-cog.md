---
name: education-cog
description: Cognitive model for online education platform with courses, lessons, homework, livestreams and certificates
---

# 在线教育系统 - 认知模型 (Cog)

## 系统概述

这是一个在线教育平台，支持课程发布、学员学习、作业提交、直播授课和证书颁发。

---

## 核心实体

<cog>
本系统包括以下关键实体：
- user：用户
  - student：学员，一种特殊的user
  - teacher：教师，一种特殊的user
  - admin：管理员，一种特殊的user
- course：课程
- lesson：课时
- enrollment：报名记录
- homework：作业
- certificate：证书
- livestream：直播
</cog>

---

## 实体详细定义

### user（用户）

<user>
- 唯一编码：按照注册时间次序生成的UUID号
- 常见分类：游客；注册用户；付费用户；VIP会员
</user>

### student（学员）

<student>
- 唯一编码：继承user的UUID，额外有学号（如 S2025001）
- 常见分类：试听学员；正式学员；VIP学员；企业学员
- 关键行为：浏览课程、报名、学习、提交作业
- 关键属性：学习进度、已完成课程数、证书数量、学习积分
</student>

### teacher（教师）

<teacher>
- 唯一编码：教师工号（如 T001, T002）
- 常见分类：专职讲师；兼职讲师；助教；特邀讲师
- 关键属性：真实姓名、专业领域、教学经验、认证资质
- 关键行为：创建课程、上传课时、批改作业、开设直播
</teacher>

### course（课程）

<course>
- 唯一编码：课程编号（如 C20251204001）
- 常见分类：免费课程；付费课程；会员课程；企业定制课程
- 关键属性：课程名称、简介、封面、价格
- 状态：草稿、审核中、已发布、已下架
</course>

### lesson（课时）

<lesson>
- 唯一编码：课时编号（课程编号 + 序号，如 C20251204001-L01）
- 常见分类：视频课时；音频课时；图文课时；直播回放
- 关键属性：课时标题、时长、资源URL、是否试听
- 状态：草稿、已发布、维护中
</lesson>

### enrollment（报名记录）

<enrollment>
- 唯一编码：报名记录ID（UUID）
- 关键属性：学员ID、课程ID、报名时间、过期时间
- 状态：学习中、已完成、已过期
- 学习进度：百分比（0-100%），根据已学课时数/总课时数计算
</enrollment>

### homework（作业）

<homework>
- 唯一编码：作业ID（UUID）
- 常见分类：课时作业；阶段作业；期末作业；项目作业
- 关键属性：课程ID、课时ID、作业标题、作业要求
- 提交状态：未提交、已提交、已批改、已逾期
</homework>

### certificate（证书）

<certificate>
- 唯一编码：证书编号（如 CERT-20251204-001）
- 常见分类：课程完成证书；考试合格证书；技能认证证书
- 关键属性：学员ID、课程ID、颁发时间、验证码
- 验证方式：可通过官网输入证书编号验证真伪
</certificate>

### livestream（直播）

<livestream>
- 唯一编码：直播ID（UUID）
- 常见分类：公开课；正式课程直播；答疑直播；研讨会
- 关键属性：课程ID、教师ID、直播标题、开始时间
- 状态：预告、直播中、已结束、回放已生成
</livestream>

---

## 关系模型

<rel>
- teacher-course：一对多（一个教师可创建多门课程）
- course-lesson：一对多（一个课程包含多个课时）
- student-course：多对多（通过enrollment连接）
- student-homework：多对多（一个学员提交多个作业）
- teacher-homework：一对多（一个教师批改多个作业）
- student-certificate：多对多（一个学员可获得多个证书）
- course-livestream：一对多（一门课程可有多场直播）
</rel>

---

## 业务规则

<business-rule>
1. 报名规则：免费课程可直接学习；付费课程需先付款；会员课程需会员权限
2. 学习进度：看完视频的80%以上才算完成该课时；所有课时完成才算完成课程
3. 作业规则：必须完成前置课时才能提交作业；逾期提交作业扣除30%分数
4. 证书规则：完成课程且总分≥60分才能获得证书；证书永久有效
</business-rule>

---

## 特殊功能

<special-feature>
1. 学习路径：可为学员推荐学习路径，按顺序学习一系列相关课程
2. 学习社区：学员可在课程讨论区提问和交流，教师或助教回答
3. 学习报告：定期生成学习报告，包括学习时长、完成课程数、学习排名
4. 积分系统：学习课时、完成作业、参与讨论可获得积分，积分可兑换优惠券
</special-feature>
