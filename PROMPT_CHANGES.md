# 提示词改动对照

基准：原 trisoul 提交 `4189f90305e545dbb82654f561fc4d83eed96d03`。原文快照在 [prompt-origin.json](test/fixtures/prompt-origin.json)，新版实际文本在 [prompts.mjs](src/prompts.mjs)、[dsh-agent.mjs](src/dsh-agent.mjs)、[hub.mjs](src/hub.mjs)、[tasks.mjs](src/tasks.mjs)、[todolist.mjs](src/todolist.mjs) 和 [canvas.mjs](src/canvas.mjs)。表中“删除”指不迁入新版，不修改原项目。

原则：尽量保持原版，保留原句与通用能力；只融合职责并调整架构不兼容的协议，以及用户逐段审定的改动。模型提示词只描述现有能力，不补写“没有某机制”的解释；note 用于按需记录，不承接旧交稿中的思维摘要要求。不以“精简”为由重写仍然适用的行为规则。

当前任务结构：todo_write 负责需求、任务和完成状态，verify_link 独立负责验证；二者共用任务数据和页面。下表保留迁移与审核过程，涉及验证合入 todo_write 的旧处理由本轮 P47–P50 覆盖。

用户随后要求恢复三类动态收尾提醒，当前已按 P51–P54 接回单模型回合结束流程。2026-09-10 又要求恢复全部与三魂无关的机制，并重构相关设置；最终处理见 P59–P70，覆盖此前的融合与删减。用户明确审定的 output、findings/plan/action、故障类比补充删改仍有效，命令安全门仍沿用先前排除要求。

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
| P07 后台身份融合 | You are the canvas scribe, maintaining the two zones of the current session's working context…；完整两区定义、Discipline 见 STATE_SYSTEM 原文快照。 | 原 STATE_SYSTEM 全文逐字恢复；状态作业独立使用 save_state，记忆消化仅使用记忆宪法与 save_context。 | 撤回首版 In this same job, also maintain 的融合；恢复原版独立状态提炼、游标、失败冷却和快照换代。 |
| P08 后台交付协议 | Output JSON (JSON only) + 正文 JSON 骨架 | Submit the result using save_context. | 取消整篇 JSON 格式锁和正文解析；正常工具调用的参数仍结构化。 |
| P09 后台工具入口 | 旧版通过正文 JSON 提交结果。 | Save this batch’s digest, task memo rewrite, and memory changes. | save_context 恢复任务记忆文档改写，pin/status 移回独立 save_state；只描述实际保存内容。 |
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
| P22 记忆整理作业指代 | duplication is acceptable (the curation job cleans it up), wrongful merging is not | 按用户审核意见，括号原文已恢复，OPS_DESC 全文重新与原版一致。 | 对应整理作业已接回，见 P55–P58。 |
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
| D09 | verify_link 独立入口、I4 收官弹回、I6 追问与 I7 旁白 | verify_link、两类收尾提醒和文字证据复核已恢复；I7 的完成/测试型/文字型统计恢复到用户面板。 | 单模型通过宿主 steering 继续执行，收尾统计直接给用户看；多魂独走与投票调度移除。 |
| D10 | TODO_NUDGE / TODO_EMPTY_NUDGE | 恢复原文软提醒，面向单主模型；空清单提醒里的 task_map 改成 todo_write。 | 保留新指令更新任务、复杂任务漏建清单的提醒；仅适配工具名和调用对象。 |
| D11 | CURATE_RULES / CURATE_FORMAT_TAIL 与 signals.overlap/conflict | 规则和信号原文恢复；原生 memory_curate 提交。分片轮巡、游标、晋升候选和使用信号已接回，见 P58/P66。 | 恢复原能力，适配单模型、DSH V3 与所选记忆范围。 |
| D12 | DIGEST_DESC.workdoc | 全文逐字恢复到 save_context.workdoc；同批消化代谢任务记忆文档。 | 撤回将任务知识文档与进度 status 视作重复的删减。 |
| D13 | DIGEST_DESC.phaseClosed | 全文逐字恢复；阶段结束放行此前批次及未覆盖的过程区间。 | 恢复原版整段阶段放行能力。 |
| D14 | 记忆 LLM picker | 语义挑选恢复，select_memories 原生工具承接 indexes；调用失败才用原本的本地词匹配。 | 正文 JSON 协议改为原生工具，其余检索行为恢复。 |
| D15 | ASK_SYSTEM / ASK_FORMAT / ANSWER_SYSTEM | 出题、作答、判分与补记全部恢复；出题使用 record_probe，作答输出普通文本。 | 压缩事实探针属于信息保留机制，恢复原文与原判分；不恢复正文 JSON 锁。 |
| D16 | 任务补注 renew/rewrite/append 与独立工作文档 | 三种方式、原版消息头、版本节流、开场/首次任务/主动/压缩重注均恢复。 | 撤回首版过度简化；DSH V3 替换接口与跨进程记录按新宿主适配。 |
| D17 | fs-observation-policy 的强制先读提示与宿主权限说明 | 禁用 fs-observation-policy，并仅从 tool:write / tool:edit 删除该政策括号。DSH 的运行环境与权限上下文按实际配置生成，默认完整访问、审批 never。 | 插件不新增产品闸门；宿主原生工具与设置保持一致。 |

## 保持原文的后台与工具部分

| 部分 | 处理 |
| --- | --- |
| 记忆 CONSTITUTION | 全文逐字保留（稳定事实、原因、日期、少记、避免过程信息）。 |
| OPS_DESC 全部 6 个字段 | 包括 P22 的整理说明在内，全文逐字保留；工具参数仍是 add/update/retire、scope、key、text、target。 |
| DIGEST_DESC.digest / compactable / nowCompactable | 逐字保留。nowCompactable 的 id 从旧字符串改为本地事件整数序号，说明语义不变。 |
| STATE_SYSTEM 全文 | 已逐字恢复，状态提炼与记忆消化独立。 |
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

## 本轮恢复：记忆整理原句与触发信号

用户要求“上一句和这一部分你都去恢复了”，指 P22 括号和 signals.overlap/conflict。它们已连同对应整理作业恢复。

| 编号 / 部分 | 原文 / 旧机制 | 当前处理 | 原因 |
| --- | --- | --- | --- |
| P55 重复记忆的后续处理 | duplication is acceptable (the curation job cleans it up), wrongful merging is not | 整句逐字恢复，OPS_DESC 六个字段均与原版一致。 | 整理作业已实际接回。 |
| P56 整理信号 | overlap: the existing memories contain duplicate / near-duplicate entries about the same matter (informs the curation job); false when uncertain；conflict: existing memories contradict each other or the new events (informs the curation job); false when uncertain | 两条描述逐字恢复到 save_context.signals 的布尔字段中；signals 及两字段必填。 | 消化作业报告重复和冲突，原生工具参数承接结构化结果。 |
| P57 整理规则与提交协议 | CURATE_RULES 整段；CURATE_FORMAT_TAIL 的 Output JSON only 和字段说明 | CURATE_RULES 整段逐字恢复；字段说明进入 memory_curate 原生工具参数。增加工具说明 Save the memory curation operations. 和提交指引 Submit the result using memory_curate.。记忆宪法原文保留。 | 保留去重、合并、退役、范围调整、使用信号参考和语言要求，按 x 架构提交后台操作。 |
| P58 调度、输入与记录 | 修改/退役已有记忆或 overlap/conflict 触发项目分片整理；空闲轮巡、分片游标、跨项目晋升候选与使用信号。 | 全部恢复：global / cross / project 分片、默认 180000ms 最短间隔、空闲与可选批次触发、片内窗口和持久游标、跨项目相同 key / 近似文本候选。项目分片公共摘要只读，会话私有层不参与长期整理；记录来源、版本与用量。 | 适配新存储与会话选定范围；取代上一版将可见层级整批混合处理的简化实现。 |

任务部分的主动简化已撤回，验证入口独立，三类收尾提醒已恢复。P58 与 D12–D16 的剩余通用机制均已接回；以下记录本次恢复所需的提示词接口变化。

## 本轮全面恢复：非三魂机制与设置

| 编号 / 部分 | 原文 / 原行为 | 新版实际文本 / 处理 | 原因 |
| --- | --- | --- | --- |
| P59 独立状态作业 | STATE_SYSTEM 与 STATE_FORMAT 的 pin/status、代谢和原文引用规则。 | STATE_SYSTEM 全文原样使用；STATE_RULES 逐字保留 JSON 外壳以外内容；新增 Save the new pinned truths and the complete status snapshot. / Submit the result using save_state. | 恢复独立状态流程，仅把正文 JSON 提交改为原生工具。记忆和其他插件注入不进入状态提炼输入。 |
| P60 任务记忆文档 | only when a "current task memo document" was given above: the full rewritten document, in English — its metabolic rewrite: fold the entries under "— new items (to fold in) —" into the body, merge same-topic knowledge into one place, fix what new events overturned, delete what's obsolete or no longer relevant; fine details need not be copied in full (the store has them all, retrievable at any time). Organize only, never invent: add no fact the document doesn't contain. Empty string if no document was given or no change is needed. | 描述逐字恢复到 workdoc 参数；输入标题仅将 rewrite rules under workdoc in the output format 改为 rewrite rules under workdoc in the tool parameters。 | 同一次消化整理文档，规则文本不改；“字段说明所在位置”随原生工具适配。异步返回时只改写它实际看过的版本。 |
| P61 阶段结束 | whether this batch marks the close of a phase (a chunk of work finished / a conclusion reached / a deliverable written and verified) — if so, all earlier process text can be condensed wholesale; when unsure fill false | 描述逐字恢复到 phaseClosed；与 compactable / nowCompactable 一起供画布选择区间。 | 恢复阶段收尾整段放行，同时保持最新状态、任务和默认用户原话的保留规则。 |
| P62 语义检索 | You are the memory retriever. Given the question (and the session's current state background), pick the relevant memory entries from the numbered candidates — better too few than too many. 后接正文 JSON 格式要求。 | 前句逐字恢复；末句改为 Submit the selected indexes using select_memories.；工具说明 Select the relevant memory entries.，indexes 说明 Indexes of relevant entries; an empty array if nothing is relevant. | 恢复语义选择与状态背景，结果通过工具编号提交。首次任务、主动补注与 recall 共用挑选器，故障时词匹配回退。 |
| P63 补注消息 | 原 SUPPLEMENT_HEAD、SUPPLEMENT_HEAD_RENEW、— new items (to fold in) —。 | 三项逐字恢复；开场头的层级按会话选择呈现。renew 按版本追加、rewrite 原位替换、append 逐批追加，默认开场不限、每批 8 条、最多 4 次、首次等待 8 秒、版本至少间隔 10 步。 | 文本沿用原版；分层范围、V3 startSeq/endSeq 和持久状态适配。范围入口仍在输入区左侧。 |
| P64 探针出题与作答 | 原 ASK_SYSTEM、ANSWER_SYSTEM；原 ASK_FORMAT 的 question/expected 和作答 JSON 外壳。 | 两份 system 原文逐字恢复；出题新增 Submit the question and reference answer using record_probe. 与 Record one factual question and its reference answer.；question 为 The question，expected 为 The reference answer (as short as possible)。作答按原 ANSWER_SYSTEM 输出答案或 UNKNOWN。 | 取消正文 JSON 外壳，保留一题事实探针、参考答案和仅依据纪要作答的纪律。原归一化与数字边界判分原样迁移。 |
| P65 压缩底稿与补记 | Pre-condensed draft from the memory hub (usable as a base; the original text above remains authoritative):；Addendum (verified facts earlier records missed; preserve each verbatim in the record):；[Addendum · key facts from the condensed span] | 三段原文恢复。底稿作为参考和过长输出的备选；探针遗漏默认随下一次压缩写入，qa/material 可立即补入。按原 judgeProbe 核对事实真正保留后才清除待补条目。 | 原文与补记语义不改。立即补记改为完整 V3 compaction 生命周期；待补内容保存到会话状态，重启可续。 |
| P66 分片整理输入 | 原 Current shard、Editable entries、公共只读摘要、Promotion candidates 与年龄/来源/版本/使用信息。 | 分片和候选说明沿用原句；使用字段映射为 x 的 at / previous / usage；项目级会话不读取范围外公共摘要，会话私有记忆不参与晋升。 | 恢复全局、跨项目、项目轮巡，接口与范围选择相匹配。 |
| P67 快照与压缩机制 | 最新状态/任务保留，旧快照不进入纪要原料；最新任务记忆文档可压缩；旧检查点合并；可选旧用户消息退役、过期快照清理；状态与压缩失败冷却。 | 行为恢复。V3 禁止 assistant/message 携带替换来源，因此过期快照使用空 system/message 替换：宿主从请求中省略它，带有效回合/步骤并可重载；不新增任何模型可见文案。 | 必要的 DSH V3 适配。强制手动整理也跳过仅有插件提示或单个旧纪要的空刀。 |
| P68 收尾统计 | I7 用户可见：完成数、测试型任务数、仅文字型任务数。 | 相同统计进入任务面板的“最近收尾”，不注入模型消息。 | 原多魂旁白渠道拆除，单模型版本通过自己的 UI 展示结果。 |
| P69 设置与用量 | 原统一/分别模型设置、off 能力判断、压缩频率、记忆范围、后台批次和上限。 | 后台可统一或按记忆/状态与探针/压缩分别配置。恢复 3/10000/6、6/20000/10、10/40000/15 三档及自定义；补注、轮巡、状态、探针、可选长度与超时均实际接线并热更新。主执行模型和普通工具设置由 DSH 管理。 | 保留通用参数，去掉魂数、投票、交稿、格式锁等设置。后台超时仍能退出无视 AbortSignal 的挂起流。 |
| P70 监控与记忆页面 | 原组件轨迹、上下文演变、输入区统计、记忆使用/版本/健康信息。 | 改为主执行、按需子代理、消化、检索、状态、整理、压缩和探针；补回轨迹、上下文历史与缓存显示、任务记忆文档、待补事实、收尾统计、分片状态和版本永久删除。 | 重构为单模型 UI；原有编辑/退役/恢复/搜索/范围选择保留。历史上下文图从本次升级后的请求开始积累，不伪造过去的帧。 |

## 现在的组成

| 层 | 来源与职责 |
| --- | --- |
| 主身份与行为 | 三官原文融合（含用户逐段核对的调整）+ 用户语言 |
| 通用 system 段 | DSH 使用指导与环境；原 task_map、todo 描述融合，verify_link 独立保留原文 |
| 项目与技能 | DSH 按当前目录和会话发现、加载 |
| 动态上下文 | 开场记忆、任务记忆文档、独立工作状态、任务快照、用户消息和工具结果，以及三类收尾提醒 |
| 工具 schema | DSH 原生工具 + note/recall + todo_write + verify_link |
| 记忆消化后台 | 原记忆宪法 + save_context（含 workdoc、phaseClosed、signals） |
| 状态提炼后台 | 原 STATE_SYSTEM + STATE_RULES + save_state |
| 记忆检索后台 | 原 PICK_SYSTEM 主体 + select_memories；状态背景与编号候选 |
| 记忆整理后台 | 原记忆宪法 + CURATE_RULES 全文 + memory_curate 工具；信号、实际变更、空闲轮巡及可选批次触发 |
| 上下文整理后台 | 原手术刀提示词、预压缩底稿与待补记，输出普通文字工作纪要 |
| 压缩探针后台 | 原出题与作答 system，record_probe + 普通答案；判分与补记 |

## 核验

自动测试对照原文快照检查主提示词、任务工具中保留的摘录/锚点/完整完成纪律、verify_link 全文、记忆宪法、OPS_DESC 和手术刀原文；本轮提示词归属与接口见 P47–P50。使用真实 DSH 加载插件、原生工具和本地模型端点，验证自然语言输出、项目指令、技能目录、记忆注入、范围绑定、V3 整理与回捞、任务与验证同一记录及跨轮恢复。任务测试验证两个工具的独立调用、统一任务删除入口、各自的查看结果及验证运行时编辑任务的联动。原文快照仅用于对照测试，不参与运行时加载。

使用用户授权的 DeepSeek v4 flash 进行了真实文件读写、技能加载、按需子代理与后台记忆联调。联网搜索需要另行配置宿主搜索提供方；本轮未把 Ark 模型密钥当作搜索凭据。

收尾提醒恢复通过真实 DSH 的回合循环验证：本地模型端点分别在任务未完成、已勾选但无证据、仅文字证据时尝试结束；三类原文均实际进入下一次模型请求，复核成功后不重复追问。另覆盖静默持久化、重新挂接、新旧链接区分、写入失败、取消和计划模式；随后执行真实测试命令并验证跨轮证据恢复。

记忆整理恢复通过真实 DSH 与本地模型端点联调：save_context 返回 overlap 后自动请求 memory_curate，并实际退役预置重复记忆。针对性测试覆盖原文一致性、合并与历史、false/conflict 信号、实际更新触发、记忆范围、最短间隔、并发用户编辑、用户来源继承、层级移动、失败及取消。

上一轮全面恢复验证：41 项自动测试全部通过，包含真实 DSH profile 与本地确定性模型端点；实际覆盖设置热更新、独立状态、语义检索、任务记忆文档换代、V3 旧快照清理、压缩探针、原文回捞、任务收尾统计和记忆管理 API。针对性测试另覆盖跨项目晋升/私有范围排除、分片游标续转、空闲尾批、状态降级、挂起流超时、底稿替代、探针事实消账，以及手动 QA 补记不嵌套维护操作。3083 的设置、任务上下文、记忆、监控与输入区统计已通过页面验证，前端无错误日志。测试使用隔离目录；原 trisoul 未改动。

## 本轮收尾核对与蓝色界面重构

本轮未改动模型提示词原文或工具参数描述。UI 文案与排布的调整仅作用于页面；以下补齐此前遗漏的原版运行行为。

| 编号 / 部分 | 原版行为 | 上轮差异 | 本轮处理与理由 |
| --- | --- | --- | --- |
| P71 消化调度 | 每会话攒批，跨会话共用串行消化队列；满批自动继续，显著用户事件立即触发。 | x 按会话分别启动后台作业，只在步骤/回合结束触发一部分批次。 | 恢复全局串行队列与事件驱动；一个作业结束后自动消费已满的后续批次。其余后台作业保持各自职责。 |
| P72 失败重试与续传 | 第一次失败退回队头；再次失败后从本进程待办中移出，失败当时不推进持久游标；重启可从游标补消化。 | x 下次触发会重新扫描同一失败区间，缺少原版的有限重试规则。 | 恢复队列重试计数和独立扫描位置；新会话从当前位置开始，已知会话按 catchupMax 续传。已审定的原生工具提交方式保持。 |
| P73 空闲尾批 | 一个全局空闲时钟，先冲刷所有会话尾批，再轮巡待整理分片。 | x 分别为会话设置空闲时钟。 | 恢复全局空闲调度，避免仍有会话活动时另一会话提前进入空闲整理；短尾批也能自动处理。 |
| P74 项目归属 | projectKeyOf / sameProject：git 根识别、旧子目录记忆归入仓库、路径边界判断、反向父目录不泄入子仓库。 | x 主要使用精确字符串匹配。 | 原 project.mjs 直接迁入；读取、同名更新、分片与跨项目候选使用同一匹配规则。会话私有标识继续按 ID 匹配。 |
| P75 检索与补注去重 | 中日韩连续文字采用相邻二字组，纯标点忽略；补注对当前文档和待注队列均按指纹判重。 | x 漏掉 Hangul，并且只检查已进入文档的指纹。 | 补齐韩文与待注队列判重；原 PICK_SYSTEM 与原生检索参数不变。 |
| P76 记忆写入 | 旧 ID 沿覆盖链找到当前版本；自动更新可升层、不降层；用户所有记忆不能由自动作业退役；相同文本不重复添加。 | x 的部分规则仅作用于整理作业，显式跨层更新可能留下两个有效版本。 | 恢复到所有自动记忆写入；继续使用 x 的版本 ID、原生工具参数和独立存储。旧项目绑定沿版本链保留，整理作业仍不能覆盖其读取后发生的用户编辑。 |
| P77 页面呈现 | 原版具有设置、监控、工作上下文和记忆管理。 | 上轮界面平铺大量参数和组件读数。 | 按用户要求统一蓝色视觉；设置分常用/高级，监控分概览/调用/上下文，工作上下文分任务/状态/记忆文档，记忆页以搜索与条目为主。版本、证据、底层参数均可展开；输入区统计和标识同步调整。 |

验证：49 项测试通过，包含真实 DSH profile 与本地模型端点联调。六类差异均先通过测试复现，再修复；另验证新会话/续传边界与多会话空闲尾批。浏览器检查了设置、监控、任务与证据、记忆列表及编辑页，实际保存并恢复了测试设置值，检查浅色与深色背景；用户原主题偏好已恢复。

## 2026-09-10 频率档位调整

| 编号 / 部分 | 调整前 | 调整后 | 原因 |
| --- | --- | --- | --- |
| P78 三档触发间隔（运行配置，提示词不变） | 状态阈值 6 / 10 / 15 条事件，压缩间隔 3 / 6 / 10 步，最小区间 10k / 20k / 40k Token；消化默认 8 条，记忆文档默认 10 步，与档位独立。 | 状态 30 / 60 / 90 条、压缩 10 / 20 / 30 步、区间 20k / 40k / 80k；档位联动消化 16 / 32 / 48 条、文档 20 / 30 / 45 步；默认适中。 | 用户检查实际会话后要求三个档位整体上调。减少状态、记忆文档和后台整理的更新频率；页面明确区分事件与模型步骤，前后端使用同一份档位定义。 |

验证：49 项测试通过，前端构建通过；浏览器切换三个档位后恢复中档，确认无未保存改动。当前实例通过设置 API 热更新至中档，运行中的会话继续执行；任务重发规则和旧快照清理设置保持不变。

## 2026-09-10 DSH 0.1.5-rc.1 适配

| 编号 / 部分 | 调整前 | 调整后 | 原因 |
| --- | --- | --- | --- |
| P79 原生文件交付工具 | trisoul_x 的 Agent preset 没有 present。 | 跟随新版 standard preset 加入 @deepseek-ai/dsh-tool-present，工具参数和说明由宿主原样提供，见下方原文。 | 用户要求升级 3083 的 DSH；接入候选版文件交付与右侧栏预览能力，不新增自定义协议。 |
| P80 宿主与官方模型目录 | DSH 相关直接依赖固定 0.1.5-alpha.1；当前官方模型列表有用户添加的临时型号。 | DSH、compaction、llm、session 同步固定 0.1.5-rc.1；当前目录补入新版默认模型 deepseek-flash，采用上游的文字/图片、100 万上下文和 systemPromptUpdate: in-history 配置。已有模型条目、密钥和模型选择保持。 | 自定义 models 数组会覆盖宿主默认目录，因此需要补入新条目才能在本实例的模型选择器中看到它；模型能力声明来自上游配置。主提示词和已有工具说明未手改。 |

P79 新增工具说明原文：

> Declare existing files accessible through the Session filesystem as final deliverables. When a file you create or update is an output the user asked to receive, you must call present after writing it and before your final response, including files created through Bash or code execution. Mentioning its path in your reply does not replace this call. The files must already exist. The user opens the current source files; their contents are not copied or preserved.

验证：rc.1 下 49 项测试通过；真实 DSH 与本地模型端点完成 present 调用并收到 Presented fixture.txt。3083 重启后核对已有会话任务、记忆条目、配置和密钥；浏览器验证长会话恢复、模型目录、右侧监控与统计，页面无错误日志。重启前已确认会话全部空闲，升级前配置和依赖清单保存在本项目忽略的 data/backups/ 下。

## 2026-09-10 Windows 插件安装适配

| 编号 / 部分 | 调整前 | 调整后 | 原因 |
| --- | --- | --- | --- |
| P81 验证工具参数说明 | `without cmd the file is run bare by its extension (node / pytest / bash)}` | `without cmd the file is run bare by its extension (node / pytest / bash / pwsh)}. Custom cmd runs in PowerShell on Windows and bash elsewhere.` | 用户要求 Windows 可用；新增 .ps1 运行器，自定义命令在 Windows 使用 PowerShell，与宿主原生 Shell 保持一致。原验证纪律全文保留。 |
| P82 未知验证文件的运行提示 | `or link a .js/.mjs/.cjs/.py/.sh file.` | `or link a .js/.mjs/.cjs/.py/.sh/.ps1 file.` | 将已支持的 PowerShell 文件加入回执提示。主系统提示词不变。 |

## 2026-09-10 BT 收尾提醒开关

| 编号 / 部分 | 调整前 | 调整后 | 原因 |
| --- | --- | --- | --- |
| P83 待办提醒的任务行 | `[todo list] Unresolved tasks remain:`，列出未完成或缺证据任务，并附证据缺口。 | 标题原文保留；验证提醒关闭时，只列未完成项的 ID、勾选状态与标题，省去证据缺口。两个提醒都开启时保持原文。 | 用户要求待办与验证提醒可独立开关；关闭验证后，待办提醒不应继续要求补验证。 |
| P84 单独开启验证时的标题 | `Every task is checked off, but these lack qualifying evidence:` | 全部已勾选时原文保留；尚有未勾选任务时用 `These tasks lack qualifying evidence:`，正文只列缺少合格证据的任务，结尾原文保留。 | 单独开启验证提醒时，也可能存在未勾选任务；避免错误声称全部已完成。 |
| P85 三类收尾提醒的触发 | 默认全部开启。 | BT 按会话保存选择：待办默认开启；缺证据提醒和文字证据复核共同受验证开关控制，默认关闭。 | 用户明确指定默认值和模型旁的 BT 入口；主系统提示词、工具说明、任务与证据记录均不改。 |
