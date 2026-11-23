[English](README_EN.md) · [简体中文](README.md) · [日本語](README_JA.md) · [한국어](README_KO.md) · Türkçe

![GLaDOS Otomatik Check-in](header.png)

<div align="center">

# GLaDOS Otomatik Check-in

![Typing SVG](https://readme-typing-svg.demolab.com?font=Fira+Code&size=22&pause=1000&color=0366D6&center=true&vCenter=true&width=600&lines=%E6%AC%A2%E8%BF%8E%E6%9D%A5%E5%88%B0+GLaDOS+%E8%87%AA%E5%8A%A8%E7%AD%BE%E5%88%B0%3B%E6%AF%8F%E5%A4%A9%E8%87%AA%E5%8A%A8%E5%AE%8C%E6%88%90%E7%AD%BE%E5%88%B0%3B%E7%BB%93%E6%9E%9C%E6%8E%A8%E9%80%81%E5%88%B0%E6%89%8B%E6%9C%BA)

<p><sub>Her gün otomatik olarak check-in yapar, sonuçlar telefona gönderilebilir.</sub></p>

**Çoklu hesap** · **8 alan adı** · **Cloudflare Turnstile** desteği: GitHub Actions ile gözetimsiz çalıştırın ya da tarayıcı uzantısıyla gerçek tarayıcınızdan check-in yapın.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python](https://img.shields.io/badge/python-3.13-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![GLaDOS Check-in](https://github.com/Devilstore/Glados-Railgun-checkin/actions/workflows/gladosCheck.yml/badge.svg?branch=master)](https://github.com/Devilstore/Glados-Railgun-checkin/actions/workflows/gladosCheck.yml)
[![Last commit](https://img.shields.io/github/last-commit/Devilstore/Glados-Railgun-checkin?branch=master)](https://github.com/Devilstore/Glados-Railgun-checkin/commits/master)
![Ziyaret sayısı](https://komarev.com/ghpvc/?username=lzr52o&color=0e75b6&style=flat)

[Hızlı Başlangıç](#快速开始) · [Tarayıcı Uzantısı](#-浏览器扩展cloudflare-备用方案) · [Hesap Yapılandırması](#配置-glados_cookies) · [Puan Dönüştürme](#积分兑换) · [Bildirim Gönderimi](#消息推送) · [Sorun Giderme](#问题排查) · [SSS](#常见问题faq)

</div>

---

> [!NOTE]
> Resmî olmayan bir projedir, GLaDOS resmî sitesiyle ilgisi yoktur. Yalnızca öğrenme ve paylaşım amaçlıdır.

## 📖Kullanım Kılavuzu

**Kısaca:** Bu betik, her gün GLaDOS check-in işlemini sizin için otomatik olarak yapar. Check-in ile kazandığınız puanları üyelik günüyle değiştirebilirsiniz; her gün siteyi açıp kendiniz check-in yapmanıza gerek kalmaz.

### İki kullanım yolu var, birini seçmeniz yeterli

| Yöntem | Kimler için | Dikkat edilecekler |
| --- | --- | --- |
| **GitHub Actions**: depoyu fork edip bulutta zamanlanmış olarak çalıştırma | Bilgisayarı açık bırakmadan tam otomatik isteyenler | Düz HTTP istekleri Cloudflare Turnstile doğrulamasını geçemez |
| **Tarayıcı uzantısı** (v1.0.1): kendi bilgisayarınızdaki gerçek tarayıcıda check-in | Cloudflare engeline takıldığınızda veya yalnızca 1 puan aldığınızda | Otomatik check-in için tarayıcının açık kalması gerekir |

İkisini birlikte de bırakabilirsiniz: normalde Actions otomatik çalışsın, Cloudflare engellediğinde uzantıyla tamamlayın. Ayrıntılı karşılaştırma için bkz. [Tarayıcı Uzantısı](#tarayıcı-uzantısı-cloudflare-yedek-çözümü).

### Neler gerekli?

- Bir GLaDOS hesabı (yoksa [davet bağlantısıyla](https://glados.space/landing/0A58E-NV28S-6U3QV-33VMG) kaydolabilirsiniz; ödeme sırasında `DEVILSTORE` indirim kodunu kullanırsanız %10 indirim alırsınız);
- Hesabınızın çerezi (Cookie): GLaDOS sitesine giriş yaptıktan sonra `F12` → `Network` → sayfayı yenileyin ve herhangi bir isteğin `Request Headers` bölümünden `Cookie` değerinin tamamını kopyalayın;
- Bir GitHub hesabı (yalnızca Actions yöntemi için gerekir).

### Üç adımda başlayın (Actions yöntemi)

1. **Bu depoyu fork edin** ve kendi hesabınıza kopyalayın.
2. Fork ettiğiniz depoda `Settings` → `Secrets and variables` → `Actions` → `New repository secret` yolunu izleyip `GLADOS_COOKIES` adında bir Secret ekleyin. Değer, hesap başına bir öğe içeren bir JSON dizisidir; yalnızca `cookie` yazmanız yeterlidir:

   ```json
   [{"cookie": "koa:sess=...; koa:sess.sig=...; gld:sess=..."}]
   ```

   Birden fazla hesap için öğe eklemeniz yeterlidir. 8 alan adını destekler; çerezin hangi siteye ait olduğunu betik otomatik olarak algılar. Tüm alanların açıklaması için bkz. [GLADOS_COOKIES Yapılandırması](#glados_cookies-yapılandırması).
3. `Actions` → `GLaDOS Check-in` → `Run workflow` yolunu açıp bir kez el ile çalıştırın. 「签到成功」veya「重复签到」görürseniz tamamdır.

### Ne zaman çalışır?

Kurulum bittiğinde her gün **09:18 ve 12:18 (Pekin saati)** olmak üzere günde iki kez otomatik çalışır; başka bir şey yapmanıza gerek yoktur. Saati değiştirmek isterseniz `.github/workflows/gladosCheck.yml` dosyasındaki `cron` değerini düzenleyin; ayrıntılar için bkz. [Çalışma Saatini Değiştirme](#çalışma-saatini-değiştirme). İstediğiniz zaman Actions sayfasından `Run workflow` ile el ile de çalıştırabilirsiniz.

### Çalışma başarısız olursa ne yapmalı?

Önce Actions günlüğüne veya bildirim mesajına bakın, sonra yazana göre hareket edin:

- `候选域名均返回「没有权限」` veya `code=-2` görürseniz: çerezin süresi dolmuştur. Siteye yeniden giriş yapıp yukarıdaki yöntemle yeni bir çerez alın ve `GLADOS_COOKIES` değerini güncelleyin;
- `Cloudflare` / `Just a moment` / `Turnstile` görürseniz, HTTP `403` / `429` / `503` alırsanız veya yalnızca 1 puan alırsanız: bu bir Cloudflare engelidir. Actions'ın düz HTTP istekleri bunu aşamaz; yeniden çalıştırmak işe yaramaz, [tarayıcı uzantısıyla](#tarayıcı-uzantısı-cloudflare-yedek-çözümü) gerçek tarayıcıdan check-in yapın;
- 「重复签到」görürseniz: bu bir hata değildir; bugün zaten check-in yaptığınız anlamına gelir;
- Bildirimde `总 None 积分` görürseniz: bu sefer toplam puan alınamamış demektir. Check-in sonucunu etkilemez, yok sayabilirsiniz.

Her günlük satırının anlamı için bkz. [Sorun Giderme](#sorun-giderme).

### İsteğe bağlı ek ayarlar

- **Otomatik puan dönüştürme**: `GLADOS_EXCHANGE_PLAN` adında bir Secret ekleyip `plan100` / `plan200` / `plan500` yazarsanız, başarılı (veya yinelenen) bir check-in sonrasında 100 / 200 / 500 puan otomatik olarak 10 / 30 / 100 güne dönüştürülür. Boş bırakırsanız dönüştürme yapılmaz; bakiye yetersizse istek başarısız olur, bakiye eksiye düşmez. Ayrıntılar için bkz. [Puan Dönüştürme](#puan-dönüştürme).
- **Sonucu telefona gönderme**: `PUSHDEER_SENDKEY` (PushDeer) veya `SERVERCHAN_SENDKEY` (ServerChan) ayarlayın; ikisini birlikte de ayarlayabilirsiniz. Ayrıntılar için bkz. [Bildirim Gönderimi](#bildirim-gönderimi).

### Sık sorulan sorular

- **Birden fazla hesap destekleniyor mu?** Evet, `GLADOS_COOKIES` içine öğe eklemeniz yeterli. Yinelenen çerezler otomatik olarak kaldırılır.
- **Check-in başarısız olursa Actions kırmızıya döner mi?** Yapılandırma hatası (çerez girilmemesi, JSON hatası) varsa evet. Çerez süresinin dolması veya sunucu hatası gibi iş hatalarında kırmızıya dönmez; bildirimleri veya günlükleri kendiniz takip etmelisiniz.
- **Zamanlanmış çalışma aniden durdu.** GitHub, herkese açık depolarda 60 gün etkinlik olmazsa zamanlanmış çalışmaları devre dışı bırakır. Bu depoda yerleşik bir keepalive vardır ama %100 garanti vermez; durduğunu fark ederseniz Actions sayfasından yeniden etkinleştirin.
- **Kendi bilgisayarımda çalıştırabilir miyim?** Evet, hata ayıklama için: `pip install -r requirements.txt` çalıştırın, `GLADOS_COOKIES` ortam değişkenini ayarlayın ve `python checkin.py` çalıştırın.

Tüm soru ve yanıtlar için bkz. [Sık Sorulan Sorular](#sık-sorulan-sorular-sss).

## 🚀Hızlı Başlangıç

> [!TIP]
> **Henüz GLaDOS hesabınız yok mu?**
>
> [**Davet bağlantısıyla**](https://glados.space/landing/0A58E-NV28S-6U3QV-33VMG) kaydolun; ödeme sırasında `DEVILSTORE` indirim kodunu kullanırsanız %10 indirim alırsınız.

1. Bu depoyu Fork edin.
2. Kendi deponuzda `Settings` → `Secrets and variables` → `Actions` → `New repository secret` bölümüne gidin ve en az `GLADOS_COOKIES` ekleyin.
3. `Actions` → `GLaDOS Check-in` → `Run workflow` bölümünü açıp bir kez el ile çalıştırın; 「签到成功」veya「重复签到」görürseniz kurulum tamamdır.
4. Sonrasında her gün **09:18 / 12:18** (Pekin saati) otomatik olarak çalışır.

| Secret | Zorunlu | Açıklama |
| --- | :---: | --- |
| `GLADOS_COOKIES` | **Evet** | Hesap listesi, sabit JSON dizisi. Bkz. [Hesap Yapılandırması](#配置-glados_cookies) |
| `PUSHDEER_SENDKEY` | Hayır | PushDeer bildirimi. Bkz. [Bildirim Gönderimi](#消息推送) |
| `SERVERCHAN_SENDKEY` | Hayır | Server酱 bildirimi. Bkz. [Bildirim Gönderimi](#消息推送) |
| `GLADOS_EXCHANGE_PLAN` | Hayır | `plan100` / `plan200` / `plan500`. Bkz. [Puan Dönüştürme](#积分兑换) |
| `GLADOS_VERBOSE` | Hayır | Ayrıntılı günlük için `true` / `1` / `yes` |

## 🧩 Tarayıcı Uzantısı (Cloudflare Yedek Çözümü)

### Nasıl Seçilir

| Kullanım Senaryosu | Öneri |
|---|---|
| Her gün bilgisayar açmak istemiyorum, Actions otomatik çalışsın yeter | Python / Actions |
| Sık sık Cloudflare, Turnstile engeline takılıyorum ya da yalnızca 1 puan alıyorum | Tarayıcı uzantısı |
| İkisini birden istiyorum | Actions kalsın, ayrıca uzantıyla telafi check-in yapın |

Actions sürümü saf HTTP istekleri kullanır ve **Cloudflare Turnstile'dan geçemez**. Günlükte şunlar görünürse:

- `Cloudflare` / `Just a moment` / `Turnstile`;
- HTTP `403` / `429` / `503`;
- `Automated check-in detected`;
- Yalnızca 1 puan, normal check-in'den açıkça az;

bu check-in'de puanların tamamının alınamadığı anlamına gelir; betiği yeniden çalıştırmak da genelde işe yaramaz.  
Bunun yerine birlikte gelen tarayıcı uzantısını kullanın: gerçek bir tarayıcıda resmî sitenin check-in düğmesine tıklayın, Turnstile'ı resmî bileşen tamamlasın.

### Kurulum ve Kullanım

1. Bu depoyu indirin ya da [Latest Release](https://github.com/Devilstore/Glados-Railgun-checkin/releases/latest) sayfasından uzantı arşivini indirip çıkarın;
2. `edge://extensions` (Chrome'da `chrome://extensions`) adresini açıp "Geliştirici modu"nu etkinleştirin;
3. "Paketlenmemiş uzantı yükle"ye tıklayıp `extension/` dizinini seçin;
4. Uzantı ayarlarını açıp "+ Hesap ekle"ye tıklayın:
   - Ana hesap için "Tarayıcı oturumu"nu seçin;
   - Diğer hesaplar için "Cookie'yi el ile gir"i seçin;
5. Kaydettikten sonra "Şimdi check-in yap"a tıklayın. Ardından "Ayarlar" bölümünden otomatik check-in, bildirim ve gönderim ayarlarını yapabilirsiniz.

Tam açıklama: [`extension/README.md`](extension/README.md)

### Sürüm

Geçerli sürüm: `1.0.1`.

- `extension/` dizini her zaman **en güncel kaynak kodu** içerir; geliştiriciler ve yerel yükleme için uygundur;
- Sürüm paketleri sürüm numarasıyla adlandırılır, ör. `glados-checkin-extension-v1.0.1.zip`;
- Yükseltirken `extension/` dizininin içeriğini değiştirip `edge://extensions` sayfasında "Yeniden yükle"ye tıklayın; hesap verileriniz korunur;
- `latest`i tek dosya adı sanmayın; GitHub Releases'taki **Latest** sürümü esas alın; sürümlü paketler geri alma ve sorun gidermeyi kolaylaştırır.

Python / Actions ve uzantı birlikte tutulabilir: Actions gözetimsiz çalıştırmayı, uzantı ise Cloudflare kısıtları altında kendi bilgisayarınızdaki telafi check-in'leri üstlenir.

## 🔧GLADOS_COOKIES Yapılandırması

**JSON dizisi** olmalıdır; hesap başına bir nesne ve yalnızca aşağıdaki dört alan kullanılabilir:

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

| Alan | Zorunlu | Açıklama |
| --- | :---: | --- |
| `cookie` | **Evet** | Tam Cookie; yeni sürümde `gld:sess` ve `gld:sess.sig` değerlerinin birlikte eklenmesi önerilir |
| `name` | Hayır | Açıklama adı; boş bırakılırsa `账号1` görünür |
| `origin` | Hayır | Cookie'nin ait olduğu alan adı; boş bırakılırsa otomatik algılanır |
| `user-agent` | Hayır | Cookie alınırken kullanılan tarayıcının UA değeri; doldurulması önerilir |

**Önemli Noktalar**

- Alan adları sabittir; başka bir alan görünürse doğrudan hata verir.
- Yinelenen Cookie'ler otomatik olarak tekilleştirilir, iki kez check-in yapılmaz.
- Eski `&` ayracı hâlâ desteklenir ama geçiş uyarısı gösterir; satır sonu ayracı, dize dizisi ve tek nesne artık desteklenmez.

**Nasıl Alınır**

Check-in sayfasında `F12` → `Network` → yenile → herhangi bir isteği seç → `Request Headers` bölümünü açıp `Cookie` ve `User-Agent` değerlerinin tamamını kopyalayın.

**Algılama Sırası** (`origin` girilmediğinde bu sırayla denenir, isabet edince durur)

`glados.cloud` → `railgun.info` → `glados.network` → `glados.rocks` → `glados.one` → `glados.space` → `glados.vip` → `glados-facility.com`

Algılama, kısa zaman aşımlarıyla hızlıca bir tur atar; yalnızca **ağ yanıtı alınamayan** alan adları bir kez daha denenir, "yetki yok" döndüren alan adları yeniden denenmez.

## 🎁Puan Dönüştürme

Yalnızca `GLADOS_EXCHANGE_PLAN` ayarlandığında çalışır ve yalnızca check-in **başarılı / yinelenen** olduğunda dönüştürür.

| Kademe | Harcanan Puan | Kazanılan Gün |
| --- | :---: | :---: |
| `plan100` | 100 | 10 gün |
| `plan200` | 200 | 30 gün |
| `plan500` | 500 | 100 gün |

Boş bırakılırsa veya geçersiz değer girilirse dönüştürme yapılmaz; bakiye yetersizse API hata döndürür ve bakiye eksiye düşmez.

## 📣Bildirim Gönderimi

| Arka Uç | Secret | Anahtarı Alma Yolu |
| --- | --- | --- |
| PushDeer | `PUSHDEER_SENDKEY` | [pushdeer.com](https://www.pushdeer.com/product.html) adresinden uygulamayı indirin, uygulama içinden pushkey alın |
| Server酱 | `SERVERCHAN_SENDKEY` | [sct.ftqq.com](https://sct.ftqq.com/) adresinde WeChat QR kodunu tarayarak giriş yapın, sayfadan SendKey alın |

Tek başına veya birlikte yapılandırılabilir (ikisine de gönderilir; herhangi biri başarılı olursa başarılı sayılır); hiçbiri yapılandırılmazsa bildirim atlanır, check-in etkilenmez.

### Bildirim İçeriği

```text
GLaDOS 签到 · 成功 1 · 失败 1 · 重复 1
#1  大号  ✅ 签到成功  glados.cloud  获得 20 积分 · 剩余 48 天 · 总 409 积分
#2  小号  🔄 重复签到  railgun.info  获得 0 积分 · 剩余 23 天 · 总 528 积分
#3  账号3  🔴 签到失败  未探测  获得 0 积分 · 剩余 None 天 · 总 None 积分
```

| İşaret | Anlamı |
| --- | --- |
| `✅ 签到成功` | Bu check-in başarılı oldu; `获得 N 积分` bu sefer kazanılan puandır |
| `🔄 重复签到` | Bugün zaten check-in yapılmış; normal bir sonuçtur, başarısızlık değildir |
| `🔴 签到失败` | Check-in başarısız oldu; bkz. [Sorun Giderme](#问题排查) |

Hesap adı sütunu yalnızca **çoklu hesap** durumunda görünür.

## ⏰Çalışma Saatini Değiştirme

Varsayılan olarak her gün **09:18 / 12:18** (Pekin saati) çalışır; `.github/workflows/gladosCheck.yml` dosyasındaki `schedule` ile kontrol edilir:

```yaml
schedule:
  - cron: '18 9,12 * * *'
    timezone: 'Asia/Shanghai'
```

Saati ayarlamak için `cron` değerini değiştirin; `timezone` sayesinde GitHub bunu Pekin saati olarak yorumlar. Örneğin yalnızca sabah çalışmasını istiyorsanız:

```yaml
schedule:
  - cron: '18 9 * * *'
    timezone: 'Asia/Shanghai'
```

## 🔍Sorun Giderme

**Normal Günlükler**

| Günlük Parçası | Açıklama |
| --- | --- |
| `✅ 命中登录域名（剩余 N 天）` | Algılama başarılı; sonraki check-in'ler yalnızca o alan adında yapılır |
| `✅ 重试后命中登录域名（剩余 N 天）` | O alan adı ilk seferde yanıt vermedi, yeniden denemede başarılı oldu |
| `[REDACTED]` | Maskeleme işareti; kimlik bilgileri değiştirilmiştir |

**Anormal Günlükler**

| Günlük Parçası | Nedeni ve Çözümü |
| --- | --- |
| `候选域名均返回「没有权限」` | Cookie süresi dolmuş ya da hesap aday alan adlarında değil → yeniden giriş yapıp Cookie'yi güncelleyin |
| `候选域名均未获得服务端响应` | Ağ/sunucu arızası, Cookie sorunu değil → daha sonra yeniden çalıştırın |
| `设备平台不匹配(loginDevice=…)` | UA, giriş yapılan cihazla uyuşmuyor → `user-agent` alanını ekleyin |
| `签到被拒：please checkin via …` | Token alan adıyla uyuşmuyor (çoğunlukla sunucu tarafı kural değişikliği) → Cookie'nin alındığı alan adını doğrulayın |
| `无权限(code=-2)` | Bu Cookie süresi dolmuş ya da hesap risk kontrolünde |
| `Cloudflare` / `Just a moment` / `Turnstile` / HTTP `403` `429` `503` | Cloudflare engeli veya hız sınırı; saf HTTP doğrulamayı tamamlayamaz → **tarayıcı uzantısıyla telafi check-in yapın**, bkz. [Tarayıcı Uzantısı](#-浏览器扩展cloudflare-备用方案) |
| `Automated check-in detected` / yalnızca 1 puan | GLaDOS otomatik check-in algıladı; betik challenge'a zaten uyum sağlıyor ama TLS/HTTP parmak izi, IP, cihaz vb. yine de tanınabilir → **tarayıcı uzantısını kullanın** ve doğrulamayı gerçek tarayıcıda tamamlayın |
| `运行超出预算` | Tek tur zaman aşımına uğradı, kalan hesaplar atlandı → sonraki turda veya el ile yeniden çalıştırın |
| `积分请求失败 / 响应解析失败 / 格式异常 / 字段无法解析` | Toplam puan alınamadı → bildirimde `总 None 积分` görünür, **check-in sonucunu etkilemez**, görmezden gelinebilir |

> Bildirimde `总 None 积分` görünmesi, yalnızca bu sefer toplam puanın alınamadığı anlamına gelir (o API bazen yavaş olur). Check-in'in başarılı olup olmamasıyla ilgisi yoktur; doğrulamak isterseniz el ile bir kez yeniden çalıştırın.

## ❓Sık Sorulan Sorular (SSS)

<details>
<summary><strong>S: Bir çalıştırma ne kadar sürer?</strong></summary>

Genelde birkaç saniyeden birkaç on saniyeye kadar. Tek tur bütçesi 300 saniyedir (yaklaşık 5 dakika); aşılırsa kalan hesaplar atlanır.

</details>

<details>
<summary><strong>S: Cookie ne kadar sürede sona erer?</strong></summary>

Sabit bir süresi yoktur. Oturumu kapatmak, şifre değiştirmek veya sunucu tarafı oturum politikası değişiklikleri daha erken geçersiz kılabilir. 「候选域名均返回没有权限」veya `code=-2` görürseniz yeniden giriş yapıp `GLADOS_COOKIES` değerini güncelleyin.

</details>

<details>
<summary><strong>S: Birden çok hesap destekleniyor mu?</strong></summary>

Evet, sayı sınırı yoktur. JSON dizisine birkaç öğe daha eklemeniz yeterli. Yinelenen Cookie'ler otomatik olarak tekilleştirilir, iki kez check-in yapılmaz.

</details>

<details>
<summary><strong>S: Her Cookie'nin hangi alan adına ait olduğunu belirtmem gerekir mi?</strong></summary>

Gerekmez, betik otomatik olarak algılar. `origin` girmenin yararı, daha az algılama isteği gönderilmesi ve günlüklerin daha anlaşılır olmasıdır. Dikkat: **farklı sitelerin Cookie'leri genelde birbirinin yerine kullanılamaz**; her Cookie gerçekten giriş yapılan siteden alınmış olmalıdır.

</details>

<details>
<summary><strong>S: <code>origin</code> yanlış girilirse ne olur?</strong></summary>

Başarısız olmaz. `origin` yalnızca **öncelikli denemedir**: isabet ederse doğrudan check-in yapılır, "yetki yok" dönerse otomatik olarak geri dönüp yerleşik 8 alan adının tamamını tarar.

</details>

<details>
<summary><strong>S: Birden çok hesap varken günlüğün hangi hesaba ait olduğunu nasıl ayırt ederim?</strong></summary>

`name` girilmezse hesaplar sırayla otomatik adlandırılır ve `🍪[账号1]`, `🍪[账号2]` olarak görünür; girilirse `🍪[1:大号]`, `🍪[2:小号]`, `🍪[3:账号3]` olarak görünür; numaralar özet bölümündeki `#1` `#2` `#3` ile eşleşir.

</details>

<details>
<summary><strong>S: Bir hesap kaç görev çalıştırır?</strong></summary>

Bir tane. Betik önce alan adını algılar, sonra yalnızca o alan adında check-in yapar; görev sayısı hesap sayısına eşittir.

</details>

<details>
<summary><strong>S: "Yinelenen check-in" başarı mı başarısızlık mı sayılır?</strong></summary>

Normal bir sonuç sayılır. Bugün zaten check-in yapıldığı anlamına gelir; sunucu `code=1` döndürür ve özet bölümünde "yinelenen" olarak ayrı sayılır, başarısızlığa eklenmez.

</details>

<details>
<summary><strong>S: Cihaz uyumsuzluğu (code=4) olursa ne yapmalıyım?</strong></summary>

GLaDOS, UA ile giriş yapılan cihazı doğrular. `user-agent` girilmişse olduğu gibi kullanılır; girilmemişse varsayılan UA kullanılır ve başarısız olursa `loginDevice` değerine göre otomatik olarak bir kez yeniden denenir; yine de başarısız olursa `user-agent` eklemeniz istenir — **Cookie'yi alırken kullandığınız tarayıcının** UA değerini girin.

</details>

<details>
<summary><strong>S: Actions yeşil görünüyor ama bildirim gelmedi?</strong></summary>

Önce ilgili Secret'ın yazımının doğru olduğunu doğrulayın (günlükte「已配置 N 个后端」yazdırılır). Unutmayın: Server酱'in `code=0` döndürmesi yalnızca **görevin kuyruğa alındığı** anlamına gelir, teslim edildiği anlamına gelmez; uzun süre gelmezse SendKey'in WeChat'e bağlı olup olmadığını kontrol edin.

</details>

<details>
<summary><strong>S: Herkese açık bir deponun zamanlanmış görevi kendiliğinden durur mu?</strong></summary>

Evet. GitHub'ın "herkese açık depolarda 60 gün üst üste etkinlik olmazsa schedule devre dışı bırakılır" kuralı vardır. Bu depoda yerleşik bir keepalive adımı düzenli olarak etkinlik üretir ama %100 garanti vermez; durduğunu fark ederseniz Actions sayfasından yeniden etkinleştirin.

</details>

<details>
<summary><strong>S: Dönüştürme ayarlanırsa puanlarım rastgele harcanır mı?</strong></summary>

Yalnızca `GLADOS_EXCHANGE_PLAN` ile belirtilen kademe çalıştırılır; **girilmemişse dönüştürme yapılmaz** ve yalnızca check-in başarılı/yinelenen olduğunda dönüştürür. Bakiye yetersizse API hata döndürür ve eksiye düşmez.

</details>

<details>
<summary><strong>S: Cloudflare engeli yüzünden puan alamazsam / yalnızca 1 puan alırsam ne yapmalıyım?</strong></summary>

Bu, saf HTTP betiğinin yapısal bir sınırlamasıdır. Günlükte Cloudflare, Turnstile, 403/429/503 görünüyorsa ya da yalnızca 1 puan alıyorsanız bu check-in tam olarak tamamlanmamıştır. Yeniden çalıştırmak büyük olasılıkla yine engellenir.

Lütfen birlikte gelen tarayıcı uzantısını [`extension/`](extension/README.md) kullanın: uzantı gerçek tarayıcıda resmî sitenin check-in düğmesine tıklar ve resmî bileşen Turnstile'ı tamamlar; 8 alan adı, çoklu hesap, otomatik dönüştürme ve bildirimi destekler.

</details>

<details>
<summary><strong>S: Check-in başarısız olursa Actions kırmızı olur mu?</strong></summary>

İki durum vardır: **yapılandırma hataları** (Cookie girilmemiş, JSON biçimi geçersiz) sıfır olmayan çıkış koduyla biter ve iş akışı kırmızı olur; **iş hataları** (Cookie süresi dolmuş `code=-2`, sunucu 5xx) kırmızı olmaz; ara sıra yaşanan başarısızlıkların her gün yanlış alarm vermesi engellenir. Bu nedenle iş hatalarını bildirim veya günlükten kendiniz takip etmeniz gerekir.

</details>

<details>
<summary><strong>S: "Kalan 0 gün" ne demek?</strong></summary>

Hesabın bugün sona erdiği anlamına gelir; check-in yine de başarılı olur. Puanla yenilemek için `GLADOS_EXCHANGE_PLAN` yapılandırmanız önerilir, ayrıntılar için bkz. [Puan Dönüştürme](#积分兑换).

</details>

<details>
<summary><strong>S: Yerel olarak çalıştırabilir miyim?</strong></summary>

Evet, hata ayıklama için. Bağımlılıkları kurup ortam değişkenini ayarladıktan sonra çalıştırın:

```bash
pip install -r requirements.txt
export GLADOS_COOKIES='[{"cookie":"koa:sess=..."}]'
python checkin.py
```

Windows PowerShell'de `$env:GLADOS_COOKIES='[...]'` kullanın.

</details>

---

## 📁Dosya Yapısı

```text
.
├── checkin.py                    # Ana program: hesap modeli / API / yapılandırma / rapor / bildirim
├── logging_config.py             # Günlük yapılandırması (Pekin saati, kısa seviye etiketleri, maskeleme)
├── requirements.txt              # Bağımlılıklar
├── README.md                     # Açıklama belgesi
├── LICENSE                       # MIT Lisansı
├── extension/                    # Tarayıcı uzantısı: Cloudflare yedek check-in çözümü
└── .github/workflows/
    └── gladosCheck.yml           # Actions zamanlanmış görevi
```

## 📝Değişiklik Günlüğü

**2026-01**

- Kod yeniden düzenlendi, sorun bulmayı kolaylaştıran günlük çıktısı eklendi, yeni URL'ler desteklendi ve puan dönüştürme stratejisi yapılandırması desteklendi.

**2026-04**

- Kod mantığı ve günlük çıktısı iyileştirildi.

**2026-09**

- Cihaz platformuna uyarlanabilir yeniden deneme eklendi, UA'yı el ile yapılandırmak gerekmez.
- Ağ katmanında otomatik yeniden deneme ve geri çekilme eklendi.
- Günlük saatleri Pekin saati olarak birleştirildi; zamanlanmış görev günde 2 kez olacak şekilde ayarlandı.
- Otomatik dönüştürme varsayılan olarak kapalıdır.

**2026-10**

- Server酱 bildirimi desteklendi.

**2026-11**

**1.0.1**

- Uzantı sürümü `1.0.1`e yükseltildi;
- Uzantı içindeki geçmiş/sürüm açıklama yorumları temizlendi;
- `SHA256SUMS.txt`, `release-notes.md`, `debug.py` gibi yerel sürüm/hata ayıklama dosyaları yok sayıldı.

**1.0.0**

- GLADOS_COOKIES biçimi JSON dizisine değiştirildi; `name` / `origin` / `cookie` / `user-agent` desteklenir; eski `&` biçimi hâlâ desteklenir.
- Hata nedeni mesajları iyileştirildi; check-in reddi, tamamlanmayan dönüştürme gibi durumlarda uygulanabilir öneriler verilir.
- Günlük otomatik maskeleme: `koa:*` / `gld:*` / Bearer → `[REDACTED]`.
- Challenge doğrulama mekanizması eklendi; alma başarısız olursa eski protokole geri dönülür.
- Cloudflare/Turnstile engeli ve 403/429/503 algılanır; puanlar eksikse günlük ve bildirim tarayıcı uzantısına geçmeyi önerir.

## 📊 Veri İstatistikleri

<div align="center">

<img src="https://github-readme-stats.vercel.app/api?username=lzr52o&show_icons=true&theme=radical" width="46%" /><img src="https://github-readme-stats.vercel.app/api/top-langs/?username=lzr52o&layout=compact&theme=radical" width="50%" />

<img src="https://github-readme-streak-stats.herokuapp.com/?user=lzr52o&theme=radical" width="96%" />

<img src="https://github-readme-activity-graph.vercel.app/graph?username=lzr52o&theme=github-compact" width="100%" />

</div>

## 🛠️ Teknoloji Yığını

<div align="center">

![Skill Icons](https://skillicons.dev/icons?i=js,py,git,github,cloudflare,apple,vscode&theme=dark)

</div>

## 😄 Günün Sözü

<div align="center">

<img src="https://www.dmoe.cc/random.php" width="160" height="140" alt="Rastgele anime görseli" /><img src="https://daily-sentence.lzr52o.workers.dev/sentence" width="440" height="140" alt="Günün sözü" /><img src="https://www.dmoe.cc/random.php?v=2" width="160" height="140" alt="Rastgele anime görseli" />

</div>

## 🎮 Katkı Grafiği

<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://contrib-rotate.lzr52o.workers.dev/rotate?theme=dark" />
  <source media="(prefers-color-scheme: light)" srcset="https://contrib-rotate.lzr52o.workers.dev/rotate?theme=light" />
  <img alt="Katkı grafiği" src="https://contrib-rotate.lzr52o.workers.dev/rotate?theme=light" />
</picture>

</div>



## ⭐Star Geçmişi

[![Star History Chart](https://api.star-history.com/svg?repos=Devilstore/Glados-Railgun-checkin&type=Date&legend=top-left)](https://star-history.com/#Devilstore/Glados-Railgun-checkin&type=Date)

---

## 📄Beyan

- Kararlı çalışma garanti edilmez; API'ler ve risk kontrol politikaları her an değişebilir.
- Yalnızca web sürümünün kullandığı herkese açık API'ler çağrılır; doğrulama kodu aşılmaz; sunucu tarafı risk kontrolü yine de reddedebilir veya düşürebilir.
- Kullanım riski kullanıcıya aittir; bu aracın kullanımından doğan hesap risk kontrolü, kısıtlama veya yasaklardan kullanıcı sorumludur.
- Bu proje [MIT](LICENSE) lisansıyla yayımlanır.

Bu proje size yardımcı olduysa lütfen bir ⭐ Star verin.

![Footer](https://capsule-render.vercel.app/api?type=waving&color=gradient&height=120&section=footer&text=GLaDOS+%E8%87%AA%E5%8A%A8%E7%AD%BE%E5%88%B0&fontSize=24&fontColor=ffffff)
