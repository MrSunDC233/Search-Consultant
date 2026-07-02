🛠️ Search Consultant (SC) 表现层与前端交互规格说明书 (PRD)

本技术文档定义了 Search Consultant (SC) 浏览器插件的前端架构、视觉规范、动效逻辑及用户交互闭环。旨在作为本地 Coder 大模型的最高开发纲领，实现低算力、高带宽、绝对防误触的极客式研究工具。

🏛️ 一、 三维页面格局与生命周期

插件放弃传统多页跳转的臃肿设计，采用单页内聚与全屏看板相结合的架构：

+-------------------------------------------------------------+
|                      Browser Window                         |
|                                                             |
|   [Popup Icon]                                              |
|         |                                                   |
|         v (Click)                                           |
|   +-----------+                                             |
|   | popup.html| -----> [Options Page] (options.html)        |
|   +-----------+                                             |
|         |                                                   |
|         +------------> [History Page] (history.html)        |
|                                                             |
+-------------------------------------------------------------+


Popup (popup.html / 日常操作窗)： * 性质： 用户点击右上角图标弹出的悬浮微型窗口，具有“阅后即焚”特性。

核心任务： 负责高频盲打、快速提炼意图、多渠道一键分发。

Options (options.html / 全局设置页)： * 性质： 独立的 Chrome 全屏配置页面。

核心任务： 托管用户自带密钥 (BYOK)、维护副提示词词库及自定义搜索引擎规则。

History (history.html / 历史轨迹看板)： * 性质： 基于 IndexedDB 支撑的独立 Chrome 全屏数据页。

核心任务： 完整留存用户的思维研究路径，提供四维一体的全文本过滤检索，支持原地二次长按发射。

🎨 二、 视觉风格指南 (Visual & Neo-Brutalist Style)

整体视觉风格融合 Neo-Brutalisim（新粗犷主义）与 Cyberpunk 极简主义，强调界面的物理确定感与极致的高反差。

1. 骨架：色块独立分流 (Block Segmentation)

设计规范： 拒绝使用细线、虚线或渐变色进行大面积区域分割。每个独立的逻辑内容块（如 Popup 的输入卡片、Options 的配置分组、History 的每张卡片）必须使用独立的纯色色块背景（例如在全局背景色 #F3F4F6 上，使用纯白色的卡片色块）区分，形成清晰的“岛屿式”布局。

全局主题色： 绿色（如 #00CC66 极客绿）。该颜色将主要用于输入框聚焦边框、下拉框高亮、以及长按进度条的高浓度推进色。

2. 灵魂：暗黑轮廓线与磨砂玻璃键 (The Frosted Key)

页面中所有的交互按钮、选择框必须严格遵循以下 CSS 物理质感：

常规状态： * 具备轻微的磨砂玻璃质感：backdrop-filter: blur(8px); background: rgba(255, 255, 255, 0.6);。

形状为圆角长方形（border-radius: 6px;）。

按钮四周必须用一圈 1.5px 的纯黑色（#000000）细实线边框 牢牢锁死。

悬浮状态 (Hover)：

当光标悬停在按钮上时，那一圈纯黑色边框啪地瞬时（或在 50ms 内）切换为纯白色（#FFFFFF）。

按钮内磨砂玻璃的不透明度可以微调（如不透明度略微加深或稍微变暗），利用极高的黑白高反差产生强烈的“物理点亮”聚焦感。

🎬 三、 克制与不突兀的动效规范 (Clips & Motion Guide)

拒绝一切华丽、无意义的缩放、旋转和飘动，动效时长严格锁定在 150ms 到 250ms 之间，主打干脆落地的物理确定感。

自定义警告弹窗 $\rightarrow$ 原地淡入淡出 (Fade In / Out)：

时长： 200ms linear。

逻辑： 弹窗与背后的磨罩在原地进行 opacity 从 0 到 1 的渐变，严禁加入任何缩放（Scale）动画，避免让用户产生视觉眩晕。

折叠内容展开 $\rightarrow$ 局部高度过渡 (Height Transition)：

时长： 250ms ease-in-out（Sigmoid 缓动曲线）或 linear。

逻辑： 历史记录卡片点击展开、或者设置项折叠栏展开时，高度平滑撑开，赋予页面物理质量感。

长按进度条 $\rightarrow$ 匀速高浓度流淌 (Linear Progress Flood)：

时长： 等同于长按触发时间（1秒或2秒），必须为 linear（绝对匀速）。

颜色： 使用比按钮本体更不透明、颜色更浓郁的主题绿色，从左至右灌满按钮。

🔄 四、 通用全局长按引擎规范 (The Long Press Engine)

为了彻底解决浏览器插件 Popup 容易因为误触空白处导致丢失上下文的行业痛点，SC 引入通用的长按状态机。按钮内部需包含一个 .progress-bar 节点。

1. 触发时间分级

日常分发级（Submit、Stop Thinking、路由方案按钮）： 默认绑定 长按 1 秒 触发。

全局控制与危险级（Save、Undo、ResetAll、清空历史）： 默认绑定 长按 2 秒 触发。

注：触发时间均可在 Options 页面由用户自定义调节。

2. 引擎核心逻辑（JS 实现伪代码）

function bindLongPress(btnElement, duration, onSuccess) {
  let timer = null;
  const progress = btnElement.querySelector('.progress-bar');
  
  const startPress = () => {
    if (btnElement.disabled) return;
    progress.style.transition = `width ${duration}ms linear`;
    progress.style.width = '100%';
    timer = setTimeout(() => {
      resetPress();
      onSuccess();
    }, duration);
  };
  
  const resetPress = () => {
    clearTimeout(timer);
    progress.style.transition = 'none';
    progress.style.width = '0%';
  };
  
  btnElement.addEventListener('mousedown', startPress);
  btnElement.addEventListener('mouseup', resetPress);
  btnElement.addEventListener('mouseleave', resetPress); // 鼠标移出按钮立刻判定为取消
}


3. 动态效率降级

绝对例外规则： 当用户在分发路由中将目标切换为 “复制到剪贴板” 时，由于该操作不具备网页跳转和破坏性，且高频使用追求极致效率，下方的方案按钮自动触发降级机制：由长按触发自动转变为“普通单击触发”，无需长按。

📱 五、 页面详细交互逻辑与 DOM 行为

1. Popup (日常发射台)

主输入框： 原生 <textarea>，高带宽文本输入，支持自动换行，带提示语。

副输入框（任务定性器 topic）： * 点按即全选（核心提效）： 当用户点击（Focus）该框时，内部填写的提示词字符串默认自动全选。用户无需按删除键，直接键盘键入即可无缝覆盖。

极客式原生撤回： 用户长按 Submit 提交后，主输入框内容在前端不主动清空。若意图模糊需要调整，提醒用户直接使用系统原生 Ctrl + Z（Mac 上 Cmd + Z） 即可瞬间恢复上次发送的长文本。

反问介入机制 (状态 B 高亮)： * 如果 T-A（Task-Agent）返回了包含 [反问开始]...[反问结束] 的模糊意图警告，SC 必须在方案区正上方激活 #clarification-zone。

视觉： 该区域必须呈现为 黄色高亮背景（如 #FFF3CD），左侧带 4px 深黄警示条（如 #FFC107），前缀加粗显示“疑问：”，后接 T-A 的简短反问。

联动： 该区域亮起的同时，光标自动重新聚焦回主输入框，方便用户直接通过 Ctrl + Z 撤回修改。

智能三语复合分发框： Popup 下方原本的下拉栏升级为支持文本键入过滤的复合组件。必须支持中文、英文、全拼（jiantieban）、简拼（jtb）三语模糊检索。用户盲打输入 bd 即可瞬间定位到“百度”，极速切换搜索路由。

2. Options (全局设置全屏页)

左侧固定目录导航： position: fixed; 锁死在左侧 1/5 空间，点击项右侧平滑滚动，滚动时目录项自动高亮对应当前区块。

右下角悬浮控制按钮组： position: fixed; bottom: 20px; right: 20px; 锁死三个圆角长方形长按键：

Save（绿色，长按 2 秒）：持久化写入 chrome.storage。

Undo（灰色，长按 2 秒）：放弃本次改动，重读旧数据刷新。

ResetAll（红色，长按 2 秒）：触发自定义警告弹窗。

副提示词库管理（折叠面板）： 默认折叠。展开后可对联想词库进行增删查改。内置开关：“新内容自动录入（每次长按提交的新短语自动追加存入本地库）” vs “纯净手动添加”。

自定义搜索方案导入： 提供规则输入框。支持用户使用伪标签格式导入（如 [title]百度[/title][url]https://www.baidu.com/s?wd=(keywords)[/url]）。SC 必须在用户长按发射时，自动将多组关键词用空格（%20）连接并无缝替换 (keywords) 占位符。

网络与大模型配置区： * 托管 API Base URL、密码隐藏式 API Key（配有眼睛图标 👁️ 切换显隐）。

测试连接（Ping 引擎）： 点击后发送最轻量握手请求。下方动态反馈绿色成功提示（附延迟毫秒数）或红色报错信息（区分网络超时或 Key 401 错误）。

高级自定义风格化警告弹窗 (Custom Modal)：

严禁调用 Edge/Chrome 原生巨丑的 confirm() 弹窗。

激活时，全屏启用高级磨砂玻璃滤镜（backdrop-filter: blur(5px); background: rgba(0,0,0,0.4);）整体变暗。

中央弹出高颜值圆角警告卡片。卡片内的“确定重置”按钮依然必须强制长按 2 秒，将误触清空概率降到绝对零度。

3. History (历史轨迹看板)

底层持久化： 全面弃用容量受限的 storage，采用 IndexedDB 存储海量思维轨迹。每次长按提交成功并获得反馈，即自动记录一次。

单页手风琴流（Inline Accordion）： 插件严禁开辟大量二级详情页。全部卡片内聚在单页列表中。

折叠状态（默认）： 左侧展示 T-A 自动提炼并输出在 [subject]...[/subject] 中的简短搜索主题（作为卡片大标题）；中间露出最核心的第一套 [keywords1] 关键词组，方便用户盲检“大概是怎么个事”；右侧带精确时间戳与长按单条删除 🗑️ 键。

展开状态： 点击卡片在原地平滑向下撑开，完全暴露出：① 用户的原始问题长文本、② 副框当时的 topic 偏好、③ T-A 完整的方案按钮组及黄色反问区。

原地二次发射： 展开后的多套方案按钮依然完全内置分发复合框与长按引擎。用户在历史记录页可以像在 Popup 里一样，直接长按把过去的思路重新发射到任意对应的搜索引擎。

四维一体超级检索： 顶部的超级搜索栏必须穿透扫描 IndexedDB 内的四个核心维度：① 提炼的标题 (subject)、② 用户的原始长文本、③ 副输入框的偏好 (topic)、④ 包含的所有备选关键词组。全字段模糊命中任意一项，该历史卡片便会瞬间过滤保留。

🏷️ 六、 数据流与术语规范锁定 (Terminology Rules)

为防止逻辑层与表现层代码混乱，全局变量与正则提取严格遵循以下命名法：

topic（用户任务定性）： 专指用户在副输入框中输入的文本或选择的降噪倾向（前端输入/设置库管理）。

subject（AI 搜索主题）： 专指 T-A 根据全文自动提炼包裹在 [subject]xxx[/subject] 标签中的简短全局主题。作为历史记录卡片的标题，同时在 Popup 方案区顶部小字高亮反馈给用户。