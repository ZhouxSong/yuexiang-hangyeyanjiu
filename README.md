# 行业深度研究报告方法论（PE 实战工作流 + 自进化闭环）

> WorkBuddy / Claude 风格 Agent 的技能包：面向私募股权投资总监的行业研究日常工作方法论。
> **多智能体编排执行引擎 + 行业生命周期模板体系 + "终版 diff → 技能自进化"闭环**。

## 这个技能解决什么

把"写一篇万字级行业深度报告"变成一条有纪律、可复核、能进化的流水线：

1. **多渠道备料**：按渠道并行派发检索子 agent（论文/行业报告/白皮书/书籍/招股书与年报/券商报告/政策监管）
2. **公司图谱**：上市公司（代码+连续口径财务）与非上市公司（融资轮次+估值）统一梳理
3. **《撰写前研究报告》交用户审核**：范围界定 + 资料汇编 + 公司图谱 + 参考文献逐篇评估 + 初步提纲
4. **审核通过后撰写初版**：逐章并行撰写 + **逐章对抗审查**（挑战清单提交用户裁决）
5. **用户改为终版**后，技能 diff 初版与终版、提炼可泛化规律、经批准后写入技能自身（自进化）

**八条铁律**：先备料后动笔、数据必核实、口径必标注、缺失必说明、引用必规范（GB/T 7714—2015）、
绝不外包理解、对抗审查不越权、进化须批准。

## 安装

### WorkBuddy 用户（推荐）

```bash
# 用户级（所有项目可用）
git clone https://github.com/ZhouxSong/yuexiang-hangyeyanjiu.git \
    ~/.workbuddy/skills/yuexiang-hangyeyanjiu1.1

# 或项目级
git clone https://github.com/ZhouxSong/yuexiang-hangyeyanjiu.git \
    <你的工作区>/.workbuddy/skills/yuexiang-hangyeyanjiu1.1
```

安装后重启/刷新会话，发送「写一篇 XX 行业深度研究报告」等指令即可触发。

### 其他兼容 Agent（Claude Code 等）

本技能遵循通用 `SKILL.md` 规范（YAML frontmatter + Markdown 正文 + `references/`），
放入对应 Agent 的 skills 目录即可。**本技能为纯方法论，无自带脚本**；
其中"多智能体编排"部分要求宿主 Agent 支持子代理（subagent）派发机制，
不支持的环境可退化为主智能体串行执行各阶段。

## 依赖

**核心流程零依赖**：技能本体全部是 Markdown 方法论文档，无需安装任何第三方包；
检索依赖宿主 Agent 自带的联网搜索能力。

**可选执行依赖**（仅当报告需要出图表或交付 Word 时，由 Agent 临时执行的内联代码用到）：

| 依赖 | 用途 |
|---|---|
| `matplotlib` | 图表制作（中文字体配置、配色、图注三要素规范见 `references/methodology.md`） |
| `python-docx` | 图表按锚点插入 Word 正文 |

```bash
pip install matplotlib python-docx   # 按需
```

**不需要**：API Key、外部服务。

## 快速上手

| 阶段 | 动作 | 关键规范文件 |
|---|---|---|
| 1 备料 | 按渠道并行派检索子 agent，权威级裁决冲突 | `methodology.md`、`conflict-resolution.md` |
| 2 图谱 | 上市/非上市公司梳理 | `data-discipline.md` |
| 3 审核 | 《撰写前研究报告》交用户 | `outline-template.md`、`literature-evaluation.md` |
| 4 初版 | 逐章撰写 + 对抗审查 | `writing-style.md`、`adversarial-review.md`、`lifecycle-templates.md`、`agent-brief-templates.md` |
| 5 终版 | 用户主导，AI 不介入 | — |
| 6 进化 | 终版 diff → 建议 → 批准后写入技能 | `evolution-loop.md` |

## 目录结构

```
SKILL.md                          技能主文件（六阶段工作流 + 八条铁律 + 编排引擎）
references/
  lifecycle-templates.md          行业生命周期四档 × 五维度模板
  evolution-loop.md               终版 diff → 技能自进化闭环规范
  literature-evaluation.md        参考文献逐篇评估四维度规范
  writing-style.md                撰写风格三要求（术语解释/去AI味/减少重复）
  methodology.md                  检索话术、字数核算、图表规范、Critic 清单
  data-discipline.md              财务核实优先级、连续口径、缺失标注
  outline-template.md             九章制行业报告大纲模板
  agent-brief-templates.md        六类子任务 brief 模板（检索/撰稿/审校/图表/对抗审查/diff）
  assignment-order-template.md    人工课题分配单模板
  conflict-resolution.md          来源权威级与多源冲突裁决规则
```

## 能力边界

✅ 万字级行业深度报告/综述、多智能体并行编排、生命周期分阶段写法、终版学习自进化
⚠️ 单项目尽调/项目判断请配合同作者其他技能；宿主 Agent 不支持子代理时退化为串行

## 隐私说明

发布前已完成敏感信息扫描与清洗（自动扫描 + 人工全文复核）：
无本机路径、用户名、密钥、私人邮箱；示例中的真实研究方向已泛化。
若发现遗漏，请提 Issue 或直接提交 PR。

## License

[MIT](LICENSE)
