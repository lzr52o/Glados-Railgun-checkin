English · [简体中文](README.md) · [日本語](README_JA.md) · [한국어](README_KO.md) · [Türkçe](README_TR.md)

![GLaDOS Auto Check-in](header.png)

<div align="center">

# GLaDOS Auto Check-in

![Typing SVG](https://readme-typing-svg.demolab.com?font=Fira+Code&size=22&pause=1000&color=0366D6&center=true&vCenter=true&width=600&lines=%E6%AC%A2%E8%BF%8E%E6%9D%A5%E5%88%B0+GLaDOS+%E8%87%AA%E5%8A%A8%E7%AD%BE%E5%88%B0%3B%E6%AF%8F%E5%A4%A9%E8%87%AA%E5%8A%A8%E5%AE%8C%E6%88%90%E7%AD%BE%E5%88%B0%3B%E7%BB%93%E6%9E%9C%E6%8E%A8%E9%80%81%E5%88%B0%E6%89%8B%E6%9C%BA)

<p><sub>Automatically complete check-in every day, with results pushable to your phone.</sub></p>

Supports **multiple accounts** · **8 domains** · **Cloudflare Turnstile**: run unattended with GitHub Actions, or check in from your real browser with the browser extension.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python](https://img.shields.io/badge/python-3.13-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![GLaDOS Check-in](https://github.com/Devilstore/Glados-Railgun-checkin/actions/workflows/gladosCheck.yml/badge.svg?branch=master)](https://github.com/Devilstore/Glados-Railgun-checkin/actions/workflows/gladosCheck.yml)
[![Last commit](https://img.shields.io/github/last-commit/Devilstore/Glados-Railgun-checkin?branch=master)](https://github.com/Devilstore/Glados-Railgun-checkin/commits/master)
![Visits](https://komarev.com/ghpvc/?username=lzr52o&color=0e75b6&style=flat)

[Quick Start](#快速开始) · [Browser Extension](#-浏览器扩展cloudflare-备用方案) · [Account Configuration](#配置-glados_cookies) · [Points Redemption](#积分兑换) · [Push Notifications](#消息推送) · [Troubleshooting](#问题排查) · [FAQ](#常见问题faq)

</div>

---

> [!NOTE]
> Unofficial project, not affiliated with GLaDOS. For learning and discussion only.

## 🚀Quick Start

> [!TIP]
> **Don't have a GLaDOS account yet?**
>
> Register with the [**invitation link**](https://glados.space/landing/0A58E-NV28S-6U3QV-33VMG); use promo code `DEVILSTORE` at checkout for 10% off.

1. Fork this repository.
2. In your own repository, go to `Settings` → `Secrets and variables` → `Actions` → `New repository secret`, and add at least `GLADOS_COOKIES`.
3. Open `Actions` → `GLaDOS Check-in` → `Run workflow` and run it manually once. When you see「签到成功」or「重复签到」, setup is complete.
4. After that it runs automatically every day at **09:18 / 12:18** (Beijing time).

| Secret | Required | Description |
| --- | :---: | --- |
| `GLADOS_COOKIES` | **Yes** | Account list, fixed JSON array, see [Account Configuration](#配置-glados_cookies) |
| `PUSHDEER_SENDKEY` | No | PushDeer push, see [Push Notifications](#消息推送) |
| `SERVERCHAN_SENDKEY` | No | ServerChan push, see [Push Notifications](#消息推送) |
| `GLADOS_EXCHANGE_PLAN` | No | `plan100` / `plan200` / `plan500`, see [Points Redemption](#积分兑换) |
| `GLADOS_VERBOSE` | No | `true` / `1` / `yes` for verbose logs |

## 🧩 Browser Extension (Cloudflare Fallback)

### How to Choose

| Scenario | Recommendation |
|---|---|
| Don't want to turn on the computer every day, happy with Actions running automatically | Python / Actions |
| Often hit Cloudflare, Turnstile, or only get 1 point | Browser extension |
| Want both | Keep Actions, and install the extension for make-up check-ins |

The Actions version uses pure HTTP requests and **cannot pass Cloudflare Turnstile**. If the log shows:

- `Cloudflare` / `Just a moment` / `Turnstile`;
- HTTP `403` / `429` / `503`;
- `Automated check-in detected`;
- Only 1 point, clearly less than a normal check-in;

it means this check-in did not earn full points, and re-running the script usually doesn't help.  
Please use the companion browser extension instead: click the check-in button on the official site in a real browser, and let the official component complete Turnstile.

### Installation and Usage

1. Download this repository, or download the extension archive from [Latest Release](https://github.com/Devilstore/Glados-Railgun-checkin/releases/latest) and extract it;
2. Open `edge://extensions` (Chrome: `chrome://extensions`) and enable "Developer mode";
3. Click "Load unpacked" and select the `extension/` directory;
4. Open the extension settings and click "+ Add account":
   - Main account: choose "Browser login session";
   - Other accounts: choose "Fill in Cookie manually";
5. After saving, click "Check in now". You can then configure auto check-in, notifications and push in "Settings".

Full instructions: [`extension/README.md`](extension/README.md)

### Version

Current version: `1.0.1`.

- The `extension/` directory always contains the **latest source code**, suitable for developers and local loading;
- Release packages are named by version, e.g. `glados-checkin-extension-v1.0.1.zip`;
- When upgrading, replace the contents of the `extension/` directory, then click "Reload" at `edge://extensions`; your account data is kept;
- Don't treat `latest` as the only file name; go by the **Latest** release on GitHub Releases — versioned packages make rollback and troubleshooting easier.

Python / Actions and the extension can both be kept: Actions handles unattended runs, and the extension handles make-up check-ins on your own machine under Cloudflare restrictions.

## 🔧Configuring GLADOS_COOKIES

Must be a **JSON array**, one object per account, with only the following four fields allowed:

```json
[
  {
    "name": "大号",
    "origin": "glados.cloud",
    "cookie": "koa:sess=...; koa:sess.sig=...; gld:sess=...",
    "user-agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) ... Safari/537.36"
  },
  {
    "name": "小号",
    "origin": "railgun.info",
    "cookie": "koa:sess=...; koa:sess.sig=..."
  },
  {
    "name": "账号3",
    "cookie": "gld:sess=..."
  }
]
```

| Field | Required | Description |
| --- | :---: | --- |
| `cookie` | **Yes** | Full Cookie; the new version recommends including both `gld:sess` and `gld:sess.sig` |
| `name` | No | Remark name; if omitted, shows `账号1` |
| `origin` | No | Domain the Cookie belongs to; auto-detected if omitted |
| `user-agent` | No | Browser UA used when the Cookie was captured; recommended to fill in |

**Key Points**

- Field names are fixed; any other field causes an error.
- Duplicate Cookies are automatically deduplicated and won't check in twice.
- The old `&` separator is still supported but prompts migration; newline separation, string arrays and single objects are no longer supported.

**How to Get Them**

On the check-in page, press `F12` → `Network` → refresh → select any request → `Request Headers`, and copy the full values of `Cookie` and `User-Agent`.

**Detection Order** (when `origin` is omitted, tried in this order, stopping at the first hit)

`glados.cloud` → `railgun.info` → `glados.network` → `glados.rocks` → `glados.one` → `glados.space` → `glados.vip` → `glados-facility.com`

Detection uses short timeouts to scan quickly; only domains with **no network response** are tried once more, and domains returning「没有权限」are not retried.

## 🎁Points Redemption

Set `GLADOS_EXCHANGE_PLAN` to enable redemption, and it only runs when check-in is **successful / duplicate**.

| Tier | Points Consumed | Days Gained |
| --- | :---: | :---: |
| `plan100` | 100 | 10 days |
| `plan200` | 200 | 30 days |
| `plan500` | 500 | 100 days |

If omitted or invalid, no redemption happens; if the balance is insufficient, the API returns failure and the balance won't go negative.

## 📣Push Notifications

| Backend | Secret | How to Get the Key |
| --- | --- | --- |
| PushDeer | `PUSHDEER_SENDKEY` | Download the App at [pushdeer.com](https://www.pushdeer.com/product.html) and get the pushkey inside the App |
| ServerChan | `SERVERCHAN_SENDKEY` | Log in by scanning the WeChat QR code at [sct.ftqq.com](https://sct.ftqq.com/) and get the SendKey on the page |

Both can be configured alone or together (both are sent; any one succeeding counts as success); if neither is configured, push is skipped and check-in is unaffected.

### Push Result

```text
GLaDOS 签到 · 成功 1 · 失败 1 · 重复 1
#1  大号  ✅ 签到成功  glados.cloud  获得 20 积分 · 剩余 48 天 · 总 409 积分
#2  小号  🔄 重复签到  railgun.info  获得 0 积分 · 剩余 23 天 · 总 528 积分
#3  账号3  🔴 签到失败  未探测  获得 0 积分 · 剩余 None 天 · 总 None 积分
```

| Marker | Meaning |
| --- | --- |
| `✅ 签到成功` | This check-in succeeded; `获得 N 积分` is the points earned this time |
| `🔄 重复签到` | Already checked in today; a normal result, not a failure |
| `🔴 签到失败` | Failed to check in; see [Troubleshooting](#问题排查) |

The account name column only appears with **multiple accounts**.

## ⏰Changing the Run Time

Default is every day at **09:18 / 12:18** (Beijing time), controlled by the `schedule` in `.github/workflows/gladosCheck.yml`:

```yaml
schedule:
  - cron: '18 9,12 * * *'
    timezone: 'Asia/Shanghai'
```

Change `cron` to adjust the time; `timezone` makes GitHub interpret it in Beijing time. For example, to keep only the morning run:

```yaml
schedule:
  - cron: '18 9 * * *'
    timezone: 'Asia/Shanghai'
```

## 🔍Troubleshooting

**Normal Logs**

| Log Snippet | Meaning |
| --- | --- |
| `✅ 命中登录域名（剩余 N 天）` | Detection succeeded; subsequent check-ins only use that domain |
| `✅ 重试后命中登录域名（剩余 N 天）` | The domain had no response the first time, but succeeded after a retry |
| `[REDACTED]` | Redaction marker; credentials have been replaced |

**Abnormal Logs**

| Log Snippet | Cause and Fix |
| --- | --- |
| `候选域名均返回「没有权限」` | Cookie expired, or the account is not on a candidate domain → log in again and update the Cookie |
| `候选域名均未获得服务端响应` | Network/server failure, not a Cookie problem → rerun later |
| `设备平台不匹配(loginDevice=…)` | UA doesn't match the login device → add the `user-agent` field |
| `签到被拒：please checkin via …` | Token doesn't match the domain (mostly due to server-side rule changes) → confirm the domain the Cookie came from |
| `无权限(code=-2)` | The Cookie is expired or the account is under risk control |
| `Cloudflare` / `Just a moment` / `Turnstile` / HTTP `403` `429` `503` | Cloudflare blocking or rate limiting; pure HTTP cannot complete verification → **use the browser extension for a make-up check-in**, see [Browser Extension](#-浏览器扩展cloudflare-备用方案) |
| `Automated check-in detected` / only 1 point | GLaDOS detected automated check-in; the script already adapts to the challenge, but TLS/HTTP fingerprint, IP, device, etc. may still be recognised → **use the browser extension** and complete verification in a real browser |
| `运行超出预算` | Single round timed out; remaining accounts were skipped → rerun in the next round or manually |
| `积分请求失败 / 响应解析失败 / 格式异常 / 字段无法解析` | Failed to fetch total points → push shows `总 None 积分`, **does not affect the check-in result**, can be ignored |

> If `总 None 积分` appears in the push, it only means the total points were not fetched this time (that API is occasionally slow). It has nothing to do with whether check-in succeeded; to confirm, manually rerun once.

## ❓FAQ

<details>
<summary><strong>Q: How long does one run take?</strong></summary>

Usually a few seconds to a few dozen seconds. The single-round budget is 300 seconds (about 5 minutes); remaining accounts are skipped if exceeded.

</details>

<details>
<summary><strong>Q: How long until a Cookie expires?</strong></summary>

No fixed expiry. Logging out, changing your password, or server-side session policy changes may invalidate it earlier. When you see「候选域名均返回没有权限」or `code=-2`, log in again and update `GLADOS_COOKIES`.

</details>

<details>
<summary><strong>Q: Does it support multiple accounts?</strong></summary>

Yes, unlimited. Just add more items to the JSON array. Duplicate Cookies are automatically deduplicated and won't check in twice.

</details>

<details>
<summary><strong>Q: Do I need to label which domain each Cookie belongs to?</strong></summary>

No, the script detects it automatically. Filling in `origin` means fewer detection requests and clearer logs. Note that **Cookies from different sites are usually not interchangeable** — each Cookie must come from the site it actually logged in to.

</details>

<details>
<summary><strong>Q: What happens if I fill in the wrong <code>origin</code>?</strong></summary>

It won't fail. `origin` is only a **preferred first try**: if it hits, check-in proceeds directly; if it returns「没有权限」, it automatically falls back to traversing all 8 built-in domains.

</details>

<details>
<summary><strong>Q: With multiple accounts, how do I tell which account a log line belongs to?</strong></summary>

If `name` is omitted, accounts are auto-named in order, showing `🍪[账号1]`, `🍪[账号2]`; if filled in, they show `🍪[1:大号]`, `🍪[2:小号]`, `🍪[3:账号3]`, with numbers matching `#1` `#2` `#3` in the summary.

</details>

<details>
<summary><strong>Q: How many tasks does one account run?</strong></summary>

One. The script detects the domain first, then checks in only on that domain; the number of tasks equals the number of accounts.

</details>

<details>
<summary><strong>Q: Does "duplicate check-in" count as success or failure?</strong></summary>

It counts as a normal result. It means you already checked in today; the server returns `code=1`, and the summary counts it separately as "duplicate", not as a failure.

</details>

<details>
<summary><strong>Q: What should I do about a device mismatch (code=4)?</strong></summary>

GLaDOS verifies the UA against the login device. If `user-agent` is filled in, it is used as-is; if not, the default UA is used, and on failure it automatically retries once based on `loginDevice`; if it still fails, it prompts you to add `user-agent` — fill in the UA of **the browser you used when capturing the Cookie**.

</details>

<details>
<summary><strong>Q: Actions is green, but I got no notification?</strong></summary>

First confirm the corresponding Secret is spelled correctly (the log prints「已配置 N 个后端」). Note that ServerChan returning `code=0` only means the **task has been queued**, not delivered; if nothing arrives for a long time, check whether the SendKey is bound to WeChat.

</details>

<details>
<summary><strong>Q: Will the scheduled task of a public repository stop on its own?</strong></summary>

Yes. GitHub disables schedules for public repositories after 60 consecutive days of inactivity. This repository has a built-in keepalive step that generates activity regularly, but it is not 100% guaranteed; if it stops running, re-enable it from the Actions page.

</details>

<details>
<summary><strong>Q: Will configured redemption spend my points randomly?</strong></summary>

It only redeems the tier specified by `GLADOS_EXCHANGE_PLAN`; **if not filled in, nothing is redeemed**, and redemption only happens on successful/duplicate check-in. If the balance is insufficient, the API returns failure and won't go negative.

</details>

<details>
<summary><strong>Q: What should I do if Cloudflare blocks me and I get no points / only 1 point?</strong></summary>

This is an inherent limitation of a pure HTTP script. When the log shows Cloudflare, Turnstile, 403/429/503, or only 1 point, this check-in did not complete fully. Re-running will most likely still be blocked.

Please switch to the companion browser extension [`extension/`](extension/README.md): the extension clicks the check-in button on the official site in a real browser, and the official component completes Turnstile; it supports 8 domains, multiple accounts, auto redemption and push.

</details>

<details>
<summary><strong>Q: Will a failed check-in turn Actions red?</strong></summary>

Two cases: **configuration errors** (Cookie not filled in, invalid JSON format) end with a non-zero exit code and the workflow turns red; **business failures** (expired Cookie `code=-2`, server 5xx) do not turn red, to avoid false alarms from occasional failures every day. So business failures need to be watched via push or logs yourself.

</details>

<details>
<summary><strong>Q: What is "0 days remaining" about?</strong></summary>

The account expires today; check-in itself still succeeds. It is recommended to configure `GLADOS_EXCHANGE_PLAN` to renew with points, see [Points Redemption](#积分兑换).

</details>

<details>
<summary><strong>Q: Can I run it locally?</strong></summary>

Yes, for debugging. Install the dependencies, set the environment variable, then run:

```bash
pip install -r requirements.txt
export GLADOS_COOKIES='[{"cookie":"koa:sess=..."}]'
python checkin.py
```

Windows PowerShell: `$env:GLADOS_COOKIES='[...]'`.

</details>

---

## 📁File Structure

```text
.
├── checkin.py                    # Main program: account model / API / config / report / push
├── logging_config.py             # Logging config (Beijing time, short level labels, redaction)
├── requirements.txt              # Dependencies
├── README.md                     # Documentation
├── LICENSE                       # MIT License
├── extension/                    # Browser extension: Cloudflare fallback check-in
└── .github/workflows/
    └── gladosCheck.yml           # Actions scheduled task
```

## 📝Changelog

**2026-01**

- Refactored the code, added log output for easier troubleshooting, support for new URLs, and support for configuring the points redemption strategy.

**2026-04**

- Optimised code logic and log output.

**2026-09**

- Device-platform adaptive retry, no manual UA configuration needed.
- Automatic network retry with backoff.
- Log times unified to Beijing time; scheduled task adjusted to twice a day.
- Auto redemption off by default.

**2026-10**

- Push support for ServerChan.

**2026-11**

**1.0.1**

- Extension upgraded to `1.0.1`;
- Cleaned up internal history/version comments inside the extension;
- Ignored local release/debug files such as `SHA256SUMS.txt`, `release-notes.md`, `debug.py`.

**1.0.0**

- GLADOS_COOKIES format changed to a JSON array, supporting `name` / `origin` / `cookie` / `user-agent`; old `&` format still supported.
- Improved failure messages; actionable advice for rejected check-ins, incomplete redemptions, and other scenarios.
- Automatic log redaction: `koa:*` / `gld:*` / Bearer → `[REDACTED]`.
- Added challenge verification; falls back to the old protocol when fetching fails.
- Detects Cloudflare/Turnstile blocking and 403/429/503; when points are incomplete, logs and push suggest switching to the browser extension.

## 📊 Statistics

<div align="center">

<img src="https://github-readme-stats.vercel.app/api?username=lzr52o&show_icons=true&theme=radical" width="46%" /><img src="https://github-readme-stats.vercel.app/api/top-langs/?username=lzr52o&layout=compact&theme=radical" width="50%" />

<img src="https://github-readme-streak-stats.herokuapp.com/?user=lzr52o&theme=radical" width="96%" />

<img src="https://github-readme-activity-graph.vercel.app/graph?username=lzr52o&theme=github-compact" width="100%" />

</div>

## 🛠️ Tech Stack

<div align="center">

![Skill Icons](https://skillicons.dev/icons?i=js,py,git,github,cloudflare,apple,vscode&theme=dark)

</div>

## 😄 Daily Sentence

<div align="center">

<img src="https://daily-sentence.lzr52o.workers.dev/sentence" width="440" height="140" alt="Daily sentence" />

</div>

## 🎮 Contribution Graph

<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://contrib-rotate.lzr52o.workers.dev/rotate?theme=dark" />
  <source media="(prefers-color-scheme: light)" srcset="https://contrib-rotate.lzr52o.workers.dev/rotate?theme=light" />
  <img alt="Contribution graph" src="https://contrib-rotate.lzr52o.workers.dev/rotate?theme=light" />
</picture>

</div>



## ⭐Star History

[![Star History Chart](https://api.star-history.com/svg?repos=Devilstore/Glados-Railgun-checkin&type=Date&legend=top-left)](https://star-history.com/#Devilstore/Glados-Railgun-checkin&type=Date)

---

## 📄Disclaimer

- No guarantee of stable operation; APIs and risk-control policies may change at any time.
- Only calls the public APIs used by the web version; does not bypass captchas; server-side risk control may still reject or downgrade.
- Use at your own risk; account risk control, restrictions or bans resulting from using this tool are the user's own responsibility.
- This project is released under [MIT](LICENSE).

If this project helped you, please give it a ⭐ Star.

## 🙏 Acknowledgements

All scripts and modules in this repo are from the following original authors. Thank you for your hard work:

[Devilstore](https://github.com/Devilstore)

## ⚠️ Disclaimer

This repo is for personal learning only. All scripts and modules belong to their original authors. No reposting, mirroring, redistribution or commercial use. Do not remove original author information. Contact me via my Telegram bot [@ccccccccoBot](https://t.me/ccccccccoBot) for removal if there is any infringement.

![Footer](https://capsule-render.vercel.app/api?type=waving&color=gradient&height=120&section=footer&text=GLaDOS+%E8%87%AA%E5%8A%A8%E7%AD%BE%E5%88%B0&fontSize=24&fontColor=ffffff)
