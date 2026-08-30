# Dsh 桌面版 · 版本记录（CHANGELOG）

> 本文件是**版本记忆**：任何变更都必须按规则实时更新版本号，并在此记录。

## 版本号规则（强制）

| 场景 | 规则 | 说明 |
| --- | --- | --- |
| **上架（发布 GitHub Release）** | 单独说明版本号 | 上架时另行说明采用的版本（此时版本更新按此前规则：大型 +0.1 / 小型 +0.01） |
| **未上架（未单独说明）的普通更新** | 补丁 +0.001 | 统一递增，**包含**此前规定的 +0.01（小型修复）与 +0.1（大型更新）两种粒度 |

- **实时更新**：每次变更后立即同步 `resources/app/package.json` 的 `version` 与"关于"页显示的版本号
- **发布方式（自 0.5.6 起）**：每次更新进仓库时，GitHub Releases **「Draft a new release」新建一个 Release（新 tag `vX.Y.Z`）**，body 简述此次更新，**保留以前版本**（不再覆盖同一 tag）；`dsh-desktop` tag 保留为**自动更新最新版通道**（资产随每次发布更新）
- **云服务器同步（自 0.5.8 起）**：每次发布（上传安装包）后，**同步 `DeepSeek-Harness-setup.exe` + `latest.yml` 到云服务器**（`dsh-desktop.cn/downloads/`，脚本 `packaging/sync-to-server.ps1`）——宣传页下载与自动更新主源走云服务器加速（GitHub 镜像为 fallback）
- **展示网页同步**（自 0.5.4 起）：每次执行版本更新时，`website/index.html`（及仓库预览页）的**版本徽章 / 下载信息同步更新**（不计入桌面版版本号、不 bump 独立版本）
- **上传仓库同步宣传网页**（自 0.5.6 起强化）：**每次上传 GitHub 仓库（发布 Release / 推送含版本变化的提交）时，必须同步更新宣传网页 `website/index.html` 的相关信息**（版本徽章、下载链接、发布说明、特性描述等），随仓库一起推送
- **宣传网站独立管理**（自 0.5.3 起）：`website/` 宣传页的更新**不计入桌面版版本号**，不 bump 版本、不记录本文件；桌面版版本号只随应用本身变更
- **社区版（DSH Desktop 官方版）**版本号独立管理，不随"我们的版本"规则变动

### 产品称呼约定（自 0.5.4 起）

- **当前环境（原"官方版"DSH Desktop 0.4.x）** → 称 **社区版**
- **复刻版** → 称 **我们的版本**（可随意更换称呼）
- 社区版 = DeepSeek 官方发布的桌面版（dataelement/dsh-desktop，GitHub Releases `v0.4.x`）；我们的版本 = 基于其定制（中文原生 / 霓虹视觉 / 悬浮卡 / 反馈直发 / 自动更新检测）

---

## 0.5.11（✅ 已上架 · 2026-08-30 发布 GitHub Release）

- **关于页品牌名统一**：「Dsh 桌面版」→「**Dsh 桌面端**」（关于页品牌名 + 反馈默认主题 + 主进程"关于"对话框/菜单/aboutDetail 文案；`app.setName`/窗口名等非关于内容保留不变）
- **关于页新增「官方 Harness 最新版」显示**：打开关于页即显示 `官方 Harness 最新：vX（本地 vY）`（启动后台查询 npm dsh 最新版 + 国内镜像 fallback，纯信息提示，不自动升级）；「检查更新」也会刷新该值
- **修复「检查更新」后下方闪烁**：官方版本行改为**稳定显示**（打开即有、reset 后保留），不再因检查过程出现/消失导致布局跳动
- **关于页温馨提示**：新增"该桌面端意在稳定，与官方 Harness 版本有半个版本时间的差距，若有新版，请耐心等待"
- **版本同步**：`package.json` / `settings-general` fallback / `CHANGELOG` / 宣传页徽章 → v0.5.11

## 0.5.10（已上架 · 发布 GitHub Release）

- **新增关闭确认 + 后台常驻（系统托盘）**：点窗口右上角 X 弹出确认框（**最小化到后台 / 退出程序**），避免误触直接退出；最小化后应用在系统托盘**持续运行**，点击托盘图标可恢复窗口，右键菜单支持「显示主窗口 / 退出」；`window-all-closed` 由「直接退出」改为**后台常驻**（无托盘时保持原退出行为，避免无入口）
- **关闭确认弹窗主题联动**：确认框改为随桌面主题（读取 `--dsw-*` 设计令牌，含社区主题插件）的**透明悬浮卡片**，主按钮为**霓虹渐变**、按钮自定义风格化；亮/暗主题随 app
- **连接手机（移动配对）窗口优化**：配对窗口**主题化**（随 `--dsw-*`）、控件**霓虹化**、内容**垂直居中**（消除底部空白）、顶部改用**主题化 Window Overlay**（消除黑色原生标题条）、霓虹控件边缘**抗锯齿优化**（1px 内描边高光）
- **手机端 Dsh 本地投射（renderMobilePage）**：手机主页**随桌面主题**（`--dsw-*` 令牌）、主控件**霓虹化**
- **版本同步**：`package.json` / `settings-general` fallback / `CHANGELOG` / 宣传页徽章 → v0.5.10

## 0.5.9（✅ 已上架 · 2026-08-22 发布 GitHub Release）

- **余额悬浮卡支持对接任意 OpenAI 兼容网关（含 ChiApi）**（原"未上架（进行中）"转正）：`llm.balance` 从"仅 DeepSeek"改为**解析当前默认 provider**——读 `agent-default-model` 取提供方 id，经 `ctx.llm.listConfigurableProviders()` 找到其 `settingsNs`+`settingsPath`，用 `settings.describe({redactSecrets:false})` 读该分节的 `baseURL`+`apiKeyEnv`（非密），再 `credentials.resolve(apiKeyEnv)` 取真实 key，查 `{baseURL}/v1/dashboard/billing/subscription`+`/usage`，余额=hard_limit-total_usage（USD 展示，÷500000=QuotaPerUnit）；DeepSeek 或解析失败走 `api.deepseek.com/user/balance` 回退；secret 仍只经 credentials 单向解析
- **修复 ChiApi baseURL 带 /v1 时 404**：baseURL 以 `/v1` 结尾时归一化为根再拼 `/v1/dashboard/billing/...`（双 /v1 → 404 → 已修，实测 USD 198.3576）
- **修复切回官方模型报错**：DeepSeek 官方 provider id 为 `deepseek-official`（原回退判断只认 `deepseek`）→ 切回官方后错误地走 ChiApi 分支报"未配置 baseURL/API Key"→ 回退判断加入 `deepseek-official`（实测 CNY 79.52）
- 两处修复均同步 `custom-patches.mjs`（升级重放一致）

## 0.5.8（✅ 已上架 · 2026-08-22 发布 GitHub Release）

- **修复反馈 SMTP 永久"发送中"**：另一台设备网络不通时，反馈发送无超时导致永久挂起 → 主进程 `desktop:send-feedback` 增加 SMTP 超时（`connectionTimeout`/`greetingTimeout`/`socketTimeout` + `sendMail` timeout，15-30s）——网络正常照常秒发；网络不通时超时报"发送失败"而非永久"发送中"；需确保设备能访问 `smtp.qq.com:465`（防火墙/运营商放行）

## 0.5.8（✅ 已上架 · 2026-08-22 发布 GitHub Release）

- **国内网络加速（解决访问 GitHub 慢/断流，第二台电脑实测）**：
  - **更新检查多源 fallback**：主源（GitHub 直连）失败自动切换国内加速镜像（`ghproxy.net` / `gh-proxy.com`）重试——`update-source.json` 的 `mirrors` 数组可自行增删
  - **更新卡片说明**（`fetchOwnReleaseNotes`）同样加镜像 fallback（api.github.com 直连慢时走镜像）
  - **插件市场（dshmarket）npm 加速**：harness 启动注入 `npm_config_registry=https://registry.npmmirror.com`（pnpm 安装走国内镜像；极新包镜像暂缺时用户可临时移除该环境变量）
  - **安装包映射云服务器（已撤回）**：曾把安装包同步到 `dsh-desktop.cn/downloads/`，实测**服务器带宽是瓶颈**（腾讯云轻量约 3Mbps，CF 隧道 0.28MB/s / IP 直连 0.36MB/s）→ **已撤回**，下载仍走 **GitHub 直链（实测 3.2MB/s，最快）**；`update-source.json` 主源回退 GitHub、mirrors 保留作 fallback；服务器 downloads 目录已清理
- **发布方式**：新建 Release `v0.5.8`（tag `v0.5.8`，body 简述）+ 更新 `dsh-desktop` 自动更新通道

## 0.5.7（✅ 已上架 · 2026-08-22 发布 GitHub Release）

- **新增内置预设「超强思考」**（id `super-thinking`）：深度思考增强模式——慢思考、多步推理、先理解再动手、穷举方案与权衡、自我校验、任务锚点收敛，适合复杂推理与分析任务；能力面与 standard 一致（完整工具/sections）；参考 dsh-routing-suite 的深度思考思路（不引入第三方注入器，纯内置预设）；升级自动重放（custom-assets 固化）
- **修复现有预设中文规则重复 bug**：`standard`/`code`/`cordis` 三个预设 persona 里中文硬规则被历史叠加重复 10 次 → 统一为 1 次（`minimal` 本就 1 次）
- **发布方式**（新约定）：新建 Release `v0.5.7`（tag `v0.5.7`，body 简述本次更新）保留历史版本；`dsh-desktop` tag 作为自动更新最新版通道同步更新资产

## 0.5.6（✅ 已上架 · 2026-08-22 发布 GitHub Release）

- **上架发布**：GitHub Release tag `dsh-desktop` 更新为 v0.5.6，资产 `DeepSeek-Harness-setup.exe`（0.5.6，152 MB）+ **`latest.yml`（本次补齐，自动更新生效）**；Release body 已写明版本与特性；宣传页已同步 v0.5.6（并修复此前同步造成的编码乱码）
- **dsh 依赖树升级至官方最新 0.1.1-rc.2**（`deepseek-ai/deepseek-harness` 的 `dsh-v0.1.1-rc.2`）：全部 197 个 `@deepseek-ai/*` 包从 0.1.0-rc.8 升级（脚本 `scripts/upgrade-dsh.mjs`，npm 全树下载 + 备份回滚 + 补丁重放）
- **余额查询补丁（rc.2 适配）**：官方 rc.2 无 `llm.balance` → 补丁为 host-apiproxy 加回 balance handler（5 处：handler/端点表/值表/schema/client）+ client-connection 加回 `llmBalanceValueSchema`（2 处）——已实测余额正常（CNY 25.95）
- **presets 保留 + 中文规则**：rc.2 npm 含 `config/agent-presets`；中文思考规则重放至 identity/4 预设/部署 persona；**修复 minimal 补丁重复 `complete: true`**（rc.2 原版已含该键）
- **更新机制重构（最终形态，替换旧"自动更新"）**：
  - **切断旧更新源**：`app-update.yml`（electron-builder `publish` 生成）指向**我们构建的版本**的 GitHub Release（`generic` + `releases/download/dsh-desktop/`）；`checkForUpdates` 只走 electron-updater，**不再检测** `@deepseek-ai/dsh` npm 与社区版（dataelement/dsh-desktop）——旧源彻底断开
  - **自动更新关闭**：启动/定时/系统恢复**不再自动检测**，完全由「设置 → 关于 → 检查更新」按钮手动触发
  - **更新卡片**：检测到新版时关于页显示卡片（**版本号 + Release 简化说明 + 下载进度条**），点击「更新」按钮后自主下载（`updates:download` IPC → `autoUpdater.downloadUpdate()`），下载完成提示"重启应用后自动安装"（`autoInstallOnAppQuit`）
  - **发布约定**：每次上架，GitHub Release **新建版本变更**，body 用**简化版当前版本更新信息**（已重写 v0.5.6 body）
- **修复余额悬浮卡未生效**：补丁漏了 client-connection 的 `api.llm.balance` 客户端方法（只有 schema/端点表）→ 浮卡 `TypeError: api.llm.balance is not a function` → 已补（第 4 处连接补丁）并固化，实测余额 CNY 52.53 正常显示
- **展示网页同步**：`website/index.html` 版本标记 v0.5.3 → v0.5.6
- **UI 大块定制全部重放（升级副作用已消除）**：官方 rc.2 覆盖的定制已逐一移植回并**固化进补丁体系**（`scripts/custom-assets/patches.json`，8 个包整文件替换：old=官方 rc.2 原版全文 → new=定制版）——`settings-general`（关于页/反馈）、sidebar（霓虹品牌，顺应 rc.2 的 `sidebar.brand.*` slot 机制）、plan（常驻开关）、commands（命令中文）、model-selection（推理中文）、settings-plugins（客制插件页）、brand-official（见下）、permission-presets（见下）
- **官方品牌插件霓虹化（rc.2 新机制覆盖修复）**：rc.2 新增 `dsh-client-ui-brand-official` 包，把 `sidebar.brand.mark` / `sidebar.brand.name` / `conversation.hero.brand.mark` 三个 slot 注入官方灰色品牌，覆盖了我们所有霓虹定制 → 重写该包注册**霓虹品牌**：左上角 logo（霓虹鲸鱼 PNG，light/dark 双主题）、品牌名（`DeepSeek Harness` 霓虹渐变字，`includeMark:false` + 渐变 fill 覆盖）、hero "The Future" 前的鱼（霓虹渐变 fill）——三个位置全部恢复霓虹
- **品牌 PNG 资源固化**：`dsh-desktop-logo-{light,dark}.png` + `logo.png` 存入 `scripts/custom-assets/`，`custom-patches.mjs` 升级重放时自动复制回 `dsh-web-frontend/dist`（官方覆盖删除后不再丢失）
- **修复与社区版端口冲突导致启动失败**：复刻版与社区版（DSH Desktop 0.4.3）移动设备桥同用固定端口 43127 → 复刻版改为 **43129**（`out/main/index.js`，社区版不受影响、无需重启）
- **恢复左下角"连接手机"按钮**：rc.2 官方删除了 sidebar 的 `data-dsh-sidebar-root` / `data-dsh-sidebar-wide` / `data-dsh-sidebar-settings` 三个 data 属性，导致 preload（`out/preload/index.cjs`）的 `mountMobileButton()` 找不到挂载点（`[data-dsh-sidebar-settings]`）→ 按钮不渲染；已在 sidebar 定制版加回三处属性（按钮 label "连接手机"/"管理手机连接"，走 `mobile:open-pairing` IPC）
- **修复新电脑首次启动崩溃（`Harness stopped unexpectedly ... plugin tree failed to load: cordis:include`）**：根因 = app 层插件包（`dsh-float-card`、`dsh-desktop-market-installer`）不在 dsh 依赖闭包，`healProfilesModuleFallback` 不为它们在 `$DSH_HOME/profiles/node_modules` 建 junction → 全新电脑（无旧 profile 数据）boot 时 bare 包解析失败。修复：`@deepseek-ai/dsh/package.json` 的 `dependencies` 加入 `dsh-float-card@0.1.0` + `dsh-desktop-market-installer@0.1.0`（heal 闭包随之包含 → junction 建立）；已固化进 `custom-patches.mjs`，fresh 环境验证 BOOT OK；**已重新打包并覆盖发布**
- **修复 package.json description 乱码**（此前写入时被错误编码为 `妗岄潰鐗?`）→ 恢复 `A cross-platform desktop shell for Dsh 桌面版`
- **反馈收件人隐藏邮箱**：收件人字段展示 **"Chi_Yu 池鱼."**（SMTP 发送目标不变，仍发往原邮箱）
- **权限系统全中文（rc.2 回归修复）**：rc.2 把权限预设标签改回英文 title case（`Read Only / Workspace Write / Full access`）→ 恢复字典化映射（`仅可查看 / 可写入工作区 / 完全权限`）：
  - `dsh-client-ui-permission-presets`：设置里权限行（"权限"/"选择新会话的默认权限模式"）+ 选项菜单 + Full access 确认对话框全中文
  - `dsh-client-ui-conversation`：composer 权限选择器（模式行显示"可写入工作区"）+ 确认对话框（"确认启用完全权限？/我已了解风险，并愿意继续/取消/启用完全权限"）
  - host 侧 `dsh-permission-presets`：预设描述、`/permission` 命令描述与输出、custom 状态描述中文
  - `dsh-client-connection`：fixture 预设表描述中文
- **下次"官方依赖树更新"= 一键流程**：`scripts/upgrade-dsh.mjs`（npm 全树升级 + 备份回滚）→ 自动跑 `custom-patches.mjs`（字符串补丁：中文思考/hero/balance/presets/权限描述 + patches.json 整文件替换：8 包 UI 定制 + 品牌 PNG 资源复制）

## 0.5.5（未上架 · 普通更新）

- **反馈收件人展示隐藏邮箱**：反馈浮窗"收件人"字段不再显示 `1096963392@qq.com`，改为展示 **"Chi_Yu 池鱼."**；SMTP 发送目标不变（仍发往原邮箱，仅 UI 隐藏）

## 0.5.4（未上架 · 普通更新）

- **自动更新检测（检测 DeepSeek 官方版本，有新版自动安装）**：
  - 主进程 `startUpdateManager` 始终启用（不再受 `update-source.json` 门控）：启动 15s 后 + 每 6 小时 + 系统恢复时自动检测
  - **主检测**：社区版（DeepSeek 官方桌面版 dataelement/dsh-desktop）GitHub `releases/latest`（tag `v0.4.x`，对应内置 dsh 版本）；**顺带检测** `@deepseek-ai/dsh` npm 官方最新版
  - **有新版自动安装**：下载官方 Windows 安装包 → 静默解压到临时目录 → 提取 `@deepseek-ai` 依赖树（含 `dsh/config/agent-presets`，npm 包不含 presets，故必须从官方安装包提取）替换本地 → **重放定制补丁**（`scripts/custom-patches.mjs`：中文思考 identity/presets、部署层 persona、hero 霓虹、host-apiproxy 仅适配DeepSeek）→ 备份回滚（失败恢复 `.bak-upgrade`）→ 自动重启 harness
  - 已升级状态记录于 `%APPDATA%\dsh-desktop-custom\dsh-upgrade-state.json`（communityTag），避免重复安装
  - 关于页检查更新文案同步（"已是最新版本（dsh x）" / "发现新版 {v}，正在自动安装"）
- **产品称呼约定**：当前环境（原"官方版"）称 **社区版**；复刻版称 **我们的版本**（见顶部约定）
- 说明：UI 大块定制（关于页/反馈/品牌/客制插件页等）的自动重放补丁暂未全覆盖，升级后如个别定制缺失请手动重放或告知补全

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
