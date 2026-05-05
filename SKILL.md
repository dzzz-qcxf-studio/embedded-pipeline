---
name: embedded-pipeline
description: 嵌入式项目入口 — 当用户说"我要做个XX"、"用STM32做XX"、"帮我做一个XX系统"时，这是唯一的入口 skill。支持新建项目和迭代升级两种模式
---

# Embedded Pipeline — 嵌入式项目唯一入口

当用户描述嵌入式项目需求时，**本 skill 是唯一的入口**。根据用户意图自动选择**新建模式**或**迭代模式**。

## 第一步：确定工作目录

收到用户消息后，**首先确定项目的工作目录**。使用 `AskUserQuestion` 让用户选择或输入：

```
AskUserQuestion({
  questions: [
    {
      question: "请选择项目的工作目录：",
      header: "工作目录",
      options: [
        { label: "当前目录", description: "使用当前工作目录作为项目根目录" },
        { label: "桌面 WORK 目录", description: "C:\\Users\\ROG\\Desktop\\WORK" },
        { label: "自定义路径", description: "手动输入一个目录路径" }
      ],
      multiSelect: false
    }
  ]
})
```

**目录确定规则：**
- 用户选择"当前目录" → 使用 Claude Code 的当前工作目录
- 用户选择"桌面 WORK 目录" → `C:\Users\ROG\Desktop\WORK`
- 用户选择"自定义路径" → 用户输入的路径
- 如果用户消息中**已经包含明确路径**（如"在 F:\projects\my-project 下做"），则跳过此问题，直接使用该路径

**确定目录后**，将该路径记为 `projectPath`，后续所有操作都基于此路径。

## 第二步：模式判断

确定工作目录后，**判断是新建项目还是迭代升级**：

### 判断流程

```
1. 检查 projectPath 下是否存在 requirements.json
2. 如果存在 → 检查用户消息是否匹配迭代关键词
3. 两个条件同时满足 → 迭代模式
4. 否则 → 新建模式
```

### 迭代关键词

| 类型 | 关键词 |
|------|--------|
| Bug修复 | 不对/有问题/不工作/报错/不准/修复/修一下 |
| 功能添加 | 加一个/增加/加上/新增/添加/支持 |
| 硬件更换 | 换成/替换成/改用/换掉 |
| 性能优化 | 优化/改进/提升/改善/精度/速度/功耗 |

### 新建模式关键词

- "我要做个XX"、"我想做一个XX"
- "用STM32/ESP32/Arduino做一个XX"
- "帮我分析一下XX的方案"
- "做一个XX系统/装置/设备"

### 不触发管道的情况（直接回答）

- 一般技术问题："PA0 的复用功能是什么？"
- 代码调试："这段代码为什么报错？"
- 知识查询："STM32F103 有几路 ADC？"
- 明确要求只做某一步："帮我生成接线图"（直接调 diagram-master）

## 新建模式管道（原有流程）

```
用户需求
  → requirements-master（需求分析 + 用户确认）
    → diagram-master（图表生成）
      → stm32_master（编译烧录）
```

执行：`Skill('requirements-master')`

## 迭代模式管道（新增流程）

```
用户修改需求
  → iteration-master（迭代分析 + 分类路由）
    → 根据类型决定后续流程：
      Bug修复/性能优化 → 直接改代码 → stm32-master
      功能添加/硬件更换 → 合并需求 → diagram-master → stm32-master
```

执行：`Skill('iteration-master')`

## 执行方式

根据模式判断结果，立即调用对应 skill，**并将 projectPath 传递给下游 skill**：
- 新建模式：`Skill('requirements-master')`，告知 projectPath 用于保存需求文件
- 迭代模式：`Skill('iteration-master')`，告知 projectPath 用于加载已有项目

后续步骤由各子 skill 内部的工作流链指令自动推进，本 skill 不干预。

## 禁止事项

- **禁止**在迭代模式下调用 requirements-master（迭代有自己的需求合并逻辑）
- **禁止**在新建模式下调用 iteration-master
- **禁止**跳过模式判断直接进入管道
- **禁止**打乱管道顺序
