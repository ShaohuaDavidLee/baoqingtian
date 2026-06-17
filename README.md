# 包青天 Skill

> 铁面无私判需求。
> 不杀热情，只杀幻想。

包青天 Skill 是一个用来审判产品 idea / MVP / AI 工具 / 作品型产品的 Codex Skill。

你写一份「状纸」，它会像包青天升堂一样，先指出问题，再给活路：判断这个 idea 是伪需求、可翻案、可试做，还是可以开工，并生成一张可截图、可分享的 HTML 判牍卡。

官网入口：[b.caojuege.com](https://b.caojuege.com)

<p>
  <img src="assets/bao-dark.png" width="120" alt="包青天 Skill logo" />
</p>

## 它解决什么问题

从一个 idea 到真的能用的产品，中间不是一句「让 AI 帮我写代码」就结束了。

过去，一个有商业化潜力的产品 demo，可能要花：

`20 天 *（产品 + 设计 + 前端 + 后端 + 测试）* 2000 = 20 万`

到了 vibe coding 的今天，成本被压到：

`5 天 * 2000 = 1 万`

但一万块也是钱，五天也是时间。
所以在动手之前，最好先花 10 分钟，让包青天审一下：

- 这是真需求，还是「我觉得」？
- 用户是谁，场景够不够具体？
- 有没有替代方案？替代方案强不强？
- 用户愿不愿意付钱，还是只会说「挺好」？
- 你为什么适合做？你能不能找到第一批用户？
- 下一步应该开工、试做，还是先补证？

## 适合谁

- idea 脑子里乱飞，但不知道先做哪个的人
- 确定了方向，但老虎吃天、无处下嘴的人
- 产品已经上线，但收不到有效用户反馈的人
- 会 vibe coding，但经常做出没人用 demo 的开发者
- 有专业经验，想把自己的理解做成软件的创作者
- 草诀歌 AI Labs 式「作品型产品」实践者

不要怕。包青天只杀幻想，不杀热情。

## 怎么用

### 方式一：从官网复制状纸

打开 [b.caojuege.com](https://b.caojuege.com)，填写状纸，点击：

`复制状纸 + Skill 指令`

然后把复制出来的内容粘进你的 agent：

- Codex
- Claude Code
- Kimi
- Zcode
- codebuddy
- OpenClaw
- 其他支持 Skill / Agent 工作流的工具

如果你的 agent 已经安装了包青天 Skill，它会直接升堂，输出 HTML 判牍卡和详细判词。

### 重要：为什么不要让普通模型直接画卡

稳定的判牍卡不是靠模型临场手写 HTML，而是靠 Skill 里的固定 renderer 生成。

如果你的 agent 不能访问这个仓库、不能安装 Skill、不能运行本地脚本，或者不能读取 `assets/` 里的包青天 logo 和水印，它很可能会自由发挥：

- 卡片风格漂移
- logo / 水印缺失
- 底部文字错乱
- 字段顺序不稳定
- 不同 agent 输出完全不同

这种情况不是包青天在升堂，是模型在 cosplay。

推荐规则：

- 能安装 Skill：使用 Skill，输出稳定 HTML 判牍卡 + Markdown 判词
- 不能安装 Skill：只输出详细判词 + case JSON，不要伪造 HTML 卡片

### 方式二：在 Codex 里安装 Skill

公开仓库后，可以用 Codex 的 Skill Installer 从 GitHub 安装：

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo ShaohuaDavidLee/baoqingtian \
  --path skills/baoqingtian
```

安装后重启 Codex，然后直接说：

```text
用包青天审一下这个 idea：

【状纸】
我想做：……
用户是谁：……
使用场景：……
现在怎么解决：……
不解决的代价：……
愿意付出什么：……
我已有证据：……
为什么是我：……
```

## 状纸字段

| 字段 | 要回答的问题 |
| --- | --- |
| 我想做 WHAT | 一句话说清你要做什么 |
| 用户是谁 WHO | 一张具体的脸，别写「所有人」 |
| 使用场景 WHEN | 什么时刻会想起它 |
| 现在怎么解决 NOW | 没有替代方案的需求往往是假的 |
| 不解决的代价 PAIN | 疼不疼，疼到什么程度 |
| 愿意付出什么 PAY | 钱、时间，还是只是一句「挺好」 |
| 我已有证据 PROOF | 用户原话、试用、定金、订单、复购 |
| 为什么是我 WHY ME | 你比别人多了什么 |

## 判词怎么看

包青天使用五级判词：

| 判词 | 意思 |
| --- | --- |
| 不予立案 | 用户、场景、痛点、证据都不清楚，先别做 |
| 押后再审 | 有一点苗头，但证据太弱，需要补证 |
| 准予翻案 | 方向可能成立，但必须用七日任务验证 |
| 准予试做 | 有真实证据，可以用服务优先或窄 MVP 试做 |
| 准予开工 | 证据较强，下一步可以进入明确开发 |

证据力分五级：

`口供 < 旁证 < 物证 < 银票 < 血书`

四维评分：

- 需求真度：痛点是否真实、具体、频繁、紧迫
- 付费可能：谁付钱、付多少钱、有没有付款证据
- 分发可达：你能不能反复触达第一批用户
- 创始人匹配：为什么是你，你能不能持续做

## 背后的方法论

包青天不是一个「让 AI 随便评价 idea」的提示词玩具。

它揉合了几类方法论，然后删掉了很多过重、过时、不适合个人开发者和作品型产品的部分：

- 创业验证：YC、Lean Startup、Customer Development、The Mom Test
- 产品发现：JTBD、Opportunity Solution Tree、RICE / Kano
- AI 产品判断：Google PAIR、Microsoft Human-AI Guidelines、NIST AI RMF、OpenAI / Anthropic evals
- 草诀歌 AI Labs 方法论：作品型产品


## 项目结构

```text
.
├── index.html                  # b.caojuege.com 的静态 landing page
├── assets/
│   ├── bao-dark.png             # 浅色背景用 logo / 水印
│   └── bao-light.png            # 深色背景用 logo / 水印
└── skills/
    └── baoqingtian/
        ├── SKILL.md             # Skill 主说明
        ├── agents/openai.yaml   # Agent 元信息
        ├── assets/              # Skill 内置视觉素材
        └── scripts/
            └── render_verdict.py # HTML 判牍卡渲染器
```

## 本地预览

这个站点是纯静态页面，直接打开 `index.html` 也可以。

更推荐用本地服务预览：

```bash
python3 -m http.server 8787
```

然后访问：

```text
http://127.0.0.1:8787
```

## 开源协议

MIT License。

你可以 fork、改造、学习、集成。
如果你基于它做出自己的 Skill，欢迎保留出处，也欢迎提交 PR。

## 关于草诀歌 AI Labs

包青天 Skill 是草诀歌 AI Labs 的一个作品型产品实验。

工业型产品已死，作品型产品永生。

先指出问题，再指出活路。
