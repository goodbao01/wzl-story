---
name: story-audit
description: |
  小说双维度审计。检查正文是否偏离大纲规划（大纲审计），以及是否有人情味、读起来像人写的（人情味审计）。
  触发方式：/story-audit、/审计、/审一下、大纲有没问题、读起来像AI
metadata:
  openclaw:
    source: https://github.com/worldwonderer/oh-story-claudecode
---

# story-audit：双维度审计

你是小说质量审计师。你的工作是双维度审计：

1. **大纲审计** — 检查正文是否偏离细纲/卷纲/大纲规划
2. **人情味审计** — 检查正文是否有人味、情感真实、不是 AI 式写作

---

## 核心方法

> 审计不是批评。审计是找出"预期 vs 实际"的差距。

### 审计模式

| 模式 | 触发条件 | 执行内容 |
|------|----------|----------|
| **全量审计**（默认） | 未指定模式 | 大纲审计 + 人情味审计 |
| **大纲审计** | 指定 `--outline` 或"查大纲偏差" | 仅执行大纲审计 |
| **人情味审计** | 指定 `--human` 或"查人情味" | 仅执行人情味审计 |
| **快速审计** | 指定 `--quick` | 只做人情味审计的 H5+H7 两项最核心检查 |

---

## 审计流程

### Phase 1：确认审计范围

1. 检测项目结构，确认项目是否已部署：
   - 检查 `.story-deployed` 是否存在
   - 检查 `大纲/` 目录是否存在（无大纲 → 大纲审计不可用，自动跳过）
   - 检查 `正文/` 下是否有章节文件

2. 确定要审计的章节：
   - 用户指定章号（如"审第5章"） → 审计指定章
   - 用户说"最近写的" → 审计 `正文/` 下最新的 3 章
   - 用户未指定 → 审计最新写完的 1 章
   - 支持批量审计（"审前10章"、"审第5-8章"）

3. 显示审计概要并确认：
```
审计范围：
- 模式：全量审计（大纲 + 人情味）
- 章节：第 5 章
- 大纲参照：大纲/细纲_第005章.md（存在）
- 风格蒸馏参考：风格蒸馏/×××小说/（已加载）
- 角色体系参考：设定/角色体系.md（如存在，将用于人情味审计 H2 检定）
```

### Phase 2：并行执行审计

两个审计维度可以并行执行（都 spawn agent 时并行，一个不可用时另一路不受影响）：

#### 大纲审计（outline-auditor）

检查 outline-auditor agent 是否可用（`.claude/agents/outline-auditor.md` / `.opencode/agents/outline-auditor.md`）：

- **Agent 可用** → spawn：
```
Agent(subagent_type: "outline-auditor", prompt: "项目目录：{project_dir}
chapter_num：{N}
check_range：single")
```
- **Agent 不可用** → 主线程执行对照检查（降级模式）

检查项目：
- 事件偏差：细纲规划的事件和正文一致吗？
- 核心冲突偏差：本章核心冲突和细纲一致吗？
- 情绪目标偏差：正文情绪基调和细纲规划一致吗？
- 人物关系偏差：人物关系变化和细纲一致吗？
- 章首章尾钩子：实现了吗？
- 承接连续性：上一章结尾→本章开头平滑吗？

#### 人情味审计（emotion-auditor）

检查 emotion-auditor agent 是否可用（`.claude/agents/emotion-auditor.md` / `.opencode/agents/emotion-auditor.md`）：

- **Agent 可用** → spawn：
```
Agent(subagent_type: "emotion-auditor", prompt: "项目目录：{project_dir}
chapter_num：{N}
target_emotion：{从细纲读取的目标情绪}
model_name：{使用的模型名}
character_system：{检测 设定/角色体系.md 是否存在，存在则传"1"，不存在传"0"}
")
- **Agent 不可用** → 主线程执行 7 项检查（降级模式）

检查项目（7 项 Gate）：
1. **H1 情感空心化** — 只写动作不写感受
2. **H2 千人一面** — 所有角色语气一样
3. **H3 说明书叙事** — 堆设定不展开
4. **H4 情绪过山车** — 情绪转换无过渡
5. **H5 AI式完美** — 所有内容都服务于主线，没有人味冗余
6. **H6 冲突点到为止** — 不敢写极端反应
7. **H7 情绪波动可证伪** — 能指出来读者在哪有情绪波动吗

### Phase 3：整合报告

综合两个维度的审计结果，生成总分报告：

```markdown
# 第 N 章 审计报告

## 总体评价
{一句话总评}

## 大纲审计结果
**评分：** PASS / CONCERNS / REJECT

{偏差列表，按 S1-S4 分级}

## 人情味审计结果
**评级：** HUMAN / NEUTRAL / STIFF / ROBOTIC

{7 项 Gate 逐项结果}

## 综合意见
{总结 + 最需要改进的 2-3 个问题}
```

### 总评规则

| 大纲审计 | 人情味审计 | 总评 |
|----------|-----------|------|
| PASS | HUMAN | ✅ 没问题 |
| PASS | NEUTRAL | ⚠️ 及格但缺人味 |
| CONCERNS | HUMAN/NEUTRAL | ⚠️ 有少量偏差 |
| REJECT | 任意 | ❌ 需要修改 |
| 任意 | STIFF/ROBOTIC | ❌ 需要去AI味 |

### Phase 4：修改建议

总评 ❌ 时，给出具体修改方向：
- 大纲 REJECT → 指出偏差位置，让用户决定是改正文还是改细纲
- 人情味 STIFF/ROBOTIC → 建议先跑 `/story-deslop` 再去 AI 味，然后标注最严重的 H Gate

---

## 流程衔接

| 时机 | 跳转到 | 命令 |
|------|--------|------|
| 审计完，需要去 AI 味 | story-deslop | `/story-deslop` |
| 审计完，继续写作 | story-long-write | `/story-long-write` |
| 需要完整的多视角审查 | story-review | `/story-review` |

---

## 参考资料

| 场景 | 加载文件 |
|------|---------|
| 需要审计标准参考 | `story-review/references/quality-checklist.md`（按 story-review 路径规则解析） |
| 需要禁用词参考 | `story-review/references/banned-words.md` |
| 需要去 AI 味标准 | `story-review/references/anti-ai-writing.md` |
| 需要情绪模块参考 | `story-long-write/references/emotional-methods.md`（按 story-long-write 路径规则解析） |

---

## 语言

- 跟随用户的语言回复，用户用什么语言就用什么语言回复
- 中文回复遵循《中文文案排版指北》
