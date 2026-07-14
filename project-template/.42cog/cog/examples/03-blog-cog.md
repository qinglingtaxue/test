---
name: blog-cog
description: Cognitive model for multi-user blog platform with posts, comments, tags, categories and subscriptions
---

# 博客系统 - 认知模型 (Cog)

## 系统概述

这是一个多用户博客平台，支持文章发布、评论互动、标签分类、订阅关注和RSS输出。

---

## 核心实体

<cog>
本系统包括以下关键实体：
- user：用户
  - author：作者，一种特殊的user
  - reader：读者，一种特殊的user
  - admin：管理员，一种特殊的user
- post：文章
- comment：评论
- category：分类
- tag：标签
- subscription：订阅
</cog>

---

## 实体详细定义

### user（用户）

<user>
- 唯一编码：按照注册时间次序生成的UUID号
- 常见分类：游客（匿名访问）；注册用户；认证作者；管理员
- 关键属性：用户名（唯一slug）、邮箱、头像、个人简介、社交链接
</user>

### author（作者）

<author>
- 唯一编码：继承user的UUID，有独立的作者主页slug（如 yangzhiping）
- 常见分类：普通作者；认证作者；特邀作者；专栏作者
- 认证标识：蓝V认证（个人）；黄V认证（机构）；红V认证（官方）
- 关键属性：笔名、专业领域、文章数、粉丝数、总阅读量
</author>

### reader（读者）

<reader>
- 唯一编码：继承user的UUID
- 常见分类：游客（未注册）；注册读者；付费会员
- 关键行为：阅读文章、点赞、评论、收藏、关注作者、订阅专栏
</reader>

### post（文章）

<post>
- 唯一编码：文章slug（URL友好格式，如 ai-product-development-guide）
- 常见分类：原创文章；转载文章；翻译文章；草稿
- 关键属性：标题、正文（Markdown）、摘要、封面图、作者ID、发布时间、更新时间
- 状态：草稿、已发布、已隐藏、已删除
- 可见性：公开、仅粉丝可见、付费可见、密码保护
- 统计数据：阅读量、点赞数、评论数、收藏数、分享数
</post>

### comment（评论）

<comment>
- 唯一编码：评论ID（UUID）
- 常见分类：文章评论；回复评论（支持多级嵌套）
- 关键属性：用户ID、文章ID、评论内容、发布时间、父评论ID（如果是回复）
- 状态：正常、待审核、已隐藏、已删除
- 审核规则：新用户评论需审核；老用户评论自动通过；敏感词触发审核
</comment>

### category（分类）

<category>
- 唯一编码：分类slug（如 technology, design, business）
- 关键属性：分类名称、描述、图标、排序权重、父分类ID（支持多级分类）
- 层级结构：最多3级（如 技术 > 前端开发 > React）
</category>

### tag（标签）

<tag>
- 唯一编码：标签slug（如 ai, product, nextjs）
- 关键属性：标签名称、使用次数、创建时间
- 分类：热门标签（使用次数>100）；普通标签
- 管理规则：作者可自由创建标签；管理员可合并重复标签
</tag>

### subscription（订阅）

<subscription>
- 唯一编码：订阅ID（UUID）
- 常见分类：关注作者；订阅专栏；订阅分类；订阅标签
- 关键属性：订阅者ID、订阅目标ID、订阅类型、订阅时间
- 通知方式：站内信、邮件、RSS
</subscription>

---

## 关系模型

<rel>
- author-post：一对多（一个作者可发布多篇文章）
- post-comment：一对多（一篇文章可有多条评论）
- comment-comment：一对多（一条评论可有多条回复，支持嵌套）
- reader-post：多对多（点赞、收藏关系）
- reader-author：多对多（关注关系，通过subscription连接）
- post-category：多对一（一篇文章属于一个主分类）
- post-tag：多对多（一篇文章可有多个标签）
</rel>

---

## 业务规则

<business-rule>
1. 发布规则：草稿可保存无限次；发布后可修改但记录修改历史
2. 评论规则：支持3级嵌套回复；评论可编辑但显示"已编辑"标记
3. 点赞规则：每个用户对每篇文章只能点赞一次；可取消点赞
4. 收藏规则：文章被删除后收藏记录保留但文章不可见
5. 关注规则：关注后可收到作者新文章通知；可设置静默关注（不通知）
6. 标签规则：每篇文章最多10个标签；标签名称2-20字符；不允许纯数字标签
7. SEO规则：文章slug唯一且永久；修改标题不影响slug；支持301重定向
</business-rule>

---

## 特殊功能

<special-feature>
1. Markdown编辑器：支持实时预览、代码高亮、图片上传、表格、数学公式
2. 文章系列：多篇文章可组织成系列（如"React入门教程"系列）
3. 阅读统计：记录阅读时长、阅读完成度、阅读来源
4. RSS输出：支持全站RSS、分类RSS、作者RSS、标签RSS
5. 搜索功能：全文搜索，支持标题、正文、标签、作者搜索
6. 推荐算法：根据阅读历史、关注作者、标签偏好推荐文章
7. 暗黑模式：支持自动切换、手动切换
</special-feature>
