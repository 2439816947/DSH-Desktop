# Dsh 桌面版 · 版本记录（CHANGELOG）

> 本文件是**版本记忆**：任何变更都必须按规则实时更新版本号，并在此记录。

## 版本号规则（强制）

| 场景 | 规则 | 说明 |
| --- | --- | --- |
| **上架（发布 GitHub Release）** | 单独说明版本号 | 上架时另行说明采用的版本（此时版本更新按此前规则：大型 +0.1 / 小型 +0.01） |
| **未上架（未单独说明）的普通更新** | 补丁 +0.001 | 统一递增，**包含**此前规定的 +0.01（小型修复）与 +0.1（大型更新）两种粒度 |

- **实时更新**：每次变更后立即同步 `resources/app/package.json` 的 `version` 与"关于"页显示的版本号
- **宣传网站独立管理**（自 0.5.3 起）：`website/` 宣传页的更新**不计入桌面版版本号**，不 bump 版本、不记录本文件；桌面版版本号只随应用本身变更
- 官方版（`DSH Desktop 0.4.x`）版本号独立管理，不随复刻版规则变动

---

## 0.5.3（小型修复）

- **修复极简模式（minimal preset）加载失败**：0.2.9 加中文思考规则时，minimal 的 persona 为**单行 YAML 标量**，规则文本中的 `Hard rule: `（冒号+空格）被 YAML 解析为 mapping 分隔符 → `bad indentation of a mapping entry (12:67)`，roster 标记 `broken`；改为 `>-` 块标量（与 standard/code 一致，块内冒号不特殊）后恢复
- 验证：`agentPreset.list` 显示全部 5 个 preset（standard/code/minimal/cordis/liangshen）均正常

## 0.5.2（小型修复）

- **修正 exe 运行时图标**：
  - 主进程新增 `app.setAppUserModelId("com.dsh.desktop.custom")`（开发版 `…dev`），任务栏图标稳定绑定 exe 霓虹资源，不再漂移
  - 重新生成 `Dsh-桌面版.ico`：从干净的 `resources/icon.png` 生成 7 帧（16/24/32/48/64/128/256px）PNG 帧霓虹 ICO（旧版经 `GetHicon()` 生成存在绿色伪影；新版逐帧验证 0 绿色）
  - 执行 Windows 图标缓存刷新（`ie4uinit.exe -show`）；若任务栏仍显示旧图标，取消固定后重新固定即可

### 打包发布（0.5.2 安装包）

- **NSIS 安装包产出**：`packaging/release/dsh-desktop-custom-0.5.2-windows-x64-setup.exe`（electron-builder 26.15.3，electron 43.4.0，`asar:false`/`npmRebuild:false` 与官方一致；NSIS 向导可改安装目录/桌面+开始菜单快捷方式；extraResources 含 icon/splash/plugin-recovery/patch/update-source/elevate/cloudflared）
- **float-card 并入 app 层**（保证新装用户有悬浮卡）：包移入 `resources/app/node_modules/dsh-float-card`，`resources/dsh-desktop.patch.yml` 增加 `- id: float-card` insert；profile 层 `cordis.patch.yml` 移除重复 insert（避免 duplicate loader entry）；`package.json` `dependencies` 声明 `dsh-float-card@0.1.0` 与 `nodemailer@^9.0.5`（electron-builder 仅保留声明的 node_modules 包，未声明会被过滤导致安装包缺悬浮卡/反馈 SMTP）
- **纯净性**：安装包无真实 DeepSeek API key（凭据存 `%APPDATA%`，安装后自行配置）、无用户数据（profile/sessions 均在 `%APPDATA%`）、无路径/用户名泄漏（扫描 `Dsh Workspace`/`24398` 0 命中）；**SMTP 授权码与反馈收件人按用户确认保留**（移除会使反馈失效）
- **代理**：Clash（clash-verge，127.0.0.1:7897）用于 electron-builder 下载 electron/NSIS 与后续 GitHub 发布

## 0.5.1（小型修改）

- **反馈浮窗新增"联系方式"输入框**（位于内容下方，选填）：发送时邮件正文末尾附 `联系方式：xxx`，便于收件人联系反馈者；字典 `about.feedbackContact` / `about.feedbackContactPlaceholder` 中英同步

## 0.5.0（大型更新）

- **新增宣传与下载网页 `website/index.html`**：单文件自包含（内联 CSS/JS/霓虹鲸鱼 SVG，无外部依赖），深色主题 + 蓝紫粉霓虹渐变，与复刻版品牌视觉一致
  - Hero（发光鲸鱼 logo、产品名、标语、版本徽章）+ 8 张特性卡片（中文原生/计划模式开关/余额悬浮卡/中文思考/关于页/客制插件/霓虹视觉/本地持久化）
  - 下载区：Windows 版下载按钮，地址**暂用占位**（`#download` + JS 拦截提示"下载即将上线"；`DOWNLOAD_URL` 常量单点替换）
  - 页脚含免责声明（非 DeepSeek 官方产品）；响应式布局（≤720px 单列）
- 版本号同步至 0.5.0（package.json / 关于页 fallback / 宣传页徽章）

## 0.4.1（小型修改）

- **反馈发送改为 SMTP 应用内直发**：`nodemailer@9.0.5`（装入复刻版 `node_modules`）经 `smtp.qq.com:465`（SSL）直发到 `1096963392@qq.com`（QQ 邮箱 SMTP 授权码），不再依赖系统邮件客户端；发送中提示"正在发送…"，失败提示重试；`mailto` 保留为无 SMTP 能力时的兜底
- 新增 `desktop:send-feedback` IPC + preload `sendFeedback`；已实测 SMTP 链路（测试邮件发送成功）

## 0.4.0（大型更新）

- **设置"关于"页新增"反馈"控件**（样式与"检查更新"统一）：点击弹出反馈浮窗，以邮件形式填写（收件人固定 `1096963392@qq.com`、主题、内容），点"发送"通过系统邮件客户端发送（新增 `desktop:open-external` IPC → `shell.openExternal(mailto:)`，preload 暴露 `dshDesktop.openExternal`）；空内容拦截提示，发送失败提示
- 中英文文案字典同步（`about.feedback*` 系列）

## 0.3.1（小型修改）

- **悬浮卡标题改为"模型品牌 + 余额"**：如 `DeepSeek余额`（原为 `DeepSeek用量`）；无模型或空间不足时显示 `余额`

## 0.3.0（大型更新）

- **悬浮卡与输入框完全对齐**：卡片顶部与输入框顶部对齐、底部与输入框底部对齐（同高，内容垂直居中），卡片内字体相应缩小（金额 20→15px、标题 13→12px、其余 12→11px 等）
- **新增"余额 ↗"入口**：点击在系统浏览器打开 DeepSeek 平台余额/充值页 `platform.deepseek.com/top_up`（复用主进程 `setWindowOpenHandler` → `shell.openExternal`）
- **累计消费调研结论**：`api.deepseek.com` 无用量接口；平台私有端点 `platform.deepseek.com/api/v0/users/get_user_summary` 可返回累计消费（需 Edge 登录态 userToken，实测可提取），但涉及读取浏览器凭据，**确认不实现自动读取**，保留手动入口
- **非 DeepSeek API 显示"仅适配DeepSeek"**：未配置 `DEEPSEEK_API_KEY` 时 `llm.balance` 返回 `"仅适配DeepSeek"`（不再显示"未配置 DeepSeek API Key（DEEPSEEK_API_KEY）"）
- 官方版同步：`dsh-host-apiproxy` 与悬浮卡 client bundle 均已更新，需重启官方版生效

## 0.2.9（小型修改）

- **任何情况下思考均为中文**：4 处系统提示注入"硬性规则：可见回复与全部思考/推理过程必须使用简体中文"——
  - `cordis` / `code` / `minimal` 三个 preset 的 persona（`@deepseek-ai/dsh\config\agent-presets\*\agent.cordis.yml`；`standard` 原本已有该规则）
  - 全局兜底：`dsh-system-prompt` 的 `harness:identity` 段落追加同一规则（覆盖所有非 complete 场景）
  - 对话压缩只压缩历史消息、不动系统提示，因此压缩后规则依然生效；已持久化会话重启后按新 persona 组装

## 0.2.8（小型修改）

- **左上角品牌词标复原为官方 "DeepSeek Harness" 美术字样式**（`BrandWordmark` 组件，含鲸鱼标记），同时**保留霓虹渐变**：注入 SVG `linearGradient`（蓝→紫→粉，水平）+ CSS `fill: url(#dsh-wordmark-neon)` 覆盖 `currentColor` 路径

## 0.2.7（小型修复）

- **修复官方版余额 `[{"expected":"number","code":"invalid_type"...`**：DeepSeek 余额接口返回**字符串金额**（如 `"total_balance": "56.62"`），而 value schema 要求 `number` → zod 校验失败；`balance` handler 增加 `toNumber` 转换（字符串→数值，无效则省略字段），两个环境均已修复

## 0.2.6（小型修复）

- **修复余额卡 `Cannot read properties of undefined (reading 'parse')`（真正根因）**：`dsh-client-connection` 包的 `UNARY_VALUE_SCHEMAS`（客户端响应解析表）缺 `llm.balance` 项 → `undefined.parse(...)` 报错；已在两个环境补上 `llmBalanceValueSchema` 定义 + 表项
- 复刻版 + 官方版 connection bundle 均已验证

## 0.2.5（小型修复）

- **修复余额卡 `Cannot read properties of undefined`**：`state.value` 可能为 `undefined`（非 `null`），原判空用 `!== null` 漏判，改为 `!= null`（同时判空 `null`/`undefined`），覆盖响应处理、金额/货币读取、meta 渲染三处
- **悬浮卡标题只显示大模型品牌**：标题取提供方品牌名（如 `DeepSeek用量`），不再细化到模型版本（如 DeepSeek-V4-Flash）

## 0.2.4（小型修改）

- **删除右上角标题栏托盘菜单的"关于"入口**：移除 preload 菜单项与命令表、主进程 `desktop-menu:execute` 的 `case "about"`
- **设置"关于"页信息与托盘关于对照**：新增 `desktop:version-info` IPC（应用版本 + 内置 Harness 版本），设置"关于"页显示"应用版本"与"内置 Harness 版本"（与托盘关于对话框信息一致），版本号从主进程实时读取（不再硬编码）

## 0.2.3（小型修复）

- **修复余额查询 zod 校验失败（`invalid_union`）**：`llm.balance` 端点改为**始终返回 `ok:true` + `value.message`**（未配置 Key / 请求失败 / 异常都返回带 message 的 value），不再返回 `err`（客户端已知错误 schema 不含 balance 的自定义 code，导致校验失败显示 `[{"code":"invalid_union"...`）
- **修复官方版 `api.llm.balance is not a function`**：官方版 0.4.3 重构后 balance handler 被误加到 `llm` 对象外（`api.balance`），已移入 `llm` 对象内（`api.llm.balance`），并修复对象结构
- `llmBalanceValueSchema` 增加 `message` 字段；悬浮卡 BalanceCard 读取 `value.message` 显示（未配置 Key → "未配置 DeepSeek API Key（DEEPSEEK_API_KEY）"）

## 0.2.2（小型修复）

- **修复官方版 `/api/llm.balance` 500**（`api.llm.balance is not a function`）：文件层修复（balance handler / 端点表 / schema / 值表 / client 方法）已补齐，需重启官方版加载
- **错误显示加固**：悬浮卡余额错误文本截断为单行（最多 64 字符 + `…`，完整信息悬停可见），任何错误不再以"一大段"刷屏
- **host 错误消息精简**：`balance` 端点 catch 返回固定短文案"余额查询失败"，详细原因放 `details`

## 0.2.1（小型修复）

- **修复 llm.balance 端点 HTTP 404**：补齐 `dsh-host-apiproxy/lib/index.js`（打包文件）内的 balance schema / 端点表 / 值表 / client 方法 —— 之前只改了 `types/fetch/*` 源文件副本，实际运行的打包文件缺失导致 404
- 悬浮卡宽度自适应：空间不足时自动收窄（最小 140px），任何情况不覆盖输入框；随界面缩放
- 悬浮卡标题 = **模型名 + 用量**（自动检测当前默认模型）；无模型或空间不足显示 **"余额用量"**；标题不省略；留 `window.__dshFloatCardTitle` hook 供自定义
- 无 API Key 时插件**默认禁用**；手动启用但无 API 时悬浮卡显示 **"暂未接入模型/模型暂未适配"**
- 客制插件页插件开关（localStorage + 事件驱动悬浮卡显隐）

## 0.2.0（大型更新）

- 品牌化：应用名 **Dsh 桌面版**、霓虹渐变（蓝→紫→粉）鲸鱼图标（侧边栏 / 窗口 / exe / 启动画面 / 关于页）
- 设置新增 **"关于"页**：品牌信息、当前模型信息、**检查更新**（自定义更新源 hook `resources/update-source.json`）
- **权限选择中文化**：只读 / 工作区写入 / 完全权限
- **计划模式开关**：输入框工具行常驻开关（"计划 / 计划中"），样式对齐权限选择
- **"/" 命令菜单中文化**：命令描述（进入或退出计划模式、压缩较早的对话历史等）
- **对话区右侧悬浮卡**（余额展示）：`dsh-float-card` 固定插件 + `conversation.float.card` 扩展点（hook），Plugins 设置页可开关
- 插件设置新增 **"客制插件"** 页（自建插件展示，插件列表英文名不变）
- **推理等级中文化**：关闭 / 低 / 高 / 最高（跟随模型默认）
- 版本号规则建立（大型 +0.1 / 小型 +0.01），复刻版 0.1.1 → 0.2.0
- 移除官方更新源，新增自定义更新源 hook
