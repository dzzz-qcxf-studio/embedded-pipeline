---
name: embedded-pipeline
description: 嵌入式项目入口 — 当用户说"我要做个XX"、"用STM32做XX"、"帮我做一个XX系统"时，这是唯一的入口 skill。串联 requirements-master → diagram-master → stm32_master 三步管道
---

# Embedded Pipeline — 嵌入式项目唯一入口

当用户描述嵌入式项目需求时，**本 skill 是唯一的入口**。由它调度后续三个 skill，确保管道不中断。

## 触发条件

用户描述嵌入式项目需求时**自动触发**（唯一入口），匹配模式：
- "我要做个XX"、"我想做一个XX"
- "用STM32/ESP32/Arduino做一个XX"
- "帮我分析一下XX的方案"
- "做一个XX系统/装置/设备"

**不触发**的情况（直接回答，不进管道）：
- 一般技术问题："PA0 的复用功能是什么？"
- 代码调试："这段代码为什么报错？"
- 知识查询："STM32F103 有几路 ADC？"
- 明确要求只做某一步："帮我生成接线图"（直接调 diagram-master）

## 管道顺序（严格执行）

```
用户需求
  → requirements-master（需求分析 + 用户确认）
    → diagram-master（图表生成）
      → stm32_master（编译烧录）
```

## 执行方式

立即调用第一步：

```
Skill('requirements-master')
```

后续步骤由各子 skill 内部的工作流链指令自动推进，本 skill 不干预。

## 禁止事项

- **禁止**跳过 requirements-master 直接写代码或生成图表
- **禁止**打乱管道顺序
