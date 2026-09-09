# 提示词改动对照

基准：原 trisoul 提交 `4189f90305e545dbb82654f561fc4d83eed96d03`。原文快照在 [prompt-origin.json](test/fixtures/prompt-origin.json)，新版实际文本在 [prompts.mjs](src/prompts.mjs)、[dsh-agent.mjs](src/dsh-agent.mjs)、[hub.mjs](src/hub.mjs) 、[tasks.mjs](src/tasks.mjs) 和 [canvas.mjs](src/canvas.mjs)。表中“删除”指不迁入新版，不修改原项目。

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
| P01 主身份 | 宿主身份 + 每魂 persona | You are trisoul_x. | DSH 插件的主执行者名称；行为主体仍来自三官原文。 |
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
| P12 read.limit | 原宿主：Maximum number of lines to return. Defaults to ${caps.limit}. 首版独立原型改为 Omit to read to the end. | 恢复 DSH 原生参数描述与实际读取上限：Maximum number of lines to return. Defaults to ${caps.limit}. | 文件读取已交回 DSH，描述随宿主实际配置生成。 |
| P13 bash 工具 | 首版只留下执行命令、全新 shell、workdir 与退出码说明。 | 恢复 DSH 原生 `tool:bash` system 段和 bash schema，包括环境、超时、输出落盘与后台作业说明。 | 对应宿主已经提供的真实能力；不再维护简化版命令执行器。 |
| P14 任务工具 | 原 task_map 定义与锚点、todo 完成标记、verify_link 验证关联分为三个工具；独立原型先合为 tasks(items)。 | 一个 `todo_write(todos)`：每条记录有 content、status、可选 source、verification.method / result。任务原文纪律与验证原文融合到同一工具说明。 | 任务定义、进展和验证属于同一条记录；去掉跨工具关联与转交，细项见 P33–P36。 |
| P15 web_fetch | 首版把 Fetch the content… 与 Cite the URL… 合成 description。 | 恢复 DSH 的 web_fetch 工具说明与独立 `tool:web-fetch` system 段；web_search 由宿主同时注册。 | 原先的分层指导恢复，网页格式转换、检索与引用说明跟随真实宿主工具。 |
| P16 recall | 原 trisoul_recall 的两通道说明；范围固定写 global + cross-project + this project，回捞用 seqRange。 | 原句主体保留；范围改为 scoped to this session’s selected memory range；原文参数改为 {from:a,to:b}，工具名 recall。scope 参数的 everything visible to this project 改为 everything visible to this session。 | 支持用户在输入区选择的三种记忆范围，不能再承诺每个会话都有全局记忆；参数对齐新工具。 |
| P17 环境与项目指令 | 首版只有 Working directory / Platform / Today 三行。 | 恢复 DSH 环境、时间、项目指令、技能目录及运行时上下文层。 | 原型缺少的通用能力由宿主提供；当前目录、日期和项目指令根据会话生成。 |
| P18 用户语言 | 原软路线 output 纪律：respond in the user's language | Respond in the user's language. | 从被删除的交稿纪律迁到主提示词。 |
| P19 文件工具指导的摆放 | 原宿主 tool:read / tool:write / tool:edit 独立 system 段；首版合入 description。 | 恢复原生工具 schema + 独立 system 段；write/edit 仅删 P20 的括号。 | 恢复 trisoul 原有分层，实际读写能力复用 DSH。 |
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
| D09 | verify_link 独立操作、I4 未完成弹回、I6 文字复核、I7 证据分型旁白 | 删除独立验证工具和程序收官闸门；验证方法与证据质量原文迁入统一任务工具。 | 用户要求把验证合入任务；证据记录与任务状态一起维护。 |
| D10 | TODO_NUDGE / TODO_EMPTY_NUDGE | 不注入官位专属提醒；原任务使用场景与完成标准并入 todo_write 的 description。 | 对齐官已融合为主执行者，清单由宿主提供。 |
| D11 | CURATE_RULES / CURATE_FORMAT_TAIL 与 signals.overlap/conflict | 不迁入独立周期整理作业；删除未使用的 overlap/conflict 文案常量；OPS_DESC 中旧作业指代按 P22 删除。 | 现有消化作业维护条目，保留同一件事更新、宁重复勿误并等规则。 |
| D12 | DIGEST_DESC.workdoc | 不迁入独立任务补注文档视图，并删除未使用的文案常量；当前工作状态由 status 维护。 | 避免同时维护两份重叠的会话工作状态。 |
| D13 | DIGEST_DESC.phaseClosed | 不迁入整段阶段放行信号，并删除未使用的文案常量；compactable / nowCompactable 原文保留。 | 按区间释放即可，不增加另一个放行通道。 |
| D14 | 记忆 LLM picker：Output JSON only {indexes:[…]} | 删除，recall 采用本地文本匹配。 | 保留原文回捞与分层记忆，避免每次召回再调用模型。 |
| D15 | ASK_SYSTEM / ASK_FORMAT / ANSWER_SYSTEM：压缩探针出题、作答、判分 | 不迁入。 | 按最小化要求不加自动验证流程；原文永远保留，仍能按序号回捞。 |
| D16 | 任务补注的 renew/rewrite/append 三种专属头与独立工作文档 | 不迁入多路线；保留开场记忆注入与状态快照追加。 | 去掉并行维护的视图和历史兼容分支。 |
| D17 | fs-observation-policy 的强制先读提示与宿主权限说明 | 禁用 fs-observation-policy，并仅从 tool:write / tool:edit 删除该政策括号。DSH 的运行环境与权限上下文按实际配置生成，默认完整访问、审批 never。 | 插件不新增产品闸门；宿主原生工具与设置保持一致。 |

## 保持原文的后台与工具部分

| 部分 | 处理 |
| --- | --- |
| 记忆 CONSTITUTION | 全文逐字保留（稳定事实、原因、日期、少记、避免过程信息）。 |
| OPS_DESC 全部 6 个字段 | 除 P22 所列旧作业括号外逐字保留；工具参数仍是 add/update/retire、scope、key、text、target。 |
| DIGEST_DESC.digest / compactable / nowCompactable | 逐字保留。nowCompactable 的 id 从旧字符串改为本地事件整数序号，说明语义不变。 |
| STATE_SYSTEM 两区定义与 Discipline | 除 P07 首句外逐字保留，包括恒真区 append-only。 |
| STATE_FORMAT 的 pin/status、代谢、语言与原文引用规则 | 除 JSON 外壳外逐字保留。 |
| 手术刀 system | 全文逐字保留 Done / Errors / Decisions / Not yet done 与原先的英文、引用纪律；合并原检查点的说明保留。 |
| 长期记忆开场头、工作状态版本头 | 原句主体保留；开场层级按已选范围显示，状态带实际事件序号。 |
| read/write/edit | DSH 当前工具 schema 与独立提示层；只删除失效政策括号（P20）。 |
| todo_write 的完成标准 | 原 todo 的真实完成、失败不得勾选、发现未完成要取消勾选等原句保留；没有把它实现成硬闸门。 |

## 本轮 DSH 适配补充

宿主基准：DSH `0.1.5-alpha.1`，提交 `5dda764ed3aa172535a7967b06ff95d9cbfe536a`。插件使用该版本的 standard Agent preset 为底稿。以下是与独立原型相比的恢复与适配，不再往主提示词中添加“某机制不存在”的解释。

| 编号 / 部分 | 适配前 | 现在 | 原因 |
| --- | --- | --- | --- |
| P24 身份层 | 自己拼接一份 system 字符串 | `trisoul-x:persona`：身份 + 三官融合原文 + REPLY_GUIDE + 用户语言；由 DSH 注册和排序 | 恢复原分层；正文不再另写。 |
| P25 通用工具层 | 8 个自写工具，部分说明合并 | DSH 原生文件、搜索、Shell、后台作业、技能、目标、用户提问、网页、按需子代理与工作流；插件提供 note、recall 和融合后的 todo_write | 通用执行复用 DSH；任务工具扩展为任务与验证的统一记录。 |
| P26 工具层中的失效括号 | DSH 默认 write/edit 仍提到已禁用的 fs-observation-policy | 组装时仅删除 ` (the default fs-observation-policy requires it)`，其余原生段落保持 | 延续 P20，避免向模型陈述不存在的强制行为。 |
| P27 按需子代理 | 独立原型未实现 | 恢复 DSH 的 subagent / subagent_fork / 查询、等待、发送与停止工具及原生说明 | 用户明确选择“保留，按需调用”；无固定三模型编队。 |
| P28 技能与项目上下文 | 独立原型未加载 | DSH 的 agent-instructions、skill-filesystem、tool-skill 按会话加载 | 原 trisoul 通用能力的一部分，不能以简化为由漏掉。 |
| P29 动态记忆头 | 固定写 global / cross-project / project | 全量时保留原层级；项目或会话模式显示已选范围和实际所属标识，其余记忆头原句保持 | 与输入区选择保持一致。 |
| P30 后台输入 | 读取自写 JSONL 的消息 | 读取 DSH 事件原文：seq、事件类型、工具名称与参数、结果、约束、状态、记忆和待复审区间 | 调整数据来源；MEMORY_CONSTITUTION / STATE_RULES / SURGEON_SYSTEM 不改。 |
| P31 检查点与回捞 | 自己重建发送历史 | DSH V3 surface 原位替换；记录头保留 seq 回捞信息、已完成/未完成语义 | DSH 持久化和轨迹能够直接理解整理事件。 |
| P32 计划模式 | 独立原型无模式切换 | 使用 DSH preset 中的 plan-mode 原文，仅在用户进入计划模式时生效 | 复用宿主已有选择能力，不增加默认执行闸门。 |

| 编号 / 部分 | 原文 / 旧机制 | 新文 / 处理 | 原因 |
| --- | --- | --- | --- |
| P33 需求锚点 | Creates and edits the todo list anchored to the user's own wording；excerpt、anchor 的 from/to 和 msg；单独 op:transcript/view | 保留首句与原任务使用纪律；`source` 保存相关用户原话，直接随任务记录一起提交 | 用户原始消息仍在 DSH 会话中；取消独立摘录表与跨表锚点，用同一任务中的原话保留需求依据。 |
| P34 验证时机 | When to link: link the evidence the moment a task is done — not in one sweep after everything is built. | When to record: record the verification result the moment a task is done — not in one sweep after everything is built. | 证据写回当前任务的 verification.result；仅把独立“关联”动作改成任务内记录。 |
| P35 验证纪律 | How a test earns its place…、What does not count… 两段；Evidence ranks… | 前两段逐字保留。证据层级段只改 Link the highest rung → Record the highest rung，reason must say why → the result must say why，其余原文保持 | 保留从需求推导验证、真实路径、证据质量与如实说明的原则；适配新的存储字段。 |
| P36 验证工具协议 | A task counts as verified only through what is linked here；独立 op:link/run/unlink/view、自动按文件扩展名执行测试 | 删除独立关联协议与“只有经过此工具才算验证”的定义。新增接口说明：Send the complete list in todos. Each task carries its own status, source wording, and verification method/result. Run checks with the available execution tools and record their observed results on the same task. When a task changes, update its status and verification to match the revised task. | 一个任务工具统一更新；执行仍用普通工具；验证未记录时页面如实显示，不设置自动拒绝完成或另一个验证循环。 |

## 现在的组成

| 层 | 来源与职责 |
| --- | --- |
| 主身份与行为 | 三官原文融合 + 完整回复纪律 + 用户语言 |
| 通用 system 段 | DSH 工具使用指导、环境与能力；任务和验证纪律合入统一 todo_write 工具 |
| 项目与技能 | DSH 按当前目录和会话发现、加载 |
| 动态上下文 | 用户选定范围内的记忆、工作状态、用户消息和工具结果 |
| 工具 schema | DSH 原生工具 + note / recall + 含验证字段的 todo_write |
| 记忆与状态后台 | 原记忆宪法 + 两区规则 + save_context 工具 |
| 上下文整理后台 | 原手术刀提示词，输出普通文字工作纪要 |

## 核验

自动测试对照原文快照检查主提示词、TASK_GUIDE 原任务纪律、TASK_VERIFY_GUIDE 原验证纪律、记忆宪法、OPS_DESC 和手术刀原文；使用真实 DSH 加载插件、原生工具和本地模型端点，验证自然语言输出、项目指令、技能目录、记忆注入、范围绑定、V3 整理与回捞、任务与验证同一记录及跨轮恢复。原文快照仅用于对照测试，不参与运行时加载。

使用用户授权的 DeepSeek v4 flash 进行了真实文件读写、技能加载、按需子代理与后台记忆联调。联网搜索需要另行配置宿主搜索提供方；本轮未把 Ark 模型密钥当作搜索凭据。
