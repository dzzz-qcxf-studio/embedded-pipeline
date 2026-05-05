# Embedded Pipeline — 嵌入式项目开发助手

## ⚙️ 配置说明

要使用完整的嵌入式项目开发工作流，您需要在Claude Code中安装以下五个技能：

1. **embedded-pipeline** - 工作流入口和调度器
   - 地址：https://github.com/dzzz-qcxf-studio/embedded-pipeline

2. **requirements-master** - 需求分析和确认（新建项目专用）
   - 地址：https://github.com/dzzz-qcxf-studio/requirements-master

3. **iteration-master** - 迭代升级（Bug修复/功能添加/硬件更换/性能优化）
   - 地址：https://github.com/dzzz-qcxf-studio/iteration-master

4. **diagram-master** - 电路图表生成
   - 地址：https://github.com/dzzz-qcxf-studio/diagram-master

5. **stm32-master** - STM32代码编译和烧录
   - 地址：https://github.com/dzzz-qcxf-studio/stm32-master

在Claude Code中安装这几个skill后，即可使用完整的嵌入式项目开发管道。

## 📋 项目简介

Embedded Pipeline 是一个专为嵌入式项目开发设计的智能技能管道系统。当用户描述嵌入式项目需求时，该系统会自动触发三步式开发流程，帮助用户从需求分析到最终实现的全过程开发。

## ✨ 核心特性

- **智能需求识别**：自动识别用户描述的嵌入式项目需求
- **目录选择**：启动时让用户选择或输入项目工作目录
- **双模式管道**：新建项目和迭代升级自动切换
- **三步式开发管道**：标准化开发流程，确保项目质量
- **四类迭代支持**：Bug修复、功能添加、硬件更换、性能优化
- **无缝技能衔接**：各步骤自动衔接，无需手动干预
- **STM32专项支持**：深度集成STM32开发工具链

## 🚀 快速开始

### 触发条件

当用户说出以下类型的描述时，系统会自动激活：

**新建项目**（完整管道）：
- "我要做一个温度监测系统"
- "用STM32做一个智能小车"
- "帮我做一个ESP32的物联网项目"
- "做一个XX装置/设备/系统"

**迭代升级**（已有项目的修改）：
- "温度传感器读数不对"（Bug修复）
- "给声控灯加一个蓝牙模块"（功能添加）
- "把DHT11换成DS18B20"（硬件更换）
- "优化一下ADC采样的精度"（性能优化）

### 开发管道流程

**新建模式：**
```
用户需求描述
  → 📂 选择工作目录（AskUserQuestion）
    → 📋 requirements-master（需求分析 + 用户确认）
      → 📊 diagram-master（电路图表生成）
        → ⚙️ stm32_master（代码编译 + 设备烧录）
```

**迭代模式：**
```
用户修改需求
  → 📂 选择工作目录（AskUserQuestion）
    → 🔄 iteration-master（迭代分析 + 分类路由）
      → 根据类型分流：
        Bug修复/性能优化 → 直接改代码 → ⚙️ stm32-master
        功能添加/硬件更换 → 合并需求 → 📊 diagram-master → ⚙️ stm32-master
```

## 📖 详细说明

### 第零步：选择工作目录

启动时弹出选择框，让用户确定项目目录：
- **当前目录**：使用 Claude Code 的当前工作目录
- **桌面 WORK 目录**：`C:\Users\ROG\Desktop\WORK`
- **自定义路径**：手动输入任意目录路径

如果用户消息中已包含明确路径（如"在 F:\projects\my-project 下做"），则跳过选择。

### 新建模式

#### 第一步：需求分析 (requirements-master)
- 解析用户需求描述
- 生成结构化需求文档
- 等待用户确认和调整

#### 第二步：图表生成 (diagram-master)
- 根据需求生成电路原理图
- 提供接线图和组件布局
- 支持多种图表格式

#### 第三步：代码实现 (stm32_master)
- 自动生成STM32项目代码
- 配置开发环境和工具链
- 编译、烧录到目标设备
- 提供调试和监控功能

### 迭代模式

#### 迭代分析 (iteration-master)
- 自动分类迭代类型（Bug修复/功能添加/硬件更换/性能优化）
- 加载已有项目上下文（requirements.json + 源码）
- 根据类型执行不同流程：
  - **Bug修复**：诊断 → 精确修复 → 重编译
  - **功能添加**：分析 → 确认 → 增量合并需求 → 更新图表 → 增量改代码 → 重编译
  - **硬件更换**：兼容性分析 → 确认 → 替换需求条目 → 更新图表 → 替换驱动 → 重编译
  - **性能优化**：分析 → 优化代码 → 重编译
- 需求变更自动备份（requirements.v{N}.json）
- 变更日志记录在 requirements.json 的 changelog 字段

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
├── SKILL.md          # 技能定义和配置（含迭代模式路由）
└── README.md         # 项目说明文档

iteration-master/
├── SKILL.md          # 迭代协议定义
├── index.js          # 入口文件
├── classifier.js     # 迭代类型分类器
├── context-loader.js # 项目上下文加载
├── requirements-merge.js  # 需求增量合并
└── iteration-prompts.js   # Prompt 模板
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