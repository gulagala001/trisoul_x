# 提示词改动对照

基准：原 trisoul 提交 `4189f90305e545dbb82654f561fc4d83eed96d03`。原文快照在 [prompt-origin.json](test/fixtures/prompt-origin.json)，新版实际文本在 [prompts.mjs](src/prompts.mjs)、[dsh-agent.mjs](src/dsh-agent.mjs)、[hub.mjs](src/hub.mjs)、[tasks.mjs](src/tasks.mjs)、[todolist.mjs](src/todolist.mjs) 和 [canvas.mjs](src/canvas.mjs)。表中“删除”指不迁入新版，不修改原项目。

原则：尽量保持原版，保留原句与通用能力；只融合职责并调整架构不兼容的协议，以及用户逐段审定的改动。模型提示词只描述现有能力，不补写“没有某机制”的解释；note 用于按需记录，不承接旧交稿中的思维摘要要求。不以“精简”为由重写仍然适用的行为规则。

当前任务结构：todo_write 负责需求、任务和完成状态，verify_link 独立负责验证；二者共用任务数据和页面。下表保留迁移与审核过程，涉及验证合入 todo_write 的旧处理由本轮 P47–P50 覆盖。

用户随后要求恢复三类动态收尾提醒，当前已按 P51–P54 接回单模型回合结束流程。

## 三官融合：保留哪些原句

| 编号 | 原文位置 | 新版处理 | 原因 |
| --- | --- | --- | --- |
| M01 | 对齐第 1 段：Read the user's actual words closely… | 整段逐字保留。 | 对齐任务范围仍然是主模型职责。 |
| M02 | 博识第 1 段：Treat your own memory as unreliable… | 整段逐字保留。 | 查证与不确定性表达仍然适用。 |
| M03 | 三官 When you have enough information to act… 段 | 选博识完整段，含共同句和 Note the difference… 补充，原句不改。 | 一位执行者不必重复三次同一组行动规则；知识核实补充保留。 |
| M04 | 三官 You are operating autonomously… 段 | 选博识完整段，逐字保留，包括用户不在场的假设和 Gathering missing information… 补充。 | 融合重复；按用户逐段核对意见保留原自主执行设定。 |
| M05 | 三官 Exception: when the user is describing a problem… 段 | 先保留对齐完整段，再追加博识、实证各自的独有尾句，逐字保留。 | 咨询时只交付评估，以及评估须标明证据与推测，三者合并。 |
| M06 | 三官 Before ending your turn… 段 | 保留对齐完整段；再追加实证的 "Complete" means verified… 原句。 | 保留完成范围与真实验证的行为标准；原任务收尾提醒现按 P51–P54 恢复。 |
| M07 | 三官 Before running a command that changes system state… 段 | 采用对齐／实证共同版本；删除博识官的扩写，具体原文见 P37。 | 2026-09-09 用户逐段核对后要求删除该补充。 |
| M08 | 三官 For actions that are hard to reverse… 段 | 选实证完整段，保留原授权边界、诚实报告及 verified 定义。 | 保留已有提示词，未新增安全门、审批系统或验证工具。 |
| M09 | 每魂的身份与工具分配 | 改为一个主模型共享普通工具；不再注入魂名与专属官位工具块。 | 多魂和私有取证循环已经移除。 |

## 实际改写与新增：原文和新文

| 编号 / 部分 | 原文或原机制 | 新版实际文本 | 为何需要改 |
| --- | --- | --- | --- |
| P01 主身份 | 宿主身份 + 每魂 persona | You are trisoul_x. | DSH 插件的主执行者名称；行为主体仍来自三官原文。 |
| P02 原生工具协议 | submit_draft 是唯一交付渠道；直调普通工具会丢弃；动作填执行栏。首版另加：Use native tool calls directly. There are no draft submissions, votes, private lookup tools or execution fields. Working notes are optional; use note when useful, without a required envelope. | 整段删除，不再注入主提示词。 | 旧交付机制已删除，无需再向模型解释它们不存在；可用工具由实际工具定义提供。 |
| P03 用户回复 | The prose delivered to the user for this step. Empty only when the execution fields carry entries — the executed actions are themselves the output. A submission with nothing queued to execute closes the turn, and this field is then the complete answer: open with the outcome itself — the sentence the user would ask for as the TLDR — then supporting detail; say plainly what remains unverified — never close on an in-progress sentence, and if a check still needs to run, queue it in the execution fields instead; nothing load-bearing may live only in findings. | 整段删除，包括从 output 改写而来的 REPLY_GUIDE 常量及其主提示词注入。 | 2026-09-09 用户逐段核对后要求直接删除 output；撤回之前将其改写为独立回复指导的处理。 |
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
| P14 任务工具 | 原 task_map 定义与锚点、todo 完成标记、verify_link 验证关联分为三个工具。 | 移植原 todolist.mjs 的数据结构与操作；统一注册 todo_write，op 合并 excerpt/add/edit/remove/check/link/run/unlink/view/transcript。 | 按用户要求合并入口；撤回此前整表覆盖、可选 source、自由 verification 字符串的简化。 |
| P15 web_fetch | 首版把 Fetch the content… 与 Cite the URL… 合成 description。 | 恢复 DSH 的 web_fetch 工具说明与独立 `tool:web-fetch` system 段；web_search 由宿主同时注册。 | 原先的分层指导恢复，网页格式转换、检索与引用说明跟随真实宿主工具。 |
| P16 recall | 原 trisoul_recall 的两通道说明；范围固定写 global + cross-project + this project，回捞用 seqRange。 | 原句主体保留；范围改为 scoped to this session’s selected memory range；原文参数改为 {from:a,to:b}，工具名 recall。scope 参数的 everything visible to this project 改为 everything visible to this session。 | 支持用户在输入区选择的三种记忆范围，不能再承诺每个会话都有全局记忆；参数对齐新工具。 |
| P17 环境与项目指令 | 首版只有 Working directory / Platform / Today 三行。 | 恢复 DSH 环境、时间、项目指令、技能目录及运行时上下文层。 | 原型缺少的通用能力由宿主提供；当前目录、日期和项目指令根据会话生成。 |
| P18 用户语言 | 原软路线 output 纪律：respond in the user's language | Respond in the user's language. | 从被删除的交稿纪律迁到主提示词。 |
| P19 文件工具指导的摆放 | 原宿主 tool:read / tool:write / tool:edit 独立 system 段；首版合入 description。 | 恢复原生工具 schema + 独立 system 段；write/edit 仅删 P20 的括号。 | 恢复 trisoul 原有分层，实际读写能力复用 DSH。 |
| P20 文件操作的宿主政策指代 | (the default fs-observation-policy requires it) | 删除这个括号，保留“先读取已有文件”“优先定点修改”等其余原句 | 该宿主政策未迁入，不能继续声称代码有此硬要求；不增加检查闸门。 |
| P21 自主执行的交互假设 | The user is not watching in real time and cannot answer questions mid-task, so asking 'Want me to…?' or 'Shall I…?' will block the work. | 原文恢复，放在 You are operating autonomously. 之后。 | 2026-09-09 用户逐段核对时明确要求保留；撤回此前基于界面实时交互能力作出的删除。 |
| P22 记忆整理作业指代 | duplication is acceptable (the curation job cleans it up), wrongful merging is not | duplication is acceptable, wrongful merging is not | 独立 curation 作业未迁入，不能承诺后续自动清理；宁重复勿误并等原规则保留。 |
| P23 工作记录恢复说明 | treat it as done and continue; don't redo or restate it. | continue from the recorded progress and carry forward unfinished work. | 压缩记录本来就包含 Not yet done，不能笼统地把整份记录都当成已完成。 |

## 因删除旧机制而不再发送的提示词

| 编号 | 原位置 / 内容 | 新版处理 | 原因 |
| --- | --- | --- | --- |
| D01 | submitDraftToolSchema / draftJsonSchema 中四格及 actions/files/edits/lookup 格式契约 | 删除整稿 schema；output 说明及其改写版按 P03 删除；findings/plan 不再作为 note 的写作要求。 | 原生工具循环直接执行动作，按需笔记无需承担旧交稿职责。 |
| D02 | submitFormatBlock、submitFirstOrder、evidenceReturnOrder、draftDiscipline | 删除格式教学、交稿直令与内层取证直令。 | 主循环直接消费原生工具结果。 |
| D03 | jsonFormatBlock / response_format / text.format 输出锁 | 删除。 | 新版本不依赖整篇回复的结构锁。 |
| D04 | 缺封皮的 mend / 代填指令 | 删除。 | 不再存在缺封皮状态。 |
| D05 | cast_ballot、state_divergence、候选卡与表决直令 | 删除。 | 没有投票、败稿和候选比较。 |
| D06 | tipsMessage：parallel timelines / never delivered | 删除。 | 不存在未执行的败者时间线。 |
| D07 | toolClassesHint、officerHint、DEDICATED_BLURBS | 删除工具阶层与官位排他说明；知识查证原则保留在博识原文。 | 所有主模型工具走同一循环。 |
| D08 | ENVELOPE_DESC.action：固定 -ing 当前动作句 | 删除独立动作栏格式要求。 | 没有独立 action/move 栏；不是改写自然回复风格。 |
| D09 | verify_link 独立入口、I4 收官弹回、I6 追问与I7旁白 | verify_link 按 P47–P50 独立；I4 两种任务提醒和 I6 文字证据复核按 P51–P54 恢复。多魂投票、独走调度与 I7 固定收官旁白未迁入。 | 按用户最新要求恢复收尾提醒及其实际触发；调度适配单主模型。 |
| D10 | TODO_NUDGE / TODO_EMPTY_NUDGE | 恢复原文软提醒，面向单主模型；空清单提醒里的 task_map 改成 todo_write。 | 保留新指令更新任务、复杂任务漏建清单的提醒；仅适配工具名和调用对象。 |
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
| todo_write 的完成标准 | 原完成时机、完成条件和取消勾选规则逐字保留；删除规则按 P42 合并。挂证据时机按 P48 归回 verify_link；编辑任务仍自动取消勾选并清除验证链接。 |

## 本轮 DSH 适配补充

宿主基准：DSH `0.1.5-alpha.1`，提交 `5dda764ed3aa172535a7967b06ff95d9cbfe536a`。插件使用该版本的 standard Agent preset 为底稿。以下是与独立原型相比的恢复与适配，不再往主提示词中添加“某机制不存在”的解释。

| 编号 / 部分 | 适配前 | 现在 | 原因 |
| --- | --- | --- | --- |
| P24 身份层 | 自己拼接一份 system 字符串 | `trisoul-x:persona`：身份 + 三官融合原文 + 用户语言；由 DSH 注册和排序 | 恢复原分层；output 改写版 REPLY_GUIDE 按 P03 删除。 |
| P25 通用工具层 | 8 个自写工具，部分说明合并 | DSH 原生通用工具 + note/recall；原任务、勾选、验证操作合成一个 todo_write | 保留通用执行能力与原任务结构。 |
| P26 工具层中的失效括号 | DSH 默认 write/edit 仍提到已禁用的 fs-observation-policy | 组装时仅删除 ` (the default fs-observation-policy requires it)`，其余原生段落保持 | 延续 P20，避免向模型陈述不存在的强制行为。 |
| P27 按需子代理 | 独立原型未实现 | 恢复 DSH 的 subagent / subagent_fork / 查询、等待、发送与停止工具及原生说明 | 用户明确选择“保留，按需调用”；无固定三模型编队。 |
| P28 技能与项目上下文 | 独立原型未加载 | DSH 的 agent-instructions、skill-filesystem、tool-skill 按会话加载 | 原 trisoul 通用能力的一部分，不能以简化为由漏掉。 |
| P29 动态记忆头 | 固定写 global / cross-project / project | 全量时保留原层级；项目或会话模式显示已选范围和实际所属标识，其余记忆头原句保持 | 与输入区选择保持一致。 |
| P30 后台输入 | 读取自写 JSONL 的消息 | 读取 DSH 事件原文：seq、事件类型、工具名称与参数、结果、约束、状态、记忆和待复审区间 | 调整数据来源；MEMORY_CONSTITUTION / STATE_RULES / SURGEON_SYSTEM 不改。 |
| P31 检查点与回捞 | 自己重建发送历史 | DSH V3 surface 原位替换；记录头保留 seq 回捞信息、已完成/未完成语义 | DSH 持久化和轨迹能够直接理解整理事件。 |
| P32 计划模式 | 独立原型无模式切换 | 使用 DSH preset 中的 plan-mode 原文，仅在用户进入计划模式时生效 | 复用宿主已有选择能力，不增加默认执行闸门。 |

| 编号 / 部分 | 原文 / 旧机制 | 新文 / 处理 | 原因 |
| --- | --- | --- | --- |
| P33 需求锚点 | excerpt / anchor 的 from/to、msg、摘录 ID，以及独立 transcript/view | 恢复原摘录表与任务锚点、引用定位、歧义消解和 transcript/view；新任务必须带 anchor。 | 用户指出可选 source 削弱了需求溯源，要求尽量保持原版；原引用语义完整迁回。 |
| P34 验证时机 | When to link: link the evidence the moment a task is done — not in one sweep after everything is built. | 恢复逐项完成时关联证据的要求，后按 P43 与勾选时机合成一句。 | 验证链接操作已恢复；2026-09-09 用户要求进一步融合重复内容。 |
| P35 验证纪律 | How a test earns its place…、What does not count…、Evidence ranks… | 原三段逐字恢复，包括 Link the highest rung 和 reason must say why。 | test/text 链接及 reason 字段已恢复，不再需要改成泛化结果备注。 |
| P36 验证工具协议 | verify_link 的 link/run/unlink/view 与真实测试执行 | 原验证要求和操作合入 todo_write；重复介绍、时机和 view 按 P41/P43/P45 融合。测试链接仍按 cmd 或文件扩展名真实执行，保留 PASS/FAIL/TIMEOUT，停止不覆盖旧结果。 | 用户要求实证官工具一起合体；撤回只记录自由文本结果的实现。 |
| P37 系统状态变更前的博识补充 | — the failure you remember is a hypothesis about this one, not a diagnosis; confirm the current state by looking at it before you act on the resemblance. | 删除这段扩写，前句恢复为 A signal that pattern-matches to a known failure may have a different cause. | 补充来自原 trisoul 博识官，不是新增文本；2026-09-09 按用户逐段核对意见删除。 |
| P38 任务入口必要适配 | To check tasks off, use the todo tool；独立 todo 的 updates/remove 参数 | 用 op:check updates completion. 说明完成态入口；原括号按 P44 融合，删除参数按 P46 统一为 op:remove + ids。 | 原来靠工具名区分完成态操作，现在用同一工具的 op 区分；重复入口随后按用户要求合并。 |
| P39 冲突参数合并 | task_map.tasks 是任务对象数组；verify_link.tasks 是运行目标 ID 数组。remove/unlink 都使用 ids。 | tasks 按 op 接受原任务对象或任务 ID；说明直接连接两份原文。ids 描述同时说明 remove 的任务 ID 与 unlink 的证据 ID。 | 保持原参数名和请求形态，用 op 区分语义。 |
| P40 任务快照 | 原 [todo list] 注入及换代、稳定 E/T/L ID、两态勾选 | 恢复原数据与注入；适配 DSH V3 的 snapshotEvents 与消息构造，原生清单读取同一 todo/write 事件。 | 保留跨轮任务状态和原文/证据关系。 |

## 2026-09-09 用户确认的任务工具去重

以下改动只融合三个工具的重合部分；原文摘录、锚点、完成条件、测试要求、无效证据判定和证据等级继续保留。实际正文和 schema 集中定义在 src/tasks.mjs。

| 编号 / 部分 | 原文 / 之前的拼接版 | 新版实际文本 / 处理 | 原因 |
| --- | --- | --- | --- |
| P41 工具介绍 | Creates and edits the todo list anchored to the user's own wording — a clear, complete todo list greatly raises the completion rate in medium-to-large tasks.<br>The todo list's completion marker.<br>The tasks' verification-link tool, used to raise the real completion rate of the todo list. | 保留第一句作为统一开头，删除后两句重复的独立工具介绍。A task counts as verified only through what is linked here. 原句移到完成与证据段末。 | 单个工具只介绍一次；三个步骤、批量需求、新指令和简单任务豁免等使用条件原文保留。 |
| P42 编辑与删除 | Keep the list honest as work progresses: remove tasks that are no longer relevant or turn out impossible from the list entirely (op:remove), and rewrite a task overtaken by newer instructions into what can actually be done (op:edit) — say why in your draft.<br>Remove a task (remove:[ids]) only when it no longer belongs on the list — irrelevant, impossible, or overtaken by newer instructions — never because it is hard; say why in your draft. | Keep the list honest as work progresses: rewrite a task overtaken by newer instructions into what can actually be done (op:edit), and remove a task from the list entirely (op:remove) only when it no longer belongs on the list — irrelevant, impossible, or overtaken by newer instructions — never because it is hard; say why in your reply. | 将两处删除规则与重复的解释要求合成一处；保留改写任务、彻底移除、全部删除条件、不能因为困难删除及说明原因。 |
| P43 完成与挂证据时机 | Decide how a task will be verified before building it, and check it off the moment it is fully done — one at a time, as you go, not all of them at the end.<br>When to link: link the evidence the moment a task is done — not in one sweep after everything is built. | Decide how a task will be verified before building it, and link the evidence and check it off the moment it is fully done — one at a time, as you go, not all of them at the end. | 在原完成时机句中加入 link the evidence，合并“逐项做、不要最后统一做”的重复要求；前置规划验证、失败/未完成/未解决错误不可勾选、批量逐项成立与取消勾选原句保留。 |
| P44 完成入口与编辑失效 | (To check tasks off, use op:check; editing a task itself clears its checkmark and verification links.)<br>op:check updates completion. | 编辑段保留 Editing a task itself clears its checkmark and verification links.；完成段保留 op:check updates completion. | check 入口只介绍一次；将编辑失效规则放回任务编辑段，行为不变。 |
| P45 查看结果 | op:view returns the full todo list including excerpts.<br>op:view returns every task with its completion state and evidence. | op:view returns the full todo list including excerpts, with every task's completion state and evidence. | 统一为一条 view 说明；实际回执将证据放在各任务下面，每个任务显示一次，同时保留摘录、锚点、勾选、证据 ID、命令、运行状态和文字证据原因。原 transcript 说明逐字保留，移到同一查看段。 |
| P46 删除入口和参数 | check 的 remove: Task ids to delete from the list entirely；op:remove 的 ids: Required for remove: ids of the tasks to delete。updates: Each entry is {"id": task id, "done": true or false} | 删除统一使用 op:remove + ids；移除 check 的 remove 参数和执行分支。updates 改为 Required for check: each entry is {"id": task id, "done": true or false}。其余摘录、锚点、证据参数保持前一版。 | 去掉模型可选择的重复删除入口；完成态只接收 updates，三份旧工具 schema/注册定义合成一份实际定义。原事件数据与稳定 ID 不变。 |

## 本轮调整：验证工具独立

用户提出“验证还是得拆出来”，因此保留 task_map 与 todo 的融合，将 verify_link 恢复为主模型可直接调用的普通工具。存储、任务与证据关联、页面和按会话串行操作继续共用。

| 编号 / 部分 | 调整前 | 新版实际文本 / 处理 | 原因 |
| --- | --- | --- | --- |
| P47 验证提示词归属 | 验证的 How a test earns its place、What does not count、Evidence ranks 和 link/run/unlink 说明放在 todo_write，独立工具介绍按 P41 删除。 | 从 todo_write 移出验证说明，verify_link 的 description 全文逐字恢复原 trisoul，包括 The tasks' verification-link tool, used to raise the real completion rate of the todo list. A task counts as verified only through what is linked here. 和完整的时机、验证纪律、证据等级、操作说明。 | 验证重新拥有独立工具，原介绍句重新适用；原全文无需再做融合改写。 |
| P48 完成时机 | Decide how a task will be verified before building it, and link the evidence and check it off the moment it is fully done — one at a time, as you go, not all of them at the end. | todo_write 恢复 Decide how a task will be verified before building it, and check it off the moment it is fully done — one at a time, as you go, not all of them at the end.；verify_link 恢复 When to link: link the evidence the moment a task is done — not in one sweep after everything is built. | 完成标记与挂证据分别属于两个工具；撤回 P43 的合句。验证判定句随 P47 归回验证工具，未删除任何完成或验证要求。 |
| P49 查看说明 | op:view returns the full todo list including excerpts, with every task's completion state and evidence. | todo_write：op:view returns the full todo list including excerpts.；verify_link：op:view returns every task with its completion state and evidence. | 恢复两个工具各自的原文查看说明和回执；任务视图显示需求与锚点，验证视图显示任务状态与证据。页面仍使用同一份记录，在任务下显示证据。 |
| P50 参数与入口 | todo_write 同时接受任务与验证 op；tasks 接受对象或 ID，ids 同时表示任务 ID 和证据 ID。 | todo_write 的 op 为 excerpt/add/edit/remove/check/view/transcript；tasks 恢复原任务对象数组与原描述，ids 恢复 Required for remove: ids of the tasks to delete。verify_link 的 op 为 link/run/unlink/view，完整参数 schema 恢复原版，包括独立的 links、运行目标 tasks 和撤销链接 ids。 | 消除同一工具中的参数复用；保留 P42/P46 的统一任务删除入口，工具共享队列以保持编辑任务时清除旧验证的联动。 |

## 本轮恢复：动态任务收尾提醒

用户审核删除项后要求“这几个加回来吧”。以下三类提示使用原版文本；动态任务条目、证据状态和 reason 引用也保留原格式。

| 编号 / 部分 | 原文 / 原行为 | 当前处理 | 原因 |
| --- | --- | --- | --- |
| P51 未完成任务 | [todo list] Unresolved tasks remain:，随后列出未完成或缺证据的任务及缺口 | 原文恢复；主模型正常收尾时，存在未勾选任务就把提示送入下一步并继续本回合。 | 恢复用户要求的未完成任务提醒，直接由同一个主模型处理。 |
| P52 勾选但证据不足 | [todo list] Every task is checked off, but these lack qualifying evidence:，随后列出缺口；末句 Link real evidence, or uncheck what is not actually done. | 原文恢复；任务均已勾选、但仍有任务缺少合格链接时触发。合格条件沿用原版：test 需实际运行通过，text 需已关联。 | 完成勾选之后仍须对照实际关联的验证证据。 |
| P53 文字证据复核 | [todo list] Tasks whose only evidence is a text record:；逐项引回 note 与 your reason no higher rung was runnable；尾句 Re-check each reason against what is actually available here. If a higher rung is runnable after all, build and link it; if not, they stay as they are. | 原文恢复；全部任务已完成并有合格证据后，对仅有文字证据且尚未问过的链接触发一次复核。 | 恢复对“为什么无法使用更强证据”的再次核对。 |
| P54 调度与复核记录 | 原来由多魂收官流程派发提醒，复核成功后将文字链接标成 asked；编辑清除、撤销重挂后重新复核。 | 接入 DSH agent/turn-stopping，通过原生 steering 继续同一回合；成功回应后静默保存已提示链接的 asked。失败、中断、未送达不算复核；复核期间新挂的链接仍待复核。计划模式和子代理不触发主任务收尾提醒。 | 保留原任务语义，适配单模型正常收尾；复核状态可跨进程恢复，保持编辑与验证的联动。 |

任务部分的主动简化已撤回，验证入口独立，三类收尾提醒已恢复。记忆整理、任务补注文档、阶段结束信号和语义检索中此前的删减仍列在 D11–D16，尚未逐段审定；它们是当前实现与原版的差异，不应把“最小化”当作用户已经认可这些删减的理由。后续按同一原则逐段核对。

## 现在的组成

| 层 | 来源与职责 |
| --- | --- |
| 主身份与行为 | 三官原文融合（含用户逐段核对的调整）+ 用户语言 |
| 通用 system 段 | DSH 使用指导与环境；原 task_map、todo 描述融合，verify_link 独立保留原文 |
| 项目与技能 | DSH 按当前目录和会话发现、加载 |
| 动态上下文 | 用户选定范围内的记忆、工作状态、用户消息、工具结果，以及恢复的三类任务收尾提醒 |
| 工具 schema | DSH 原生工具 + note/recall + todo_write + verify_link |
| 记忆与状态后台 | 原记忆宪法 + 两区规则 + save_context 工具 |
| 上下文整理后台 | 原手术刀提示词，输出普通文字工作纪要 |

## 核验

自动测试对照原文快照检查主提示词、任务工具中保留的摘录/锚点/完整完成纪律、verify_link 全文、记忆宪法、OPS_DESC 和手术刀原文；本轮提示词归属与接口见 P47–P50。使用真实 DSH 加载插件、原生工具和本地模型端点，验证自然语言输出、项目指令、技能目录、记忆注入、范围绑定、V3 整理与回捞、任务与验证同一记录及跨轮恢复。任务测试验证两个工具的独立调用、统一任务删除入口、各自的查看结果及验证运行时编辑任务的联动。原文快照仅用于对照测试，不参与运行时加载。

使用用户授权的 DeepSeek v4 flash 进行了真实文件读写、技能加载、按需子代理与后台记忆联调。联网搜索需要另行配置宿主搜索提供方；本轮未把 Ark 模型密钥当作搜索凭据。

收尾提醒恢复通过真实 DSH 的回合循环验证：本地模型端点分别在任务未完成、已勾选但无证据、仅文字证据时尝试结束；三类原文均实际进入下一次模型请求，复核成功后不重复追问。另覆盖静默持久化、重新挂接、新旧链接区分、写入失败、取消和计划模式；随后执行真实测试命令并验证跨轮证据恢复。
