# futures-script-optimizer

期货公司互联网推广部门「跑量脚本优化专家」Skill。

面向代理商营销视频脚本优化与金融营销合规初审，最终可接入部门工作台。

> **重要声明**：本仓库**不包含**公司内部原始业务资料（xlsx/docx 等）。所有知识库内容均为从原始资料中结构化提取、并严格保留原文表述与来源说明的 markdown 版本，仅供 Skill 运行时参考。原始资料的处置、版本与合规口径以公司正式文件为准。

---

## 一、项目用途

帮助期货公司互联网推广部门把「脚本优化 + 合规初审」这套经验固化为一个可复用的 Agent Skill：

1. **脚本优化**：把代理商初版口播脚本改写为「钩子 / 卖点 / 转化」三段式，强化 Hook 吸引力、压缩无效科普、补全转化引导。
2. **合规初审**：基于公司合规违禁表述知识库（48 条话术，A/B/C/D 四类分级）对脚本做前置筛查，命中红线即转人工，不自动放行。
3. **案例复用**：从历史「优化前后对比」案例中检索相似场景，复用已验证的修改规律。

## 二、文件结构

```
futures-script-optimizer/
├── SKILL.md                    # Skill 主文件（frontmatter + 执行流程 + 自检清单）
├── README.md                   # 本文件：用途、结构、使用方式、版本管理
├── CHANGELOG.md                # 版本变更记录
├── .gitignore                  # 排除临时文件、凭据、本地配置、公司原始资料
├── references/                 # 知识库（与执行指令分离，Skill 运行时引用）
│   ├── compliance-rules.md     #   合规违禁表述知识库（48 条 + 四类分级 + 人工复核清单）
│   ├── hook-library.md         #   Hook 素材库（结构化提取，非原文全文）
│   ├── case-patterns.md        #   案例修改规律（A 类通用 / B 类特殊 / C 类证据不足）
│   └── knowledge-sources.md    #   资料来源、三态标注口径、缺口声明
└── evals/                      # 测试机制
    ├── README.md               #   测试流程说明
    ├── test-template.md        #   测试案例模板
    ├── scoring-rubric.md       #   评分表（合规红线一票否决）
    ├── cases/                  #   测试案例
    │   └── case-001.md         #     首个测试案例示例
    └── results/                #   测试结果（与 Skill 版本关联，默认不入库）
```

## 三、使用方式

### 3.1 安装（部署为本地 Skill）

本 Skill 遵循 Agent Skills 开放格式，安装只需把整个目录放到指定位置即可，**无需编译、无需依赖**。

**方式 A：git clone（推荐，可同步更新）**

```bash
# 1. 克隆到本地 skills 目录（用户级，对所有项目生效）
git clone git@github.com:yidan-lau/futures-script-optimizer.git ~/.workbuddy/skills/futures-script-optimizer

# 2.（可选）只想在当前项目内生效，改放项目级目录
git clone git@github.com:yidan-lau/futures-script-optimizer.git <你的项目>/.workbuddy/skills/futures-script-optimizer
```

**方式 B：下载 ZIP（无需 git）**

1. 打开仓库页面 → 绿色 `Code` 按钮 → `Download ZIP`。
2. 解压后，把整个 `futures-script-optimizer` 文件夹复制到：
   - 用户级：`~/.workbuddy/skills/futures-script-optimizer`（Windows 为 `C:\Users\<你的用户名>\.workbuddy\skills\futures-script-optimizer`）
   - 项目级：`<你的项目>/.workbuddy/skills/futures-script-optimizer`

**安装后更新（仅方式 A 适用）**

```bash
cd ~/.workbuddy/skills/futures-script-optimizer
git pull
```

### 3.2 使用示例

安装后，在 WorkBuddy 对话中用自然语言即可触发，无需记命令。

**示例 1：脚本优化**

> 帮我优化这个期货开户脚本：〔粘贴代理商初版口播脚本〕

Skill 会按「诊断 → 合规初审 → 案例检索 → 改写 → 合规复审 → 输出」流程，输出：
- 优化稿（按【钩子】【内容】【转化】三段，钩子给两版备选）
- 需要注意和确认的事项

**示例 2：合规初审（不改写）**

> 对这段口播文案做合规初审，只查合规问题，不用改写：〔粘贴脚本〕

**示例 3：改写 Hook**

> 把这段脚本的开头 Hook 改成痛点式，给我两个备选：〔粘贴脚本〕

**触发关键词**：跑量脚本、脚本优化、口播改写、合规初审、Hook 改写、开户脚本、期货营销文案。

### 3.2 在豆包等通用 AI 里使用（无需安装）

如果同事不用 WorkBuddy，可复制 [`PROMPT.md`](./PROMPT.md) 里的**通用提示词**，粘贴到豆包（或 ChatGPT、通义等）对话框，即可让该 AI 执行同样的「合规初审 + 脚本改写」。完整规则以 `SKILL.md` 和 `references/` 为准，提示词为压缩摘录版。

### 3.3 运行测试

参见 `evals/README.md`。核心思路：

1. 从 `evals/cases/` 取一条测试案例；
2. 让 Skill 对案例原稿执行完整流程；
3. 按 `evals/scoring-rubric.md` 打分；
4. 结果写入 `evals/results/`，文件名与 Skill 版本关联。

## 四、版本管理方式

- 采用语义化版本 `vX.Y.Z`，记录于 `SKILL.md` frontmatter 的 `version` 字段与 `CHANGELOG.md`。
- **X（主版本）**：执行流程 / 合规分级框架发生结构性变化。
- **Y（次版本）**：新增或调整规则、案例规律、测试用例。
- **Z（修订）**：文案、格式、来源说明等非功能性修正。
- 每次改动在 `CHANGELOG.md` 顶部追加一条记录，标明日期与变更内容。
- 测试结果文件命名含版本号（如 `case-001_v0.1.0_20261010.md`），用于追溯「某结论是哪个版本产出的」。

## 五、合规与使用边界

- **不得擅自修改公司规则**：知识库严格以原始资料为唯一依据，不新增公司未提供的禁词，不把整理者常识当规则。
- **不得编造**：不虚构产品事实、业务政策、投放结果。未确认内容一律标注「待人工确认 / 人工复核」。
- **不自动对外**：本 Skill 不做自动发布、不自动同步外部文档、不调用外部业务系统；所有投放动作需人工确认。
- **红线一票否决**：命中合规知识库「A 明确禁止」或「D 需监管沟通」条目时，Skill 不得自动放行或改写后投放，必须转人工。
