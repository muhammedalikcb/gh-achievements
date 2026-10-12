# gh-achievements

[![Build](https://github.com/muhammedalikcb/gh-achievements/actions/workflows/build.yml/badge.svg)](https://github.com/muhammedalikcb/gh-achievements/actions/workflows/build.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
![Kotlin](https://img.shields.io/badge/Kotlin-2.4-7F52FF?logo=kotlin&logoColor=white)

[English](README.md) · **Türkçe**

GitHub profilinde, yaptığın işlere göre kazandığın küçük rozetler var: Pull Shark, YOLO, Quickdraw, Starstruck gibi. Bunlara achievement deniyor. GitHub hangi rozetleri aldığını gösteriyor ama bir sonrakine ne kadar kaldığını ve onu nasıl alacağını söylemiyor.

`gh-achievements` GitHub API'sinden verilerini çekip ne kadar kaldığını söylüyor, örneğin "Pull Shark x2 için 12 merge edilmiş PR daha lazım". Yanına da bir sonraki adım için kısa bir ipucu ekliyor.

<img src="docs/example-card.svg" alt="Örnek rozet kartı" width="495">

## Neler yapabiliyor?

- Terminalde renkli bir ilerleme tablosu gösteriyor. İstersen JSON ya da Markdown çıktısı da alabilirsin.
- Profil README'ne koyabileceğin bir SVG kart üretiyor. Kart açık ve koyu temaya kendisi uyuyor.
- GitHub Action ile kartını her gün güncelliyor.
- Bütün rozetleri, kademelerini, nasıl kazanıldıklarını, ipuçlarını ve sık sorulan soruları tek bir rehberde topluyor: [docs/ACHIEVEMENTS.tr.md](docs/ACHIEVEMENTS.tr.md)
- Türkçe ve İngilizce çalışıyor. Sistem dilin Türkçe ise çıktı otomatik Türkçe geliyor, istersen `--lang` ile kendin seçebilirsin.

## Kurulum

İki şeye ihtiyacın var:

1. JDK 17 veya üstü. `java -version` ile kontrol edebilirsin. macOS'ta `brew install openjdk@17` ile kurabilirsin.
2. Bir GitHub token'ı. [GitHub CLI](https://cli.github.com/) kullanıyorsan ve `gh auth login` ile giriş yaptıysan başka bir şey yapmana gerek yok, araç token'ı oradan alıyor. Kullanmıyorsan token'ı `GITHUB_TOKEN` ya da `GH_TOKEN` ortam değişkenine koyman yeterli. Public veriler için ek yetkisi olmayan bir token iş görüyor.

Sonra:

```bash
git clone https://github.com/muhammedalikcb/gh-achievements.git
cd gh-achievements
./gradlew installDist
alias gh-achievements="$PWD/build/install/gh-achievements/bin/gh-achievements"
```

Bu `alias` sadece o an açık olan terminalde geçerli. Kalıcı olsun istersen satırı `~/.zshrc` dosyana ekle, ama `$PWD` yerine reponun tam yolunu yaz. Yoksa yeni açtığın terminalde `$PWD` başka bir klasörü gösterir ve komut bulunamaz.

Windows'ta `gradlew.bat installDist` ile derleyip `build\install\gh-achievements\bin\gh-achievements.bat` dosyasını çalıştır.

## Kullanım

```bash
gh-achievements                  # kendi hesabın
gh-achievements torvalds         # başka bir kullanıcı
gh-achievements ali ayse         # birden fazla kişiyi karşılaştır
gh-achievements catalog          # bütün rozetler ve nasıl kazanılacakları
```

Örnek çıktı:

```
@muhammedalikcb için GitHub rozetleri

Kademeli
  Pull Shark     ██░░░░░░░░  4 merge edilmiş PR         açıldı, x2 için 16 (12 kaldı)
    → Zaten kullandığın projelerde küçük düzeltmeler ara: kırık linkler, eskimiş dokümanlar, ...
  Starstruck     ██░░░░░░░░  4 yıldız (tek repoda)      16 olunca açılır (12 kaldı)  muhammedalikcb/securecheck
  Galaxy Brain   ░░░░░░░░░░  0 kabul edilen cevap       2 olunca açılır (2 kaldı)

Tek seferlik
  ✔ Quickdraw            bir issue ya da PR'ı 5 dakika içinde kapattı
  ✔ YOLO                 review almadan PR merge etti
  ✔ Public Sponsor       herkese açık olarak birine sponsor
  ? Pair Extraordinaire  API'den okunamıyor, profilinden kontrol et

Profil rozetleri
  ✔ Developer Program Member
  ✘ Security Bug Bounty Hunter
  ...
```

Çıktıyı okurken:

- Kademeli rozetler belli sayılara ulaştıkça seviye atlıyor. Örneğin Pull Shark 2 PR'da açılıyor, 16'da bronz (x2), 128'de gümüş (x3), 1024'te altın (x4) oluyor. Çubuk, bir sonraki kademeye ne kadar yaklaştığını gösteriyor.
- Tek seferlik rozetler bir kez kazanılıyor, kademeleri yok.
- Profil rozetleri aslında achievement değil, profilinde görünen özel işaretler. Developer Program üyeliği gibi.
- `→` ile başlayan satırlar bir sonraki adım için ipucu. İstemezsen `--no-tips` ile kapatabilirsin.

### Seçenekler

| Seçenek | Ne işe yarar |
| --- | --- |
| `-f, --format text\|json\|markdown\|svg` | Çıktı biçimi, varsayılan `text` |
| `--json` | `--format json` ile aynı |
| `-l, --lang en\|tr` | Dil, varsayılan olarak sistem dilinden seçilir |
| `-o, --output <dosya>` | Terminal yerine dosyaya yazar |
| `--theme auto\|light\|dark` | SVG kartın teması, `auto` kartı görüntüleyen kişinin temasına uyar |
| `--no-tips` | İpuçlarını gizler |
| `--no-color` | Renksiz çıktı (`NO_COLOR` ortam değişkeni de çalışır) |

Birden fazla kullanıcıyı `svg` dışındaki bütün biçimlerde karşılaştırabilirsin, SVG kart tek kişi için üretiliyor. `catalog` komutu da `--format markdown` ve `--format json` ile çalışıyor. `docs/` altındaki rehberler `catalog --format markdown` ile üretiliyor.

## Profiline rozet kartı ekle

GitHub'da kullanıcı adınla aynı isimde bir repo açarsan (örneğin `ali/ali`), o reponun README'si profil sayfanın en üstünde görünür. Kartı oraya koyarsan her gün kendiliğinden güncellenir.

1. Profil repona [examples/profile-card.yml](examples/profile-card.yml) dosyasını `.github/workflows/achievements.yml` adıyla ekle.
2. Türkçe kart istiyorsan dosyadaki `# lang: tr` satırının başındaki `#` işaretini sil.
3. README'ne şu satırı ekle:

   ```markdown
   ![GitHub rozetlerim](achievements.svg)
   ```

4. Reponun Actions sekmesinden "Update achievements card" iş akışını bir kez elle çalıştır. Sonrasında her gün 06:00 UTC'de (Türkiye saatiyle 09:00) kendisi çalışır ve sadece kart değiştiyse commit atar.

`output` ayarını değiştirirsen iş akışındaki `git add` satırını da aynı dosya adıyla güncelle.

Action'ın ayarları:

| Ayar | Varsayılan | |
| --- | --- | --- |
| `username` | repo sahibi | Kimin kartı oluşturulacak |
| `output` | `achievements.svg` | Yazılacak dosya |
| `format` | `svg` | `svg`, `markdown` ya da `json` |
| `lang` | `en` | `en` ya da `tr` |
| `theme` | `auto` | `auto`, `light` ya da `dark` |
| `token` | `github.token` | GitHub API için token |

## Neleri kontrol ediyor?

| Rozet | Neye bakılıyor | Kademeler |
| --- | --- | --- |
| Pull Shark | Açtığın ve merge edilen pull request'ler | 2, 16, 128, 1024 |
| Starstruck | En çok yıldız alan reponun yıldız sayısı (fork'lar sayılmaz) | 16, 128, 512, 4096 |
| Galaxy Brain | Discussions'ta kabul edilen cevapların | 2, 8, 16, 32 |
| Quickdraw | 5 dakika içinde kapatılan bir issue ya da PR | |
| YOLO | Review almadan kendin merge ettiğin bir PR | |
| Public Sponsor | En az bir herkese açık sponsorluk | |
| Profil rozetleri | Developer Program, Bug Bounty Hunter, Campus Expert, GitHub Star | |

API'nin göremediği rozetler (Pair Extraordinaire, PRO, Security advisory credit) ve artık verilmeyenler de [Türkçe rozet rehberinde](docs/ACHIEVEMENTS.tr.md) anlatılıyor.

## Bilinen sınırlar

- Pair Extraordinaire sayılamıyor, çünkü GitHub API'si bir kullanıcının co-author olduğu commit'leri vermiyor.
- Quickdraw ve YOLO sadece son 100 PR ve issue'ya bakıyor.
- Başkalarına bakarken sadece public aktiviteleri görünüyor. Bu yüzden sayıları profillerinde gördüğünden düşük çıkabilir.
- Kurallar resmi değil. GitHub kesin kriterleri yayınlamıyor, eşikler topluluğun gözlemlerine dayanıyor. Ana kaynak [Schweinepriester/github-profile-achievements](https://github.com/Schweinepriester/github-profile-achievements) listesi. Bir şey değiştiyse issue açarsan çok sevinirim.

## Katkı

Hata bildirimi, çeviri ya da yeni bir kontrol eklemek istersen ayrıntılar [CONTRIBUTING.md](CONTRIBUTING.md) dosyasında. Bütün rozet metinleri [`Catalog.kt`](src/main/kotlin/dev/kocabey/achievements/Catalog.kt) dosyasında duruyor, bir açıklamayı düzeltmek istersen bakman gereken yer orası. Yeni bir dil eklemek için `Lang.kt` ve `Ui.kt` dosyalarına da dokunman gerekiyor, adımlar yine CONTRIBUTING.md'de.

## Lisans

[MIT](LICENSE)
