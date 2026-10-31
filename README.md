---
AIGC:
    Label: "1"
    ContentProducer: 001191440300708461136T1XGW3
    ProduceID: ce3f8cef097dc1f6809fc41965e9d97c_4f751f90d02311f1acb8525400638852
    ReservedCode1: ih2y6TX7FiaVCJl+04dfNdpBfobX/tMAYKE4pzMRoRzWvT7iW7nqjY+ZAtpDls4tGDHwGmzeekBIncvpiBlAjET7t/EWf+YZQ99pWQaAcDVHxnU02D/sEwFp3Y4TeP56YVixK56Nl22PGi8MAf1BBdfAxmJ9CJ7mZmeSI9ci3NtV/Y63UqaH34o0ztw=
    ContentPropagator: 001191440300708461136T1XGW3
    PropagateID: ce3f8cef097dc1f6809fc41965e9d97c_4f751f90d02311f1acb8525400638852
    ReservedCode2: ih2y6TX7FiaVCJl+04dfNdpBfobX/tMAYKE4pzMRoRzWvT7iW7nqjY+ZAtpDls4tGDHwGmzeekBIncvpiBlAjET7t/EWf+YZQ99pWQaAcDVHxnU02D/sEwFp3Y4TeP56YVixK56Nl22PGi8MAf1BBdfAxmJ9CJ7mZmeSI9ci3NtV/Y63UqaH34o0ztw=
---

# Skill-Recommender · AI 工具推荐器

**按需自动安装合适的 SKILL 们，不用到处找。**

用户只需说清楚想做什么（如「帮我做一个心理学圆桌讨论网站」），本技能会：

1. **拆解任务**：把需求拆成多个子领域（代码开发 / UI设计 / 测试 / 提示词……）
2. **匹配技能包**：从 1986 条实测入库的 AI 工具库为每个子领域匹配最合适的 SKILL
3. **过滤低质**：用实测记录、来源、安装量三层筛选，不用自己踩坑
4. **附实测结论**：每条 SKILL 带实测得分与一句话结论，所见即所得，信任有依据
5. **一键安装**：一次性输出整套安装命令，复制即可全部装齐

## 内容

- `SKILL.md` — 技能定义（WorkBuddy / Codex 兼容）
- `tools_library.json` — AI 工具库数据（1986 条，含 category/domain/description/tags/dimensions/test/install/downloads 等字段）

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

- 工具库由「选AI看实测」站点长期采集整理，当前 1986 条，覆盖开发与效率、自动化与智能体、写作文案、办公自动化、文档处理、视频剪辑、搜索分析、设计视觉、安全与元工具等 20+ 类目。
- **1593 条附实测记录**（得分 + 一句话结论），369 条附实测视频；第三方评测仅保留 200 字以内摘要并附原文链接，不搬运全文；仅收录允许使用的开源/免费工具。
- 实时数据源：https://tools-api.xn--oby2cs85ktjf.com/

## 使用示例

| 用户问题 | 输出 |
|---|---|
| 帮我做个心理学圆桌网站 | 技能包：代码开发 + UI设计 + 测试 + 提示词，每条附实测与安装命令 |
| 我该装什么写公众号的 SKILL | 技能包：选题 + 写作 + 去AI味 + 排版，每条附实测与安装命令 |
| 帮我配一套做保险销售的 SKILL | 技能包：销售话术 + 营销文案 + 客户管理，每条附实测与安装命令 |

## License

数据与脚本供个人学习与研究使用；商业用途请联系作者。
*（内容由AI生成，仅供参考）*
