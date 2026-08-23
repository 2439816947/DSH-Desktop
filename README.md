# DeepSeek Harness 桌面端

DeepSeek Harness 的 Windows 桌面端，双击即用，无需预装 Node.js。
DeepSeek Harness 的 Windows 桌面端网站访问即可查看详情：https://dsh-desktop.cn/

## 特性

- **中文界面**：权限选择、计划模式、命令菜单、推理等级中文化；思考均为简体中文
- **余额悬浮卡**：对话区实时展示 DeepSeek 账户余额
- **霓虹视觉**：蓝紫粉渐变鲸鱼品牌，覆盖窗口、侧边栏、关于页
- **应用内反馈**：设置 → 关于 → 反馈，邮件直发
- **本地优先**：数据存于本机 `%APPDATA%\dsh-desktop-custom\`，只监听 `127.0.0.1` 随机端口
- **手机连接**：双端对话可互通，离开电脑手机发送工作。
## 安装

1. 从 [GitHub Releases](https://github.com/2439816947/DSH-Desktop/releases) 下载 `DeepSeek-Harness-setup.exe`
2. 运行安装（Windows SmartScreen 提示时选择"仍要运行"）
3. 首次打开后，在设置中配置模型提供方与 API Key，即可开始对话

## 构建

```sh
npm install
npx electron-builder --win --x64 --config electron-builder.yml
```

## 许可

外壳代码 MIT；DeepSeek Harness 本体 MIT。
