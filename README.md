# E-Devlet E-İmza — macOS Apple Silicon (M1/M2/M3/M4) Kurulumu (arm64)

e-Devlet E-İmza'yı (Türksat, resmî `elektronik-imza.jar`) **MacBook, MacBook Pro, MacBook Air, Mac mini ve iMac**'de native çalıştırın. Apple Silicon (M serisi, ARM tabanlı işlemci) için arm64 `.app`; gömülü Java 11 — Rosetta gerekmez, ayrı Java kurulumu gerekmez.

**Kimler için:** e-Devlet'i e-imza ile kullanan tüm meslek grupları — avukatlar, mali müşavirler (SMMM/YMM), doktorlar ve sağlık profesyonelleri, mühendis ve mimarlar, eczacılar, gümrük müşavirleri, şirket yetkilileri (MERSİS), kamu çalışanları, EKAP/ihale kullanıcıları ve e-imza kartı olan tüm vatandaşlar.

## Kimler için?

- **Avukatlar:** e-Devlet üzerinden belge imzalayan ve resmî sorgulama yapanlar.
- **Mali müşavirler:** Vergi, SGK ve kurum işlemleri için e-imza kullananlar.
- **Doktorlar ve sağlık profesyonelleri:** e-Devlet ve sağlık işlemlerinde e-imza kullananlar.
- **Mühendis ve mimarlar:** Proje, ruhsat ve resmî yazışma süreçleri.
- **Şirket yetkilileri:** MERSİS ve kurum başvuruları için e-imza.
- **Gümrük müşavirleri, eczacılar, banka/sigorta profesyonelleri:** Resmî işlemlerde e-imza.
- **Kamu çalışanları ve tüm vatandaşlar:** e-imza kartıyla e-Devlet'e giren herkes.

## "Mac'te e-imza çalışmıyor" arayanlar için kısa cevaplar

- **Neden Mac'te sorun çıkıyor?** Uygulama Java Web Start (JNLP) tabanlı dağıtılıyor; Java Web Start Java 11 ve sonrasında kaldırıldı. Bu paket, uygulamayı gömülü Java 11 ile native `.app`'e çevirir ve arm64 kart erişimi için gerekli yamayı uygular.
- **MacBook / MacBook Pro / MacBook Air / iMac / Mac mini'de çalışır mı?** Evet — Apple Silicon (M1/M2/M3/M4) tüm modeller destekli. Intel Mac desteklenmez.
- **Rosetta veya Java kurmam gerekir mi?** Hayır; paket ARM native (arm64) ve Java uygulamaya gömülüdür.
- **İmza için ek olarak ne gerekir?** e-imza kartı + kart okuyucu + **arm64 AKİS sürücüsü** (aşağıda).

## Neden Mac'te çalışmıyordu? Nasıl çözdük?

- **Sorun:** e-Devlet E-İmza istemcisi modern Mac'lerde kurulamıyor/açılamıyordu.
- **Neden:** Uygulama Java Web Start (JNLP) ile dağıtılıyordu; Java Web Start Java 11 ile kaldırıldı. Ayrıca kart erişimini sağlayan IAIK katmanında arm64 (connect) sorunu vardı ve jar güncellenince eski yama betiği hata veriyordu.
- **Çözüm:** `jpackage` + gömülü **arm64 Java 11** ile native `.app` paketleme; IAIK arm64 connect-fix yaması; jar artık uyumlu sınıflar içerdiğinde yamanın hata vermeden atlanıp `javap` doğrulamasıyla devam etmesi (bu depodaki düzeltme — aşağıda).

## Kurulum (3 yol)

1. **Tek satır (resmî olmayan topluluk betiği; kaynaktan derler):**

```
curl -fsSL https://raw.githubusercontent.com/adeministratorr/edevlet-eimza-mac-arm64/main/kur.sh | bash
```

2. **Homebrew (hazır DMG ile en hızlı):**

```bash
brew install --cask adeministratorr/tap/edevlet-eimza     # hazır DMG
brew install adeministratorr/tap/edevlet-eimza            # kaynaktan derler
```

3. **Hazır DMG:** [Releases](../../releases) → `EdevletEimza-arm64-*.dmg`

Diğer araçlar (aynı tap): [PTT KEP Webmail](https://github.com/adeministratorr/pttkep-eimza-mac-arm64) · [PTT UETS / e-Tebligat](https://github.com/adeministratorr/uets-eimza-mac-arm64) · [TNB Teknoloji İmza](https://github.com/adeministratorr/tnb-eimza-mac-arm64) · [UHAP İmzalama](https://github.com/adeministratorr/uhap-eimza-mac-arm64) · [UDF QuickLook](https://github.com/adeministratorr/udf-quicklook-extension) · [UYAP Doküman Editörü](https://github.com/adeministratorr/ude-mac-arm64-1)

---

## Bu depodaki düzeltme (fix)

Bu depo **Said Sürücü**'nün https://github.com/saidsurucu/edevlet-eimza-mac-arm64 projesine dayanır; asıl teorik çalışma (JNLP → jpackage hattı, Java 11 gömme, IAIK arm64 connect-fix patch'i) ona aittir. Bu sürüm küçük bir bakım düzeltmesiyle ayrılıyor:

- `scripts/build.sh > patch_jar`: patcher üstten hiç `.class` üretmediyse (`work/out` yok) bu, jar'ın **zaten uyumlu** olduğu anlamına gelir (Türksat jar'ı artık sürüm-uyumlu sınıflar içeriyor). Eski betik bu durumda `patcher hiç .class üretmedi` hatasıyla `all`'u hata veriyordu. Artık bu durumda patch atlanıp `javap` doğrulaması ile devam eder.
- Standart `kur.sh` ve `scripts/build.sh` davranışları aynen korunur.

## Gereksinimler

- macOS Apple Silicon (arm64) — MacBook, MacBook Pro, MacBook Air, Mac mini, iMac
- Komut satırı araçları (betik Xcode CLT kurulmasını kendisi ister)
- TÜBİTAK AKİS sürücüsü — **Apple Silicon (Arm) paketi**: https://akiskart.bilgem.tubitak.gov.tr/tr/destek/
- e-imza kartı okuyucu + e-imza kartı

## Not

Resmî kaynak jar: https://static.turkiye.gov.tr/downloads/e-imza/edevlet-eimza.jnlp
