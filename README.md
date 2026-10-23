# tdpeyzaj.com – Statik Site (Cloudflare)

WordPress sitesinin (tdpeyzaj.com) birebir statik kopyası. Yayınlanacak tüm dosyalar `public/` klasöründedir.

```
public/
  index.html            Ana sayfa (WordPress çıktısı, yollar yerelleştirildi)
  404.html              Bulunamayan sayfalar → ana sayfaya yönlendirir
  _redirects            Eski WordPress adresleri (/index.php/..., /wp-admin, /feed) → /
  _headers              Görsel/font önbellekleme
  robots.txt, sitemap.xml
  wp-content/uploads/   Tüm fotoğraflar (orijinal yollarla)
  wp-content/themes/    Fontlar (Cardo, Inter)
  wp-includes/          Menü CSS/JS
wrangler.toml           Cloudflare Workers yapılandırması
```

## Yerelde görüntüleme
```
python -m http.server 8787 --directory public
```
Sonra http://localhost:8787 adresini açın.

## Cloudflare'e yükleme

**Seçenek A – Panelden (en kolay):** Cloudflare Dashboard → Workers & Pages → Create → Pages → *Upload assets* → `public` klasörünün içeriğini sürükleyip bırakın.

**Seçenek B – Komut satırı:**
```
npx wrangler login
npx wrangler deploy
```

## Domain bağlama
1. tdpeyzaj.com'u Cloudflare'e ekleyip registrar'daki nameserver'ları Cloudflare'in verdikleriyle değiştirin.
2. Proje → *Custom domains* → `tdpeyzaj.com` ve `www.tdpeyzaj.com` ekleyin.
3. Çalıştığını doğruladıktan sonra eski WordPress hostingini kapatabilirsiniz.

## İçerik güncelleme
Metinler `public/index.html` içinde düz HTML olarak durur. Yeni görseli `public/wp-content/uploads/...` altına koyup `index.html`'deki ilgili `src`/`srcset` değerini değiştirin, sonra tekrar yükleyin.
