# Novel Factory v3.0 — AI小说工厂系统

[OH-Story](oh-story-claudecode-main/)（规划引擎）× [Nuwa Writer](nuwa-skill-main/)（正文生成引擎）深度融合版。

基于 Claude Code 的网文写作全流程工具链，覆盖扫榜、拆文、长篇/短篇写作、去AI味、封面生成。

## 快速安装

### 前置条件

- 安装 [Claude Code](https://claude.ai/code)（推荐）或 OpenCode
- 确保 Claude Code 能联网

### 安装步骤

```bash
# 1. 克隆仓库
git clone git@github.com:goodbao01/wzl-story.git
cd wzl-story

# 2. 在 Claude Code 中打开本目录
claude .

# 3. 在 Claude Code 中执行部署命令
/story-setup
```

Claude Code 会自动部署 hooks、agents、rules 等全套写作基础设施。

### 可选：只安装 OH-Story（不包含 Nuwa Writer）

如果只需要 OH-Story 的扫榜/拆文/写作流程，也可以单独安装：

```bash
git clone https://github.com/worldwonderer/oh-story-claudecode.git
# 在 Claude Code 中运行 /story-setup
```

## 系统架构

```
Novel Factory
├── Master Controller           ← 总调度
├── Model Adapter System        ← 模型适配层
│   ├── GPT Adapter             ← 修复解释/说教/过度正确
│   ├── DeepSeek Adapter        ← 修复人设漂移/桥段重复
│   ├── Claude Adapter          ← 修复节奏慢/文艺病
│   └── Gemini Adapter          ← 修复说明书式写作/设定堆砌
│
├── OH-Story                    ← 规划引擎
│   ├── 世界观搭建
│   ├── 力量体系
│   ├── 主线/支线
│   ├── 卷纲
│   └── 章纲
│
├── Writing System              ← 写作引擎
│   ├── Nuwa Writer             ← 正文生成（3000-5000字/章）
│   ├── Scene Generator         ← 场景生成
│   └── Dialogue Generator      ← 对白生成
│
├── Editing System               ← 编辑链
│   ├── Story Auditor            ← 剧情漏洞/逻辑检查
│   ├── Human Editor             ← 去AI味
│   ├── Emotion Editor           ← 情绪增强
│   └── Final Editor             ← 终审评分
│
├── Evaluation System            ← 质量评估
│   └── Reader Simulator         ← 读者模拟评分
│
├── Memory System                ← 多维度记忆
│   ├── Book/Character/Relationship/World
│   ├── Item/Event/Timeline
│   └── Foreshadow Memory
│
└── Style System                 ← 风格管理
    ├── Style Distiller          ← 风格蒸馏
    ├── Author DNA               ← 作者偏好记录
    └── Style Guard              ← 防串书守卫
```

## 核心铁律

1. **展示，不讲述** —— Show, don't tell
2. **情绪优先于信息**
3. **人物优先于设定**
4. **冲突优先于解释**
5. **对话优先于旁白**
6. **不总结、不说教、不替读者思考**
7. **一本书一个记忆库，一本书一个风格库** —— 禁止跨书污染
8. **冲突密度高于信息密度**

## 可用命令

| 命令 | 功能 |
|------|------|
| `/story-setup` | 部署写作基础设施 |
| `/story-long-scan` | 长篇扫榜（起点/番茄排行） |
| `/story-short-scan` | 短篇扫榜（知乎/七猫/黑岩） |
| `/story-long-analyze` | 长篇拆文（黄金三章/节奏/爽点） |
| `/story-short-analyze` | 短篇拆文 |
| `/story-long-write` | 长篇写作（大纲→日更→续写） |
| `/story-short-write` | 短篇写作 |
| `/story-deslop` | 去AI味 |
| `/story-audit` | 双维度审计（大纲/人情味） |
| `/story-review` | 多视角对抗式审查 |
| `/story-cover` | 封面生成 |
| `/story-import` | 导入已有小说 |
| `/story-style-distill` | 风格蒸馏 |

## 项目结构

```
wzl-story/
├── oh-story-claudecode-main/   ← OH-Story 规划引擎 + 全部 skill 命令
│   └── skills/
│       ├── story-setup/        ← 基础设施部署
│       ├── story-long-write/   ← 长篇写作
│       ├── story-short-write/  ← 短篇写作
│       ├── story-long-scan/    ← 长篇扫榜
│       └── ...                 ← 其余技能
├── nuwa-skill-main/            ← Nuwa Writer 正文生成引擎
└── 说明.txt                    ← 总架构说明
```

## 维护者

- **OH-Story** — 原作：[worldwonderer](https://github.com/worldwonderer/oh-story-claudecode)
- **Nuwa Writer** — 原作：[huashu](https://github.com/nuwa-skill)
- **Novel Factory 融合版** — [Goodbao01](https://github.com/goodbao01/wzl-story)
