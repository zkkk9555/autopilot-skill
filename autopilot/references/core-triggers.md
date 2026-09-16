# Core triggers — 8 个常用 skill 什么时候必须调

> SKILL.md 触发表只负责指路，这张表负责拍板。每个核心 skill 一条：入口条件（什么时候必须调）+ 产物约定（调完必须留下什么）+ 跳过即 malformed（什么情况下算漏调）。剩下的 17 个走文末四问模板，不要凭感觉。

读法：先看入口条件，命中就调；调完对照产物约定，缺产物等于没调；明明命中却没调，JOURNAL 里要写 deviation + 下轮纠正。

## 8 core

- **`grill-with-docs`** — 入口（开局专用）：想法还模糊，有工作目录可以留痕（要改仓库里的东西、需求超过一句话能说清）。产物：打磨后的结论进 `spec.md` 或 `NOTES.md`，模糊点收敛到可写的程度。不调即 malformed：带着模糊需求直接写 spec/拆票。中途（已进 ticket/implement/resolve 上下文）一律用裸 `grilling`，调 `grill-with-docs`/`grill-me` = malformed（user-invoked 互调，见编排铁律 + 中途 grill 专节）。
- **grill→prototype 出口（V2.012，作者主循环）**：grill 中遇到 ungrillable 问题（手感/外观/状态机行为——"how should it look / how should it behave 是关键问题"，聊天答不上来）→ handoff 开原型会话 → 跑完 handoff 回来继续 grill。低保真问题（URL 放哪、字段叫啥）聊天答；高保真问题非跑起来看不见，硬烤 = malformed。
- **`to-spec`** — 入口：要动手写代码/改数据，且还没有可照着做的 spec。产物：`.scratch/<slug>/spec.md`（Problem/Solution/User-Stories/Implementation/Testing/Out-of-Scope/Glossary/Notes + `Checks:` 三行）。不调即 malformed：无 spec 直接 implement。
- **`to-tickets`** — 入口：spec 里超过 1 个可独立演示的行为，或跨文件、跨系统、有先后顺序。产物：`issues/NN-<slug>.md`（What-to-build/Anchors/Blocked-by/Status/验收项），blocker 在前。不调即 malformed：大活一锅烩在一个 ticket 里做。
- **`implement`** — 入口：有 `ready-for-agent` 的票。产物：票对应的代码改动，一票一改，ticket 内写清改了哪里。Lane B 1–3 个 slice 可直接嵌在 spec 里不拆票。不调即 malformed：绕过 ticket 直接改（Lane A 单行 typo/doc 除外）。
- **`tdd`** — 入口：每次 implement 里吞下一个 seam（可断言的行为点）之前。产物：先红后绿（红：失败输出 + 退出码；绿：同样命令通过），seam 写在 Testing Decisions 里。不调即 malformed：先写实现后补测试，或无红直接绿。
- **`code-review`** — 入口：每次提交之前（Lane A 单行 typo/doc 除外）。产物：Standards + Spec 双轴结论，hard violation（红构建、红线 breach、行为与 spec 相反）必须修，smell 只记录。不调即 malformed：未经 review 直接 commit。
- **`diagnosing-bugs`** — 入口：有报错、有必现/偶现症状、有用户原话（症状文本本身就算一条，哪怕没给复现路径），有复现路径或出错位置更好，任中其一。产物：Phase-1 反馈环先行（同一命令在当前 bug 上变红、修完变绿，断言代码缝，不编用户操作路径）+ `NOTES.md` 诊断过程，见 `references/bug-flow.md`。不调即 malformed：没建反馈环就 theorising 修 bug；零信息指连症状都没有（比如光说"修一下那个 bug"），才写 `BLOCKED.md`/`NEEDS-HUMAN.md` 先要信息。
- **`wayfinder`** — 入口：同时满足三条才进：(a) 现在写不出所有 ticket 的 Anchors+verify，(b) 决策链看不见（先画出来才知道先后），(c) 还没有指向这个目标的 map。产物：`map.md` + 决策票。well-scoped 的活（Lane B 现在就能写票）永不进，不调不算错，进错了（该 B 的画了 map）算浪费要在 trace 里记。

## 剩下 17 个的四问模板

命中核心 8 个之外，先问四问再定：

1. 有没有工作目录可留痕？没有 → `grill-me`（无目录打磨）或 `grilling`（还要零副作用、一次性、无 carry-over 才用 bare primitive）。本条只定开局路由；中途（已进 ticket/implement/resolve）一律裸 `grilling`，见中途 grill 专节（V2.018）。
2. 要不要查外部事实？要 → `research` 丢给后台，继续手头活。
3. 要不要跑个一次性验证？要 → `prototype`（网页看 UI/状态机看 LOGIC/引擎看 headless 数字时间线），`handoff` 双向摆渡。**入口放宽（V2.012）**：不只"两方案争执"，"how should it look / how should it behave 是关键问题"就跑——wayfinder 的 prototype 票种同理，关键问题需高保真答案时默认放一张 prototype 票。
4. 是不是人墙/边界？发版花钱 credential 合规 → `wizard` 先 scope 后 STOP；跨目录/同事或版本级换 harness（见 handoff 版本交接包） → `handoff`；等别人脑子里的答案 → `to-questionnaire`（具名收件人 + 问题 + 回填钩子）；半路 conflict → `resolving-merge-conflicts`（`git status` 报 merging/rebasing 才进）；词含糊 → `domain-modeling`；两实现打架/缝错层 → `codebase-design`；闲时巡检 → `improve-codebase-architecture`（只建议不改，**产出 deepening 候选必须落 issue 等人拍板，不许 driver 自答拍板**——作者原话：人是 strategic programmer，agent 是 tactical）；第一次进仓 → `setup-matt-pocock-skills`（一次）；学东西 → `teach`；话没说明白 → `wait-what`；写 agent 文档 → `writing-for-agents`；一堆 raw 输入 → `triage`（见 triage 全套规则，永不 triage `to-tickets` 的输出）。

## handoff 版本交接包：版本级换 harness（V2.013）

用户说"换个地方做下一版""去 Codex 继续""回来接着做 006"这类话 → 调 handoff 生成**版本交接包**，不是轮次内搬运：当前版本交付了什么、decisions（含出处）、待办（含阻塞）、日志本位置（本机绝对路径）、建议 skill（下个版本开局该调谁）。文档落系统临时目录，路径告诉用户；**不自动开新对话**——handoff 只写信不送信，新对话用户自己开、文档自己粘。回来时同一套：交接包写清"从哪回来、带回什么"，落地后按包内待办继续。

跟作者用法的区别：作者是一轮内来回搬两三次（grill→prototype→grill），这里是版本之间搬一次——频率低，单次价值更高（版本交接丢的东西比轮次交接多得多）。

## triage 全套：状态机 + brief + out-of-scope（V2.012，作者视频 + 正身）

Lane E（raw pile）进 lane 前先走 triage 状态机，不是直接开干：

- **双标签**：每票恰好一 category（bug/enhancement）+ 一 state（needs-triage/needs-info/ready-for-agent/ready-for-human/wontfix）。标签串以 setup 落的 `docs/agents/triage-labels.md` 为准。
- **ready-for-agent 必须附 brief**：光改标签不算 ready——brief 写清做什么、锚点、验收（见上游 AGENT-BRIEF.md 精神）。无 brief 的 ready = malformed。implement 只认"标签+brief 双全"的票。
- **out-of-scope 目录**：拒绝过的 enhancement 写进 `.out-of-scope/`（一事一记），下次同类请求命中直接 wontfix，不重审。已实现的东西不进这个目录（那是 HIT，不是拒绝）。
- **triage→diagnosing-bugs 衔接**：triage 出的 bug 票必须独立复现一遍——**不轻信 reporter 结论**（作者原话），复现步骤自己重做，复现证据进 brief。
- **spec 审计定位（V2.012，作者"spec 用完"精神）**：spec 冻结为基线后不再改（V2.009 已有），审计只看 issues + CHANGELOG，不回看 spec——spec 是目的地文档，不是活档案。证据链不断（issues 全链 + 水位线日志），但 spec 本体封存。

## 纯理解任务：不进 lane（V2.005，不改仓，不建标记，不计工程轮）

用户只说理解一下项目、看看代码，没有任何改动要求 → 不进 A/B/C/D/E 任何 lane，不写 spec，不碰仓库文件，不建 WORKFLOW-ACTIVE，不计工程轮次。做法：T0 分流判无 lane → 通读 ground-truth（README/入口/配置）→ 可派一个 Explore 子代理扫全仓 → 对话内给理解报告。setup 的首次进仓也不触发，等首个真实工程轮再做。只读轮无 verify 可贴，按 delivery-check 精神给零改动声明 + 可抽查清单。

**理解→动手边界（V2.016）**：理解轮内出现改动意图（"顺手修了""顺便加了"）→ 停手，另起工程轮（理解报告先交付，改动走新一轮 T0 + Gate 0 + lane）。一轮之内"先理解后动手"必须有书面 transition（"理解结束，转 Lane X"），无 transition 的混合轮 = malformed。反例：同轮先写"判为纯理解轮"又写"走 Lane C 先 chart"——两张皮，必拆两轮。

## 子 skill 只回文档：inline 执行并注记（V2.005）

子 skill 经 Skill 工具调用只返回文档文本、不实际执行 → 按文档步骤 inline 手动执行，效果等同调用，并在 trace/JOURNAL 注记 substitution。T0 ask-matt 若只回通用文档、无 Lane 裁决，Gate 0/shield/lane 判定按 prelane/bug-flow 自行落子并记录。

## 正身优先：fallback 只留给"未安装"（V2.006）

子 skill 已安装 → 动手前必须先经 Skill 工具加载它（哪怕它只返回文档正文），然后按其正文 inline 代执行——这叫代执行，合法。`references/` 里的速记版 fallback 只允许在对应 skill 未安装时使用。既没调 Skill 工具、也没读正身就干活 = malformed，JOURNAL 必记 deviation（未加载正身）。已装与否以本机技能目录实测为准（`~/.zcode/skills/`），不靠记忆。

## research 硬入口（V2.006 立项，V2.010 量化）

任务所需的外部事实（选型对比、强弱排序、上游/竞品行为、文档真相）→ 调 research 正身，走它的形态：后台代理调查 + 带引文的 Markdown 落盘，主线程继续干活；写进 spec/ticket 的事实必须有落盘文件或可引用来源。对话内顺手答一句常识不算任务调研。直接搜索顶替 research 形态 = deviation 必记。

**量化门（V2.010）**：同一任务需要 ≥2 处外部事实 → 必须调 research；只有 1 处且 5 分钟内可确认 → 可顺手查，但写进 spec 的事实仍需注明来源。

## 自扫清单：15 秒自问（V2.006 立项，V2.008 扩展）

对照**已加载的**判据自问，绝不重新读文件——重读只在会话首轮、compact 后、拿不准时。以下任一时刻命中，就过一遍 8 核心 + research/prototype/handoff/wizard 四支持，命中即调正身：

- **常规自扫**：每吞完一个 slice（Lane B/C）、每完成一个诊断步骤（Lane D）。
- **红转绿后**：问"这个缝还锁着别的行为吗？还能再往前锁一步吗？"——邻接行为、边界、护栏用例，值得锁就加（v1.033 日志测试、v1.034 诱饵测试都是绿了之后才发现还能再锁一步）。
- **同一问题连续失败 ≥3 次**：停下，把踩坑固化进 NOTES 或脚本再继续——连踩七坑靠自觉不是靠机制（v1.051 打包）。
- **新轮次病灶与既往已修缝同文件**：本轮冒烟必须覆盖既往缝（三缝冒烟，V2.008 转正）。
- **出现第 2 种修复方案之争，或单轮改动跨 ≥3 源文件**：自扫提示考虑 codebase-design——设计分叉靠 review 兜底就太晚了。
- **中途起雾（V2.018）**：prototype 回来结论未定、research 回包与 spec 冲突、seam 形状未定、review 报 spec 相反、hypothesise 多假设并存、红转绿后有邻接可锁、连败 ≥3 次——命中 6 硬门禁即调裸 `grilling`（见中途 grill 专节），命中 2 软提醒建议调。中途无起雾不硬烤，有起雾不烤 = malformed（硬门禁）或 trace 欠一句（软提醒）。

## 外部 review 意见处理（V2.008）

CodeRabbit/人类在 PR 上的意见：**先复现，有效才修，修完在 review 线程贴红→绿证据回复**；无效意见驳回并附证据（先数学实证再动手，v1.034 反例：CodeRabbit 说会压穿 min，实证后确认有效才改）。tautological 断言可拒（jsdom 无布局能力，快照锁不住样式行为）——但审稿人坚持就加上，无害的锁。混入无关文件的 PR（继承分支带脏 commit）→ 重建干净分支强推，diff 收敛到本次工作。

## 评审与 verify 永不豁免（V2.006，用户政策）

用户政策：宁可多花 token，也不让人参与。code-review（双轴）在每次提交之前必跑，纯静态 UI/展示层改动也不豁免——评审收益小时走快档（单轴 diff 自查 + 全量 verify 贴绿），但不许零评审提交。verify 同理：能自己跑就跑，多跑几遍、多贴几段原文都行；库没有验证命令就自发明 seam 断言 + 冒烟。人的参与只保留在 STOP 清单（高危/外部可见/真缺 token 换不到的信息）里。非 git 仓库：code-review 正身的 git 流程（fixed point/diff）无处安放时，快档就是明文许可的完整替代，不算漏、不必再问人。

## setup 判据（V2.006 立项，V2.010 状态位）

**状态位**：setup 跑完必须在项目里留下机器可查的标记——`docs/agents/` 目录存在即视为已跑（setup 的官方产物）。driver 不猜、不问、不看记忆：标记存在 → 跳过；标记不存在 → 本轮先跑 setup，再进 lane。

**新项目默认先跑**：空仓、无 AGENTS.md、无 tracker 痕迹 → 等同于"setup 没跑过"，进门第一轮先跑 setup（AFK 自答默认值：local-markdown tracker + 默认 triage 标签 + 单上下文 + 自答建 AGENTS.md）。纯理解轮（只读+汇报）不算 first touch、不触发 setup，但也不许动代码。连续推迟 = deviation 必记。

## grill 方向盘：每次改动先打磨（V2.010，向官方靠拢；V2.018 放宽到 frontier-empty）

官方规则：每次改动前都走 grill。开局路由：有目录用 `grill-with-docs`，无目录用 `grill-me`，零副作用一次性用裸 `grilling`（见 8 core + 四问 Q1）。AFK 自答保留（用户不在场），但"走 grill 流程"这个动作不许省：需求超过"改一个已定位的东西"时（新功能、新行为、新系统、新文案结构），必须有 frontier 提问 → 自答 → 收敛的书面过程进 spec/NOTES，然后才写 spec。"需求一句话说清就不调"是漏洞——一句话的需求恰恰最需要打磨。单行 typo/doc、已定位的纯修复，可跳过 grill，但要在 trace 里写一句为什么跳过。**轮次（V2.018，质量优先不计 token）**：开局 grill 跑到 frontier-empty 为止，软上限 4 轮、硬上限 6 轮；收敛判据 = 空（frontier 空）+ 稳（连续一轮无新增决策点）+ 净（每项自答有引用：文件/issue/子代理报告，无引用不计收敛）；发散刹车 = frontier 连续两轮不缩小即 STOP 写 `NEEDS-HUMAN.md`；不可逆清单/数值项永不计入收敛，单独走 HITL。找事实派子代理（research/prototype/Explore）并行，不占 grill 轮次；无证据的 `NEEDS-HUMAN` = malformed。

## 中途 grill 专节：continuous-grilling（V2.018，质量优先）

开局之后全程可烤。中途只许裸 `grilling`（按需配 `domain-modeling` consult：只读词汇表定词，不跑 session、不写 `CONTEXT.md`）；中途调 `grill-with-docs`/`grill-me` = malformed（user-invoked 互调）。中途产物只写三处：ticket `## Answer` 增量（frontier 问答 ≤3 组 + 结论 ≤5 行）+ map 指针 + `NOTES.md`；ADR/spec/`CONTEXT.md` 落盘归外层统一做，内层直写 = malformed。审计口径：只扫 JOURNAL `Skills called` 行 + Skill 调用痕，不扫正文提及；开局首个 user-invoked 不计套娃。

**6 硬门禁（命中未调裸 `grilling` = malformed）**：①prototype 回来必回烤（手感/外观跑起来才知道 → 结论变决策，1–2 轮）；②research 回包必烤（外部事实落盘后"事实变决策"，与 spec 冲突处逐条裁决）；③Lane C resolve 挖出 spec 范围外的新系统/依赖/管线/成本/合规 → 先烤方向（还做不做/Destination 改不改/STOP 还是另起 map）再按结论走，禁直接 Collapse/Build；④tdd 吞缝前缝位未定（两实现打架/调用方知道太多/第二同形 adapter/跨 ≥3 文件）→ 先微烤接口形状与调用方知识，收敛再定 seam，consult codebase-design 在烤后；⑤code-review 报"行为与 spec 相反/红线 breach"→ 先回烤（改码还是改 spec），结论写进 ticket/NOTES 再修；⑥diagnosing-bugs hypothesise（Phase-1 红已建后）→ 必烤假设语句排序（3–5 可证伪假设排下一实验序，见 bug-flow 加注）。中途单点默认 1–2 轮，frontier 未空且有进展可加 1 轮；B-vs-D 拿不准允许先烤 1 轮分流判据（可复现否/有无 Phase-1 红/行为错还是需求不明），烤完必须落 B 或 D，禁连烤。

**2 软提醒（建议调，不记 malformed，未调写一句为什么）**：①红转绿后"还能再锁一步吗"→ 烤一轮候选 seam（邻接行为/边界/护栏），值得锁就加，不值得写一句；②同一问题连续失败 ≥3 次 → 停下重烤策略（停不停/踩坑如何固化/冒烟怎么补），只记 NOTES 不重烤不许继续。切片/Collapse 边界争议（>3 slices 转 Lane C、parked-collapse 多分支）暂放建议级，V2.019 再议。

## prototype 硬入口：跑起来才知道就先跑（V2.010）

出现以下任一，动手写生产代码之前必须二选一：①两种方案争执（A 还是 B 拿不准）；②状态机/交互/手感跑起来才知道。选 prototype（30 分钟内可跑出：按官方分支选 LOGIC 可分享单文件 / UI 多变体，throwaway 标记、一键即跑、无持久化，结论折进真代码，原型挂 throwaway 位置留指针）或 codebase-design 的 design-it-twice（跑不出来时）。两个都不调直接开写 = malformed。

## codebase-design 前置到 tdd（V2.010）

tdd 吞缝之前先问一句"缝的位置和接口形状定了吗"——没定（两个实现打架、调用方知道太多、第二个同形 adapter 出现），先翻 codebase-design 词汇表做 consult（读正身、不跑 session；跑不出来才走 design-it-twice 并行设计），定了再写测试。官方 tdd 正文原话的落地。（V2.018：consult 之前先过一轮裸 `grilling` 微烤——只问接口形状与调用方知识，收敛再定 seam；见中途 grill 专节硬门禁④。）

## 编排铁律：user-invoked 不互调（V2.010，官方原文）

官方规则：user-invoked skill 只编排，可调 model-invoked，但永远不调另一个 user-invoked。driver 自己是编排者，调谁都行；但被调的 user-invoked skill（to-spec、to-tickets、implement、grill-with-docs、wayfinder、triage 等）不许再调另一个 user-invoked——to-spec 里不许调 implement，implement 里不许调 code-review（implement 正文的"/tdd…/code-review"指运行其纪律，不是调其 skill）。model-invoked（tdd、code-review、diagnosing-bugs、prototype、research、codebase-design、`grilling`、`domain-modeling` 等）可由任何人调——其中裸 `grilling` 与 `domain-modeling` consult 中途任何上下文可调；`grill-me`/`grill-with-docs` 是 user-invoked（正身 `disable-model-invocation: true`），中途禁调（见中途 grill 专节 V2.018）。违反 = malformed。

## 长任务 spec 演进位：冻结基线 + issues 增量（V2.009）

跨轮长任务（Lane C，多轮同一目标）：第一轮落定的 `spec.md` 即冻结为 M0 基线，后续轮次不再改它——增量只记决策票（`issues/NN-*.md`）+ CHANGELOG，审计以 issues + CHANGELOG 为准。审计类轮次不再因"spec 没更新"误报。短任务（单轮 Lane B）不受影响，照旧 spec 内嵌 slices。

## tdd 不适用的既定解释（V2.009）

纯渲染/文案/纯文档/只读审计轮没有可吞的 seam → 记 `tdd n/a` 是合规的，不是漏用。但门禁不许裸奔：回归全绿 + 包验/语法（或哈希验签）必须有。连续四轮以上同一解释无歧义即视为既定解释，不必每轮论证。
