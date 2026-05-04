# Embedded Pipeline — 嵌入式项目开发助手

## ⚙️ 配置说明

要使用完整的嵌入式项目开发工作流，您需要在Claude Code中安装以下四个技能：

1. **embedded-pipeline** - 工作流入口和调度器
   - 地址：https://github.com/dzzz-qcxf-studio/embedded-pipeline

2. **requirements-master** - 需求分析和确认
   - 地址：https://github.com/dzzz-qcxf-studio/requirements-master

3. **diagram-master** - 电路图表生成
   - 地址：https://github.com/dzzz-qcxf-studio/diagram-master

4. **stm32-master** - STM32代码编译和烧录
   - 地址：https://github.com/dzzz-qcxf-studio/stm32-master

在Claude Code中安装这几个skill后，即可使用完整的嵌入式项目开发管道。

## 📋 项目简介

Embedded Pipeline 是一个专为嵌入式项目开发设计的智能技能管道系统。当用户描述嵌入式项目需求时，该系统会自动触发三步式开发流程，帮助用户从需求分析到最终实现的全过程开发。

## ✨ 核心特性

- **智能需求识别**：自动识别用户描述的嵌入式项目需求
- **三步式开发管道**：标准化开发流程，确保项目质量
- **无缝技能衔接**：各步骤自动衔接，无需手动干预
- **STM32专项支持**：深度集成STM32开发工具链

## 🚀 快速开始

### 触发条件

当用户说出以下类型的描述时，系统会自动激活：

- "我要做一个温度监测系统"
- "用STM32做一个智能小车"
- "帮我做一个ESP32的物联网项目"
- "做一个XX装置/设备/系统"

### 开发管道流程

```
用户需求描述
  → 📋 requirements-master（需求分析 + 用户确认）
    → 📊 diagram-master（电路图表生成）
      → ⚙️ stm32_master（代码编译 + 设备烧录）
```

## 📖 详细说明

### 第一步：需求分析 (requirements-master)
- 解析用户需求描述
- 生成结构化需求文档
- 等待用户确认和调整

### 第二步：图表生成 (diagram-master)
- 根据需求生成电路原理图
- 提供接线图和组件布局
- 支持多种图表格式

### 第三步：代码实现 (stm32_master)
- 自动生成STM32项目代码
- 配置开发环境和工具链
- 编译、烧录到目标设备
- 提供调试和监控功能

## ⚠️ 使用限制

**不适用场景**（直接回答，不进入管道）：
- 技术细节查询："PA0引脚的功能是什么？"
- 代码调试："这段代码为什么报错？"
- 硬件规格查询："STM32F103有几路ADC？"
- 单步骤需求："只帮我生成接线图"

## 🔧 系统要求

- VS Code 编辑器
- 支持的嵌入式平台：STM32、ESP32、Arduino等
- 相关开发工具链已安装

## 📁 项目结构

```
embedded-pipeline/
├── SKILL.md          # 技能定义和配置
└── README.md         # 项目说明文档
```

## 🤝 贡献指南

欢迎提交Issue和Pull Request来改进这个项目！

## 📄 许可证

本项目采用 MIT 许可证 - 查看 [LICENSE](LICENSE) 文件了解详情

## 🆘 问题反馈

如果您在使用过程中遇到问题，请：

1. 检查触发条件是否符合要求
2. 确认相关工具链已正确安装
3. 提交详细的Issue描述

---

**注意**：这是一个VS Code技能系统的一部分，需要在相应的技能框架中运行。
