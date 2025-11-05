[[中文](#中文) | [English](#english)]


## 中文

> 声明：本脚本的全部代码均来自 [linux.do 的分享贴](https://linux.do/t/topic/1052708)，感谢 LeYangZi 的分享；如有侵权请立刻联系删除。

# chatgpt-easyuse-exporter
A simple yet powerful script to export your ChatGPT Team conversation history into multiple formats — including Markdown (MD), HTML, and JSON. Perfect for backing up, archiving, or analyzing your chats with flexible, structured data output.

## 使用教程（中文）

本教程适用于从未使用过用户脚本（UserScript）的用户，带你从零到一完成：安装 Tampermonkey、根据官方文档开启所需权限、导入本脚本、在 ChatGPT 页面运行导出。

> 适用站点：`https://chatgpt.com/*` 
### 一、安装 Tampermonkey（油猴）

根据你的浏览器，前往扩展商店安装 Tampermonkey：

- Chrome/Chromium/Edge: [Chrome Web Store - Tampermonkey](https://chromewebstore.google.com/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo)
- Firefox: [Firefox Add-ons - Tampermonkey](https://addons.mozilla.org/firefox/addon/tampermonkey/)
- Safari: 请在 Mac App Store 搜索 “Tampermonkey”

安装完成后，确认浏览器右上角扩展栏能看到 Tampermonkey 图标。

![截图A：浏览器扩展商店 Tampermonkey 页面/已安装状态](https://private-user-images.githubusercontent.com/102719366/510088661-db47d3e7-e09a-4c6c-90bd-52c49bc934c9.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjIzNDIwNjgsIm5iZiI6MTc2MjM0MTc2OCwicGF0aCI6Ii8xMDI3MTkzNjYvNTEwMDg4NjYxLWRiNDdkM2U3LWUwOWEtNGM2Yy05MGJkLTUyYzQ5YmM5MzRjOS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUxMTA1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MTEwNVQxMTIyNDhaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT05YzZiNGY1ZDA0MDhhY2RlMmNlYzQwZGQ1MWMwMGU4NDgyNjFjYThlNWM2MTMxOTg0ZDg4OGJiNWI3MzMzZDAzJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.lPzW7Fpc7_2qov2hqGbud3A27mNzmC4FVtMzFemehnE)

### 二、开启（或确认）Tampermonkey 权限（参考官方教程）

为确保脚本能正确在 ChatGPT 页面运行与发起网络请求，请参考 Tampermonkey 官方文档完成必要的权限配置（我们不重复撰写流程）：

- 官方文档入口（英文）：[Tampermonkey Documentation](https://www.tampermonkey.net/documentation.php)
- 常见设置/权限说明（英文）：[Tampermonkey FAQ](https://www.tampermonkey.net/faq.php)

你可在 Tampermonkey 图标 → 仪表盘（Dashboard）→ Settings（设置）中，根据官方说明调整权限与配置（如需要可将 Config Mode 设为 Advanced 以显示更多选项）。

![截图B：Tampermonkey 仪表盘 Settings 页面，突出权限/设置区域](https://private-user-images.githubusercontent.com/102719366/510089048-1f67b4d4-5ffe-4b1c-ba26-0e6466eaa3f6.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjIzNDIwNjgsIm5iZiI6MTc2MjM0MTc2OCwicGF0aCI6Ii8xMDI3MTkzNjYvNTEwMDg5MDQ4LTFmNjdiNGQ0LTVmZmUtNGIxYy1iYTI2LTBlNjQ2NmVhYTNmNi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUxMTA1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MTEwNVQxMTIyNDhaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT03Y2Q1MjI3NjAxZjNhMmFlYTM1MDkyNzBlNjEyYTMzMDAyOGEzYWFjN2M1ZWE2NGQ5M2U4YjY0NzNkNDliZGMxJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.b29xx1Y4kzk9EnD-6BF0gbx9G4B-CwFHM8gZOqRhp4g)

### 三、导入本用户脚本

有两种常见方式导入脚本，任选其一：

1) 直接创建并粘贴脚本
- 点击浏览器工具栏的 Tampermonkey 图标 → 仪表盘（Dashboard）。
- 点击 “+” 或 “Create a new script…” 新建脚本。
- 打开本仓库的 `userscript.js`，复制全部内容，粘贴到编辑器中，保存（Ctrl/Cmd + S）。

2) 从文件导入
- 打开 Tampermonkey 仪表盘（Dashboard）→ Utilities（工具）。
- 使用 “Import from file” 选择本地的 `userscript.js` 文件导入，保存并启用。

![截图C：Tampermonkey 新建脚本/导入脚本页面，展示粘贴内容或选择文件的位置](https://private-user-images.githubusercontent.com/102719366/510089516-b5c2b46d-2189-4166-a073-f6f0670a7a63.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjIzNDIwNjgsIm5iZiI6MTc2MjM0MTc2OCwicGF0aCI6Ii8xMDI3MTkzNjYvNTEwMDg5NTE2LWI1YzJiNDZkLTIxODktNDE2Ni1hMDczLWY2ZjA2NzBhN2E2My5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUxMTA1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MTEwNVQxMTIyNDhaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT04NGVlYjM1N2RjZDQwMzE3OWI1ZDU4NDA1ZmNjNWQ5NDVmNzRjYjQ3MTRlYjNhODBlYjEyMmVkYWQ1MjFmMjQwJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.GsfspZQmMLO6Zy0P48rvaPxeVqH8AWj01XciadyqOOE)

保存后，请确保脚本处于 “已启用（Enabled）” 状态。

### 四、在 ChatGPT 页面运行脚本

1) 打开 `https://chatgpt.com`（或 `https://chat.openai.com`）。
2) 页面加载约 2 秒后，右下角会出现一个按钮：
   - 按钮文案：`导出会话 / Export Conversations`
3) 点击该按钮，会弹出导出选项对话框：
   - 导出格式：JSON、Markdown、HTML（三选任意，可多选）
   - 导出范围：个人空间 / 团队空间
   - 团队空间：会自动检测 Workspace ID；如未检测到，你可在输入框粘贴 `ws-...` 格式的 ID
   - 按钮：取消 / 开始导出

![截图D：ChatGPT 页面右下角的 “导出会话 / Export Conversations” 按钮位置](https://private-user-images.githubusercontent.com/102719366/510091014-a3bc281b-559b-4883-a770-fa4f907411d1.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjIzNDIwNjgsIm5iZiI6MTc2MjM0MTc2OCwicGF0aCI6Ii8xMDI3MTkzNjYvNTEwMDkxMDE0LWEzYmMyODFiLTU1OWItNDg4My1hNzcwLWZhNGY5MDc0MTFkMS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUxMTA1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MTEwNVQxMTIyNDhaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1iMGU0ODNlNTRmZDJiOWU5NTUwNWMyNzY4YzhiMjdhMGUzYmNhMjhhYjBjYWZjMDVlMTZiZjgyNWMyNWIwMzRkJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.rBeb2_ulTSeHikaoOtdm6EECXy1xdyPtdd2HZKXgJb4)

![截图E：弹出的导出对话框（格式勾选、个人/团队切换、Workspace ID 输入框、开始导出按钮）](https://private-user-images.githubusercontent.com/102719366/510091060-95521427-cf22-407c-aa00-17874743d8a1.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjIzNDIwNjgsIm5iZiI6MTc2MjM0MTc2OCwicGF0aCI6Ii8xMDI3MTkzNjYvNTEwMDkxMDYwLTk1NTIxNDI3LWNmMjItNDA3Yy1hYTAwLTE3ODc0NzQzZDhhMS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUxMTA1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MTEwNVQxMTIyNDhaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT03ZmIzNmYwZmFjNDRhNWZkMGIwMDljZDZhODkyZmIwMmQyMjUzZjQ4YTYxNjI1MDhjZjY3N2U2YTAwMjAwZWI1JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.PfsUBtvPQ37UTHMmPXDTYa5qhU1hqZWKuFc6XFC0qf4)

点击 “开始导出 / Start Export” 后，右下角按钮会显示进度状态，例如：
- “获取项目外对话 / Fetching conversations outside projects…”
- “获取项目列表 / Fetching project list…”
- “生成 ZIP 文件 / Generating ZIP file…”

导出完成后会自动下载 ZIP 文件：
- 个人空间：`chatgpt_personal_backup_YYYY-MM-DD.zip`
- 团队空间：`chatgpt_team_backup_<workspaceId>_YYYY-MM-DD.zip`

### 五、常见问题（FAQ）

- 看不到右下角按钮？
  - 确认脚本在 Tampermonkey 中处于 “已启用”。
  - 确认访问的域名是 `chatgpt.com` 或 `chat.openai.com`。
  - 刷新页面，等待 2 秒。

- 弹窗提示 “无法获取 Access Token”？
  - 请在 ChatGPT 中打开任意一个对话页面后再试，或刷新页面后重试。

- 团队空间导出失败？
  - 确认 Workspace ID 有效（形如 `ws-...`）。
  - 在弹窗中粘贴有效 ID 后重试。

- 导出内容包含哪些元素？
  - 脚本会获取对话详情，并按时间顺序导出为 JSON/Markdown/HTML，HTML 版本包含基础样式和代码块高亮容器（不联网静态查看）。

### 六、隐私与权限说明（简要）

- 本脚本仅在 `chatgpt.com` / `chat.openai.com` 作用域下工作。
- 访问令牌（Access Token）仅用于向 ChatGPT 官方接口发起导出请求，不会上传到第三方。
- ZIP 文件在本地浏览器生成并下载。

如需了解 Tampermonkey 的权限工作方式与最佳实践，请查阅官方文档：
- [Tampermonkey Documentation](https://www.tampermonkey.net/documentation.php)
- [Tampermonkey FAQ](https://www.tampermonkey.net/faq.php)


## English

> Attribution: All code of this userscript comes from the post on [linux.do](https://linux.do/t/topic/1052708). Thanks to LeYangZi for sharing. If this infringes your rights, please contact me for removal immediately.

# chatgpt-easyuse-exporter
Export your ChatGPT Team conversation history into multiple formats — Markdown (MD), HTML, and JSON — for backup, archiving, or analysis. The exporter runs entirely in your browser and generates a ZIP file locally.

## Quick Guide

This guide is for first-time users of userscripts. You will install Tampermonkey, follow Tampermonkey’s official documentation to enable required permissions, import this script, and run it on the ChatGPT website.

- Supported site: `https://chatgpt.com/*`

### 1) Install Tampermonkey

Install Tampermonkey from your browser’s extension store:

- Chrome/Chromium/Edge: [Chrome Web Store - Tampermonkey](https://chromewebstore.google.com/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo)
- Firefox: [Firefox Add-ons - Tampermonkey](https://addons.mozilla.org/firefox/addon/tampermonkey/)
- Safari: search “Tampermonkey” in the Mac App Store

After installation, ensure the Tampermonkey icon appears in the browser toolbar.

![Screenshot A: Tampermonkey in the browser extension store / installed](https://private-user-images.githubusercontent.com/102719366/510088661-db47d3e7-e09a-4c6c-90bd-52c49bc934c9.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjIzNDIwNjgsIm5iZiI6MTc2MjM0MTc2OCwicGF0aCI6Ii8xMDI3MTkzNjYvNTEwMDg4NjYxLWRiNDdkM2U3LWUwOWEtNGM2Yy05MGJkLTUyYzQ5YmM5MzRjOS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUxMTA1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MTEwNVQxMTIyNDhaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT05YzZiNGY1ZDA0MDhhY2RlMmNlYzQwZGQ1MWMwMGU4NDgyNjFjYThlNWM2MTMxOTg0ZDg4OGJiNWI3MzMzZDAzJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.lPzW7Fpc7_2qov2hqGbud3A27mNzmC4FVtMzFemehnE)

### 2) Enable (or verify) permissions in Tampermonkey (follow the official docs)

To ensure the script runs correctly on ChatGPT and can make network requests, follow the official Tampermonkey documentation (we do not duplicate their guide here):

- Docs: [Tampermonkey Documentation](https://www.tampermonkey.net/documentation.php)
- FAQ: [Tampermonkey FAQ](https://www.tampermonkey.net/faq.php)

Open the Tampermonkey icon → Dashboard → Settings and configure as recommended by the docs (you may set Config Mode to Advanced to reveal more options).

![Screenshot B: Tampermonkey Dashboard → Settings, highlight permissions](https://private-user-images.githubusercontent.com/102719366/510089048-1f67b4d4-5ffe-4b1c-ba26-0e6466eaa3f6.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjIzNDIwNjgsIm5iZiI6MTc2MjM0MTc2OCwicGF0aCI6Ii8xMDI3MTkzNjYvNTEwMDg5MDQ4LTFmNjdiNGQ0LTVmZmUtNGIxYy1iYTI2LTBlNjQ2NmVhYTNmNi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUxMTA1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MTEwNVQxMTIyNDhaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT03Y2Q1MjI3NjAxZjNhMmFlYTM1MDkyNzBlNjEyYTMzMDAyOGEzYWFjN2M1ZWE2NGQ5M2U4YjY0NzNkNDliZGMxJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.b29xx1Y4kzk9EnD-6BF0gbx9G4B-CwFHM8gZOqRhp4g)

### 3) Import this userscript

Two common ways — pick one:

1) Create and paste
- Click Tampermonkey icon → Dashboard.
- Click “+” or “Create a new script…”.
- Open `userscript.js` from this repo, copy all contents, paste into the editor, then save (Ctrl/Cmd + S).

2) Import from file
- Tampermonkey Dashboard → Utilities.
- Use “Import from file” and select the local `userscript.js`, then save and enable it.

![Screenshot C: New/Import script page showing paste/file selection](https://private-user-images.githubusercontent.com/102719366/510089516-b5c2b46d-2189-4166-a073-f6f0670a7a63.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjIzNDIwNjgsIm5iZiI6MTc2MjM0MTc2OCwicGF0aCI6Ii8xMDI3MTkzNjYvNTEwMDg5NTE2LWI1YzJiNDZkLTIxODktNDE2Ni1hMDczLWY2ZjA2NzBhN2E2My5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUxMTA1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MTEwNVQxMTIyNDhaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT04NGVlYjM1N2RjZDQwMzE3OWI1ZDU4NDA1ZmNjNWQ5NDVmNzRjYjQ3MTRlYjNhODBlYjEyMmVkYWQ1MjFmMjQwJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.GsfspZQmMLO6Zy0P48rvaPxeVqH8AWj01XciadyqOOE)

Ensure the script is Enabled after saving.

### 4) Run on the ChatGPT website

1) Open `https://chatgpt.com`.
2) After the page loads (~2 seconds), a button appears at bottom-right:
   - Label: `导出会话 / Export Conversations`
3) Click it to open the export dialog:
   - Formats: JSON, Markdown, HTML (select any)
   - Scope: Personal / Team
   - Team: Workspace ID is auto-detected; if not, paste a valid `ws-...` ID
   - Buttons: Cancel / Start Export

![Screenshot D: Bottom-right “Export Conversations” button on ChatGPT](https://private-user-images.githubusercontent.com/102719366/510091014-a3bc281b-559b-4883-a770-fa4f907411d1.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjIzNDIwNjgsIm5iZiI6MTc2MjM0MTc2OCwicGF0aCI6Ii8xMDI3MTkzNjYvNTEwMDkxMDE0LWEzYmMyODFiLTU1OWItNDg4My1hNzcwLWZhNGY5MDc0MTFkMS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUxMTA1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MTEwNVQxMTIyNDhaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1iMGU0ODNlNTRmZDJiOWU5NTUwNWMyNzY4YzhiMjdhMGUzYmNhMjhhYjBjYWZjMDVlMTZiZjgyNWMyNWIwMzRkJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.rBeb2_ulTSeHikaoOtdm6EECXy1xdyPtdd2HZKXgJb4)

![Screenshot E: Export dialog (formats, personal/team, Workspace ID, Start Export)](https://private-user-images.githubusercontent.com/102719366/510091060-95521427-cf22-407c-aa00-17874743d8a1.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjIzNDIwNjgsIm5iZiI6MTc2MjM0MTc2OCwicGF0aCI6Ii8xMDI3MTkzNjYvNTEwMDkxMDYwLTk1NTIxNDI3LWNmMjItNDA3Yy1hYTAwLTE3ODc0NzQzZDhhMS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUxMTA1JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MTEwNVQxMTIyNDhaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT03ZmIzNmYwZmFjNDRhNWZkMGIwMDljZDZhODkyZmIwMmQyMjUzZjQ4YTYxNjI1MDhjZjY3N2U2YTAwMjAwZWI1JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.PfsUBtvPQ37UTHMmPXDTYa5qhU1hqZWKuFc6XFC0qf4)

When you click “Start Export”, the bottom-right button shows progress like:
- “Fetching conversations outside projects…”
- “Fetching project list…”
- “Generating ZIP file…”

ZIP file names:
- Personal: `chatgpt_personal_backup_YYYY-MM-DD.zip`
- Team: `chatgpt_team_backup_<workspaceId>_YYYY-MM-DD.zip`

### FAQ

- Don’t see the button?
  - Ensure the script is Enabled in Tampermonkey.
  - Ensure the domain is `chatgpt.com`.
  - Refresh and wait 2 seconds.

- “Failed to obtain Access Token”?
  - Open any conversation in ChatGPT, then retry; or refresh the page and try again.

- Team export failed?
  - Verify the Workspace ID (like `ws-...`).
  - Paste a valid ID and try again.

- What gets exported?
  - Full conversation details in chronological order to JSON/Markdown/HTML. The HTML includes basic styling and code-block rendering and can be viewed offline.

### Privacy & Permissions (brief)

- The script only runs on `chatgpt.com`.
- The Access Token is used only for official ChatGPT endpoints and is not uploaded elsewhere.
- The ZIP is generated and downloaded locally in your browser.

For Tampermonkey’s permission model and best practices, see:
- [Tampermonkey Documentation](https://www.tampermonkey.net/documentation.php)
- [Tampermonkey FAQ](https://www.tampermonkey.net/faq.php)

---