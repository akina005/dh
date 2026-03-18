---

# 🛰️ Hax 自动化续期项目部署指南

本说明书旨在帮助你从零开始，在 GitHub 上部署一套基于 AI 视觉识别和 Telegram 自动授权的 Hax.co.id 续期系统。

## 一、 核心准备清单

在正式操作前，请确保你已经获取了以下信息：

| 分类 | 参数名称 | 获取途径 |
| --- | --- | --- |
| **TG API** | `API_ID` & `API_HASH` | 访问 [my.telegram.org](https://my.telegram.org) 申请 |
| **TG 通信** | `BOT_TOKEN` & `CHAT_ID` | 通过 [@BotFather](https://t.me/BotFather) 和 [@userinfobot](https://t.me/userinfobot) 获取 |
| **AI 识别** | `AI_KEY` & `BASE_URL` | 你的 AI 接口提供商（需支持 GPT-4o 或类似视觉模型） |
| **账号** | `TELEGRAM_PHONE` | 你 Hax 账号绑定的手机号（带国家码，如 +86...） |

---

## 二、 关键步骤：获取 `TG_STRING_SESSION`

这是整个项目最关键的一步，它让 GitHub 云端环境能够代表你本人去点击“确认登录”按钮。

1. **环境准备**：在你的电脑上安装 Python 3。
2. **安装库**：打开终端（macOS）或命令行（Windows），输入：
```bash
pip3 install pyrogram tgcrypto

```


3. **运行脚本**：在本地运行 `get_session.py`。
4. **验证登录**：
* 输入你的手机号（+86...）。
* 在你的 Telegram App 中接收官方发送的 **5 位数验证码**并填入终端。
* 如果有两步验证，输入密码。


5. **保存结果**：终端会输出一串以 `BQH...` 开头的极长乱码。**完整复制这串乱码**，它就是你的 `TG_STRING_SESSION`。

---

## 三、 GitHub 部署流程（手把手教学）

### 第一步：创建私有仓库

1. 在 GitHub 上创建一个新的仓库（建议设为 **Private** 以保护隐私）。
2. 将以下 5 个核心文件上传至仓库：
* `hax_renw.py`
* `simple_bypass.py`
* `requirements.txt`
* `.github/workflows/hax_auto.yml`
* `README.md`



### 第二步：配置环境变量（Secrets）

GitHub Actions 运行需要读取你的密匙。请按以下路径操作：

1. 点击仓库上方的 **Settings**（设置）。
2. 在左侧菜单点击 **Secrets and variables** -> **Actions**。
3. 点击 **New repository secret**。
4. **你需要依次添加以下 9 个变量**（名字必须完全一致）：

> * `TELEGRAM_API_ID`
> * `TELEGRAM_API_HASH`
> * `TG_STRING_SESSION` （刚才在本地生成的长字符串）
> * `TELEGRAM_PHONE`
> * `TELEGRAM_BOT_TOKEN`
> * `MY_CHAT_ID`
> * `AI_BASE_URL`
> * `AI_API_KEY`
> * `AI_MODEL_NAME`
> 
> 

---

## 四、 启动与日常监控

### 1. 激活自动化

* 点击仓库顶部的 **Actions** 选项卡。
* 在左侧列表点击你的脚本任务名称。
* 点击右侧的 **Run workflow** -> **Run workflow**。

### 2. 状态反馈

* **运行日志**：你可以实时点击运行中的任务查看脚本是否卡在 Cloudflare 或正在识别验证码。
* **通知报告**：一旦续期成功，你的 Telegram 机器人会发来一条包含**网页实时截图**、**到期日期**和**北京时间**的图文报告。

---

## 五、 维护注意事项（必读）

* **Session 失效**：如果你在手机上主动退出了“所有其他设备”，该 Session 会失效，需要重新执行【第二步】。
* **AI 余额**：验证码识别需要消耗 AI 额度，请保持额度充裕，否则会因为验证码无法通过而导致续期失败。
* **不要改动文件名**：`simple_bypass.py` 必须与 `hax_renw.py` 在同一目录下，否则程序无法引用绕过逻辑。

---
