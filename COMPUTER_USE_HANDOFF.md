# Computer Use 当前交接

2026-09-12。已恢复开发；本文记录当前可用入口、验证证据和未完成项，不是功能已完成的声明。

## 最新：全前端融合改版（2026-09-12）

- 用户明确授权全部前端调整，采用 DSH 与 Codex 融合风格。保留宿主会话／项目／模型／文件功能和蓝色强调，统一中性色、字体、细边框、留白及深浅色；本次不追求整套像素复刻。
- 输入区下方合并为一条工具栏：工作台、电脑、分享／预览图标和统计。记忆范围与 BT 仍在输入区；宿主统计弹窗保留，消息过程记录改为轻量折叠样式，应用／网页引用使用一致的消息气泡。
- 新 `trisoul-x-workbench` 集中任务、记忆、电脑、监控，切换复用同一个宿主标签并保留未保存编辑。旧三个 X 面板和 Computer Use 标签继续兼容，新增入口只推荐工作台。设置、批注、窗口分享及堆叠／独立预览均已统一；独立预览跟随宿主主题变化。
- 最终验证日志在 `/tmp/trisoul-ui-redesign/`：`frontend-fifth.log` 通过工作台复用、编辑保留、设置保存／撤销、深浅色及窄窗；`computer-ui-first.log` 两浏览器全流程通过，`computer-ui-final.log` 内置浏览器复核并增加独立预览主题切换断言；`native-ui-current.log` 原生双窗口／光标／停止／堆叠通过；`unit-verified.log` 构建与针对性回归通过。截图已看图，持久副本在 `cu-artifacts/frontend-2026-09-12/`。
- 保留调试记录：新前端测试最初未等待 DSH 窄窗自动收栏，现按实际布局状态等待；截图库在有限主题过渡结束后取图。带空格路径构建夹具漏拷已有 `annotation-geometry.mjs`，补齐依赖，未改断言。旧测试原生 socket 的进程构建过期，会话轨迹确认被版本校验拒绝；改用 `/tmp/trisoul-ui-redesign/native-current.sock` 独立当前服务后通过，没有绕过版本检查或修改用户主服务。
- 当前 3083 原生版本匹配，两项权限保留，无需更新／重启。前端构建已写入本机正在使用的插件，刷新页面加载新界面；此次未改后端执行、任务、记忆或提示词。未清理／回退／提交／推送。能力基线中的 Windows、锁屏、复杂 CSS 命中与其他未验收项仍保留。

## 最新：对话页内堆叠预览（2026-09-12）

- 用户确认的形态：直接悬浮在对话页面里，像视频画中画；多个目标堆叠，点击进入对应窗口。`FloatingPreview` 已从侧栏移至会话输入区常驻组件，选择目标后自动出现；浮层定位在输入区上方，监听输入区和侧栏尺寸变化，避免遮挡发消息。关闭后可从“悬浮预览”重开，原独立 PiP 仅保留为右上角可选“弹出预览”。
- 同会话记录已选目标及原生窗口身份，独立只读观察流允许旧目标继续更新；目标关闭时移除卡片。可展开多张卡片。浮层观察 actor 不允许发送输入或修改样式，跨会话目标拒绝，关闭不关闭目标／任务；两种浏览器原控制流程保留。
- 点击原生卡片通过原生 `show_window` 核对进程身份和准确窗口，提升该窗口并激活应用；不会重新打开或按同名猜窗口。内置网页打开 TrisoulX 对应浏览器面板；扩展定位并显示原标签。原生点击与浏览器进入仍分别沿用各自控制生命周期。
- 新后端验证 `stack-backend-final.log` 4/4（系统临时目录 `/tmp/trisoul-cu-*`），另 `stack-lifecycle.log` 通过：两目标持续更新、旧目标关闭不影响当前目标、只读 actor 拒绝输入、跨会话拒绝、点击选择和最后观察者清理。
- 真实 DSH 原生最终 `inline-native-final.log` 通过（19.6 秒），两个应用真实画面及前台 PID 验证、堆叠／展开／点击、停止后继续更新、自动页内显示、关闭与输入框不遮挡断言通过。截图 `trisoul-cu-native-ui-HUPY38/conversation-preview-stack.png`（系统临时目录）已看图，320×260 小窗在对话输入区上方。
- 调试记录保留：`stack-native-ui.log` 首先暴露测试 has 定位器错误；`inline-position-final.log` 暴露旧测试点击侧栏引导被浮层挡住，同时修复浮层随输入区宽度变化定位。现在原生 UI 使用正式常驻入口。`inline-browser-ui.log` 的内置失败是旧光标定位器匹配两处预览，现限定原侧栏并保留原位置断言；扩展在显示真实标签后出现裁剪帧，旧测试将 1280×577 画面按完整 1280×720 视口缩放，导致检查 y=201 而实际箭头在 y≈250。`inline-extension-diagnostic.log` 和 `trisoul-cu-ui-GgH4pO/cursor-observed-1.png` 保留证据；改为均匀像素比例和实际可见高度，原 >20 深色像素、<2px 等断言未放宽。
- `inline-browser-final.log` 继续复现页内浮层挡住 DSH 的 @ 应用选择菜单；采用菜单打开时暂时隐藏浮层、菜单关闭后恢复原观察连接的方式修复，保留真实鼠标点击的原测试。没有为通过测试强制点击穿透浮层。
- 最终真实 DSH 两浏览器 `conversation-preview-final.log` 2/2：内置 45.2 秒、扩展 49.8 秒，保留原圈选／框架／样式／接管／真实像素／安装／主题／窄窗检查，并验证页内默认显示、可选弹出和点击原标签。新 `stack-closed-app.log` 单项通过，旧应用关闭不清空或误报当前目标。没有重复模型付费实测，沿用已通过 v41 证据；已在 liveCalls=0 后更新 3083，监听 PID **77607**；认证接口 200、enabled=true、previewTargets 已返回。原生构建 **634b2536e78da13ffcb53d1ac98079ea8c585077afb0b5688c55bb6b03d9a42d** 与已安装一致，updateAvailable=false、restartRequired=false，辅助功能／屏幕录制权限保留。未清理／回退／提交／推送，构建和 `git diff --check` 通过。当前按用户描述交付，不宣称与 Codex 像素完全一致。

## 当前交付范围：额度收尾版（2026-09-12）

- 最新用户已进一步描述并纠正位置：像视频画中画，多个目标堆叠，点击预览进入对应窗口；**直接显示在对话页面内**。本轮据此实现自动页内堆叠，独立 PiP 降为可选。不得再要求截图作为此描述范围的开工条件；精确 Codex 像素外观仍无实物逐屏证据，不能声称像素完全一致。
- 用户周额度剩约 10%，明确要求：修已知 bug，完成 UI 和操控悬浮窗预览，然后暂时上线。Windows、复杂 CSS 命中及其余范围继续保留，当前不追加开发；不得将本次暂时上线描述为全面对齐 Codex。
- 已确认 QQ 第二轮 `setValue` 返回后内容为空；新增隔离 ControlledEditor 在 180ms 后回退输入，旧实现复现失败（`/tmp/trisoul-cu-set-value-before.log`）。原生现在在 400ms 内分次检查同一 AX 控件，未保留请求值即报错并中断批次，不自动重试／粘贴／发送。`/tmp/trisoul-cu-set-value-final.log` 通过：正常中文、延迟回退、后续保存被阻止、恢复输入、前台和物理鼠标保持。没有向真实 QQ 发送新消息；这不是 QQ 全任务验收。
- `qq` 绑定丢失与开发重启对应：原对话 2026-09-11 16:56:51Z 调用后，我们 16:57:57Z 重启了 3083，17:01:21Z 下一轮使用旧变量失败。新增运行时代际通知，已有 CU 历史且 worker 未存活时，在下一步提示重新绑定；不移动系统消息，不重复已完成发送。P123 记录完整上下文文本；`/tmp/trisoul-cu-runtime-context-final.log` 2/2，已有实际 DSH 模型请求验证见 `runtime-context-ui.log`。
- 新 `floating-preview.jsx` 通过宿主已有 `react-dom` portal 渲染真正 Document Picture-in-Picture 窗口，无额外 React 副本。面板提供“悬浮预览”，统一观察流显示网页／Mac 应用、助手鼠标和停止按钮；小窗只读，关闭只释放观察，停止后继续看图。切换目标／会话、禁用、刷新和卸载关闭所属小窗；支持浏览器不足时明示。蓝色状态和紧凑工具栏、默认收起的调用／截图保留。
- Mac 实际 DSH 悬浮窗 `/tmp/trisoul-cu-floating-native-ui.log` 通过（16.6 秒）：真实画面更新、小窗和主面板光标、图像坐标边界、停止后无光标且继续更新、关闭保留目标。`trisoul-cu-native-ui-VJtOSG/native-floating-cursor.png`（系统临时目录）已看图，480×360 布局和按钮完整。PiP API 在真实 Chromium 页面运行；未把无头截图当 OS 置顶位置实拍，窗口拖放位置由系统管理。
- 调试记录保留：第一轮 UI 在运行中重建前端，热重载使设置面板收起；后续改为构建后再运行。第二轮末尾错误地检查此前已被旧测试主动关闭的标签，改为核对停止前仍打开的实际标签全部保留。另修复预览收到同文档 navigation 事件后一直显示连接中的问题。没有删减既有断言。
- 最终两后端完整 DSH UI `/tmp/trisoul-cu-floating-release-ui.log` 2/2：内置 45.0 秒、扩展 51.1 秒；实际 PiP 打开／关闭、480×360 像素边界、停止按钮生效、停止保留当前标签、刷新关闭小窗，加上原圈选／框架／样式／接管／安装／主题／窄窗断言全部保留。`trisoul-cu-ui-Qn0T7b/floating-preview.png` 和 `pane-dark.png` 已看图。首次实测批次中的 `qq` 错误不靠重发真实消息验收。
- **已暂时上线到本机 3083**：liveCalls=0 后重启，当前监听 PID **69166**；认证 state=200。通过主服务安装更新原生运行时，版本 0.1.1、协议 1，构建 `9271f493ab5cc6485158b45e88095a07625a367d0866e63ae5bd932950017c3b`；安装接口 200、updateAvailable=false、restartRequired=false，辅助功能和屏幕录制均保留已授权。包含前轮跨框架批注和官方 v41 已通过版本；本轮未重复模型付费实验。最终构建、相关测试及 `git diff --check` 通过，没有清理／回退／提交／推送。
- **下一步**：本轮停止扩展开发，用户刷新 3083，打开 Computer Use → 悬浮预览即可使用。余项保持未完成，待用户恢复额度或另行指定后再推进。

## 最新：跨源嵌套框架批注与 v41（2026-09-12）

- B19 现在可选择同源／跨源嵌套框架中的元素并预览其样式。新增 `annotation-documents.mjs` 与纯几何 `annotation-geometry.mjs`，复用实际 frame bindings，逐进程采集后释放独立 CDP 会话；主模型的 AX／编号状态不变。用浏览器内容四角映射 iframe 边框、padding、旋转／透视、内外滚动和 pinch；外层 paint 顺序仍高于被遮挡的子框架。框架身份、URL 和实际多边形随 PNG／TXT 进入草稿。
- 框架视口从隔离世界读取，网页覆盖 `innerWidth` 不能伪造几何。样式作用于对应文档的真实节点，恢复失败保留正确的子 CDP 会话；进程切换后的导航证明读取新 CDP 文档身份，不采信网页变量。`pagehide` 同步恢复临时样式，避免仅等渲染器计时器；完整 BFCache 专项仍未做。
- 新回归 `/tmp/trisoul-cu-frame-annotation-final-backend.log` 5/5：两后端嵌套／同名控件、实际颜色像素、隐藏框架和滚动裁剪、外层遮挡、透视、网页变量伪装、子导航失效与样式恢复，保留既有取消／失败重试／卸载断言。独立框架场景采集约 0.2–0.4 秒，仅为此本地夹具测量，不是 Codex 速度对照。
- 实际 DSH 内置浏览器 UI `/tmp/trisoul-cu-frame-annotation-ui-verified.log` 通过（42.5 秒），框架内点选、样式恢复、路径／多边形附件和发送验证通过，原 UI 回归保留；`trisoul-cu-ui-detlYc/page-annotation-child-frame.png` 已看图。扩展本轮有后端和真实模型验证，未重复无关的完整扩展 UI 流程。
- **真实官方 v41**：`/var/folders/ft/_5z_kgq54p9cxp87fx5m8z240000gp/T/trisoul-cu-live-ogkaac/report.json`，36.484 秒、2 个含图请求、0 工具错误／旁路，zeroError=true。批注来自真实 DSH 采集接口；模型先观察后将内层填为“框架批注验收”、点击按钮，外层仍为空白，标签保留。初始批注按文字发送，图片证据另存 `user-annotation.png`；模型自己的工具截图实际进入请求。新增可选实测开关和上下文格式见 P122。
- 保留失败及修复：`annotation-frames.log` 首先暴露测试视口不足／测试清理顺序；`annotation-frames-verified.log` 的模型所属测试视口在 Stop 后按既有规则恢复，改由测试观察者设置自己的固定视口。真实 UI `frame-annotation-ui.log` 发现无 CSS 的 Document 节点被误当 overflow 裁剪容器，主页面滚动后元素清单为空；已按实际 CSS 值裁剪，并增加滚动后的真实像素断言，没有删减旧断言。
- **后续保留项（当前暂缓）**：任意 CSS mask／clip-path 等复杂可见命中，以及完整 UI 与同任务质量／速度对照。全局窗口分享快捷键、Windows／锁屏及其余基线缺项都保留，目标 active，不能称全面完成。

## 最新：真实网页样式预览（2026-09-12）

- **批注页面 → 选择元素 → 调整样式** 已接入宽高、字号、颜色和间距。点击预览后暂停助手，在实际页面的所选节点应用临时 CSS，采集真实 PNG／元素位置后恢复；图片和修改前／拟议样式经原 DSH 草稿附件发送。可显示原图重新选择，继续操控前需恢复助手控制；没有改源码或自动发送。
- `browser-style-preview.mjs` 复核冻结快照的 DOM／几何及真实节点，使用隔离世界保存样式；恢复时精确还原原 style 属性，若网页同期改样式，则仅恢复仍属于预览的属性。渲染器有 5 秒自动恢复，但不把定时器当成功回执；恢复失败保留事务／连接，禁止恢复控制，重试停止确认。卸载前也必须确认临时样式恢复。请求取消、停止、禁用和导航沿用现有控制生命周期。
- 两后端真实颜色像素、宽度、成功／失败／取消恢复、网页自身改动、恢复失败后重试、自动恢复及卸载连接顺序：`/tmp/trisoul-cu-style-preview-final-backend.log` 3/3。原停止／释放／连续对话框／卸载／丢回复回归 `/tmp/trisoul-cu-style-preview-stop-regression.log` 4/4；组件与 HTTP 既有回归 `/tmp/trisoul-cu-style-preview-component.log` 3/3。
- 实际 DSH UI：内置在 `/tmp/trisoul-cu-style-preview-ui-verified.log` 通过（39.1 秒）；扩展最终 `/tmp/trisoul-cu-style-preview-extension-final.log` 通过（47.8 秒），保留原光标精度、控制、主题、窄窗和安装断言。展开样式编辑器会缩小预览，提交按钮可见；`trisoul-cu-ui-fqsd1E/page-annotation-style.png` 已看图（系统临时目录）。元数据明确预览已恢复，修改源码仍需后续实际执行，见 P121。
- 保留失败：首次 UI `/tmp/trisoul-cu-style-preview-ui.log` 的后续光标脚本误用了停止前的 JS 变量，现按既有 reset 语义重新绑定同一标签；未改光标容差或停止行为。扩展中原生更新检查读到旧版本（`style-preview-ui-verified.log`）；原先仅等 busy 文案消失，不能证明更新完成。现先核对实际更新 POST 的成功响应，再保留版本和权限检查，最终通过。
- **仍需推进**：B19 子框架内部与非矩形／复杂遮挡精确选择、完整外观和真实模型反馈任务对照；本轮为真实浏览器及 DSH 链路验证，不冒充新增 v41 实测。Windows、锁屏、全局分享快捷键和其他基线缺项仍保留，目标未完成。
- 已构建并在 liveCalls=0 后更新 3083，监听 PID **60048**；认证 state=200、enabled=true，新增 annotation-style 路由已加载并拒绝不存在的观察者。原生二进制未改，最终 `git diff --check` 通过，未清理／回退／提交／推送。

## 最新：网页元素批注（2026-09-12）

- **批注页面 → 选择元素** 已接入：在冻结画面点击控件，显示蓝框、元素身份与按需展开的样式；PNG／TXT 加入现有草稿并由 DSH 发送。新 `browser-annotation.mjs` 使用观察连接的 `DOMSnapshot.captureSnapshot`，在真实截图两侧核对 DOM、位置及样式，并核对缩放／滚动几何；不调用页面输入、不取得模型租约，也不修改页面 DOM。前后不一致或导航时拒绝；关闭请求和禁用会取消读取。
- 主文档与 shadow DOM 的可见元素按绘制顺序和层级点选；过滤滚动容器外的节点，图片坐标跟随缩放。当前最多 5000 个可见元素，截断与子框架缺项在界面明示。**子框架内部、复杂非矩形裁剪／遮挡的精确命中、样式修改预览仍待实现**；计算样式展示不冒充样式预览。下一步继续这些 B19 缺项，不重做本轮已通过的基础链路。
- 两后端截图／结构／控制权测试 `/tmp/trisoul-cu-annotation-elements-final.log` 3/3；追加缩放、采集中禁用与断流测试 `/tmp/trisoul-cu-annotation-elements-lifecycle-final.log` 2/2。中间 `annotation-elements-lifecycle.log` 仅最后一个断流断言先遇到“已关闭”；现分别检查关闭，再重新启用检查断流，没有删减断言。
- 实际 DSH UI：内置 `/tmp/trisoul-cu-annotation-elements-ui.log`（51.9 秒）、扩展 `/tmp/trisoul-cu-annotation-elements-extension-ui.log`（47.0 秒），含真实点击控件、正确身份／样式进入 TXT、保持原草稿并发送，原圈选、取消、接管、导航和小鼠标回归保留。`trisoul-cu-ui-YdKcYQ/page-annotation-element.png` 已看图，控件蓝框正确；截图位于系统临时目录。HTTP 认证／禁用／取消及已有流测试 `/tmp/trisoul-cu-annotation-elements-http.log` 3/3。
- 文本上下文变更记入 P120。没有改系统模型提示词、任务、验证或记忆机制；完整目标保持 active，不能称全面对标完成。用户问到“全局窗口分享快捷键”时已说明：在别的应用按快捷键将当前窗口加入草稿，具体按键未定、入口未实现，与现在回到 TrisoulX 手选窗口有别。
- 本轮已构建并更新 3083，重启前 liveCalls=0；监听 PID **56229**，认证 state=200、enabled=true，新 annotation 路由按预期拒绝不存在的观察者。未向用户网页或应用发送测试输入，未改原生二进制，未清理／回退／提交／推送；最终 `git diff --check` 通过。

## 最新：网页圈选批注接入 DSH（2026-09-12）

- B19 浏览器面板新增 **批注页面**。冻结已显示的真实预览后圈选、写说明，生成原尺寸蓝框 PNG 与记录页面／标签／时间／像素区域／用户说明的 TXT，加入现有草稿，经原 DSH 附件流程保存和发送。圈选不发网页输入，也不取得控制权；网页后续变化不替换冻结图片。当前只实现截图区域批注，DOM 元素选择和样式预览仍待完成。
- 实现：`page-annotation.jsx`；`browser-preview.jsx` 只增加已加载帧通知及失效清理，`computer-use.jsx` 接入面板入口。没有更改工具调用分组、任务、验证、上下文、记忆或模型选择。用户附件文字变化见 P119。
- 两种浏览器实际 DSH UI 测试通过：`/tmp/trisoul-cu-annotation-ui-verified.log`（内置 47.1 秒）、`/tmp/trisoul-cu-annotation-extension-ui.log`（扩展 57.7 秒）。验证原草稿、原图尺寸／区域／蓝框字节、冻结不变、零网页输入、取消时无残留，最终模型请求同时收到文字附件与图片；原接管、导航、小鼠标、主题和窄窗检查保留。
- 初轮 `/tmp/trisoul-cu-annotation-ui.log` 在模型请求断言失败，原因是 DSH 将草稿和文件信息放入两个文字块；断言改为拼接同条用户消息的文字块，并继续要求该消息带 `image_url`，没有移除图片断言。截图 `trisoul-cu-ui-wPWnck/page-annotation.png` 已看图。
- 重要边界先复现再修复：跨会话取消编码本已拒绝旧附件，但新会话继承 busy 状态；`annotation-session-before.log` 保留失败，清除会话局部状态后 `/tmp/trisoul-cu-annotation-session-final.log` 通过，新会话可正常添加。此测试使用实际 React／浏览器编码，DSH 保存发送另由上述 UI 验证。
- 深色和窄窗截图 `trisoul-cu-ui-wR7Ynk/page-annotation-dark.png`／`page-annotation-narrow.png` 已看图；窄窗原布局需要滚动才能看到提交按钮，已调整对话框高度与截图上限，最终回归见后续记录。上述截图目录均位于系统临时目录 `/var/folders/ft/_5z_kgq54p9cxp87fx5m8z240000gp/T/`。
- 3083 窗口分享更新的第一次就绪探测超时，但后续已确认启动正常，监听 PID 48362，认证 state/setup 均 200、enabled=true；原生 installed=true、updateAvailable=false、restartRequired=false。最终前端更新后的状态以本节补充为准；未打开用户浏览器或请求钥匙串。
- 最终布局／会话修复后扩展 UI `/tmp/trisoul-cu-annotation-extension-final.log` 通过（48.9 秒），保留全部原断言，并要求窄窗主按钮无需滚动即可见。`trisoul-cu-ui-Mu0igV/page-annotation-narrow.png` 已看图，蓝框、说明和提交均可见。构建与 `git diff --check` 通过；3083 在确认 liveCalls=0 后已更新，监听 PID **52611**、认证 state=200、enabled=true，前端包含本轮批注。未清理、回退、提交或推送。

## 最新：Chrome 更新验证与窗口分享（2026-09-12）

- 用户已更新系统 Chrome，静态版本 **153.0.8010.37**。使用这一安装程序及独立测试配置通过 WebMCP 回归：`/tmp/trisoul-cu-webmcp-chrome153.log`。没有访问用户 Chrome 配置或请求真实钥匙串授权。
- 官方 v41 WebMCP 专项通过：`/var/folders/ft/_5z_kgq54p9cxp87fx5m8z240000gp/T/trisoul-cu-live-Fnypai/report.json`，48.734 秒、2 个含图请求、0 工具错误／操作错误／旁路，zeroError=true。任务通过页面公开工具两次改备注，中间替换工具并重新发现，最终 `2:新工具已生效`、截图和标签保留正确。新增可选 `TRISOUL_CU_TEST_WEBMCP=1`，Mac 模型实测夹具也统一使用独立配置与测试钥匙串；生产启动配置未改成测试钥匙串。
- **分享窗口** 已接入输入区：用户选择 Mac 窗口后，PNG 与可访问性文字 TXT 加入当前草稿，由 DSH 正常保存和发送；不自动发送，不覆盖原草稿。使用独立的原生只读会话，跳过输入租约并保持模型的目标、AX 编号与差量基准；PID 生命周期检查拒绝已重启窗口。窗口身份由服务端实际应用解析，不能被客户端名称伪造。
- 关闭对话框／请求、禁用或卸载会取消读取；临时读会话清理失败留在原生登记中，可继续确认清理，不能静默当作成功。实现位于 `window-share.jsx`、`http.mjs`、`manager.mjs`、`native.mjs`、原生 `Service.m`。这只完成输入区手动选择的分享流程；**全局 Appshots 快捷键和前台上下文入口仍未完成**。
- 原生隔离窗口验证：`/tmp/trisoul-cu-window-share-native-final.log`，真实截图／文字、与已有输入所有者并存、基准对象与编号表不变、正文控件连续性、前台／鼠标／输入不变，拒绝重启身份和不存在窗口后仍保留模型租约。
- HTTP／生命周期：`/tmp/trisoul-cu-window-share-lifecycle-final.log` 5/5，认证、禁用、用户取消、关闭与清理失败重试；不创建模型会话。真实 DSH UI：`/tmp/trisoul-cu-window-share-ui.log` 通过，验证原草稿、两种附件与发送内容，保留原接管／导航／光标等检查；截图 `trisoul-cu-ui-pLhnwe/window-share-draft.png` 已看图。截图目录位于上述系统临时目录。
- **保留发现**：最初用“分享后整棵 AX 树无变化”检查失败；进一步在分享前连续读取也无法得到无变化结果，变化来自系统标题栏的匿名 AXGroup。分享验证改为直接检查模型基准对象、完整编号表与正文控件，并继续保留前台／鼠标／输入断言。失败日志 `window-share-native.log`／`window-share-native-verified.log` 保留；该标题栏差量噪声仍是 C09 待处理项，未靠删减节点或改动作结果掩盖。
- 提示词及测试配置改动见 P117–P118；Windows／锁屏、全局快捷键、批注、专用内容导出与完整质量／速度对照仍未完成，不能称全面对标已完成。

## 最新：页面导出、素材与 WebMCP 接入

- B14 当前页通过 `tab.content.export()` 导出 MHTML；B16 通过 `tab.capabilities.get('pageAssets')` 清点当前主文档／开放 shadow DOM 和浏览器已观察资源，保存已加载的图片、字体、样式、视频。清单在导航或重新读取后失效；坏图／不可读资源明确失败，取消清理未完成目录。专用 GSuite／YouTube、完整子框架 DOM、未缓存／blob／流媒体资源仍未完成。
- 文件经 DSH `saveFileStream` 存为持久附件，调用卡保持默认收起；展开显示文件名与打开入口，刷新后使用附件的持久路径。没有把导出图片自动铺成大图；附件失败保留原路径且标记工具错误。实现 `browser-content.mjs`、`runtime.mjs`、`tools.mjs`、`computer-use.jsx`。
- B15 使用 Chromium 原生 `WebMCP.enable/invokeTool/cancelInvocation`，不模拟网页应用 API。接口为 `tab.capabilities.get('webmcp').fetchTools()`，再 `description()`／`call()`。注册改变／导航后旧句柄拒绝；取消失败保留原调用及连接，重试停止完成前禁止恢复。
- **版本限制**：系统 Chrome **152.0.7977.83** 没有把取消信号传给测试工具回调，真实失败保留；当前明确要求 Chromium 153+，旧版不出现在能力清单并解释原因。兼容性测试用隔离 Chromium **153.0.8010.12**；内置浏览器启用自身 WebMCP 实验特性，外置 Chrome 保留用户设置。没有替换用户日常 Chrome。WebMCP 跨进程 iframe／声明式表单边界和真实模型调用仍待验收。
- 参考为既有本机 Browser 26.903.61454 的 pageAssets／WebMCP 文档及本机实际 CDP 协议；协议演进补核 [WebMCP 规范](https://webmachinelearning.github.io/webmcp/) 和 [Chrome 官方说明](https://developer.chrome.com/docs/ai/webmcp)。不是对 Codex 所有内容导出形式和页面行为的完整验收。

验证：

- `/tmp/trisoul-cu-content-attachments-verified.log` 16/16：两种浏览器的实际资源字节、坏图、懒加载、失效、取消清理、持久运行时和附件流／失败反馈。
- `/tmp/trisoul-cu-content-ui.log`：实际 DSH 内置浏览器 UI 通过，文件入口在默认折叠内、存为持久附件、重载后可用；约 30.7 秒，截图 `trisoul-cu-ui-VycyyR/saved-tool-card.png` 已看图。没有重跑无关扩展 UI；扩展资源／导出后端另有实测。
- 官方 v41 内容任务：`/var/folders/ft/_5z_kgq54p9cxp87fx5m8z240000gp/T/trisoul-cu-live-q70P6E/report.json`，34.907 秒，8 个含图请求，更新后的页面、3 张图片及 1 份样式字节正确，1 张坏图准确失败，标签保留；0 CU 操作错误／旁路，1 次 verify_link 缺 reason，因此 zeroError=false。此轮在自动附件接入前完成；附件接入后的真实 DSH 链路由上述 UI 测试验证。
- `/tmp/trisoul-cu-webmcp-final.log` 3/3：两种后端在兼容 Chromium 上真实注册、执行、工具错误、替换与导航失效、停止确认失败保留并重试、取消后没有延迟写入；旧版能力过滤也验证。`webmcp-check`、`webmcp-managed-check`、`webmcp-verified` 日志保留旧 Chrome 信号差异和测试浏览器普通钥匙串启动失败。
- **钥匙串干扰**：普通启动 Chrome for Testing 曾弹出用户钥匙串访问窗口，用户因此询问“谷歌密钥”。不需要 Google API 密钥；没有读取其密钥或钥匙串内容。已确认相关测试进程全部退出；隔离夹具改用测试钥匙串，不能把该测试通过当作真实登录态通过。不要再重复普通启动该测试浏览器的探测。
- 提示词与开发约定变更见 P114–P116。Windows／锁屏及原生 QQ 的完整任务／速度仍未完成；本轮不是“全面对标完成”的声明。

- 以上代码已更新到 3083，重启前全局 liveCalls=0；当前监听 PID **44053**，state 返回 200。未为更新打开 Chrome 或请求钥匙串授权。最终纯代码／运行时验证见 /tmp/trisoul-cu-content-final-unit.log（18/18）；未修改原生二进制，未清理、回退、提交或推送。

## 最新推进方式与代码修复

- 用户要求优先做本环境能快速编码、自动验证的工作。**Codex 外观对照仍由助手负责**；系统授权、日常扩展安装、实际 QQ／Office 试用和其他设备验收可交用户。给用户短步骤及预期结果，修错与实现继续由主模型承担；不让人工验收阻塞无关开发，不因此删减范围或宣称已验收。
- 原生安装指纹现在按当前源码读取，不再把首次结果或临时读取失败缓存至进程结束；安装发布前的指纹比对、签名、启动验证和失败恢复保持原样。隔离源码副本测试已复现旧进程读不到更新，并验证同一实例读取新指纹及读取失败后恢复；没有修改用户原生源码来造测试。
- 操作状态新增会话内完整计数（总数、成功、失败、取消及方法次数），最近操作列表仍保持 60 条。真实模型评估按完整计数检查早期失败、截图和键盘场景中的 paste／setValue 尝试，避免截断掩盖错误。旧报告缺少计数且达到 60 条时仍标未知，不追改为通过；计数随运行实例存在，完整历史仍以会话事件为准。
- 先复现的日志 `/tmp/trisoul-cu-fast-fixes-before.log`；修复后 `/tmp/trisoul-cu-fast-fixes-verified.log` 7/7，通过长记录截断、早期失败、取消、方法计数、跨回合保留、会话隔离、不可变快照及新旧评估数据检查。产品模型提示词未变。
- `/tmp/trisoul-cu-fast-fixes-manager.log` 相关管理器回归 7/7；官方 v41 无界面浏览器报告 `/var/folders/ft/_5z_kgq54p9cxp87fx5m8z240000gp/T/trisoul-cu-live-xjO2pK/report.json`：44.097 秒，结果正确且标签保留、0 旁路；完整统计 27 次操作／1 次操作失败，4 次工具错误（待办摘录、误用 tab.setChecked、猜定位器超时、重复声明变量），zeroError=false。报告链路验证通过，任务不是无错完成，失败保留且未重跑覆盖。
- 3083 已在确认全局 liveCalls=0 后后台重启，当前监听 PID **39798**；state 返回新的 operationStats，模型图像能力仍正常。没有打开浏览器或操作用户应用。本轮未改前端／原生二进制，无需重复 UI 或原生鼠标验收；最终语法和 `git diff --check` 通过。

## 最新交付：连续操作收起、模型提示与 QQ 输入（2026-09-11）

- **入口**：`http://127.0.0.1:3083`，输入区下方常驻 **Computer Use**，点击打开控制面板。启动用 `node scripts/launch-macos.mjs`，沿用 `data/dsh` 和 `trisoul-x` profile，登录 token 仅留在私有日志，不写入交接。手动接管后仍需点击 **恢复助手控制**。
- **刷屏修复**：连续 CU 调用及对应过程文字现在合并为默认收起的一组，运行中与中断回合也生效；其他工具、用户消息、上下文更新和最终回答保留边界。展开后每次调用仍为紧凑行，截图按需挂载；次数、失败、停止状态可见。`computer-groups.mjs/.jsx` 仅做展示投影，不改会话事件、模型上下文、任务或记忆。实际 QQ 历史已看到 9 次调用（含 2 次失败）合并。Codex 的全部 UI 状态仍未逐屏对照，不能称完整复刻。
- **模型视觉提示**：输入区与面板显示当前配置是否仅文本；截图工具结果按实际请求模型补充“未看到像素”的说明。DSH 元数据字段是 `inputModalities`；下一轮 pending 选择只影响界面，不能错用于当前工具结果。未知能力不误判为仅文本，不自动切模型，附件仍保留。
- **QQ 输入**：首次观察请求 Chromium/Electron 按需 AX；无控件焦点时允许使用匹配的 AX 焦点窗口，仍拒绝窗口缺失／不匹配。网页控件使用已验证的进程定向事件和会话期间后台焦点；停止、回合结束与传输 EOF 清理焦点，清理失败保留租约供重试。未改为全局键鼠。Sonoma 256 字节事件记录修复保留。
- **实际 QQ 验证**：先关闭惰性 AX，再由自研服务开启；搜索框键入 `TrisoulX验收20260911`，读取实际值后恢复原 `a a a`，未发送消息。证据 `/tmp/trisoul-cu-qq-held-focus.log`，目录 `trisoul-qq-input-TCIv7V`。此前失败和恢复实验保留，不重发用户两个 QQ 任务。
- **大 AX 输出**：超限时保留 UTF-8 完整的前段和按范围读取提示，原字符串仍可静默获取；保持原输出预算。避免 QQ 大树被整块丢弃。
- **浏览器交付页**：工具说明及未保留的自建标签可见截图提醒 `markDeliverable()`／`markHandoff()`，静默截图不输出提醒。生命周期未改，未标记的中间页仍关闭。此前失败 `trisoul-cu-live-2EfkVy` 保留；修复后真实 v41 浏览器任务通过，见下方。
- 提示词新增与原文对照见 `PROMPT_CHANGES.md` **P110–P112**。网页输入配方的 MIT 来源已补入 `THIRD_PARTY_NOTICES.md`。

### 本轮验证与未通过项

以下临时目录都位于 `/var/folders/ft/_5z_kgq54p9cxp87fx5m8z240000gp/T/`，日志不相加冒充覆盖率。

| 证据 | 结果／边界 |
| --- | --- |
| `/tmp/trisoul-cu-targeted-final.log` | 17/17：连续分组、模型能力、运行时大文本／临时页提示、HTTP 流 |
| `/tmp/trisoul-cu-ui-final-verified.log` | 内置、扩展两后端 UI 通过，约 29.7／29.8 秒；分组、延迟图片、模型切换提示、重载、接管、导航、光标、主题及窄屏；截图 `trisoul-cu-ui-S8v319`、`trisoul-cu-ui-iE1ui4` |
| `/tmp/trisoul-cu-native-final-verified.log` | 普通键盘、无 AX 控件焦点、粘贴均通过；其中网页路径前台检查因同时启动 Chrome UI 夹具失败，保留记录 |
| `/tmp/trisoul-cu-web-cleanup-verified.log` | 随后独立串行网页路径 3/3 通过，原前台和鼠标断言均保留且通过；含停止和真实原生 MCP EOF 后焦点释放，目录 `trisoul-cu-keyboard-gNH3t3` |
| `trisoul-cu-live-tlYpeu/report.json` | 官方 v41 浏览器：28.961 秒，6 个含图请求，表单和延迟目标正确、交付页保留，0 工具错误／旁路，zeroError=true |
| `trisoul-cu-live-hacn6k/report.json` | 官方 v41 网页焦点＋隐藏 AX 控件焦点夹具：113.945 秒，26 次 CU、25 个含图请求；中文／emoji／换行、最终文字和保存正确，0 旁路；1 次待办摘录错误、1 次不存在的 `cua.getScreenshot()`、1 次窗口失效后恢复；历史达到 60 条上限，zeroError=null，不能称无错通过 |

- 最后一项实测的独立 `trisoul-cu-keyboard-FPnIml/live-final.json` 确认结束后逻辑焦点释放；前台从 QQ PID 1576 变为 ChatGPT PID 948，鼠标也变化，因此“不干扰用户输入”本次未通过。测试期间用户在用电脑，不能仅凭这个差异判定服务抢焦点，也不能抹去失败。此前独立串行测试已经通过相同不变断言；目前不要求用户继续让出鼠标。
- 原生实测虽最终成功，模型反复检查菜单／猜 API，恢复步骤过多；**下一项优先核对快捷键可观察反馈与窗口选择，减少这条真实任务中的无效重试**。完整轨迹已提取到最后报告同目录 `computer-use-calls.json`，不要为找原因先重跑所有测试。
- 光标 UI 测试曾在现有 35ms 动画中途取样，已改为至多等待 250ms 收敛，仍保留原 `<2px` 断言；没有更改产品光标或放宽容差。失败日志保留。

### 最新运行状态

- 3083 已重启加载本轮后端与构建好的前端，监听 PID **39798**（会变化，最新后端更新见上方）；两段 QQ 会话检查为空闲。新页面再次确认连续 9 次操作合并、默认收起，常驻入口可见；历史记录未重发或修改。
- 源码／磁盘／主 DSH socket 运行指纹一致：`4ba15fb7db148352b9ca79b87b105ce034fd811ffdf3cd0cf7cc25f29be8d779`，原生版本 0.1.1、协议 1，`updateAvailable=false`、`restartRequired=false`；辅助功能和录屏已授权。主 socket 是 `/var/folders/ft/_5z_kgq54p9cxp87fx5m8z240000gp/T/trisoul-cu-502-590d35f8ced88b44.sock`。
- 日常 Chrome 的 TrisoulX 扩展仍未连接；Codex 自己的 Chrome 连接不等于 TrisoulX 扩展已连接。用户可以使用已安装的内置浏览器。
- 更新时旧 3083 进程缓存了开发前的 expectedBuild，第一次安装拒绝指纹不一致且未替换文件；重启加载当前代码后更新成功。该缓存缺陷现已由上方代码修复；指纹检查保留。
- 本轮构建、针对性回归和最终 `git diff --check` 通过。工作区未提交／未跟踪源码全部保留；没有清理、回退、提交或推送。

## 目标、范围与推进约定

- 正确项目是 `/Users/mac/Projects/trisoul_x`。`/Users/mac/Projects/trisoul` 保持只读，不能因旧对话默认目录在旧项目就改错仓库。TrisoulX 是 DSH 插件，Computer Use 是其中的模块，不是 skill。
- 用户要求操控能力、质量、速度、界面和小鼠标尽量接近 Codex；可以重写，不为省事保留不合适代码。现有大量实现已落地，应基于当前代码和证据判断保留或替换；避免重复整体调研，有证据表明某部分重写更优时可以重写。
- 四类移到以后可选，当前不做：ChatGPT 账号／会话／云端 Work／手机远程体系；ChatGPT 专用 Office add-in／官方连接器生态；ChatGPT 浏览器侧边聊天／历史同步；Codex 插件市场／内部注册／专属包装复刻。通过 UI 操作 Office、连接已有浏览器及目标引用仍保留。
- 其余讨论范围都保留：核心操控与完整 UI、停止／接管、安装更新卸载、Appshots、浏览器文件流／批注／素材／导出／WebMCP、Windows、锁屏，以及同任务质量／速度对照。详细条目看 `COMPUTER_USE_BASELINE.md`；不要默认加载它的大量历史记录。
- 开发对话建议使用用户所选同一主模型的 **High**；复杂原生崩溃、并发或关键审查确有需要时再评估 Xhigh。不是更换弱模型，不委派 Luna，也不自动组建多代理。
- 用户对耗时和无效消耗已非常不满。先让现有成果可实际使用，每轮完成明确功能或解决真实阻碍；必要测试与已授权 v41 实测照常。不要反复扩大验收清单、重跑无关全量检查，或为了绿色结果放宽断言。先前 60%／65% 已撤回。
- 工作区有大量未提交、未跟踪的产品源码和测试，全部应视为待保留工作；不要 `git clean`、覆盖或回退。尚未授权 commit／push。任务、锚点、验证、上下文和记忆机制必须保留；提示词变动记录于 `PROMPT_CHANGES.md`。

## 当前实现在哪里

| 路径 | 已有内容 |
| --- | --- |
| `src/computer-use/` | 持久 JS、工具路由、目标所有权、原生 MCP 客户端、浏览器动作／CDP 连接、图片坐标来源、实时预览、安装更新 |
| `native/computer-use/` | 自研 macOS 服务：AX 编号与差量、窗口绑定、后台键鼠／滚动／富文本、光标、录制、会话清理 |
| `browser-extension/` | 已有 Chrome 扩展、Native Messaging、按标签控制和停止；开发版安装更新，不是正式商店分发 |
| `src/client/computer-use*`、`browser-*`、`native-preview.jsx`、`assistant-cursor.jsx`、`computer-reference.jsx`、`computer-setup.jsx` | DSH 工具卡片、目标引用、设置、两种浏览器预览与接管、原生实时画面及小鼠标 |
| `test/computer-use-*.test.mjs`、`test/fixtures/computer-use/` | 有针对性的测试和真实隔离页面／AppKit 应用；仅部分依赖已授权原生服务 |
| `scripts/test-computer-use-live.mjs` | 经 DSH 执行真实 DeepSeek 模型，检查操作结果、工具轨迹和旁路；不通过隐藏脚本替模型完成任务 |

## 最近已核对的状态和证据

- macOS 14.8.7；原生服务安装在 `/Users/mac/Applications/Trisoul Computer Use.app`。版本 0.1.1、协议 1，本轮已更新原生构建；实际指纹与 3083 状态见本文最新运行状态。辅助功能、录屏已授权；接手时确认实际状态即可，勿要求用户重复授权。
- 独立测试服务 socket 为 `/var/folders/ft/_5z_kgq54p9cxp87fx5m8z240000gp/T/trisoul-cu-502.sock`。普通 DSH 按自己的目录生成另一个 socket，不要假设二者相同。PID 随重启变化。
- `scripts/start.mjs` 使用本地当前代码；默认 DSH_HOME 为 `data/dsh`、profile 为 `trisoul-x`、端口 3083。本轮已启动并更新本地 DSH，见最新运行状态。启动前检查端口和现有进程；启动输出可能含登录凭证，不能写入公开文档。
- 最近修复的实质缺陷：Sonoma 的 `SLSEventRecordLength()` 返回 256，旧焦点事件只分配 248 字节，导致偶发原生崩溃。`Input.m` 现查询实际长度、完整初始化，每条事件重新初始化。不要退回硬编码 248 的旧配方。
- `native.mjs` 现可在输入传输断线后确认原服务退出或原会话已离开注册表，再释放目标和重置。没有静默重放失败操作。
- macOS 14 实时预览用 Quartz 单窗口变化帧；15+ 使用 ScreenCaptureKit，但后者尚无真机验收。原生预览目前只读，手动操作在真实应用窗口完成。

以下日志均位于 `/tmp/trisoul-cu-eval.xxbEnx/`，交接时已读取统计；不是当前完整套件全部通过的证明，不能相加冒充覆盖率。

| 日志 | 结果及范围 |
| --- | --- |
| `screenshot-options-verified.log` | 14/14，两种浏览器的裁剪／全图坐标、视口恢复、失败后连接释放及扩展回归 |
| `viewport-before-load-verified.log` | 2/2，新页在网站脚本执行前应用请求尺寸 |
| `native-focus-recovery-final.log` | 17/17，后台输入、40 轮焦点／截图交错、真实传输终止与恢复、预览和 JS 运行时 |
| `native-focus-paste-ui-verified.log` | 12/12，真实 DSH 原生 UI／光标／停止后观察、富文本和剪贴板生命周期 |

- 最近构建和 `git diff --check` 通过。最新产品改动后未重跑完整套件。旧 `native-keyboard-surface-final.log` 的全局鼠标静止断言仍有未确认失败，未证明是代码问题还是人工干扰，不可删除断言。
- 前轮普通 AppKit 真实 v41 报告（非本轮网页／隐藏焦点场景）：`/var/folders/ft/_5z_kgq54p9cxp87fx5m8z240000gp/T/trisoul-cu-live-PTWkoy/report.json`。中文／emoji／换行、第二行改字、Command+S 正确，39.2 秒、7 次 CU、8 个含图片请求、0 原生动作错误、0 旁路；另有一次 `verify_link` 缺 `reason`，所以 `zeroError=false`。同目录 `computer-use-calls.json` 和 `final-browser.png` 已核读／看图。失败前例是 `trisoul-cu-live-WLrJsz`，不能删除失败记录。

## 最后一个未落地的实验

- 产品中的 `tab.screenshot({fullPage:true})` 仍拒绝活动双指缩放；默认／区域截图已保持缩放和平移。完整全页截图对其他 CDP 客户端模拟参数的恢复仍未解决。相关文件是 `browser-screenshot.mjs`、`browser-actions.mjs` 和 `computer-use-screenshot-options.test.mjs`。
- 已验证普通及本插件视口下全图完整、滚动位置保持、缩小后的全图坐标命中。Codex 同一隔离页面的黑盒对照返回 1800×1600 JPEG，DPR 为 2；页面滚动到 700 后全图截图将其复位至 0。此对照没有验证 Codex 双指缩放行为，不能推广；也不应复制已发现的副作用。
- **可接着验证的线索**：`/tmp/trisoul-cu-eval.xxbEnx/full-page-rect-probe.mjs` 及同名 `.log`。自有浏览器中，全页捕获把 visual offset 转成 layout scroll；先恢复布局滚动，再通过 `DOM.scrollIntoViewIfNeeded` 对 HTML 根元素指定原视觉视口矩形，两个实验均恢复了完整 visual/layout 几何，无需合成用户触摸事件。
- 这只证明该隔离页面的可行性，尚未写进产品，也未覆盖外置浏览器、不同页面结构／缩放、取消和并发。脚本中的旧 fixture URL 已失效，运行时创建新隔离页面；不要为读取旧日志先重跑所有探针。`full-page-scale-probe.log`、`full-page-viewport-probe.log` 保留前两种不完整恢复结果。

## 实测配置与注意事项

- 用户已授权使用官方 `deepseek-official` 的 `deepseek-v4.1-flash-expires-on-0910` 实测，现有评估配置 reasoning 为 `max`。开发对话的 High 不等于修改测试模型配置。
- 私有 key 文件 `/tmp/trisoul-cu-eval.xxbEnx/model-key`，本次仅核实存在且权限 0600，没有读取／展示值。通过 `TRISOUL_CU_TEST_KEY_FILE` 交给评估脚本；不要把 key 放进代码、文档、命令参数、终端输出或提示词。
- 原生键盘场景先使用 `test/fixtures/computer-use/keyboard.mjs` 创建自己的应用，再设置 `TRISOUL_CU_TEST_NATIVE_KEYBOARD=1`、`TRISOUL_CU_TEST_NATIVE_APP`、`TRISOUL_CU_TEST_NATIVE_REPORT` 和实际原生 binary／socket。测试结束关闭自己创建的应用；不要照抄已退出 fixture 的 bundle ID。
- 原生测试按需串行，避免前台和剪贴板测试相互影响。曾有旧测试夹具覆盖用户剪贴板且未恢复，已向用户说明；保护逻辑已修复，不得移除。真实输入／取消／剪贴板验证仍须保留。
- Codex Computer Use 工具曾明确拒绝访问 Codex、SecurityAgent、UserNotificationCenter；不能用自研后端绕过。同样不应操作用户无关应用来测试。Windows、锁屏与当前 macOS 未覆盖场景需要对应真实环境证据。

## 新对话如何开始

先快速核对当前代码、进程和这份交接，再让用户能从明确入口实际使用已有版本。按最新范围推进一个具体可交付结果，选相关测试和必要真实模型验证；出现难题先给出证据与可选路线，避免围绕同一低频边界无限探索。避免重复已有整体调研，发现证据冲突应核查与修正；不要把现有测试通过误写成全面对标已完成。
