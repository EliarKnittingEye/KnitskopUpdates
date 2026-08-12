# Knitskop Güncelleme Dağıtımı

ELIAR Knitskop masaüstü uygulamasının **güncelleme kaynağı**. Bu depo
herkese açıktır; uygulama içindeki otomatik güncelleme buraya bakar ve
kullanıcıların GitHub hesabı olmadan kurulum dosyalarını indirebilmesini
sağlar.

> Uygulama kaynak kodu burada değildir; o özel depoda tutulur.

## İndirme

Son sürüm: **[Releases](https://github.com/EliarKnittingEye/KnitskopUpdates/releases/latest)**

| İşletim sistemi | Dosya |
|---|---|
| Windows | `Knitskop-Windows-Setup.exe` |
| macOS (Apple Silicon) | `Knitskop-macOS-arm64.dmg` |

> macOS'ta uygulama imzasız dağıtıldığı için ilk açılışta "geliştirici
> doğrulanamadı" uyarısı çıkar: **Sağ tık → Aç** deyin.

## Nasıl çalışır

Uygulama `latest.json` dosyasına bakar:

```
https://raw.githubusercontent.com/EliarKnittingEye/KnitskopUpdates/main/latest.json
```

`version` alanı kurulu sürümden **yeni** ise güncelleme önerilir. Sürümler
sayısal olarak karşılaştırılır (2.0.10 > 2.0.9), sadece "farklı mı" diye
bakılmaz — böylece eski bir manifest yanlışlıkla sürüm düşürmeye yol açmaz.

## Yeni sürüm yayınlama

1. Kurulum dosyalarını üretin (uygulama deposunda `npm run pack:mac` /
   `pack:win`).
2. Bu depoda `v<sürüm>` etiketiyle bir Release açın ve iki dosyayı yükleyin.
3. `latest.json`'u güncelleyin: `version`, `notes`, her iki platform için
   `url`, `sha256`, `sizeBytes`.

SHA-256 hesaplamak için:

```bash
shasum -a 256 "ELIAR Knitskop V2-2.0.0-arm64.dmg"   # macOS
certutil -hashfile Knitskop-Windows-Setup.exe SHA256  # Windows
```

`sha256` alanı **doldurulmalıdır** — uygulama indirdiği dosyayı bu özetle
doğrular, uyuşmazsa kurulumu reddeder.
