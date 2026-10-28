# English Learning-skill — 专四口语思辨陪练

一个可开源的 AI Skill：以「问题 + 参考句型 → 口头作答 → 转写 / 优化 / 点评 → 【继续】或【重新回答】」的闭环，逐轮陪练英语口语思辨表达，面向专四（TEM-4）口语备考。

## 项目解决什么问题

专四口语考试中的思辨表达，难的不是"会英语"，而是三个具体问题：

1. **拿到话题不知道说什么**——没有话题库、没有开口支架，临时想观点容易卡壳。
2. **说了没人知道错在哪**——中式英语（直译语序、中式搭配、逻辑断层）被当成"没问题"，越练越固化。
3. **练着练着就停在同一层**——回答完就结束，缺少深一层论证的训练。

本 Skill 用"逐轮话题 + 一正一反参考句型 + 忠实转写 + 定向优化 + 简短点评 + 深度追问"的闭环，一次陪练同时解决这三个问题。

## 主要功能

- **运行口令**：说「**开始口语练习**」即启动陪练；该口令为全局口令，任何时候说出都回到开局、开启一轮新话题。
- **逐轮思辨训练**：每轮先给出英文思辨 Topic，再给出一正一反两句参考句型（纯英文），用户可顺着某一立场展开，也可自提观点。
- **忠实转写**：【我的原句】原样保留用户口述英文（含语法错误、碎片句、口头禅），不改正、不美化、不补全（禁止幻觉）。
- **定向优化**：【优化句】仅生成 1 版，优先修正四类问题——中文直译语序、中式搭配、逻辑断层、母语负迁移；拒绝浅层同义词替换（如 important→crucial），核心改动加粗标出，适配口头表达、不过度书面化。
- **简短点评**：中文 2～4 句，点明本次中式英语的具体现象，并分别说明逻辑、句式、用词地道度。
- **双选项推进**：反馈后提供【继续】——同话题深度追问（并重新给出一正一反参考句型）；【重新回答】——同题重练，循环转写 / 优化 / 点评，直到【继续】。
- **随时换题**：用户说"换个话题 / 再来一个"，回到开局给出新 Topic 与新参考句型。
- **边界友好**：中文作答、空作答、与话题无关的作答均如实转写并提示重新作答。

## 目录结构

```
english learning-skill/
├── SKILL.md              # Skill 主文件：frontmatter + 训练流程状态机 + 输出细则 + 完整示例
├── agents/
│   └── openai.yaml       # Agent 界面元数据（display_name / short_description / default_prompt）
└── README.md             # 本文件
```

## 安装方法

1. 将本项目整个文件夹复制到支持 Skill 机制的 AI Agent 的 skills 目录。以豆包办公等环境为例，即复制到其 `workspace/.user_skills/` 下：
   ```
   <agent-workspace>/.user_skills/english-learning-skill/
   ├── SKILL.md
   └── agents/
       └── openai.yaml
   ```
   建议目录名使用 `english-learning-skill`（与 `SKILL.md` 中的 `name` 保持一致）；若你的 Agent 支持通过路径引用 Skill，直接指向本目录即可。
2. 确认目录内包含 `SKILL.md` 与 `agents/openai.yaml` 两个文件。
3. 重启或刷新 Agent，使其加载新 Skill（若环境支持热加载则无需重启）。

## 使用方法

1. 对 AI 说运行口令：「**开始口语练习**」即启动训练（等价于开启专四口语思辨陪练，先给出一个新话题和两句参考句型）。
2. 用户口头作答（环境支持语音时直接说；不支持时把口述内容以文字发来，仍按【我的原句】处理）。
3. 之后按提示逐轮作答即可：

| 用户输入 | AI 行为 |
| --- | --- |
| 口头作答（或以文字转述口述内容） | 输出【我的原句】【优化句】【简短点评】+ 两个选项 |
| 【继续】 | 就当前话题深度追问，给出新一轮追问与新的参考句型 |
| 【重新回答】 | 不换话题，重新等待作答，再次转写 / 优化 / 点评（可循环） |
| 换个话题 / 再来一个 | 回到开局，给出新 Topic 与新参考句型 |

## 输入输出示例

**AI 开局**

> **Topic**: Should college students take part-time jobs while studying?
> **参考句型（Pro）**: Taking a part-time job helps college students gain real-world experience that classes alone cannot provide.
> **参考句型（Con）**: However, part-time jobs may take time and energy away from academic study, which is still a student's first priority.

**用户作答**

> I think part-time job can let me learn many knowledge and improve ability. Because now society need experience, so I want to try.

**AI 反馈**

> **①【我的原句】**
> I think part-time job can let me learn many knowledge and improve ability. Because now society need experience, so I want to try.
>
> **②【优化句】**
> I think **taking a part-time job can help me gain practical knowledge and build up my skills**, **which will look good on my resume**. **After all, employers today care more about real experience than grades**, so I'd like to give it a try.
>
> **③【简短点评】**
> 本次中式英语有两处：一是 “learn many knowledge”——knowledge 不可数，地道搭配是 gain/acquire knowledge，属中式搭配；二是 “Because... so...” 把因果硬拆成两个碎片句，属逻辑断层。逻辑上“积累经验 → 对就业有利 → 想尝试”方向成立；句式上建议合并为一个因果句；用词上 “improve ability” 偏笼统，可换成 build up my skills。
>
> 请选择：**【继续】**（同话题深度追问）或 **【重新回答】**（同题重新作答）

## 暂不支持

- 网页抓取、PDF 处理、图形界面。
