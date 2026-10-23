# tdpeyzaj.com – Statik Site (Cloudflare)

TDI Peyzaj Mimarlık tanıtım sitesi. Eski WordPress sitesinin statik kopyası üzerine SEO ve küçük eklemeler yapıldı. Yayınlanan tüm dosyalar `public/` klasöründedir.

```
public/
  index.html            Ana sayfa
  404.html              Bulunamayan sayfalar → ana sayfaya yönlendirir (noindex)
  _redirects            Eski WordPress adresleri (/index.php/..., /wp-admin, /feed) → /
  _headers              Görsel/font önbellekleme
  robots.txt, sitemap.xml
  og-image.jpg          Sosyal medyada paylaşım görseli (1200×630)
  wp-content/uploads/   Tüm fotoğraflar (orijinal yollarla; kolajın .webp sürümleri de burada)
  wp-content/themes/    Fontlar (Cardo, Inter)
  wp-includes/          Menü CSS/JS
wrangler.toml           Cloudflare Workers yapılandırması (worker adı: tdpeyzaj-com)
```

## Yayın
GitHub (`koddev/tdpeyzaj_com`, `main` dalı) Cloudflare Workers'a bağlı. **`main`'e her push siteyi ~1 dakikada otomatik günceller.** Elle yayın gerekirse: `npx wrangler deploy`.

Domain ayarları (Cloudflare):
- `tdpeyzaj.com` ve `www.tdpeyzaj.com` → Worker custom domain
- www → tdpeyzaj.com 301 yönlendirme (Rules → Redirect Rules)
- Always Use HTTPS açık
- Web Analytics açık (otomatik kurulum, çerezsiz) → Cloudflare → Analytics → Web analytics

## Yerelde görüntüleme
```
python -m http.server 8787 --directory public
```
Sonra http://localhost:8787 adresini açın.

## index.html'e yapılan eklemeler
Hepsi `index.html` içinde, kendi `id`'li `<style>`/`<script>` bloklarındadır:
- `<head>`: başlık, meta açıklama, canonical, Open Graph/Twitter etiketleri, firma bilgisi (JSON-LD, `ProfessionalService`)
- `tdp-mobile-css`: mobil düzen düzeltmeleri (harita genişliği, ekip 2 sütun, iletişim ortalama)
- `tdp-back-to-top-*`: sağ altta başa dön butonu
- `tdp-whatsapp-css`: sol altta WhatsApp butonu (0531 744 35 92)
- `tdp-lightbox-*`: kolaja tıklayınca büyük görünüm

Telefon/adres değişirse **üç yeri** birlikte güncelleyin: footer metni, JSON-LD bloğu ve WhatsApp linki (`wa.me/90…`).

## İçerik güncelleme
Metinler `public/index.html` içinde düz HTML olarak durur. Proje eklemek için "PROJELERİMİZ" altındaki listeye yeni bir `<li>…</li>` satırı ekleyin. Yeni görseli `public/wp-content/uploads/...` altına koyup ilgili `src`/`srcset` değerini değiştirin. Kolajı değiştirirseniz `.webp` sürümlerini ve `og-image.jpg`'yi de yenileyin.

## Google'a tanıtma (bir kerelik)
1. **Google Search Console** – https://search.google.com/search-console → *Mülk ekle* → **Alan adı**: `tdpeyzaj.com`. Doğrulama için verilen TXT kaydını Cloudflare → DNS → Records → *Add record* (Type: TXT, Name: `@`) olarak ekleyin; ya da sihirbaz Cloudflare'i tanırsa "Cloudflare ile doğrula"yı seçin. Doğrulandıktan sonra *Site haritaları* → `https://tdpeyzaj.com/sitemap.xml` gönderin, *URL denetimi* → `https://tdpeyzaj.com/` → *Dizine eklenmesini iste*.
2. **Google İşletme Profili** – https://business.google.com → işletme adı "TDI Peyzaj Mimarlık", kategori "Peyzaj mimarı", adres `1322 Cad. No: 38/11 Aşağıöveçler, Çankaya/Ankara`, telefon `0531 744 35 92`, web sitesi `https://tdpeyzaj.com`. Bilgileri sitedekiyle birebir aynı yazın. Haritadaki mevcut "TDI PEYZAJ MIMARLIK" kaydı sizinse onu sahiplenin, yenisini açmayın.
3. **Bing Webmaster Tools** – https://www.bing.com/webmasters → *Search Console'dan içe aktar*.
4. Kontrol: https://search.google.com/test/rich-results ve https://www.opengraph.xyz adreslerine `https://tdpeyzaj.com` yazın.
