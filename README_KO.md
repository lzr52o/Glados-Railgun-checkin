[English](README_EN.md) · [简体中文](README.md) · [日本語](README_JA.md) · 한국어 · [Türkçe](README_TR.md)

![GLaDOS 자동 체크인](header.png)

<div align="center">

# GLaDOS 자동 체크인

![Typing SVG](https://readme-typing-svg.demolab.com?font=Fira+Code&size=22&pause=1000&color=0366D6&center=true&vCenter=true&width=600&lines=%E6%AC%A2%E8%BF%8E%E6%9D%A5%E5%88%B0+GLaDOS+%E8%87%AA%E5%8A%A8%E7%AD%BE%E5%88%B0%3B%E6%AF%8F%E5%A4%A9%E8%87%AA%E5%8A%A8%E5%AE%8C%E6%88%90%E7%AD%BE%E5%88%B0%3B%E7%BB%93%E6%9E%9C%E6%8E%A8%E9%80%81%E5%88%B0%E6%89%8B%E6%9C%BA)

<p><sub>매일 자동으로 체크인하고 결과를 휴대폰으로 푸시받을 수 있습니다.</sub></p>

**다중 계정** · **8개 도메인** · **Cloudflare Turnstile** 지원: GitHub Actions로 무인 운영도 가능하고, 브라우저 확장 프로그램으로 실제 브라우저에서 체크인할 수도 있습니다.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python](https://img.shields.io/badge/python-3.13-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![GLaDOS Check-in](https://github.com/Devilstore/Glados-Railgun-checkin/actions/workflows/gladosCheck.yml/badge.svg?branch=master)](https://github.com/Devilstore/Glados-Railgun-checkin/actions/workflows/gladosCheck.yml)
[![Last commit](https://img.shields.io/github/last-commit/Devilstore/Glados-Railgun-checkin?branch=master)](https://github.com/Devilstore/Glados-Railgun-checkin/commits/master)
![방문자 수](https://komarev.com/ghpvc/?username=lzr52o&color=0e75b6&style=flat)

[빠른 시작](#快速开始) · [브라우저 확장 프로그램](#-浏览器扩展cloudflare-备用方案) · [계정 설정](#配置-glados_cookies) · [포인트 교환](#积分兑换) · [푸시 알림](#消息推送) · [문제 해결](#问题排查) · [자주 묻는 질문](#常见问题faq)

</div>

---

> [!NOTE]
> 비공식 프로젝트이며 GLaDOS 공식과 무관합니다. 학습 및 교류 목적으로만 사용해 주세요.

## 🚀빠른 시작

> [!TIP]
> **GLaDOS 계정이 아직 없으신가요?**
>
> [**초대 링크**](https://glados.space/landing/0A58E-NV28S-6U3QV-33VMG)로 가입하세요. 결제 시 할인 코드 `DEVILSTORE`를 사용하면 10% 할인됩니다.

1. 이 저장소를 Fork합니다.
2. 자신의 저장소에서 `Settings` → `Secrets and variables` → `Actions` → `New repository secret`으로 이동해 최소 `GLADOS_COOKIES`를 추가합니다.
3. `Actions` → `GLaDOS Check-in` → `Run workflow`를 열어 수동으로 한 번 실행하고, 「签到成功」또는「重复签到」이 표시되면 완료입니다.
4. 이후 매일 **09:18 / 12:18**(베이징 시간)에 자동으로 실행됩니다.

| Secret | 필수 | 설명 |
| --- | :---: | --- |
| `GLADOS_COOKIES` | **예** | 계정 목록, 고정 JSON 배열. [계정 설정](#配置-glados_cookies) 참조 |
| `PUSHDEER_SENDKEY` | 아니요 | PushDeer 푸시. [푸시 알림](#消息推送) 참조 |
| `SERVERCHAN_SENDKEY` | 아니요 | Server酱 푸시. [푸시 알림](#消息推送) 참조 |
| `GLADOS_EXCHANGE_PLAN` | 아니요 | `plan100` / `plan200` / `plan500`. [포인트 교환](#积分兑换) 참조 |
| `GLADOS_VERBOSE` | 아니요 | `true` / `1` / `yes`로 상세 로그 출력 |

## 🧩 브라우저 확장 프로그램(Cloudflare 대체 방안)

### 선택 방법

| 사용 상황 | 추천 |
|---|---|
| 매일 컴퓨터를 켜고 싶지 않고 Actions 자동 실행으로 충분함 | Python / Actions |
| Cloudflare, Turnstile에 자주 걸리거나 1포인트만 받음 | 브라우저 확장 프로그램 |
| 둘 다 쓰고 싶음 | Actions는 유지하고 확장 프로그램으로 보완 체크인 |

Actions 버전은 순수 HTTP 요청이라 **Cloudflare Turnstile을 통과할 수 없습니다**. 로그에 다음과 같은 내용이 나타나면:

- `Cloudflare` / `Just a moment` / `Turnstile`;
- HTTP `403` / `429` / `503`;
- `Automated check-in detected`;
- 1포인트만 받아 정상 체크인보다 명백히 적음;

이번 체크인에서 포인트를 온전히 받지 못했다는 뜻이며, 스크립트를 다시 실행해도 대개 소용없습니다.  
대신 함께 제공되는 브라우저 확장 프로그램을 사용하세요. 실제 브라우저에서 공식 사이트의 체크인 버튼을 클릭하면 공식 컴포넌트가 Turnstile을 완료합니다.

### 설치 및 사용 방법

1. 이 저장소를 다운로드하거나 [Latest Release](https://github.com/Devilstore/Glados-Railgun-checkin/releases/latest)에서 확장 프로그램 압축 파일을 다운로드해 압축을 풉니다.
2. `edge://extensions`(Chrome은 `chrome://extensions`)를 열고 "개발자 모드"를 켭니다.
3. "압축 해제된 확장 프로그램 로드"를 클릭하고 `extension/` 디렉터리를 선택합니다.
4. 확장 프로그램 설정을 열고 "+ 계정 추가"를 클릭합니다.
   - 주 계정은 "브라우저 로그인 상태"를 선택합니다.
   - 다른 계정은 "Cookie 직접 입력"을 선택합니다.
5. 저장한 뒤 "지금 체크인"을 클릭합니다. 이후 "설정"에서 자동 체크인, 알림, 푸시를 설정할 수 있습니다.

전체 설명: [`extension/README.md`](extension/README.md)

### 버전

현재 버전: `1.0.1`.

- `extension/` 디렉터리에는 항상 **최신 소스 코드**가 들어 있으며, 개발자와 로컬 로드에 적합합니다.
- 릴리스 패키지는 버전으로 이름이 정해집니다. 예: `glados-checkin-extension-v1.0.1.zip`.
- 업그레이드할 때는 `extension/` 디렉터리 내용을 교체한 뒤 `edge://extensions`에서 "다시 로드"를 클릭하세요. 계정 데이터는 유지됩니다.
- `latest`를 유일한 파일 이름으로 생각하지 마세요. GitHub Releases의 **Latest**를 기준으로 하세요. 버전별 패키지가 롤백과 문제 해결에 더 편리합니다.

Python / Actions와 확장 프로그램은 함께 유지할 수 있습니다. Actions가 무인 운영을 맡고, 확장 프로그램이 Cloudflare 제한 하에서 내 컴퓨터에서의 보완 체크인을 맡습니다.

## 🔧GLADOS_COOKIES 설정

**JSON 배열**이어야 하며 계정당 하나의 객체로, 아래 네 가지 필드만 사용할 수 있습니다.

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

| 필드 | 필수 | 설명 |
| --- | :---: | --- |
| `cookie` | **예** | 전체 Cookie. 새 버전에서는 `gld:sess`와 `gld:sess.sig`를 함께 포함하는 것을 권장합니다 |
| `name` | 아니요 | 비고 이름. 입력하지 않으면 `账号1`로 표시됩니다 |
| `origin` | 아니요 | Cookie가 속한 도메인. 입력하지 않으면 자동 탐지됩니다 |
| `user-agent` | 아니요 | Cookie를 추출할 때의 브라우저 UA. 입력하는 것을 권장합니다 |

**핵심 사항**

- 필드 이름은 고정되어 있으며, 다른 필드가 나타나면 바로 오류가 발생합니다.
- 중복된 Cookie는 자동으로 중복 제거되어 체크인이 중복되지 않습니다.
- 기존 `&` 구분자는 계속 호환되지만 마이그레이션을 안내하는 표시가 나옵니다. 줄바꿈 구분, 문자열 배열, 단일 객체는 이미 지원되지 않습니다.

**가져오는 방법**

체크인 페이지에서 `F12` → `Network` → 새로 고침 → 아무 요청이나 선택 → `Request Headers`를 열고 `Cookie`와 `User-Agent`의 전체 값을 복사합니다.

**탐지 순서**(`origin`을 입력하지 않은 경우 아래 순서대로 시도하며, 적중하면 중단)

`glados.cloud` → `railgun.info` → `glados.network` → `glados.rocks` → `glados.one` → `glados.space` → `glados.vip` → `glados-facility.com`

탐지는 짧은 타임아웃으로 빠르게 한 바퀴 훑습니다. **네트워크 응답이 없었던** 도메인만 한 번 더 시도하고, 「권한 없음」을 반환한 도메인은 재시도하지 않습니다.

## 🎁포인트 교환

`GLADOS_EXCHANGE_PLAN`을 설정해야 실행되며, 체크인이 **성공 / 중복**인 경우에만 교환합니다.

| 등급 | 소모 포인트 | 획득 일수 |
| --- | :---: | :---: |
| `plan100` | 100 | 10일 |
| `plan200` | 200 | 30일 |
| `plan500` | 500 | 100일 |

입력하지 않거나 잘못된 값을 입력하면 교환하지 않습니다. 잔액이 부족하면 API가 실패를 반환하고 음수가 되지 않습니다.

## 📣푸시 알림

| 백엔드 | Secret | 키 가져오는 방법 |
| --- | --- | --- |
| PushDeer | `PUSHDEER_SENDKEY` | [pushdeer.com](https://www.pushdeer.com/product.html)에서 앱을 다운로드하고 앱 안에서 pushkey를 가져옵니다 |
| Server酱 | `SERVERCHAN_SENDKEY` | [sct.ftqq.com](https://sct.ftqq.com/)에서 WeChat QR 코드를 스캔해 로그인하고 페이지에서 SendKey를 가져옵니다 |

단독으로 또는 동시에 설정할 수 있습니다(둘 다 발송되며, 어느 하나라도 성공하면 성공으로 간주합니다). 둘 다 설정하지 않으면 푸시를 건너뛰고 체크인에는 영향을 주지 않습니다.

### 푸시 내용

```text
GLaDOS 签到 · 成功 1 · 失败 1 · 重复 1
#1  大号  ✅ 签到成功  glados.cloud  获得 20 积分 · 剩余 48 天 · 总 409 积分
#2  小号  🔄 重复签到  railgun.info  获得 0 积分 · 剩余 23 天 · 总 528 积分
#3  账号3  🔴 签到失败  未探测  获得 0 积分 · 剩余 None 天 · 总 None 积分
```

| 표시 | 의미 |
| --- | --- |
| `✅ 签到成功` | 이번 체크인에 성공했습니다. `获得 N 积分`는 이번에 얻은 포인트입니다 |
| `🔄 重复签到` | 오늘 이미 체크인했습니다. 정상적인 결과이며 실패가 아닙니다 |
| `🔴 签到失败` | 체크인에 실패했습니다. [문제 해결](#问题排查) 참조 |

계정 이름 열은 **다중 계정**일 때만 나타납니다.

## ⏰실행 시간 변경

기본값은 매일 **09:18 / 12:18**(베이징 시간)이며, `.github/workflows/gladosCheck.yml`의 `schedule`로 제어합니다.

```yaml
schedule:
  - cron: '18 9,12 * * *'
    timezone: 'Asia/Shanghai'
```

`cron`을 바꾸면 시간을 조정할 수 있습니다. `timezone` 덕분에 GitHub이 베이징 시간으로 해석합니다. 예를 들어 아침 한 번만 남기려면:

```yaml
schedule:
  - cron: '18 9 * * *'
    timezone: 'Asia/Shanghai'
```

## 🔍문제 해결

**정상 로그**

| 로그 조각 | 설명 |
| --- | --- |
| `✅ 命中登录域名（剩余 N 天）` | 탐지에 성공했습니다. 이후에는 해당 도메인에서만 체크인합니다 |
| `✅ 重试后命中登录域名（剩余 N 天）` | 해당 도메인은 처음에 응답이 없었지만 재시도 후 성공했습니다 |
| `[REDACTED]` | 마스킹 표시. 자격 증명이 대체되었습니다 |

**비정상 로그**

| 로그 조각 | 원인과 처리 방법 |
| --- | --- |
| `候选域名均返回「没有权限」` | Cookie가 만료되었거나 계정이 후보 도메인에 없음 → 다시 로그인하고 Cookie를 업데이트하세요 |
| `候选域名均未获得服务端响应` | 네트워크/서버 장애이며 Cookie 문제가 아님 → 나중에 다시 실행하세요 |
| `设备平台不匹配(loginDevice=…)` | UA와 로그인 기기가 일치하지 않음 → `user-agent` 필드를 보완하세요 |
| `签到被拒：please checkin via …` | 토큰과 도메인이 맞지 않음(대부분 서버 측 규칙 변경) → Cookie의 출처 도메인을 확인하세요 |
| `无权限(code=-2)` | 해당 Cookie가 만료되었거나 계정이 위험 관리 대상입니다 |
| `Cloudflare` / `Just a moment` / `Turnstile` / HTTP `403` `429` `503` | Cloudflare 차단 또는 속도 제한으로 순수 HTTP로는 인증을 완료할 수 없음 → **브라우저 확장 프로그램으로 보완 체크인**하세요. [브라우저 확장 프로그램](#-浏览器扩展cloudflare-备用方案) 참조 |
| `Automated check-in detected` / 1포인트만 받음 | GLaDOS가 자동 체크인으로 판단했습니다. 스크립트는 challenge에 이미 대응했지만 TLS/HTTP 지문, IP, 기기 등이 여전히 식별될 수 있습니다 → **브라우저 확장 프로그램**을 사용해 실제 브라우저에서 인증을 완료하세요 |
| `运行超出预算` | 한 라운드가 시간 초과되어 남은 계정이 건너뛰어졌습니다 → 다음 라운드나 수동으로 다시 실행하세요 |
| `积分请求失败 / 响应解析失败 / 格式异常 / 字段无法解析` | 총 포인트 조회 실패 → 푸시에 `总 None 积分`로 표시되지만 **체크인 결과에는 영향을 주지 않습니다**. 무시해도 됩니다 |

> 푸시에 `总 None 积分`이 나타나는 것은 이번에 총 포인트를 가져오지 못했다는 뜻일 뿐입니다(해당 API는 가끔 느립니다). 체크인 성공 여부와는 무관합니다. 확인하려면 수동으로 한 번 다시 실행하세요.

## ❓자주 묻는 질문(FAQ)

<details>
<summary><strong>Q: 한 번 실행하는 데 얼마나 걸리나요?</strong></summary>

보통 몇 초에서 수십 초입니다. 한 라운드 예산은 300초(약 5분)이며, 초과하면 남은 계정은 건너뜁니다.

</details>

<details>
<summary><strong>Q: Cookie는 얼마나 지나면 만료되나요?</strong></summary>

고정된 기한이 없습니다. 로그아웃, 비밀번호 변경, 서버 측 세션 정책 조정 등으로 더 일찍 무효화될 수 있습니다. 「候选域名均返回没有权限」이나 `code=-2`가 나타나면 다시 로그인하고 `GLADOS_COOKIES`를 업데이트하세요.

</details>

<details>
<summary><strong>Q: 여러 계정을 지원하나요?</strong></summary>

지원합니다. 수량 제한이 없습니다. JSON 배열에 항목을 더 추가하기만 하면 됩니다. 중복된 Cookie는 자동으로 중복 제거되어 체크인이 중복되지 않습니다.

</details>

<details>
<summary><strong>Q: 각 Cookie가 어느 도메인에 속하는지 표시해야 하나요?</strong></summary>

필요 없습니다. 스크립트가 자동으로 탐지합니다. `origin`을 입력하면 탐지 요청이 줄고 로그가 더 명확해집니다. 참고로 **각 사이트의 Cookie는 보통 서로 호환되지 않습니다**. 각 Cookie는 실제로 로그인한 사이트에서 가져온 것이어야 합니다.

</details>

<details>
<summary><strong>Q: <code>origin</code>을 잘못 입력하면 어떻게 되나요?</strong></summary>

실패하지 않습니다. `origin`은 **우선 시도**일 뿐입니다. 적중하면 바로 체크인하고, 「권한 없음」을 반환하면 자동으로 되돌아가 내장된 8개 도메인을 모두 순회합니다.

</details>

<details>
<summary><strong>Q: 다중 계정일 때 로그가 어느 계정 것인지 어떻게 구분하나요?</strong></summary>

`name`을 입력하지 않으면 순서대로 자동 이름이 붙어 `🍪[账号1]`, `🍪[账号2]`로 표시됩니다. 입력하면 `🍪[1:大号]`, `🍪[2:小号]`, `🍪[3:账号3]`로 표시되며, 번호는 요약의 `#1` `#2` `#3`과 대응합니다.

</details>

<details>
<summary><strong>Q: 계정 하나가 몇 개의 작업을 실행하나요?</strong></summary>

하나입니다. 스크립트는 먼저 도메인을 탐지한 뒤 해당 도메인에서만 체크인하므로 작업 수는 계정 수와 같습니다.

</details>

<details>
<summary><strong>Q: "중복 체크인"은 성공인가요, 실패인가요?</strong></summary>

정상적인 결과입니다. 오늘 이미 체크인했다는 뜻이며 서버는 `code=1`을 반환합니다. 요약에서는 "중복"으로 따로 집계되며 실패에 포함되지 않습니다.

</details>

<details>
<summary><strong>Q: 기기 불일치(code=4)가 발생하면 어떻게 해야 하나요?</strong></summary>

GLaDOS는 UA와 로그인 기기를 검증합니다. `user-agent`를 입력했다면 그대로 사용합니다. 입력하지 않았다면 기본 UA를 사용하고, 실패 시 `loginDevice`를 기준으로 자동으로 한 번 재시도합니다. 그래도 실패하면 `user-agent` 보완을 안내합니다. **Cookie를 추출할 때 사용한 브라우저**의 UA를 입력하세요.

</details>

<details>
<summary><strong>Q: Actions는 초록색인데 알림이 오지 않아요.</strong></summary>

먼저 해당 Secret의 철자가 맞는지 확인하세요(로그에「已配置 N 个后端」이 출력됩니다). 참고로 Server酱이 `code=0`을 반환하는 것은 **작업이 대기열에 들어갔다**는 뜻일 뿐, 전달되었다는 뜻이 아닙니다. 오랫동안 도착하지 않으면 해당 SendKey가 WeChat에 연결되어 있는지 확인하세요.

</details>

<details>
<summary><strong>Q: 공개 저장소의 예약 작업이 스스로 멈추나요?</strong></summary>

네. GitHub에는 "공개 저장소는 연속 60일 동안 활동이 없으면 schedule이 비활성화된다"는 규칙이 있습니다. 이 저장소에는 keepalive 단계가 내장되어 정기적으로 활동을 발생시키지만 100% 보장되지는 않습니다. 멈춘 것을 발견하면 Actions 페이지에서 다시 활성화하세요.

</details>

<details>
<summary><strong>Q: 교환을 설정하면 포인트가 멋대로 쓰이지 않나요?</strong></summary>

`GLADOS_EXCHANGE_PLAN`으로 지정한 등급만 실행합니다. **입력하지 않으면 교환하지 않습니다**. 또한 체크인 성공/중복인 경우에만 교환합니다. 잔액이 부족하면 API가 실패를 반환하고 음수가 되지 않습니다.

</details>

<details>
<summary><strong>Q: Cloudflare에 차단되어 포인트를 못 받거나 1포인트만 받으면 어떻게 해야 하나요?</strong></summary>

순수 HTTP 스크립트의 고유한 한계입니다. 로그에 Cloudflare, Turnstile, 403/429/503이 나타나거나 1포인트만 받으면 이번 체크인이 완전히 끝나지 않은 것입니다. 다시 실행해도 대개 계속 차단됩니다.

함께 제공되는 브라우저 확장 프로그램 [`extension/`](extension/README.md)을 사용하세요. 확장 프로그램은 실제 브라우저에서 공식 사이트의 체크인 버튼을 클릭하고 공식 컴포넌트가 Turnstile을 완료합니다. 8개 도메인, 다중 계정, 자동 교환, 푸시를 지원합니다.

</details>

<details>
<summary><strong>Q: 체크인에 실패하면 Actions가 빨간색이 되나요?</strong></summary>

두 가지 경우로 나뉩니다. **설정 오류**(Cookie 미입력, JSON 형식 오류)는 0이 아닌 종료 코드로 끝나 워크플로가 빨간색이 됩니다. **업무상 실패**(Cookie 만료 `code=-2`, 서버 5xx)는 빨간색이 되지 않습니다. 가끔 발생하는 실패로 매일 오보가 나는 것을 피하기 위해서입니다. 따라서 업무상 실패는 푸시나 로그로 직접 확인해야 합니다.

</details>

<details>
<summary><strong>Q: "남은 0일"은 무슨 뜻인가요?</strong></summary>

계정이 오늘 만료된다는 뜻입니다. 체크인 자체는 여전히 성공합니다. `GLADOS_EXCHANGE_PLAN`을 설정해 포인트로 갱신하는 것이 좋습니다. 자세한 내용은 [포인트 교환](#积分兑换)을 참조하세요.

</details>

<details>
<summary><strong>Q: 로컬에서 실행할 수 있나요?</strong></summary>

가능합니다. 디버깅용입니다. 종속성을 설치하고 환경 변수를 설정한 뒤 실행하세요.

```bash
pip install -r requirements.txt
export GLADOS_COOKIES='[{"cookie":"koa:sess=..."}]'
python checkin.py
```

Windows PowerShell에서는 `$env:GLADOS_COOKIES='[...]'`를 사용하세요.

</details>

---

## 📁파일 구조

```text
.
├── checkin.py                    # 주 프로그램: 계정 모델 / API / 설정 / 보고서 / 푸시
├── logging_config.py             # 로그 설정(베이징 시간, 짧은 수준 태그, 마스킹)
├── requirements.txt              # 종속성
├── README.md                     # 설명 문서
├── LICENSE                       # MIT 라이선스
├── extension/                    # 브라우저 확장 프로그램: Cloudflare 대체 체크인
└── .github/workflows/
    └── gladosCheck.yml           # Actions 예약 작업
```

## 📝업데이트 로그

**2026-01**

- 코드를 리팩터링하고, 문제 파악을 위한 로그 출력을 추가하고, 새 URL을 지원하고, 포인트 교환 전략 설정을 지원했습니다.

**2026-04**

- 코드 로직과 로그 출력을 최적화했습니다.

**2026-09**

- 기기 플랫폼 적응형 재시도에 대응해 UA 수동 설정이 필요 없어졌습니다.
- 네트워크 계층 자동 재시도 및 백오프에 대응했습니다.
- 로그 시간을 베이징 시간으로 통일했습니다. 예약 작업을 하루 2회로 조정했습니다.
- 자동 교환은 기본적으로 꺼져 있습니다.

**2026-10**

- Server酱 푸시에 대응했습니다.

**2026-11**

**1.0.1**

- 확장 프로그램 버전을 `1.0.1`로 업그레이드했습니다.
- 확장 프로그램 내부의 기록/버전 설명 주석을 정리했습니다.
- `SHA256SUMS.txt`, `release-notes.md`, `debug.py` 등 로컬 릴리스/디버그 파일을 무시하도록 했습니다.

**1.0.0**

- GLADOS_COOKIES 형식을 JSON 배열로 바꾸고 `name` / `origin` / `cookie` / `user-agent`를 지원합니다. 기존 `&` 형식도 계속 호환됩니다.
- 실패 원인 안내를 최적화했습니다. 체크인 거부, 교환 미완료 등의 상황에서 실행 가능한 제안을 제공합니다.
- 로그 자동 마스킹: `koa:*` / `gld:*` / Bearer → `[REDACTED]`.
- challenge 검증 메커니즘을 추가했습니다. 가져오기에 실패하면 기존 프로토콜로 되돌아갑니다.
- Cloudflare/Turnstile 차단과 403/429/503을 식별합니다. 포인트가 불완전하면 로그와 푸시에서 브라우저 확장 프로그램으로 바꾸도록 안내합니다.

## 📊 데이터 통계

<div align="center">

<img src="https://github-readme-stats.vercel.app/api?username=lzr52o&show_icons=true&theme=radical" width="46%" /><img src="https://github-readme-stats.vercel.app/api/top-langs/?username=lzr52o&layout=compact&theme=radical" width="50%" />

<img src="https://github-readme-streak-stats.herokuapp.com/?user=lzr52o&theme=radical" width="96%" />

<img src="https://github-readme-activity-graph.vercel.app/graph?username=lzr52o&theme=github-compact" width="100%" />

</div>

## 🛠️ 기술 스택

<div align="center">

![Skill Icons](https://skillicons.dev/icons?i=js,py,git,github,cloudflare,apple,vscode&theme=dark)

</div>

## 😄 오늘의 한마디

<div align="center">

<img src="https://www.dmoe.cc/random.php" width="160" height="140" alt="랜덤 애니메이션 이미지" /><img src="https://daily-sentence.lzr52o.workers.dev/sentence" width="440" height="140" alt="오늘의 한마디" /><img src="https://www.dmoe.cc/random.php?v=2" width="160" height="140" alt="랜덤 애니메이션 이미지" />

</div>

## 🎮 기여 그래프

<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://contrib-rotate.lzr52o.workers.dev/rotate?theme=dark" />
  <source media="(prefers-color-scheme: light)" srcset="https://contrib-rotate.lzr52o.workers.dev/rotate?theme=light" />
  <img alt="기여 그래프" src="https://contrib-rotate.lzr52o.workers.dev/rotate?theme=light" />
</picture>

</div>



## ⭐Star 추이

[![Star History Chart](https://api.star-history.com/svg?repos=Devilstore/Glados-Railgun-checkin&type=Date&legend=top-left)](https://star-history.com/#Devilstore/Glados-Railgun-checkin&type=Date)

---

## 📄고지 사항

- 안정적인 실행을 보장하지 않습니다. API와 위험 관리 정책은 언제든 조정될 수 있습니다.
- 웹 버전이 사용하는 공개 API만 호출합니다. 인증 코드를 우회하지 않습니다. 서버 측 위험 관리로 거부되거나 등급이 낮아질 수 있습니다.
- 사용에 따른 위험은 본인이 부담합니다. 이 도구 사용으로 인한 계정 위험 관리, 권한 제한 또는 차단은 사용자가 직접 책임집니다.
- 이 프로젝트는 [MIT](LICENSE) 라이선스로 배포됩니다.

이 프로젝트가 도움이 되었다면 ⭐ Star를 눌러 주세요.

## 🙏 감사

이 저장소에 수록된 스크립트와 모듈은 다음 원작자들의 것입니다. 수고에 감사드립니다：

[Devilstore](https://github.com/Devilstore)

## ⚠️ 면책 조항

이 저장소는 개인 학습용입니다. 모든 스크립트와 모듈의 저작권은 원작자에게 있습니다. 전재, 미러링, 재배포, 상업적 이용을 금지합니다. 원작자 정보를 삭제하지 마세요. 침해가 있으면 Telegram 봇 [@ccccccccoBot](https://t.me/ccccccccoBot)으로 연락 주시면 삭제하겠습니다.

![Footer](https://capsule-render.vercel.app/api?type=waving&color=gradient&height=120&section=footer&text=GLaDOS+%E8%87%AA%E5%8A%A8%E7%AD%BE%E5%88%B0&fontSize=24&fontColor=ffffff)
