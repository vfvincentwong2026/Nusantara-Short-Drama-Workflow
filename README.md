# Nusantara-Short-Drama-Workflow
基于 Nusantara 知识图谱的 AI 短剧全自动生产流水线  
# 🎬 Nusantara Short-Drama Workflow

> 基于 Nusantara 知识图谱的 AI 短剧全自动生产流水线  
> —— 从文化素材到成片，一键式工业化生成

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![ComfyUI](https://img.shields.io/badge/ComfyUI-Integrated-purple)](https://github.com/comfyanonymous/ComfyUI)

---

## 📌 项目定位

**Nusantara Short-Drama Workflow** 是一个专为**印尼群岛（Nusantara）文化题材**设计的 AI 短剧自动化生产线。

它不只是一个普通的视频生成工具，而是：

- **与 [Nusantara 知识图谱](https://github.com/vfvincentwong2026/nusantara-open) 深度耦合**，可直接调用图谱中的历史事件、人物关系、神话设定和原创叙事暗线作为创作素材；
- **封装了业界领先的短剧生成工作流**（如剧本拆解、分镜设计、角色一致化、视频合成等），支持一键批量生产；
- **提供可视化界面与 API 双模式**，既适合创作者直接使用，也能被其他 AI Agent 集成。

**一句话概括**：*让枯燥的文化知识自动变成精彩的短剧视频。*

---

## 🚀 核心特性

| 模块 | 功能 |
|------|------|
| 📖 **智能剧本生成** | 从知识图谱提取“故事钩子”，自动生成完整剧本（含对话、旁白） |
| 🎭 **角色与资产库** | 基于图谱人物描述，生成一致的参考图，并管理服饰、道具 |
| 🎞️ **分镜与视觉化** | 自动拆解剧本为分镜脚本，生成提示词，批量产出视频片段 |
| 🎛️ **视觉一致性保障** | 集成 IP-Adapter / LoRA，确保角色、场景在多镜头中保持统一 |
| ✅ **自动化质检** | 视频时长、分辨率、人物完整性自动检测，不合格片段重试 |
| 🔗 **无缝集成知识图谱** | 通过 REST API / MCP 协议，实时查询人物、事件、地名等背景信息 |
| 🌐 **多语言支持** | 支持输出英、中、印尼语字幕和配音，适应不同发行平台 |

---

## 🧩 与 Nusantara 知识图谱的集成方式

本工作流将知识图谱视为“**世界观引擎**”，在以下关键环节使用：

1. **选材阶段**  
   - 调用图谱的 `/api/topics` 获取热门题材，或直接选取某条**叙事暗线**（例如“被遗忘的第四界”）。
   - 自动提取暗线相关的**人物清单**、**历史节点**、**地理背景**。

2. **剧本创作**  
   - 将图谱中的人物关系、性格标签、重大事件注入提示词，让大模型创作出符合文化语境的剧情。
   - 若剧本涉及特定历史事件（如 `bubat-tragedy`），自动补充背景描述，避免事实错误。

3. **角色设计**  
   - 利用图谱中的 `character` 节点属性（外貌、服饰、武器）生成高清参考图，确保形象有据可依。

4. **分镜与场景**  
   - 查询图谱中的 `location` 节点，获取地域风土细节，使场景设计更真实。

5. **衍生创作**  
   - 成片后可将视频元数据回传至图谱（可选），形成“知识 → 内容 → 知识”的闭环。

---

## 🏗️ 系统架构
┌─────────────────────────────────────────────────────────────┐
│ 用户输入 │
│ (题材选择 / 叙事暗线 / 自定义剧本) │
└─────────────────┬───────────────────────────────────────────┘
▼
┌─────────────────────────────────────────────────────────────┐
│ Nusantara 知识图谱 (外部服务) │
│ 提供：人物/事件/设定 → 通过 API / MCP 查询 │
└─────────────────┬───────────────────────────────────────────┘
▼
┌─────────────────────────────────────────────────────────────┐
│ 剧本与分镜生成模块 │
│ - 剧本大模型 (GPT/Claude/本地模型) │
│ - 结构化输出 (JSON 分镜表) │
└─────────────────┬───────────────────────────────────────────┘
▼
┌─────────────────────────────────────────────────────────────┐
│ 资产准备模块 │
│ - 角色图生成 (Stable Diffusion + LoRA) │
│ - 背景图生成 (根据 location 信息) │
└─────────────────┬───────────────────────────────────────────┘
▼
┌─────────────────────────────────────────────────────────────┐
│ ComfyUI 视频生成管线 (或调用第三方 API) │
│ - 图生视频 (SVD / H3 / Kling) │
│ - 多镜头批量渲染 │
└─────────────────┬───────────────────────────────────────────┘
▼
┌─────────────────────────────────────────────────────────────┐
│ 质检与合成 │
│ - 自动校验 + 重试 │
│ - 剪辑拼接 + 字幕/配音 │
└─────────────────┬───────────────────────────────────────────┘
▼
┌─────────────────────────────────────────────────────────────┐
│ 最终成片 (MP4) │
└─────────────────────────────────────────────────────────────┘

text

---

## 🛠 快速开始

### 前提条件
- Python 3.10+
- Docker (推荐，用于一键启动周边服务)
- 访问 [Nusantara 知识图谱 API](https://github.com/vfvincentwong2026/nusantara-open) 的权限（公开可用）
- （可选）ComfyUI 环境（若使用本地视频生成）或第三方视频生成 API Key

### 安装步骤

```bash
# 1. 克隆仓库
git clone https://github.com/vfvincentwong2026/nusantara-shortdrama-workflow.git
cd nusantara-shortdrama-workflow

# 2. 创建虚拟环境并安装依赖
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt

# 3. 配置环境变量（编辑 .env 文件）
cp .env.example .env
# 填写 NUSANTARA_API_BASE, VIDEO_GEN_API_KEY 等

# 4. 启动 Web 控制台 (可选)
python app.py
基础使用示例
python
from workflow import ShortDramaPipeline

# 初始化工作流（自动连接知识图谱）
pipeline = ShortDramaPipeline(
    knowledge_graph_endpoint="https://nusantara-open.example.com/api",
    video_gen_provider="comfyui"  # 或 "kling", "h3"
)

# 方式一：直接选择一条叙事暗线
result = pipeline.run_from_darkline("forgotten-fourth-realm")

# 方式二：自定义主题
result = pipeline.run_from_topic("满者伯夷帝国", num_episodes=5)

# 结果包含视频路径、元数据、日志
print(f"成片保存至: {result.video_path}")
更详细的用法见 文档。

📦 依赖项概览
requests – 调用知识图谱 API

openai / anthropic – 剧本生成 (可替换为本地模型)

diffusers + transformers – 图像生成 (可选)

comfyui-client – 若对接 ComfyUI 需额外安装

moviepy – 视频合成

fastapi + uvicorn – 提供 Web API (可选)

🤝 贡献指南
我们欢迎所有形式的贡献，包括但不限于：

新增视频生成引擎适配器 (如接入新的 API)

优化剧本提示词模板

完善自动化质检逻辑

增加更多知识图谱集成接口

翻译文档

请阅读 CONTRIBUTING.md 了解具体流程。

📄 许可证
本项目采用 MIT 许可证，你可以自由使用、修改和商用，但请保留版权声明。

🙏 致谢
Nusantara 知识图谱 – 提供扎实的文化数据基础

ComfyUI – 强大的节点式视频生成工具

所有参与测试与反馈的创作者

📬 联系与交流
提交 Issue 或 Discussion

加入 Telegram 群组 (链接待补充)

