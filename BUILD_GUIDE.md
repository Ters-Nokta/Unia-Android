# Unia — Bilgisayarsız APK Derleme Kılavuzu

Bu proje bir Android **kaynak projesidir** (henüz APK değil). APK'ya çevirmek
için tek gereken, projeyi ücretsiz bir **bulut derleme** servisine koymak.
Aşağıdaki yöntem tamamen **telefondan** yapılabilir ve GitHub Actions kullanır.

Projeye zaten bir iş akışı dosyası ekledim:
`.github/workflows/build.yml` — kod repo'ya girer girmez derlemeyi otomatik başlatır.

---

## Yöntem: GitHub Actions (telefon tarayıcısından)

### 1) GitHub hesabı + boş repo aç
1. Tarayıcıdan **github.com** → giriş yap / kayıt ol.
2. Sağ üst **“+” → New repository**.
3. Bir isim ver (ör. `unia`), istersen **Private** seç, **Create repository**.

### 2) Proje dosyalarını repo'ya yükle
Amacımız: `settings.gradle.kts`, `app/`, `gradle/`, `.github/` gibi dosyaların
**repo kökünde** olması.

En kolay yol (dosya yükleme):
1. Boş repo sayfasında **“uploading an existing file”** bağlantısına dokun
   (yoksa **Add file → Upload files**).
2. Zip'i telefonda bir dosya yöneticisiyle aç, içindeki `unia` klasörünün
   **içeriğini** seçip yükle.
3. Aşağıda **Commit changes**.

> Telefonda klasörlü toplu yükleme bazen zahmetli olur. İki kolay alternatif:
> - **Workflow dosyasını elle oluştur:** Repo'da **Add file → Create new file**,
>   dosya adı kutusuna aynen şunu yaz: `.github/workflows/build.yml`
>   (slash'lar otomatik klasör açar), içine bu projedeki `build.yml`'in içeriğini
>   yapıştır, commit'le. Kalan dosyaları da “Upload files” ile ekle.
> - **Git istemcisi uygulaması** (ör. *MGit*, *Termux*): boş repo'yu klonla,
>   zip'i içine çıkar, `commit` + `push`. Kod GitHub'a gider.

### 3) Derlemeyi izle
1. Repo'da üstteki **Actions** sekmesine gir.
2. **“Build APK”** işi otomatik başlar (ilk sefer birkaç dakika sürebilir).
3. **Yeşil tik** = başarılı. Kırmızı ✗ olursa işe girip logdaki hatayı okuyabilirsin.

### 4) APK'yı indir
1. Tamamlanan işe dokun → en altta **Artifacts** bölümü.
2. **unia-debug-apk**'yı indir → içinden `app-debug.apk` çıkar.
3. Telefonuna kur (Ayarlar'dan **bilinmeyen kaynaklara izin** gerekebilir).

---

## Notlar

- **Debug APK:** Doğrudan kurulur; Play Store'a yüklenmez. Store için “release”
  imzalı derleme gerekir (kendi imza anahtarını yönetmen gereken ayrı bir adım).
- **Dikey / yatay:** Uygulama şu an **yatay (landscape)** açılır. Telefonda
  dikey istersen `app/src/main/AndroidManifest.xml` içindeki
  `android:screenOrientation="sensorLandscape"` satırını
  `android:screenOrientation="unspecified"` yap (tek satır).
- **İnternet gerekir:** Uygulama müziği internetten (YouTube/Piped/Invidious
  üzerinden) çözüp akıttığı için çevrimdışı çalışmaz.
- **Daha küçük APK istiyorsan (opsiyonel, önce yeşil derlemeyi doğrula):**
  `app/build.gradle.kts` içindeki `release` bloğunda `isMinifyEnabled = true`
  yapıp WebView köprüsü için ProGuard kuralı ekleyebilirsin. Bunu test etmeden
  açmanı önermem; ilk hedef çalışan bir derleme.

## Alternatif bulut servisleri
Aynı mantıkla repo'ya bağlanıp bulutta derleyen başka servisler: **Codemagic**,
**AppCircle**. Hepsinde akış aynı: kodu bir Git repo'ya koy → servis derlesin →
APK'yı indir.
