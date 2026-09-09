# 提示词改动对照

基准：原 trisoul 提交 `4189f90305e545dbb82654f561fc4d83eed96d03`。原文快照在 [prompt-origin.json](test/fixtures/prompt-origin.json)，新版实际文本在 [prompts.mjs](src/prompts.mjs)、[tools.mjs](src/tools.mjs) 和 [context.mjs](src/context.mjs)。表中“删除”指不迁入新版，不修改原项目。

原则：保留原句，融合重复职责；删除不适配新架构的要求与事实假设。模型提示词只描述现有能力，不补写“没有某机制”的解释；note 用于按需记录，不承接旧交稿中的思维摘要要求。不以“精简”为由重写仍然适用的行为规则。

## 三官融合：保留哪些原句

| 编号 | 原文位置 | 新版处理 | 原因 |
| --- | --- | --- | --- |
| M01 | 对齐第 1 段：Read the user's actual words closely… | 整段逐字保留。 | 对齐任务范围仍然是主模型职责。 |
| M02 | 博识第 1 段：Treat your own memory as unreliable… | 整段逐字保留。 | 查证与不确定性表达仍然适用。 |
| M03 | 三官 When you have enough information to act… 段 | 选博识完整段，含共同句和 Note the difference… 补充，原句不改。 | 一位执行者不必重复三次同一组行动规则；知识核实补充保留。 |
| M04 | 三官 You are operating autonomously… 段 | 选博识完整段，删除 P21 所列非实时交互假设；其余原句保留，包括 Gathering missing information… 补充。 | 融合重复；当前页面支持实时观看，不再假定用户不在场。 |
| M05 | 三官 Exception: when the user is describing a problem… 段 | 先保留对齐完整段，再追加博识、实证各自的独有尾句，逐字保留。 | 咨询时只交付评估，以及评估须标明证据与推测，三者合并。 |
| M06 | 三官 Before ending your turn… 段 | 保留对齐完整段；再追加实证的 "Complete" means verified… 原句。 | 保留完成范围与真实验证的行为标准；不实现程序收官闸门。 |
| M07 | 三官 Before running a command that changes system state… 段 | 选博识完整段，包含 the failure you remember is a hypothesis…；原句不改。 | 保留根据现场证据行动的要求。 |
| M08 | 三官 For actions that are hard to reverse… 段 | 选实证完整段，保留原授权边界、诚实报告及 verified 定义。 | 保留已有提示词，未新增安全门、审批系统或验证工具。 |
| M09 | 每魂的身份与工具分配 | 改为一个主模型共享普通工具；不再注入魂名与专属官位工具块。 | 多魂和私有取证循环已经移除。 |

## 实际改写与新增：原文和新文

| 编号 / 部分 | 原文或原机制 | 新版实际文本 | 为何需要改 |
| --- | --- | --- | --- |
| P01 主身份 | 宿主身份 + 每魂 persona | You are trisoul_x. | 独立应用的新名称；行为主体仍来自三官原文。 |
| P02 原生工具协议 | submit_draft 是唯一交付渠道；直调普通工具会丢弃；动作填执行栏。首版另加：Use native tool calls directly. There are no draft submissions, votes, private lookup tools or execution fields. Working notes are optional; use note when useful, without a required envelope. | 整段删除，不再注入主提示词。 | 旧交付机制已删除，无需再向模型解释它们不存在；可用工具由实际工具定义提供。 |
| P03 用户回复 | The prose delivered to the user for this step. Empty only when the execution fields carry entries — the executed actions are themselves the output. A submission with nothing queued to execute closes the turn, and this field is then the complete answer: open with the outcome itself — the sentence the user would ask for as the TLDR — then supporting detail; say plainly what remains unverified — never close on an in-progress sentence, and if a check still needs to run, queue it in the execution fields instead; nothing load-bearing may live only in findings. | When you finish the turn, your prose is the complete answer: open with the outcome itself — the sentence the user would ask for as the TLDR — then supporting detail; say plainly what remains unverified — never close on an in-progress sentence, and if a check still needs to run, run it with tools first. | 保留完整回复、结果优先和如实说明验证状态的原句；执行栏改为实际工具执行，删除 findings / 工作笔记与最终回复的旧绑定。 |
| P04 findings 迁移 | What you newly established this step — what you observed, what the results told you. Written as yourself mid-task, not a reporter summarizing someone else: open on the finding — 'Turns out …', 'Looking at `server.js`, …'. Code entities in backticks; error messages and measured values verbatim. Facts land once, in the step that established them — never retell earlier findings or restate the task. Empty when nothing new has come in yet (e.g. the first step, before any results). Prose only, no lists. | 删除整段，不作为 note 的说明。 | 这是旧交稿的逐步摘要、叙述人称和格式要求；按需工作笔记无需复述每一步观察。 |
| P05 plan 迁移 | How you'll proceed after this step, at whatever depth you've actually thought it through — when you've already settled the design, write it out in full, key code included: thinking not recorded here is gone when the step ends. No need to re-paste code that ships in files/edits this step. Only what you newly settled or changed; empty when the plan you already recorded still stands — never pad. | 删除整段，不作为 note 的说明。 | 原生会话保留上下文，不再要求每步重述思考和方案；原 files/edits 字段指代也随整段删除。 |
| P06 工作笔记入口 | 四格 findings、plan 每步交稿。首版入口：Save a working note when useful; this is optional, not a delivery channel. 参数：The working note, in ordinary prose | Save a useful fact, decision, or unfinished plan for later work. 参数 text：Note content | 仅说明笔记可以保存什么；去掉交付渠道对比、每步摘要和文体限制。 |
| P07 后台身份融合 | You are the canvas scribe, maintaining the two zones of the current session's working context (session-scoped only; cross-session long-term facts belong to the memory hub — do not duplicate them):<br>- Pinned truths: hard constraints from the user's own words, explicitly settled decisions, key facts that must not be violated. Append-only, one fact per entry, short sentences, each understandable on its own.<br>- Status: a snapshot of the current task's plan / progress / conclusions, rewritten wholesale each time, concise.<br>Discipline: when unsure, keep it out of the pinned zone; process noise stays out of the status zone. | In this same job, also maintain the two zones of the current session's working context (session-scoped only; cross-session long-term facts belong to the memory hub — do not duplicate them):<br>- Pinned truths: hard constraints from the user's own words, explicitly settled decisions, key facts that must not be violated. Append-only, one fact per entry, short sentences, each understandable on its own.<br>- Status: a snapshot of the current task's plan / progress / conclusions, rewritten wholesale each time, concise.<br>Discipline: when unsure, keep it out of the pinned zone; process noise stays out of the status zone. | 记忆与工作状态由同一次后台调用处理，只改开头身份连接句；两区定义与纪律完整保留。 |
| P08 后台交付协议 | Output JSON (JSON only) + 正文 JSON 骨架 | Submit the result using save_context. | 取消整篇 JSON 格式锁和正文解析；正常工具调用的参数仍结构化。 |
| P09 后台工具入口 | 旧版通过正文 JSON 提交结果。首版：Commit this maintenance batch once. This records context and memories; it does not perform the user task. | Save this batch’s digest, working state, and memory changes. | 只说明 save_context 实际保存的内容，删除“不执行用户任务”的多余对比。 |
| P10 状态字段 | STATE_FORMAT 开头的 JSON 强制句与示例对象 | 移除这两行；pin/status 的增量、全量与代谢规则原文迁入后台 system 和字段描述。 | 字段说明需要继续被模型读到，但不再要求正文套 JSON。 |
| P11 历史回捞参数 | call trisoul_recall with {"query":"what you need","seqRange":{"start":lo,"end":hi}} | call recall with {"query":"what you need","from":lo,"to":hi} | 对应新版工具名称与扁平参数；工作记录头另有 P23 修正。 |
| P12 read.limit | Maximum number of lines to return. Defaults to ${caps.limit}. | Maximum number of lines to return. Omit to read to the end. | 新版没有宿主默认读取截断；传 limit 才限制行数。 |
| P13 bash 工具 | 原 bashDescription 的执行说明 + DSH 环境变量、沙箱、截断落盘、后台作业与升级授权说明。 | Execute a bash command (`bash -c`) and return its stdout/stderr. Each call runs in a fresh shell: no state (cwd, variables, functions) persists between calls — pass `workdir` instead of using `cd`. Non-zero exits are reported as `[exit code: N]`. | 前三句逐字保留。宿主沙箱、环境注入、输出截断和后台任务服务未迁入，相关说明必须删除。 |
| P14 tasks 接口 | task_map 的节选/锚点/op 操作 + todo 的完成态操作。 | A clear, complete todo list greatly raises the completion rate in medium-to-large tasks. Use it when the task takes three or more distinct steps, when the user lists several things at once, or when new instructions arrive mid-task — capture them right away. Skip it for single-step work and plain conversation — a list there is overhead, not help. Supply the complete current list in items. Decide how a task will be verified before building it, and check it off the moment it is fully done — one at a time, as you go, not all of them at the end. Never check off a task while its tests are failing, the implementation is partial, or an error on it is unresolved; when several are checked together, that must hold for every one of them. Uncheck a task that turns out not to be done. Remove a task only when it no longer belongs on the list — irrelevant, impossible, or overtaken by newer instructions — never because it is hard; say why in your reply. | 合成一个按需清单工具。保留触发时机、完成标记与如实维护的原句；结构操作改为 items 全量清单，draft 改 reply。 |
| P15 web_fetch | Fetch the content of a specific HTTP(S) URL and return it decoded to text. + 独立 system 中的引用要求。 | Fetch the content of a specific HTTP(S) URL and return it decoded to text. Cite the URL as a markdown link when you use its content. | 原 description 加原 system 中 Cite the URL… 句，融合到一个工具说明。没有改写原句。 |
| P16 recall | 原 trisoul_recall 的两通道说明全文 | Search your long-term memory, or retrieve the verbatim text behind a condensed work record. (1) Recall: pass query (natural language) to get related memories (scoped to what this project can see: global + cross-project + this project); optionally narrow with scope. (2) Retrieve originals: condensed records in the context are tagged with "seq a..b" — when you need the original content that was condensed away, pass {from:a,to:b} to get the raw event text of that span verbatim (no model rewriting). | 原句保留，只把 seqRange:{start:a,end:b} 改成 {from:a,to:b}；工具名改为 recall。 |
| P17 环境块 | envBlock(): Working directory / Platform / Today | 三个标签与原语义保留，目录与日期取新会话。 | 新项目独立运行，不能继续使用原 profile 的目录或路由。 |
| P18 用户语言 | 原软路线 output 纪律：respond in the user's language | Respond in the user's language. | 从被删除的交稿纪律迁到主提示词。 |
| P19 文件工具指导的摆放 | 原宿主 tool:read / tool:write / tool:edit 的独立 system 段 | 合入对应工具 description；read 指导逐字保留，write/edit 只删 P20 所列括号 | 新版没有宿主提示词注册服务，指导仍需完整送达模型。 |
| P20 文件操作的宿主政策指代 | (the default fs-observation-policy requires it) | 删除这个括号，保留“先读取已有文件”“优先定点修改”等其余原句 | 该宿主政策未迁入，不能继续声称代码有此硬要求；不增加检查闸门。 |
| P21 自主执行的交互假设 | The user is not watching in real time and cannot answer questions mid-task, so asking 'Want me to…?' or 'Shall I…?' will block the work. | 删除这一句，其余自主执行规则保留。 | 当前对话和工具结果实时显示；是否自主行动取决于任务授权，不需要假定用户无法观看。 |
| P22 记忆整理作业指代 | duplication is acceptable (the curation job cleans it up), wrongful merging is not | duplication is acceptable, wrongful merging is not | 独立 curation 作业未迁入，不能承诺后续自动清理；宁重复勿误并等原规则保留。 |
| P23 工作记录恢复说明 | treat it as done and continue; don't redo or restate it. | continue from the recorded progress and carry forward unfinished work. | 压缩记录本来就包含 Not yet done，不能笼统地把整份记录都当成已完成。 |

## 因删除旧机制而不再发送的提示词

| 编号 | 原位置 / 内容 | 新版处理 | 原因 |
| --- | --- | --- | --- |
| D01 | submitDraftToolSchema / draftJsonSchema 中四格及 actions/files/edits/lookup 格式契约 | 删除整稿 schema；output 的回复规则按 P03 迁移；findings/plan 不再作为 note 的写作要求。 | 原生工具循环直接执行动作，按需笔记无需承担旧交稿职责。 |
| D02 | submitFormatBlock、submitFirstOrder、evidenceReturnOrder、draftDiscipline | 删除格式教学、交稿直令与内层取证直令。 | 主循环直接消费原生工具结果。 |
| D03 | jsonFormatBlock / response_format / text.format 输出锁 | 删除。 | 新版本不依赖整篇回复的结构锁。 |
| D04 | 缺封皮的 mend / 代填指令 | 删除。 | 不再存在缺封皮状态。 |
| D05 | cast_ballot、state_divergence、候选卡与表决直令 | 删除。 | 没有投票、败稿和候选比较。 |
| D06 | tipsMessage：parallel timelines / never delivered | 删除。 | 不存在未执行的败者时间线。 |
| D07 | toolClassesHint、officerHint、DEDICATED_BLURBS | 删除工具阶层与官位排他说明；知识查证原则保留在博识原文。 | 所有主模型工具走同一循环。 |
| D08 | ENVELOPE_DESC.action：固定 -ing 当前动作句 | 删除独立动作栏格式要求。 | 没有独立 action/move 栏；不是改写自然回复风格。 |
| D09 | verify_link 描述、I4 未完成弹回、I6 文字复核、I7 证据分型旁白 | 不迁入。 | 按最小化要求，不建立强制验证与收官闸门；实证行为原句仍保留。 |
| D10 | TODO_NUDGE / TODO_EMPTY_NUDGE | 不再按官位自动注入；任务使用场景保留在 tasks 工具原文。 | 没有对齐官专属提醒；减少每步机制消息。 |
| D11 | CURATE_RULES / CURATE_FORMAT_TAIL 与 signals.overlap/conflict | 不迁入独立周期整理作业；删除未使用的 overlap/conflict 文案常量；OPS_DESC 中旧作业指代按 P22 删除。 | 现有消化作业维护条目，保留同一件事更新、宁重复勿误并等规则。 |
| D12 | DIGEST_DESC.workdoc | 不迁入独立任务补注文档视图，并删除未使用的文案常量；当前工作状态由 status 维护。 | 避免同时维护两份重叠的会话工作状态。 |
| D13 | DIGEST_DESC.phaseClosed | 不迁入整段阶段放行信号，并删除未使用的文案常量；compactable / nowCompactable 原文保留。 | 按区间释放即可，不增加另一个放行通道。 |
| D14 | 记忆 LLM picker：Output JSON only {indexes:[…]} | 删除，recall 采用本地文本匹配。 | 保留原文回捞与分层记忆，避免每次召回再调用模型。 |
| D15 | ASK_SYSTEM / ASK_FORMAT / ANSWER_SYSTEM：压缩探针出题、作答、判分 | 不迁入。 | 按最小化要求不加自动验证流程；原文永远保留，仍能按序号回捞。 |
| D16 | 任务补注的 renew/rewrite/append 三种专属头与独立工作文档 | 不迁入多路线；保留开场记忆注入与状态快照追加。 | 去掉并行维护的视图和历史兼容分支。 |
| D17 | 宿主工具提示中的 fs-observation-policy / sandbox_permissions / justification | 不迁入对应机制说明与参数。 | 新项目不装宿主安全门和升级审批；常规 read/write/edit 描述及实际参数保留。 |

## 保持原文的后台与工具部分

| 部分 | 处理 |
| --- | --- |
| 记忆 CONSTITUTION | 全文逐字保留（稳定事实、原因、日期、少记、避免过程信息）。 |
| OPS_DESC 全部 6 个字段 | 除 P22 所列旧作业括号外逐字保留；工具参数仍是 add/update/retire、scope、key、text、target。 |
| DIGEST_DESC.digest / compactable / nowCompactable | 逐字保留。nowCompactable 的 id 从旧字符串改为本地事件整数序号，说明语义不变。 |
| STATE_SYSTEM 两区定义与 Discipline | 除 P07 首句外逐字保留，包括恒真区 append-only。 |
| STATE_FORMAT 的 pin/status、代谢、语言与原文引用规则 | 除 JSON 外壳外逐字保留。 |
| 手术刀 system | 全文逐字保留 Done / Errors / Decisions / Not yet done 与原先的英文、引用纪律；合并原检查点的说明保留。 |
| 长期记忆开场头、工作状态版本头 | 原句保留，变量改取新存储。 |
| read/write/edit description 与文件参数名 | 原描述、file_path/content/offset/old_string/new_string/replace_all 保留；limit 默认说明的变化见 P12。 |
| tasks 的完成标准 | 原 todo 的真实完成、失败不得勾选、发现未完成要取消勾选等原句保留；没有把它实现成硬闸门。 |

## 核验

自动测试对照原文快照，检查主提示词的保留段落（仅扣除 P21）、记忆宪法、OPS_DESC（仅扣除 P22）和手术刀全文。首版已通过真实模型调用验证自然语言、原生工具和后台提交；本轮提示词清理运行现有回归测试，不重复调用付费 API。原文快照仅供对照与测试，不参与运行时加载。
