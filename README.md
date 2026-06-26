# Novel Factory v3.0 — AI小说工厂系统

[OH-Story](oh-story-claudecode-main/) × [Nuwa Writer](nuwa-skill-main/) 深度融合版。

## 架构

```
Novel Factory
├── Master Controller    ← 总调度
├── Model Adapter System ← 模型适配层 (GPT/DeepSeek/Claude/Gemini)
├── OH-Story            ← 规划引擎（世界观看/大纲/卷纲/章纲）
├── Nuwa Writer         ← 正文生成引擎
├── Evaluation System   ← 质量评审
├── Style System        ← 风格蒸馏/隔离
├── Memory System       ← 多维度记忆库
└── Editing System      ← 编辑链（审稿→去AI味→终审）
```

## 核心铁律

1. 展示，不讲述
2. 情绪优先于信息
3. 人物优先于设定
4. 冲突优先于解释
5. 不总结、不说教、不替读者思考

## 项目结构

```
wzl-story/
├── nuwa-skill-main/           ← Nuwa Writer 正文生成
├── oh-story-claudecode-main/  ← OH-Story 规划引擎
└── 说明.txt                   ← 总架构说明
```

## 开始使用

> 需要 Claude Code 环境。在本目录运行 `/story-setup` 部署基础设施。
