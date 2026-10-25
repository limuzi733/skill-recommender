# Skill-Recommender · AI 工具推荐器

一个开箱即用的 AI 工具/SKILL 推荐技能：用户问「我该装什么 SKILL / 帮我找能做 XX 的工具」时，自动从 1616 条实测入库的 AI 工具库中按需求打分推荐，并给出安装方式。

## 内容

- `SKILL.md` — 技能定义（WorkBuddy / Codex 兼容）
- `tools_library.json` — AI 工具库数据（1616 条，含 type/category/domain/description/tags/install 等字段）

## 安装方式

### WorkBuddy（对话安装）
在 WorkBuddy 对话中发送：
> 安装这个 skill：https://github.com/<你的仓库地址>

或手动将本仓库文件放入技能目录 `~/.workbuddy/skills/skill-recommender/`，重启 WorkBuddy 后生效。

### Codex（对话安装）
在 Codex 对话中发送：
> install skill from https://github.com/<你的仓库地址>

或放入 `~/.codex/skills/skill-recommender/`，新开会话后生效。

### 手动放置
```
~/.workbuddy/skills/skill-recommender/
├── SKILL.md
└── tools_library.json
```

## 数据说明

- 工具库由「选AI看实测」站点长期采集整理，当前 1616 条，覆盖开发效率、自动化智能体、写作文案、办公自动化、文档处理、视频剪辑、搜索分析、设计视觉、安全元工具等 12 大类。
- 第三方评测内容仅保留 200 字以内摘要并附原文链接，不搬运全文；仅收录允许使用的开源/免费工具。
- 实时数据源：https://tools-api.xn--oby2cs85ktjf.com/

## 使用示例

| 用户问题 | 推荐结果 |
|---|---|
| 帮我找能批量处理 Excel 的 SKILL | 办公自动化类目下按 name/tags 打分 Top 3 |
| 我该装什么写公众号的 SKILL | 写作文案类目推荐 + 安装命令 |

## License

数据与脚本供个人学习与研究使用；商业用途请联系作者。
