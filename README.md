# ProjectLearn - 智能知识学习与评测系统

> 导入文档 → AI 自动构建知识体系 → 智能评测 → 掌握度跟踪

ProjectLearn 是一款 Windows 桌面应用，能够将你的学习资料（TXT、PDF、Word、PPT、Excel 等）自动转化为结构化的知识体系，并利用大语言模型（LLM）进行智能问答与知识掌握度评估。

## ✨ 核心功能

| 功能 | 说明 |
|------|------|
| 📂 **多格式文档导入** | 支持 TXT、PDF、DOCX、PPTX、XLSX 等主流格式 |
| 🤖 **AI 知识提取** | 自动分析文档，提取学科分类、章节结构和知识点 |
| 📚 **三级知识体系** | 类别 → 章节 → 知识点，层次清晰的知识组织 |
| 🔍 **RAG 智能问答** | 检索增强生成，基于文档内容精准回答问题 |
| 📝 **知识检测** | 多种出题模式（顺序/随机/易错点），自定义题目范围 |
| 📊 **掌握度评估** | 五级掌握度（未掌握/了解/理解/熟练/精通），可视化柱状图 |
| 📈 **学习报告** | AI 生成的综合性学习评估报告 |
| 🔗 **多 LLM 支持** | 内置 DeepSeek、通义千问、智谱、Moonshot、百度千帆、OpenAI 等预设 |

## 🚀 快速开始

### 1. 环境要求

- Windows 7+ (64位)
- Visual C++ 2019+ 运行时库（若缺少，请安装 [VC_redist.x64.exe](https://aka.ms/vs/17/release/vc_redist.x64.exe)）

### 2. 配置 LLM API

1. 启动程序后，点击工具栏的 **设置** 按钮
2. 选择一个 LLM 提供商预设（如 DeepSeek、通义千问等）
3. 填入你的 API Key
4. 点击 **测试连接**，确认配置正确
5. 保存配置

或者直接编辑 `data/config.json`：

```json
{
  "api_endpoint": "https://api.deepseek.com",
  "api_key": "your-api-key-here",
  "model": "deepseek-chat",
  "provider": "DeepSeek"
}
```

### 3. 开始使用

1. **上传文档**：切换到「文件管理」Tab，点击上传按钮，选择学习资料
2. **分析文档**：选中已上传的文件，点击分析按钮，AI 将自动提取知识点和题目
3. **知识检测**：切换到「知识检测」Tab，选择题库范围，开始答题
4. **查看评估**：切换到「知识评估」Tab，查看掌握度图表和评估报告
5. **浏览知识**：切换到「知识分类」Tab，搜索和浏览知识体系

## 📖 功能详解

### 文件管理
- 支持拖拽或点击上传文件
- 右键菜单可执行：上传、分析、查看原文、删除
- 支持多选文件批量分析

### 知识检测
- **三种出题模式**：
  - 顺序：按题库顺序出题
  - 随机：随机抽取题目
  - 易错点：优先出掌握度低的题目
- **范围控制**：精确控制题目范围，如 `1-40;43-50` 表示第1-40题和第43-50题
- **命名范围**：保存常用范围配置，方便快速切换
- **实时评分**：每题提交后立即获得 AI 评分和评语

### 知识评估
- 掌握度柱状图直观展示各等级题目数量
- 按知识点查看详细得分记录
- AI 生成的综合评估报告，分析薄弱环节
- 树形视图浏览完整的知识目录

### 知识分类
- 三级树形结构展示所有知识点
- 全文搜索 + 语义检索两种模式
- 知识点内容支持富文本查看和编辑

## 🛠️ 从源码构建

### 前提条件

- Visual Studio 2019 或 2022
- Windows SDK 10.0+

### 构建步骤

```powershell
# 1. 克隆仓库
git clone <your-repo-url>
cd learn

# 2. 用 Visual Studio 打开解决方案
start project_learn.sln

# 3. 选择 Release | x64 配置

# 4. 生成 → 生成解决方案 (Ctrl+Shift+B)
```

输出文件位于 `Release/` 目录。

## 📁 项目结构

```
project_learn/          # 源代码
├── project_learn.cpp   # 程序入口
├── MainWindow.h        # 主窗口 GUI（Win32 API）
├── KnowledgeEngine.h   # 核心知识引擎（文件分析/问答/评测）
├── LLMClient.h         # LLM API 客户端
├── RAGEngine.h         # RAG 检索增强引擎（分块/嵌入/向量检索）
├── HttpClient.h        # HTTP 客户端（WinHTTP）
├── ConfigManager.h     # 配置管理
├── JsonHelper.h        # JSON 解析
├── FileHandler.h       # 文件处理
├── Storage.h           # 数据存储（JSON 读写）
├── KnowledgeDB.h       # SQLite 数据库管理
├── OfficeExtractor.h   # Office 文档文本提取
├── PdfTextExtractor.h  # PDF 文本提取
└── sqlite3.h/c         # SQLite3 嵌入式数据库

Release/data/           # 运行时数据目录
├── config.json         # LLM API 配置
├── qa_history.json     # 问答历史
├── mastery.json        # 掌握度数据
├── knowledge_catalog.json  # 知识目录
└── knowledge.db        # 知识点数据库
```

## 🔧 技术架构

```
┌───────────────┐     ┌─────────────────┐     ┌─────────────┐
│   Win32 GUI   │────▶│ KnowledgeEngine │────▶│  LLMClient  │
│  (4个Tab页)   │     │   (核心引擎)    │     │  (API调用)  │
└───────────────┘     └───────┬─────────┘     └─────────────┘
                              │
              ┌───────────────┼───────────────┐
              ▼               ▼               ▼
       ┌──────────┐   ┌────────────┐   ┌────────────┐
       │RAGEngine │   │  Storage   │   │KnowledgeDB │
       │(向量检索) │   │ (JSON存储) │   │ (SQLite)   │
       └──────────┘   └────────────┘   └────────────┘
```

## 📋 支持的 LLM 提供商

| 提供商 | 默认模型 |
|--------|----------|
| DeepSeek | deepseek-chat |
| 通义千问 | qwen-plus |
| 智谱 GLM | glm-4-flash |
| Moonshot | moonshot-v1-8k |
| 百度千帆 | ernie-speed-128k |
| OpenAI | gpt-3.5-turbo |
| 自定义 | 自行填写 |

> 注：Embedding 功能固定使用 `text-embedding-3-small` 模型，如果你的 LLM 提供商不支持该模型，RAG 语义检索功能将不可用。

## ⚠️ 注意事项

1. **API Key 安全**：API Key 以明文存储在本地 `config.json` 中，请勿分享该文件
2. **网络要求**：需要能够访问所选 LLM 提供商的 API 端点
3. **文档大小**：Office 文档解压限制 64MB，PDF 限制 8MB
4. **响应格式**：依赖 LLM 返回标准 JSON 格式，格式异常可能导致解析失败
5. **Embedding 模型**：向量嵌入使用 text-embedding-3-small，请确认你的提供商支持该模型

## 📄 许可证

本项目仅供学习和个人使用。

---

详细的架构说明和开发文档请查看 [说明书.md](./说明书.md)
