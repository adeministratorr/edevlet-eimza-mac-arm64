# e-Devlet E-İmza — macOS (Apple Silicon / arm64)

e-Devlet E-İmza'yı (Türksat, resmî `elektronik-imza.jar`) Apple Silicon Mac'te
native .app olarak derleyip kuran kurulum aracı.

Bu depo **Said Sürücü**'nün https://github.com/saidsurucu/edevlet-eimza-mac-arm64
projesine dayanır; asıl teorik çalışma (jnlp → jpackage hattı, Java 11 gömme,
IAIK arm64 connect-fix patch'i) ona aittir. Ölümüyle değil, küçük bir
bakım-düzeltmesiyle ayrılıyor:

## Bu depodaki fark (fix)
- `scripts/build.sh > patch_jar`: patcher üstten hiç `.class` üretmediyse
  (`work/out` yok) bu, jar'ın **zaten uyumlu** olduğu anlamına gelir
  (Türksat jar'ı artık sürüm-uyumlu sınıflar içeriyor). Eski betik bu durumda
  `patcher hiç .class üretmedi` hatasıyla `all`'u forma hata veriyordu.
  Artık bu durumda patch atlanıp `javap` doğrulaması ile devam eder.
- Standart `kur.sh` ve `scripts/build.sh` davranışları aynen korunur.

## Kullanım
Tek satır (resmî olmayan topluluk betiği):
```
curl -fsSL https://raw.githubusercontent.com/adeministratorr/edevlet-eimza-mac-arm64/main/kur.sh | bash
```

Homebrew (saidsurucu/tap ve adeministratorr/tap):
```
brew tap adeministratorr/tap
HOMEBREW_NO_SANDBOX=1 brew install adeministratorr/tap/edevlet-eimza
```

## Gereksinimler
- macOS Apple Silicon (arm64)
- Komut satırı araçları (betik Xcode CLT kurulmasını kendisi ister)
- TÜBİTAK AKİS sürücüsü — **Apple Silicon (Arm) paketi**:
  https://akiskart.bilgem.tubitak.gov.tr/tr/destek/
- e-imza kartı okuyucu + e-imza kartı

## Not
Resmî kaynak jar: https://static.turkiye.gov.tr/downloads/e-imza/edevlet-eimza.jnlp
