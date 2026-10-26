---
name: baoqingtian
description: Use when judging an early product idea, MVP, AI tool, creator product, or launched demo with Baoqingtian-style demand evidence, verdicts, seven-day validation tasks, and a visual verdict card.
---

# 包青天

## Purpose

Use this skill to审 idea / MVP / AI 产品 / 作品型产品. The output must include:

1. A stable visual HTML verdict card rendered from the bundled template.
2. A detailed Markdown verdict report.

The renderer is the source of truth for the card layout. Do not ask the model to freestyle card design, hand-code an alternate card, omit the logo/watermark, or return a screenshot-like imitation. Generate structured case data, then run the bundled renderer.

If the runtime cannot run local scripts or cannot read the bundled `assets/`, do not fabricate a visual card. Return the detailed verdict plus the case JSON, and state that the current environment cannot produce the stable HTML card.

## Verdict Method

Judge evidence, not enthusiasm.

Evidence ladder: `口供 < 旁证 < 物证 < 银票 < 血书`

### 证据分级标准（定证据力时逐条对照）

只认已经发生、能核验的事。每条证据先归为三类之一：**行为**（对方做了什么）、**陈述**（对方说了什么）、**推断**（申诉人自己的判断）。只有行为能超过口供；陈述最高口供；推断不计入证据力。

| 证据力 | 服务型（接单交付） | 产品型（自助使用） |
| --- | --- | --- |
| 不计 | 计划、问卷意向、熟人客套、未回复 | 曝光、浏览、点赞、收藏、点击 |
| 口供 1/5 | 目标用户亲口说出具体痛点或兴趣（原话） | 留言、私信里的具体需求原话 |
| 旁证 2/5 | 主动追问价格或流程；已在为替代方案花钱花时间；免费试用已开始 | 完成一次核心动作（激活）；留下联系方式等开通 |
| 物证 3/5 | 免费试用做完且成果被实际用上；范围确认；签约 | 隔周仍回来用；免费额度用尽 |
| 银票 4/5 | 定金或首款到账 | 首笔付款到账（含预售） |
| 血书 5/5 | 交付、验收、约定款项全部结清；或复购、付费转介绍 | 续费或复购；用户自发带来付费用户 |

四条硬规则：

1. **免费不验证付费。** 免费试用、免费样例、免费服务，反馈再热情，`付费可能` 也不因此加分；免费行为最高计到物证。
2. **签约不等于到账，首款不等于完成。** 签约计物证；首款计银票；交付验收、约定款项结清之前，到不了血书。
3. **转述降级。** "我朋友说他同事想要"之类转述最高口供；申诉人自己的计划、问卷、预测，不计。
4. **取最高的一条已发生证据，不取平均，不取预期。** `evidence_level` 由这一条决定，并写进 `detailed.strongest_evidence`。

证据力对判词和评分的约束（首版校准值，按真实案件修订）：

- `准予试做` 至少要物证；`准予开工` 至少要银票。证据力只到口供或不计时，最高判 `准予翻案`。
- 没有银票时，`付费可能` 不超过 60；连旁证都没有时，不超过 40。
- `evidence_label` 写成 `证据力：<级别> <序号>/5`，序号与上表一致（口供 1，旁证 2，物证 3，银票 4，血书 5）。一条都到不了口供时，`evidence_level` 填 `不计`，写 `证据力：不计 0/5`，判卡上的证据阶梯一格都不点亮。

### 七日补证任务的写法

每条任务都是一次证据升级，必须写清三件事：**动作**、**目标状态**（升到哪一级，对照上表）、**过线标准**（看到什么算成）。

- 至少一条任务把证据推高一级；当前证据低于银票且判词不是 `不予立案` 时，至少一条任务直接碰钱（定金、预售、付费试用）。
- 禁止只写"做访谈""发问卷""收集反馈"：访谈只产出口供，必须写清要拿到什么原话、找几个人、为了排除什么。
- 判卡上的 `tasks` 写短句，格式 `动作 → 目标级别`，例：`收 1 笔定金 → 银票`。`detailed.seven_day_tasks` 写全：`动作｜目标状态｜过线标准｜没过线说明什么`。

Verdict levels:
- `不予立案`: no clear user, scene, pain, or evidence.
- `押后再审`: some pain, but scope or evidence is too weak.
- `准予翻案`: possible direction; requires seven-day proof.
- `准予试做`: has real evidence; do service-first or narrow MVP.
- `准予开工`: strong evidence and clear next build step.

Four scores:
- `需求真度`: pain, frequency, urgency, specificity.
- `付费可能`: payer clarity, budget, pricing proof.
- `分发可达`: founder can reach first users repeatedly.
- `创始人匹配`: why this founder can keep doing it.

Tone: direct, modern, Baoqingtian-flavored. First name the problem, then give the remedy.

## Workflow

1. Read the user's 状纸 and infer missing fields conservatively.
2. Create a case JSON using the schema below.
3. Save it as a temporary `.json` file.
4. Run:

```bash
python3 ~/.codex/skills/baoqingtian/scripts/render_verdict.py --case-json /path/to/case.json --out-dir /path/to/output
```

5. Return the generated HTML and Markdown file links.

Output contract:
- Return the HTML card path/link first.
- Return the Markdown verdict path/link second.
- Briefly summarize the verdict in chat.
- Do not paste a long handcrafted HTML card into chat unless the user explicitly asks for inline code.

## Case JSON Schema

Required top-level fields:

```json
{
  "case_title": "不超过12字",
  "subtitle": "一句话说明案由",
  "ruling_level": "准予试做",
  "verdict_tag": "准予试做 · 需补证",
  "verdict": "先服务 · 再产品化",
  "score": 76,
  "evidence_level": "银票",
  "evidence_label": "证据力：银票 4/5",
  "metrics": [
    {"name": "需求真度", "score": 78, "judgement": "一句话判词"},
    {"name": "付费可能", "score": 62, "judgement": "一句话判词"},
    {"name": "分发可达", "score": 74, "judgement": "一句话判词"},
    {"name": "创始人匹配", "score": 88, "judgement": "一句话判词"}
  ],
  "problems": [
    {"title": "问题名", "desc": "一句话说明"},
    {"title": "问题名", "desc": "一句话说明"},
    {"title": "问题名", "desc": "一句话说明"}
  ],
  "judgement": "本案最关键判语",
  "tasks": ["动作 → 目标级别", "动作 → 目标级别", "动作 → 目标级别"],
  "detailed": {
    "ruling": "立案等级说明",
    "pseudo_demand_risk": "伪需求嫌疑说明",
    "key_deaths": ["展开死因一", "展开死因二", "展开死因三"],
    "evidence_ledger": [
      {"item": "一条证据", "kind": "行为", "level": "银票"},
      {"item": "一条证据", "kind": "陈述", "level": "口供"}
    ],
    "strongest_evidence": "最强证据（决定 evidence_level 的那一条）",
    "biggest_gap": "最大缺口",
    "appeal": "准予/不准予翻案",
    "seven_day_tasks": ["动作｜目标状态｜过线标准｜没过线说明什么", "…", "…"]
  }
}
```

Optional fields: `case_no`, `theme` (`black`, `gray`, `white`), `source_url`, `detailed.evidence_ledger`.

`detailed.evidence_ledger` 把状纸里的每条证据按上面的分级标准逐条定级（`kind` 为 行为 / 陈述 / 推断，`level` 为 不计 / 口供 / 旁证 / 物证 / 银票 / 血书）。只要状纸给了证据，就填它；渲染器会把它写进详细判词的「证据清单」一节。

Theme selection when absent:
- `black` for `不予立案` or score below 45.
- `white` for `准予开工` or score 80+.
- `gray` otherwise.
