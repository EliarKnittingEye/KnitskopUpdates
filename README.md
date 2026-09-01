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

Uygulama deposunda (özel) üç komut:

```bash
rm -rf release
npm run pack:mac && npm run pack:win   # kurulum dosyalarini uret
npm run verify:pack                    # paket gercekten aciliyor mu
npm run prepare:release                # release/upload/ + latest.json hazirla
npm run publish:release                # bu depoya yayinla
```

`publish:release` sırayı garanti eder: Release oluşturur, dosyaları yükler,
sunucudan boyut/durum doğrular, dosyaları **gerçekten indirip SHA-256
karşılaştırır** ve `latest.json`'u ancak bunların hepsi geçtiyse günceller.

> `latest.json` dosyalar yüklenmeden güncellenirse sahadaki bütün kurulumlar
> var olmayan bir dosyayı indirmeye çalışır. Bu yüzden manifest her zaman en
> son adımdır.

`sha256` alanı **doldurulmalıdır** — uygulama indirdiği dosyayı bu özetle
doğrular, uyuşmazsa kurulumu reddeder. `prepare:release` bunu kendiliğinden
hesaplar; elle hesaplamak gerekirse:

```bash
shasum -a 256 Knitskop-macOS-arm64.dmg                # macOS
certutil -hashfile Knitskop-Windows-Setup.exe SHA256  # Windows
```
