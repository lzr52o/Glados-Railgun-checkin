[English](README_EN.md) · [简体中文](README.md) · 日本語 · [한국어](README_KO.md) · [Türkçe](README_TR.md)

![GLaDOS 自動チェックイン](header.png)

<div align="center">

# GLaDOS 自動チェックイン

![Typing SVG](https://readme-typing-svg.demolab.com?font=Fira+Code&size=22&pause=1000&color=0366D6&center=true&vCenter=true&width=600&lines=%E6%AC%A2%E8%BF%8E%E6%9D%A5%E5%88%B0+GLaDOS+%E8%87%AA%E5%8A%A8%E7%AD%BE%E5%88%B0%3B%E6%AF%8F%E5%A4%A9%E8%87%AA%E5%8A%A8%E5%AE%8C%E6%88%90%E7%AD%BE%E5%88%B0%3B%E7%BB%93%E6%9E%9C%E6%8E%A8%E9%80%81%E5%88%B0%E6%89%8B%E6%9C%BA)

<p><sub>毎日自動でチェックインし、結果をスマホにプッシュできます。</sub></p>

**複数アカウント** · **8つのドメイン** · **Cloudflare Turnstile** に対応：GitHub Actionsで無人運用も、ブラウザ拡張機能で実際のブラウザからチェックインもできます。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python](https://img.shields.io/badge/python-3.13-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![GLaDOS Check-in](https://github.com/Devilstore/Glados-Railgun-checkin/actions/workflows/gladosCheck.yml/badge.svg?branch=master)](https://github.com/Devilstore/Glados-Railgun-checkin/actions/workflows/gladosCheck.yml)
[![Last commit](https://img.shields.io/github/last-commit/Devilstore/Glados-Railgun-checkin?branch=master)](https://github.com/Devilstore/Glados-Railgun-checkin/commits/master)
![アクセス数](https://komarev.com/ghpvc/?username=lzr52o&color=0e75b6&style=flat)

[クイックスタート](#快速开始) · [ブラウザ拡張機能](#-浏览器扩展cloudflare-备用方案) · [アカウント設定](#配置-glados_cookies) · [ポイント交換](#积分兑换) · [プッシュ通知](#消息推送) · [トラブルシューティング](#问题排查) · [よくある質問](#常见问题faq)

</div>

---

> [!NOTE]
> 非公式プロジェクトであり、GLaDOS公式とは関係ありません。学習と交流のみを目的としています。

## 🚀クイックスタート

> [!TIP]
> **GLaDOSアカウントをまだお持ちでない場合**
>
> [**招待リンク**](https://glados.space/landing/0A58E-NV28S-6U3QV-33VMG)から登録してください。決済時に割引コード `DEVILSTORE` を使うと10%オフになります。

1. このリポジトリをForkします。
2. 自分のリポジトリで `Settings` → `Secrets and variables` → `Actions` → `New repository secret` を開き、最低限 `GLADOS_COOKIES` を追加します。
3. `Actions` → `GLaDOS Check-in` → `Run workflow` を開いて手動で1回実行し、「签到成功」または「重复签到」と表示されれば完了です。
4. その後は毎日 **09:18 / 12:18**（北京時間）に自動実行されます。

| Secret | 必須 | 説明 |
| --- | :---: | --- |
| `GLADOS_COOKIES` | **はい** | アカウント一覧、固定のJSON配列。[アカウント設定](#配置-glados_cookies)を参照 |
| `PUSHDEER_SENDKEY` | いいえ | PushDeerプッシュ。[プッシュ通知](#消息推送)を参照 |
| `SERVERCHAN_SENDKEY` | いいえ | Server酱プッシュ。[プッシュ通知](#消息推送)を参照 |
| `GLADOS_EXCHANGE_PLAN` | いいえ | `plan100` / `plan200` / `plan500`。[ポイント交換](#积分兑换)を参照 |
| `GLADOS_VERBOSE` | いいえ | `true` / `1` / `yes` で詳細ログを出力 |

## 🧩 ブラウザ拡張機能（Cloudflareの代替手段）

### 選び方

| 利用シーン | おすすめ |
|---|---|
| 毎日パソコンを開きたくない、Actionsの自動実行で十分 | Python / Actions |
| CloudflareやTurnstileによく引っかかる、1ポイントしかもらえない | ブラウザ拡張機能 |
| 両方使いたい | Actionsを残しつつ、拡張機能で補完チェックイン |

Actions版は純粋なHTTPリクエストのため、**Cloudflare Turnstileを通過できません**。ログに次のような内容が出た場合：

- `Cloudflare` / `Just a moment` / `Turnstile`；
- HTTP `403` / `429` / `503`；
- `Automated check-in detected`；
- 1ポイントしかもらえず、通常のチェックインより明らかに少ない；

今回のチェックインでポイントを満額獲得できていないことを意味します。スクリプトを再実行しても通常は改善しません。  
代わりに付属のブラウザ拡張機能を使ってください。実際のブラウザで公式サイトのチェックインボタンをクリックし、Turnstileは公式コンポーネントに完了させます。

### インストールと使い方

1. このリポジトリをダウンロードするか、[Latest Release](https://github.com/Devilstore/Glados-Railgun-checkin/releases/latest)から拡張機能の圧縮ファイルをダウンロードして解凍します；
2. `edge://extensions`（Chromeは `chrome://extensions`）を開き、「開発者モード」を有効にします；
3. 「パッケージ化されていない拡張機能を読み込む」をクリックし、`extension/` ディレクトリを選択します；
4. 拡張機能の設定を開き、「+ アカウントを追加」をクリックします：
   - メインアカウントは「ブラウザのログイン状態」を選択；
   - その他のアカウントは「Cookieを手動入力」を選択；
5. 保存後「今すぐチェックイン」をクリックします。その後「設定」で自動チェックイン、通知、プッシュを設定できます。

詳細な説明：[`extension/README.md`](extension/README.md)

### バージョン

現在のバージョン：`1.0.1`。

- `extension/` ディレクトリには常に**最新のソースコード**が入っており、開発者やローカルでの読み込みに適しています；
- リリースパッケージはバージョン名で命名されます。例：`glados-checkin-extension-v1.0.1.zip`；
- アップグレード時は `extension/` ディレクトリの中身を置き換え、`edge://extensions` で「再読み込み」をクリックします。アカウントデータは保持されます；
- `latest` を唯一のファイル名だと思わないでください。GitHub Releasesの **Latest** を基準にしてください。バージョン付きパッケージの方がロールバックや調査がしやすくなります。

Python / Actionsと拡張機能は併用できます。Actionsが無人運用を担い、拡張機能がCloudflare制限下での手元での補完チェックインを担います。

## 🔧GLADOS_COOKIESの設定

**JSON配列**である必要があり、アカウントごとに1つのオブジェクトで、以下の4つのフィールドのみ使用できます：

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

| フィールド | 必須 | 説明 |
| --- | :---: | --- |
| `cookie` | **はい** | 完全なCookie。新バージョンでは `gld:sess` と `gld:sess.sig` の両方を含めることを推奨 |
| `name` | いいえ | 備考名。未入力の場合は `账号1` と表示 |
| `origin` | いいえ | Cookieが属するドメイン。未入力の場合は自動検出 |
| `user-agent` | いいえ | Cookie取得時のブラウザUA。入力を推奨 |

**要点**

- フィールド名は固定で、他のフィールドがあるとエラーになります。
- 重複したCookieは自動で重複排除され、二重にチェックインしません。
- 旧来の `&` 区切りも引き続き互換ですが、移行を促す表示が出ます。改行区切り、文字列配列、単一オブジェクトはすでに非対応です。

**取得方法**

チェックインページで `F12` → `Network` → 更新 → 任意のリクエストを選択 → `Request Headers` を開き、`Cookie` と `User-Agent` の完全な値をコピーします。

**検出順序**（`origin` 未入力の場合、この順序で試し、ヒットしたら停止）

`glados.cloud` → `railgun.info` → `glados.network` → `glados.rocks` → `glados.one` → `glados.space` → `glados.vip` → `glados-facility.com`

検出は短いタイムアウトで素早く一巡します。**ネットワーク応答がなかった**ドメインだけもう一度試し、「権限がありません」を返したドメインは再試行しません。

## 🎁ポイント交換

`GLADOS_EXCHANGE_PLAN` を設定して初めて実行され、チェックインが**成功 / 重複**の場合にのみ交換します。

| プラン | 消費ポイント | 獲得日数 |
| --- | :---: | :---: |
| `plan100` | 100 | 10日 |
| `plan200` | 200 | 30日 |
| `plan500` | 500 | 100日 |

未入力または不正な値の場合は交換しません。残高不足の場合、APIは失敗を返し、マイナスになることはありません。

## 📣プッシュ通知

| バックエンド | Secret | キーの取得方法 |
| --- | --- | --- |
| PushDeer | `PUSHDEER_SENDKEY` | [pushdeer.com](https://www.pushdeer.com/product.html)でアプリをダウンロードし、アプリ内でpushkeyを取得 |
| Server酱 | `SERVERCHAN_SENDKEY` | [sct.ftqq.com](https://sct.ftqq.com/)でWeChatのQRコードをスキャンしてログインし、ページでSendKeyを取得 |

単独でも両方同時でも設定できます（両方に送信され、いずれか1つ成功すれば成功とみなします）。どちらも設定しない場合はプッシュをスキップし、チェックインには影響しません。

### プッシュ内容

```text
GLaDOS 签到 · 成功 1 · 失败 1 · 重复 1
#1  大号  ✅ 签到成功  glados.cloud  获得 20 积分 · 剩余 48 天 · 总 409 积分
#2  小号  🔄 重复签到  railgun.info  获得 0 积分 · 剩余 23 天 · 总 528 积分
#3  账号3  🔴 签到失败  未探测  获得 0 积分 · 剩余 None 天 · 总 None 积分
```

| マーク | 意味 |
| --- | --- |
| `✅ 签到成功` | 今回のチェックインが成功しました。`获得 N 积分` は今回獲得したポイントです |
| `🔄 重复签到` | 今日はすでにチェックイン済みです。正常な結果で、失敗ではありません |
| `🔴 签到失败` | チェックインに失敗しました。[トラブルシューティング](#问题排查)を参照 |

アカウント名の列は**複数アカウント**の場合にのみ表示されます。

## ⏰実行時間の変更

デフォルトは毎日 **09:18 / 12:18**（北京時間）で、`.github/workflows/gladosCheck.yml` の `schedule` で制御します：

```yaml
schedule:
  - cron: '18 9,12 * * *'
    timezone: 'Asia/Shanghai'
```

`cron` を変更すれば時間を調整できます。`timezone` により、GitHubが北京時間として解釈します。例えば朝1回だけ残す場合：

```yaml
schedule:
  - cron: '18 9 * * *'
    timezone: 'Asia/Shanghai'
```

## 🔍トラブルシューティング

**正常なログ**

| ログの断片 | 説明 |
| --- | --- |
| `✅ 命中登录域名（剩余 N 天）` | 検出に成功しました。以後はそのドメインでのみチェックインします |
| `✅ 重试后命中登录域名（剩余 N 天）` | そのドメインは最初応答がありませんでしたが、再試行で成功しました |
| `[REDACTED]` | マスキング標識。認証情報は置き換え済みです |

**異常なログ**

| ログの断片 | 原因と対処 |
| --- | --- |
| `候选域名均返回「没有权限」` | Cookieの期限切れ、またはアカウントが候補ドメインにない → 再ログインしてCookieを更新 |
| `候选域名均未获得服务端响应` | ネットワーク/サーバーの障害で、Cookieの問題ではありません → 後で再実行 |
| `设备平台不匹配(loginDevice=…)` | UAとログインデバイスが一致しません → `user-agent` フィールドを補完 |
| `签到被拒：please checkin via …` | tokenとドメインが一致しません（多くはサーバー側のルール変更）→ Cookieの取得元ドメインを確認 |
| `无权限(code=-2)` | そのCookieが期限切れ、またはアカウントがリスク管理下にあります |
| `Cloudflare` / `Just a moment` / `Turnstile` / HTTP `403` `429` `503` | Cloudflareによるブロックまたは制限で、純粋なHTTPでは認証を完了できません → **ブラウザ拡張機能で補完チェックイン**してください。[ブラウザ拡張機能](#-浏览器扩展cloudflare-备用方案)を参照 |
| `Automated check-in detected` / 1ポイントのみ | GLaDOSが自動チェックインと判定しました。スクリプトはchallengeに対応済みですが、TLS/HTTPフィンガープリント、IP、デバイスなどが識別される可能性があります → **ブラウザ拡張機能**を使い、実際のブラウザで認証を完了してください |
| `运行超出预算` | 1回の実行がタイムアウトし、残りのアカウントがスキップされました → 次回または手動で再実行 |
| `积分请求失败 / 响应解析失败 / 格式异常 / 字段无法解析` | 合計ポイントの取得に失敗しました → プッシュに `总 None 积分` と表示されますが、**チェックイン結果には影響しません**。無視して構いません |

> プッシュに `总 None 积分` と出るのは、今回合計ポイントを取得できなかっただけです（そのAPIはときどき遅くなります）。チェックインの成否とは関係ありません。確認したい場合は手動で1回再実行してください。

## ❓よくある質問（FAQ）

<details>
<summary><strong>Q：1回の実行はどれくらい時間がかかりますか？</strong></summary>

通常は数秒から数十秒です。1回の実行予算は300秒（約5分）で、超過すると残りのアカウントはスキップされます。

</details>

<details>
<summary><strong>Q：Cookieの有効期限はどれくらいですか？</strong></summary>

固定の期限はありません。ログアウト、パスワード変更、サーバー側のセッション方針の変更などで前倒しで無効になることがあります。「候选域名均返回没有权限」や `code=-2` が出た場合は、再ログインして `GLADOS_COOKIES` を更新してください。

</details>

<details>
<summary><strong>Q：複数アカウントに対応していますか？</strong></summary>

対応しています。数に制限はありません。JSON配列に項目を追加するだけです。重複したCookieは自動で重複排除され、二重にチェックインしません。

</details>

<details>
<summary><strong>Q：各Cookieがどのドメインに属するかを指定する必要がありますか？</strong></summary>

必要ありません。スクリプトが自動で検出します。`origin` を入力すると、検出リクエストが減り、ログが分かりやすくなります。なお、**各サイトのCookieは通常互換性がありません**。各Cookieは実際にログインしたサイトのものでなければなりません。

</details>

<details>
<summary><strong>Q：<code>origin</code> を間違えて入力するとどうなりますか？</strong></summary>

失敗しません。`origin` は**優先的に試す**だけのものです。ヒットすればそのままチェックインし、「権限がありません」を返した場合は自動でフォールバックし、内蔵の8つのドメインをすべて巡回します。

</details>

<details>
<summary><strong>Q：複数アカウントの場合、ログがどのアカウントのものか見分けられますか？</strong></summary>

`name` 未入力の場合は順番に自動命名され、`🍪[账号1]`、`🍪[账号2]` と表示されます。入力した場合は `🍪[1:大号]`、`🍪[2:小号]`、`🍪[3:账号3]` と表示され、番号はまとめの `#1` `#2` `#3` と対応します。

</details>

<details>
<summary><strong>Q：1つのアカウントでいくつのタスクが走りますか？</strong></summary>

1つです。スクリプトはまずドメインを検出し、そのドメインでのみチェックインするため、タスク数はアカウント数と等しくなります。

</details>

<details>
<summary><strong>Q：「重複チェックイン」は成功ですか、失敗ですか？</strong></summary>

正常な結果です。当日すでにチェックイン済みであることを意味し、サーバーは `code=1` を返します。まとめでは「重複」として別途カウントされ、失敗には入りません。

</details>

<details>
<summary><strong>Q：デバイス不一致（code=4）が出たらどうすればいいですか？</strong></summary>

GLaDOSはUAとログインデバイスを照合します。`user-agent` を入力していればそのまま使われます。未入力の場合はデフォルトUAを使い、失敗時は `loginDevice` に基づいて自動で1回再試行します。それでも失敗する場合は `user-agent` の補完を促されます。**Cookieを取得したときのブラウザ**のUAを入力してください。

</details>

<details>
<summary><strong>Q：Actionsは緑なのに通知が届きません。</strong></summary>

まず対応するSecretのスペルが正しいか確認してください（ログに「已配置 N 个后端」と表示されます）。なお、Server酱が `code=0` を返すのは**タスクがキューに入った**ことだけを意味し、配信されたわけではありません。長時間届かない場合は、そのSendKeyがWeChatに紐付けられているか確認してください。

</details>

<details>
<summary><strong>Q：公開リポジトリの定期タスクは自分で止まりますか？</strong></summary>

はい。GitHubには「公開リポジトリは連続60日間活動がないとscheduleが無効化される」というルールがあります。このリポジトリにはkeepaliveステップが内蔵されており定期的に活動を発生させますが、100%の保証はありません。止まっているのを見つけたら、Actionsページから再度有効化してください。

</details>

<details>
<summary><strong>Q：交換を設定するとポイントを勝手に使われませんか？</strong></summary>

`GLADOS_EXCHANGE_PLAN` で指定したプランのみ実行します。**未入力なら交換しません**。また、チェックイン成功/重複の場合にのみ交換します。残高不足の場合、APIは失敗を返し、マイナスになることはありません。

</details>

<details>
<summary><strong>Q：Cloudflareにブロックされてポイントがもらえない / 1ポイントしかもらえない場合はどうすればいいですか？</strong></summary>

これは純粋なHTTPスクリプトの固有の制限です。ログにCloudflare、Turnstile、403/429/503が出る、または1ポイントしかもらえない場合、今回のチェックインは完全には完了していません。再実行しても大抵は引き続きブロックされます。

付属のブラウザ拡張機能 [`extension/`](extension/README.md) を使ってください。拡張機能は実際のブラウザで公式サイトのチェックインボタンをクリックし、公式コンポーネントがTurnstileを完了します。8つのドメイン、複数アカウント、自動交換、プッシュに対応しています。

</details>

<details>
<summary><strong>Q：チェックインに失敗するとActionsは赤くなりますか？</strong></summary>

2つの場合があります。**設定エラー**（Cookie未入力、JSON形式が不正）は0以外の終了コードで終了し、ワークフローは赤くなります。**業務上の失敗**（Cookie期限切れ `code=-2`、サーバーの5xx）は赤くなりません。偶発的な失敗で毎日誤報が出るのを避けるためです。そのため、業務上の失敗はプッシュやログで自分で確認する必要があります。

</details>

<details>
<summary><strong>Q：「残り0日」とはどういうことですか？</strong></summary>

アカウントが当日で期限切れになるということです。チェックイン自体は成功します。`GLADOS_EXCHANGE_PLAN` を設定してポイントで更新することをおすすめします。詳しくは[ポイント交換](#积分兑换)を参照してください。

</details>

<details>
<summary><strong>Q：ローカルで実行できますか？</strong></summary>

できます。デバッグ用です。依存関係をインストールし、環境変数を設定してから実行します：

```bash
pip install -r requirements.txt
export GLADOS_COOKIES='[{"cookie":"koa:sess=..."}]'
python checkin.py
```

Windows PowerShellでは `$env:GLADOS_COOKIES='[...]'` を使います。

</details>

---

## 📁ファイル構成

```text
.
├── checkin.py                    # メインプログラム：アカウントモデル / API / 設定 / レポート / プッシュ
├── logging_config.py             # ログ設定（北京時間、短いレベルラベル、マスキング）
├── requirements.txt              # 依存関係
├── README.md                     # 説明ドキュメント
├── LICENSE                       # MITライセンス
├── extension/                    # ブラウザ拡張機能：Cloudflare代替チェックイン
└── .github/workflows/
    └── gladosCheck.yml           # Actionsの定期タスク
```

## 📝更新履歴

**2026-01**

- コードをリファクタリングし、問題特定のためのログ出力を追加、新しいURLに対応、ポイント交換方針の設定に対応しました。

**2026-04**

- コードのロジックとログ出力を最適化しました。

**2026-09**

- デバイスプラットフォームの適応再試行に対応し、UAの手動設定が不要になりました。
- ネットワーク層の自動再試行とバックオフに対応しました。
- ログ時刻を北京時間に統一しました。定期タスクを1日2回に調整しました。
- 自動交換はデフォルトでオフになりました。

**2026-10**

- Server酱へのプッシュに対応しました。

**2026-11**

**1.0.1**

- 拡張機能のバージョンを `1.0.1` にアップグレードしました；
- 拡張機能内部の履歴/バージョン説明コメントを整理しました；
- `SHA256SUMS.txt`、`release-notes.md`、`debug.py` などのローカルリリース/デバッグファイルを無視するようにしました。

**1.0.0**

- GLADOS_COOKIESの形式をJSON配列に変更し、`name` / `origin` / `cookie` / `user-agent` に対応しました。旧来の `&` 形式も引き続き互換です。
- 失敗時のメッセージを改善しました。チェックイン拒否、交換未完了などの場面で実行可能な提案を出します。
- ログの自動マスキング：`koa:*` / `gld:*` / Bearer → `[REDACTED]`。
- challenge検証メカニズムを追加しました。取得に失敗した場合は旧プロトコルにフォールバックします。
- Cloudflare/Turnstileのブロックと403/429/503を識別します。ポイントが不完全な場合、ログとプッシュでブラウザ拡張機能への切り替えを促します。

## 📊 データ統計

<div align="center">

<img src="https://github-readme-stats.vercel.app/api?username=lzr52o&show_icons=true&theme=radical" width="46%" /><img src="https://github-readme-stats.vercel.app/api/top-langs/?username=lzr52o&layout=compact&theme=radical" width="50%" />

<img src="https://github-readme-streak-stats.herokuapp.com/?user=lzr52o&theme=radical" width="96%" />

<img src="https://github-readme-activity-graph.vercel.app/graph?username=lzr52o&theme=github-compact" width="100%" />

</div>

## 🛠️ 技術スタック

<div align="center">

![Skill Icons](https://skillicons.dev/icons?i=js,py,git,github,cloudflare,apple,vscode&theme=dark)

</div>

## 😄 毎日の一言

<div align="center">

<img src="https://daily-sentence.lzr52o.workers.dev/sentence" width="440" height="140" alt="毎日の一言" />

</div>

## 🎮 コントリビューショングラフ

<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://contrib-rotate.lzr52o.workers.dev/rotate?theme=dark" />
  <source media="(prefers-color-scheme: light)" srcset="https://contrib-rotate.lzr52o.workers.dev/rotate?theme=light" />
  <img alt="コントリビューショングラフ" src="https://contrib-rotate.lzr52o.workers.dev/rotate?theme=light" />
</picture>

</div>



## ⭐Starの推移

[![Star History Chart](https://api.star-history.com/svg?repos=Devilstore/Glados-Railgun-checkin&type=Date&legend=top-left)](https://star-history.com/#Devilstore/Glados-Railgun-checkin&type=Date)

---

## 📄声明

- 安定した動作を保証しません。APIやリスク管理の方針はいつでも変更される可能性があります。
- ウェブ版が使用する公開APIのみを呼び出します。キャプチャを回避しません。サーバー側のリスク管理により拒否または降格される可能性があります。
- 利用は自己責任です。本ツールの利用によるアカウントのリスク管理、権限制限、凍結については利用者自身が責任を負います。
- 本プロジェクトは [MIT](LICENSE) ライセンスで公開されています。

このプロジェクトが役に立ったら、⭐ Starをお願いします。

## 🙏 謝辞

本リポジトリに収録されているスクリプトとモジュールは、以下の原作者によるものです。ご尽力に感謝します：

[Devilstore](https://github.com/Devilstore)

## ⚠️ 免責事項

本リポジトリは個人の学習用です。すべてのスクリプトとモジュールの著作権は原作者に帰属します。転載、ミラー、再配布、商用は禁止です。原作者の情報を削除しないでください。侵害がある場合は Telegram ボット [@ccccccccoBot](https://t.me/ccccccccoBot) までご連絡いただければ削除します。

![Footer](https://capsule-render.vercel.app/api?type=waving&color=gradient&height=120&section=footer&text=GLaDOS+%E8%87%AA%E5%8A%A8%E7%AD%BE%E5%88%B0&fontSize=24&fontColor=ffffff)
