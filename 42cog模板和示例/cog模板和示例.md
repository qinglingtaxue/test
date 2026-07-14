---
name: project-cog
description: Cognitive model defining core entities and relationships
---

简明版本

 推荐：简明写法（大多数项目使用）
 
   ---
name: project-cog
description: Cognitive model defining core entities and relationships
---

# 认知模型 (Cog)

> 认知模型基于人心系统模型（感性-智商-理商），核心框架是：**智能体 + 信息 + 上下文**
>
> **核心原则**：
> - 每个实体需要：**唯一编码**（如何被识别）、**分类方式**（人类明确，避免AI乱分类）
> - 不是让你掌握很多先验知识，而是让AI意识到你是什么样的人
> - 使用XML语义闭合标签

## 推荐：简明写法

**大多数项目推荐使用这种简洁的写法**，直接描述核心实体和关系：

<cog>
本系统包括以下关键实体：
- [实体1名称]：[简要说明]
  - [子类型1]：[说明]
  - [子类型2]：[说明]
- [实体2名称]：[简要说明]
- [实体3名称]：[简要说明]
</cog>

<[实体1名称]>
- 唯一编码：[如何唯一识别，例如：UUID、slug、日期编码等]
- 常见分类：[分类1]；[分类2]；[分类3]
</[实体1名称]>

<[实体2名称]>
- 唯一编码：[如何唯一识别]
- 常见分类：[分类1]；[分类2]
</[实体2名称]>

<rel>
- [实体1]-[实体2]：[关系类型，如：一对多、多对多]（[关系说明]）
- [实体2]-[实体3]：[关系类型]（[关系说明]）
</rel>
  
  详细版本

## 可选：详细写法

**仅在需要更详细的属性定义和复杂关系时使用**：

### 核心实体

<entities>

<entity id="entity-001" type="[类型]">
  <name>[实体名称]</name>
  <description>[实体描述]</description>
  <attributes>
    <attribute name="[属性名]" type="[类型]">[描述]</attribute>
    <attribute name="[属性名]" type="[类型]">[描述]</attribute>
  </attributes>
  <identification>[如何唯一识别这个实体]</identification>
  <classification>[如何分类这个实体]</classification>
</entity>

<entity id="entity-002" type="[类型]">
  <name>[实体名称]</name>
  <description>[实体描述]</description>
  <attributes>
    <attribute name="[属性名]" type="[类型]">[描述]</attribute>
    <attribute name="[属性名]" type="[类型]">[描述]</attribute>
  </attributes>
  <identification>[如何唯一识别这个实体]</identification>
  <classification>[如何分类这个实体]</classification>
</entity>

</entities>

### 实体关系

<relationships>

<relationship id="rel-001" type="[关系类型]">
  <from>entity-001</from>
  <to>entity-002</to>
  <description>[关系描述]</description>
  <cardinality>[1:1 / 1:N / N:M]</cardinality>
</relationship>

<relationship id="rel-002" type="[关系类型]">
  <from>entity-002</from>
  <to>entity-003</to>
  <description>[关系描述]</description>
  <cardinality>[1:1 / 1:N / N:M]</cardinality>
</relationship>

</relationships>

### 上下文信息

<context>

#### 业务上下文
[描述项目的业务背景和领域知识]

#### 用户上下文
[描述目标用户的特征和使用场景]

#### 技术上下文
[描述技术选型和架构决策的背景]

</context>

## 注意事项

1. **实体完整性**：每个核心实体都要定义清楚
2. **关系明确性**：实体间的关系要明确基数
3. **分类清晰**：分类方式要让AI能够理解
4. **唯一标识**：每个实体要有明确的识别方式
5. **上下文丰富**：提供足够的背景信息帮助AI理解
