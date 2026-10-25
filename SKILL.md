---
AIGC:
    Label: "1"
    ContentProducer: 001191440300708461136T1XGW3
    ProduceID: ce3f8cef097dc1f6809fc41965e9d97c_dd50c01bcf6e11f19f8752540064ee0f
    ReservedCode1: cD8UlEwKM5kI0/tCkH17lFL9YLznnWECimvcx6OfHFIDcZGFDJSxcvmuGzalYzdJES47lkuUY77p3CkMH1rTmKiE6kLhhsnvYtwm9uZCUDuZT8FLzNmHaXjQaGJUr7ReUcj1rdxIu5bo6FEUNKP9e0Km5ncTXoGFU2ZneEqqD2vAWjEBupK9BWixzxE=
    ContentPropagator: 001191440300708461136T1XGW3
    PropagateID: ce3f8cef097dc1f6809fc41965e9d97c_dd50c01bcf6e11f19f8752540064ee0f
    ReservedCode2: cD8UlEwKM5kI0/tCkH17lFL9YLznnWECimvcx6OfHFIDcZGFDJSxcvmuGzalYzdJES47lkuUY77p3CkMH1rTmKiE6kLhhsnvYtwm9uZCUDuZT8FLzNmHaXjQaGJUr7ReUcj1rdxIu5bo6FEUNKP9e0Km5ncTXoGFU2ZneEqqD2vAWjEBupK9BWixzxE=
name: skill-recommender
display_name: AI工具推荐器
version: 1.0.0
author: 台州银溪科技
description: 用户问「我该装什么 SKILL / 帮我找能做XX的 SKILL / 推荐个工具」时，不依赖用户复述需求：先读取本机 AI 客户端（WorkBuddy/Codex）会话记录确认用户真实诉求，再从工具库中推荐合适的 SKILL 并给出安装方式；没有会话记录时用引导式追问确认需求后再推荐。
allowed-tools:
  - Read
  - Write
  - Edit
  - AskUserQuestion
metadata:
  trigger: 安装什么 skill、该装什么、推荐 SKILL、帮我找能做XX的工具
  source: 参照 humanizer-zh 格式封装；工作流来自选AI看实测站 SKILL 推荐器
  revision: "2026-10-24"
---



# Skill-Recommender

用户问「我该装什么 SKILL / 帮我找能做XX的 SKILL / 推荐个工具」时，不依赖用户复述需求：先尝试读取本机 AI 客户端会话记录确认用户真实诉求，再从工具库中推荐合适的 SKILL 并给出安装方式。没有会话记录时，用引导式追问确认需求后再推荐。

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

最后用一句话概括核心诉求。此步结论是第四步打分的依据。

## 第三步【引导确认】（小白无会话记录时）

没有找到可用的会话记录，或提炼结果明显不足以推荐时，用 `AskUserQuestion` 逐步追问，每轮给 2-4 个选项，按以下顺序：

1. 想完成什么任务类型：写内容 / 办公效率 / 企业运营 / 电商
2. 主要用哪个 AI 助手：Codex / WorkBuddy / 都行
3. 使用频率和复杂度：偶尔简单 / 经常重复 / 复杂专业
4. 费用要求：免费优先 / 可以付费 / 无所谓

用户回答完再进入第四步推荐。

## 第四步【推荐 SKILL】

1. 读取工具库：优先请求远端 `https://tools-api.xn--oby2cs85ktjf.com/`（无参数返回整库 JSON）；网络不可用或地址仍是占位时，回退读取本机 `C:\Users\60593\AppData\Roaming\Tencent\Marvis\User\oAN1i2aejvbUvTMMmbRcPAQNJiUM\workspace\conv_e765b3402e5d449cbf2a573dbe1757e7\output\data\tools\tools_library.json`
2. 优先推荐 `type=skill` 的条目。按核心诉求与条目的 name / description / tags / category 匹配打分：
   - name 命中 +5
   - tags 命中 +3
   - category 命中 +2
   - description 命中 +1
   - `type=skill` 额外优先（非 skill 条目 -2）
3. 返回 Top 3（主推 1 个 + 备选 2 个），每条给出：
   - 名称
   - 类型
   - 一句话描述
   - 安装方式：有 `install.command` 时给出命令；没有则说明平台支持即可
4. 推荐话术对小白友好，直接说「这个 SKILL 装进 Codex 可以帮你做XX」。

## 输出格式

用简洁 Markdown 列表/表格呈现推荐结果。示例：

| 优先级 | 名称 | 类型 | 描述 | 安装方式 |
|---|---|---|---|---|
| 主推 | xx | skill | 帮你做XX | `...` |

- 不编造工具库中不存在的 SKILL 名称。
- 匹配不到时如实说明「工具库里没有匹配的 SKILL」，建议换个说法重新描述，或去官网查看。
*（内容由AI生成，仅供参考）*
