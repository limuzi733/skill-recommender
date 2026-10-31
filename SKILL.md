---
AIGC:
    Label: "1"
    ContentProducer: 001191440300708461136T1XGW3
    ProduceID: ce3f8cef097dc1f6809fc41965e9d97c_4e31e9ddd02311f19f8752540064ee0f
    ReservedCode1: iJw+xcrH1yBRF19qFgxY2LQ8JIK40bYHa2Pwqzh+nRrUmsnOC6PSYjwz5k/XX8oJaw7U+ngUGhaFI5DYWaoY9z1GUCyK3dtgVS87lcPnoCYoVS/dRUXePPbfCV5MspWoiJfeUOkuAmuQU3udYlCkPJeV+ayN1prQYqp97mQy3fWhc1YRBhH+Kwcu3E4=
    ContentPropagator: 001191440300708461136T1XGW3
    PropagateID: ce3f8cef097dc1f6809fc41965e9d97c_4e31e9ddd02311f19f8752540064ee0f
    ReservedCode2: iJw+xcrH1yBRF19qFgxY2LQ8JIK40bYHa2Pwqzh+nRrUmsnOC6PSYjwz5k/XX8oJaw7U+ngUGhaFI5DYWaoY9z1GUCyK3dtgVS87lcPnoCYoVS/dRUXePPbfCV5MspWoiJfeUOkuAmuQU3udYlCkPJeV+ayN1prQYqp97mQy3fWhc1YRBhH+Kwcu3E4=
---

---
AIGC:
    Label: "1"
    ContentProducer: 001191440300708461136T1XGW3
    ProduceID: ce3f8cef097dc1f6809fc41965e9d97c_dd50c01bcf6e11f19f8752540064ee0f
    ReservedCode1: cD8UlEwKM5kI0/tCkH17lFL9YLznnWECimvcx6OfHFIDcZGFDJSxcvmuGzalYzdJES47lkuUY77p3CkMH1rTmKiE6kLhhsnvYtwm9uZCUDuZT8FLzNmHaXjQaGJUr7ReUcj1rdxIu5bo6FEUNKP9e0Km5ncTXoGFU2ZneEqqD2vAWjEBupK9BWixzxE=
    ContentPropagator: 001191440300708461136T1XGW3
    PropagateID: ce3f8cef097dc1f6809fc41965e9d97c_dd50c01bcf6e11f19f8752540064ee0f
    ReservedCode2: cD8UlEwKM5kI0/tCkH17lFL9YLznnWECimvcx6OfHFIDcZGFDJSxcvmuGzalYzdJES47lkuUY77p3CkMH1rTmKiE6kLhhsnvYtwm9uZCUDuZT8FLzNmHaXjQaGJUr7ReUcj1rdxIu5bo6FEUNKP9e0Km5ncTXoGFU2ZneEqqD2vAWjEBupK9BWixzxE=
name: ai-tool-recommender
display_name: AI工具推荐器
display_name_en: "AI Tool Recommender"
version: 1.1.0
author: 台州银溪科技
description: 用户问「我该装什么 SKILL / 帮我找能做XX的 SKILL / 推荐个工具」时，先读取本机 AI 客户端（WorkBuddy/Codex）会话记录确认真实诉求，再把任务拆解成若干子领域，从工具库为每个子领域匹配最合适的 SKILL 组成技能包，用实测记录/来源/安装量过滤低质项并附实测结论，最后一次性输出整套安装命令，按需自动安装合适的 SKILL 们，不用到处找；没有会话记录时用引导式追问确认需求后再执行。
description_zh: "用户问「我该装什么 SKILL / 帮我找能做XX的 SKILL / 推荐个工具」时，先读取本机 AI 客户端（WorkBuddy/Codex）会话记录确认真实诉求，再把任务拆解成若干子领域，从工具库为每个子领域匹配最合适的 SKILL 组成技能包，用实测记录/来源/安装量过滤低质项并附实测结论，最后一次性输出整套安装命令，按需自动安装合适的 SKILL 们，不用到处找；没有会话记录时用引导式追问确认需求后再执行。"
description_en: 'When the user asks "which SKILL should I install" or "help me find a tool for XX": first read local AI client (WorkBuddy/Codex) session logs to confirm the real intent, then break the task into sub-domains, match the best SKILL for each sub-domain from the tool library to form a SKILL pack, filter low-quality items using test records/sources/download counts and attach real test conclusions, finally output the full install command list so users can auto-install the right SKILLs without searching everywhere; if no session logs exist, ask guided follow-up questions first.'
allowed-tools:
  - Read
  - Write
  - Edit
  - AskUserQuestion
metadata:
  trigger: 安装什么 skill、该装什么、推荐 SKILL、帮我找能做XX的工具、我要做XX需要哪些技能、帮我配一套 SKILL
  source: 参照 humanizer-zh 格式封装；工作流来自选AI看实测站 SKILL 推荐器
  revision: "2026-10-31"
---



# Skill-Recommender

用户问「我该装什么 SKILL / 帮我找能做XX的 SKILL / 推荐个工具」时，不依赖用户复述需求：先尝试读取本机 AI 客户端会话记录确认用户真实诉求，再把任务拆解成若干子领域，从工具库中为每个子领域匹配最合适的 SKILL 组成「技能包」；用实测记录、来源、安装量过滤低质项，附实测结论增强说服力，最后一次性输出整套安装命令。没有会话记录时，用引导式追问确认需求后再执行。

## 第一步【找上下文】

1. 先判断当前对话是否已能确定用户需求（用户已明确说出任务类型、平台、场景等）。能确定则直接进入第二步，跳过文件读取。
2. 信息不足时，按以下两个目录读取**最近修改**的 JSONL 会话文件（取 1-3 个，按修改时间倒序；文件很大时只读尾部若干行或用搜索定位，不要整文件读入）：
   - WorkBuddy：`C:\Users\60593\.workbuddy\projects\<项目名>\*.jsonl`
   - Codex：`C:\Users\60593\.codex\sessions\<年>\<月>\<日>\rollout-*.jsonl`
3. 提取 `role=user` 的消息内容：
   - content 为字符串时，用正则提取 `<user_query>...</user_query>` 标签内的文本；
   - content 为数组时，遍历其中的 `input_text` / `text` 字段做同样提取。
4. 剔除系统注入内容：`<system-reminder>`、`<task-notification>`、BOOTSTRAP.md、工具调用回执等非用户诉求文本。

## 第二步【提炼需求】

从提取到的用户诉求中回答三个问题：

1. 用户想做什么事（任务类型/目标）？
2. 卡在哪（反复抱怨、多次返工、被拒绝的点）？
3. 需要什么能力（写作、办公、数据处理、自动化……）？

最后用一句话概括核心诉求。此步结论是第四步拆解的依据。

## 第三步【引导确认】（小白无会话记录时）

没有找到可用的会话记录，或提炼结果明显不足以推荐时，用 `AskUserQuestion` 逐步追问，每轮给 2-4 个选项，按以下顺序：

1. 想完成什么任务类型：写内容 / 办公效率 / 企业运营 / 电商 / 开发软件
2. 主要用哪个 AI 助手：Codex / WorkBuddy / 都行
3. 使用频率和复杂度：偶尔简单 / 经常重复 / 复杂专业
4. 费用要求：免费优先 / 可以付费 / 无所谓

用户回答完再进入第四步。

## 第四步【拆解任务为子领域】（核心能力）

把第二步的核心诉求拆成 **2-5 个「子领域」**，覆盖完成该任务所需的能力链路。子领域从工具库类目中选取，常见映射：

| 任务方向 | 常见子领域 |
|---|---|
| 开发一个软件/网页项目 | 代码开发、前端UI设计、代码测试、架构设计、文档编写 |
| 做内容/公众号 | 选题策划、写作与文案、去AI味润色、排版与配图、数据分析 |
| 办公/数据处理 | 办公自动化、表格处理、文档处理、批量脚本 |
| 企业运营/销售 | 营销文案、销售话术、客服自动化、数据分析 |
| 视频/设计 | 视频脚本、视频剪辑、配图设计、提示词工程 |

示例：用户要「做一个心理学大师圆桌会议网站」→ 拆为：代码开发 + 前端UI设计 + 测试 + 提示词工程（人格设定）。

拆解结果必须逐条说明「为什么这个子领域是完成任务的必要环节」，让用户能看懂、认同。

## 第五步【匹配技能包】

1. 读取工具库：优先请求远端 `https://tools-api.xn--oby2cs85ktjf.com/`（无参数返回整库 JSON）；网络不可用或地址仍是占位时，回退读取本机 `tools_library.json`。
2. 对**每个子领域**独立匹配 1-2 个最优 SKILL（`type=skill` 优先）。打分规则：
   - name 命中 +5
   - tags 命中 +3
   - category 命中 +2
   - description 命中 +1
   - 非 skill 条目 -2
3. 汇总得到「技能包」：通常 3-8 个 SKILL，覆盖任务完整链路。**严禁只推 1 个**——除非工具库中确实只有一个匹配（此时如实说明原因）。

## 第六步【质量过滤】（筛掉低质项）

对第五步候选做三层过滤，未通过的在结果中降权或剔除：

1. **实测优先**：有 `test.score` / `test.verdict` 非空的条目优先（同分时取 `tested_date` 更新的）；带「未实测」标签的条目降权，宁可少推也不推没验证过的。
2. **来源可信**：优先有 `links.github` / `links.official`，或 origin 来自官方/知名评测来源的条目；来源缺失的标注「来源未知」。
3. **安装量参考**：`downloads` 非空且数值大的优先（可作第三方背书）。

## 第七步【附实测结果】（增强说服力）

每条推荐必须附上它的实测证据，让用户不用自己再测一遍：

- 实测得分（`test.score`，如 4.5/5）
- 一句话实测结论（`test.verdict`）
- 前 2 条实测要点（`test.points`）
- 实测日期（`test.tested_date`）
- 无实测的条目明确标注「未实测，谨慎使用」，不装作有验证。

格式示例：`[Humanizer-zh] 实测 4.5/5 ·「AI 味文本改写后大幅精简自然，去除空泛铺垫」· 2026-10-23`

实测证据来源为「选AI看实测」团队自测与第三方公开评测（B站UP主 / 51CTO 等，均附原文链接），与用户自己测试互为印证，解决「自己测的 vs 别人测的」信任差。

## 第八步【批量安装】

按 `install.platforms` 把技能包的安装命令分组输出：

- **WorkBuddy** 对话安装：`安装这个 skill：<github链接>`，或放入 `~/.workbuddy/skills/<技能名>/` 后重启生效
- **Codex** 对话安装：`install skill from <github链接>`，或放入 `~/.codex/skills/<技能名>/` 后新开会话生效
- **Claude Code**：使用 `install.command` 给出的原始命令（如 `/plugin install <来源>`）

一次性输出**整套**安装命令清单，用户复制即可全部装齐，不用到处找。

## 输出格式

用表格呈现技能包：

| 子领域 | 推荐 SKILL | 实测 | 一句话结论 | 安装命令 |
|---|---|---|---|---|
| 代码开发 | xx | 4.5/5 | 结构清晰，bug 率低 | `...` |
| UI设计 | yy | 未实测 | 谨慎使用 | `...` |

- 不编造工具库中不存在的 SKILL 名称；匹配不到的子领域如实说明。
- 所有推荐必须能追溯到工具库真实条目（id/name 唯一）。
*（内容由AI生成，仅供参考）*
