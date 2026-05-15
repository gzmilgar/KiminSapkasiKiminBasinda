# Kimin Şapkası Kimin Başında?

Lessons Learned · Şapka Değişimi Atölyesi · NTT DATA SAP Practice

## Yapı

```
/
├── index.html              Ana sayfa — workshop özeti + 2 materyal kartı
├── ekipler.html            6 takım × ortalama 12-13 üye + arama
├── brifing.html            Ekibe verilen tek sayfalık brifing
├── netlify.toml            Netlify yapılandırması
├── README.md
└── kor-nokta/              ⌐ JÜRİ İÇİN — public navigasyonda yok
    ├── kartlar.html        Jüri karakter kartları (şifreli)
    └── notlar.html         Jüri ipuçları (şifreli)
```

## İki katmanlı koruma

Jüri içeriği iki ayrı yöntemle korunuyor:

**Katman 1 — Path obscurity (asıl koruma)**

`kor-nokta/` klasörüne *public site'tan hiçbir link gitmiyor.* Ana navigasyonda da, kartlarda da, footer'da da bu path geçmiyor. Workshop'un mottosundan ("Yeni şapkanın görüş alanı, eski koltuğunun *kör noktasıdır*") tematik bir referans — ama public site bunu hiç söylemez.

UI danışmanları kaynak koduna baksa bile bu path'i göremez. URL'i sadece sen jürilere manuel paylaşacaksın.

**Katman 2 — AES-GCM şifreleme (sempatik)**

Doğru URL'e ulaşan biri için bile içerik kaynak kodda açık değil. AES-GCM 256-bit (PBKDF2-SHA256, 100k iterasyon) ile şifrelendi. Şifre: **`fısıltı`**

> Bu artık ana koruma değil — URL'i bilen jüriye sıcak bir karşılama. Sayfa açıldığında "Şşşt. Aramızda kalsın." diyor, jüri şifreyi biliyor (sözlü olarak fısıldanır), giriyor, içerik açılıyor.

**DevTools'a bakanlar için bonus:** Console açıldığında "Jüri olduğuna emin misin? :)" yazısı çıkıyor. Sayfa kaynağına bakan UI danışmanına sempatik bir göz kırpma.

### Şifre detayı

- Şifre: `fısıltı` (Türkçe karakterler ve büyük/küçük harf otomatik normalize edilir — `fisilti`, `FISILTI`, `Fısıltı` hepsi çalışır)
- Yanlış şifre → input shake animasyonu + "Şifre tutmadı, bir daha dene"
- Doğru şifre sessionStorage'ta saklanır (sekme açıkken bir daha sormaz, sekme kapanınca temizlenir)
- `body.locked` class'ını dev tools'tan silen biri için decoy: "Jüri olduğuna emin misin? :)" kırmızı vurguyla

## Jüriye URL paylaşımı

Etkinlik öncesi jüriye paylaşılacaklar:

```
https://[site-url]/kor-nokta/kartlar.html       Karakter kartları
https://[site-url]/kor-nokta/notlar.html        Gizli notlar
Şifre: fısıltı
```

Public site'ta bu linkler hiçbir yerde geçmez. Jüri olmayan ekip üyeleri:
- Public navigasyonda göremez
- Kaynak koduna baksa da bulamaz
- Path'i tahmin etmesi pratikte mümkün değil

## Hosting

Statik site — Netlify, Vercel, GitHub Pages, S3, Nginx hepsiyle çalışır.

**Netlify Drop:**
1. https://app.netlify.com/drop adresine git
2. `kimin-sapkasi-netlify.zip` dosyasını sürükle (kök seviyeli zip)

**Yerel test:**
```bash
cd kimin-sapkasi-site
python3 -m http.server 8000
# http://localhost:8000
```

> Web Crypto API güvenli context gerektirir. `file://` ile bazı tarayıcılarda decrypt çalışmayabilir; `localhost` veya `https://` ile sorunsuz.

## Tasarım

- **Palet:** Paper #F6F7FA · Ink #0F1B2D · Gold #B8923D
- **Tipografi:** Inter (gövde) + Fraunces (display, ölçülü) + JetBrains Mono (etiketler)
- **Hareket:** Scroll progress bar, IntersectionObserver reveal-on-scroll, number counters, hat-icon hover rotation
- **Erişilebilirlik:** `prefers-reduced-motion: reduce` desteği

## Yazdırma

`brifing.html` ve `ekipler.html` A4 portrait için optimize. Ctrl/Cmd+P ile yazdır; navigasyon, scroll bar, motion, search alanı print'te otomatik gizlenir.

## İçerik

**Vaka:** MARJ-7 — Müşterinin 10 yıllık in-house marj hesaplama programı, gece batch'i, "anlık görmek istiyorum" talebi.

**Şapkalar (6 takım):** Silindir · Fötr · Kasket · Fes · Bere · Panama

**Şapka değişim eşleşmeleri:**
- Junior Dev → Solution Architect
- Senior Dev → Proje Yöneticisi
- Solution Architect → Junior Dev
- Proje Yöneticisi → Group Manager
- Team Lead → Müşteri Temsilcisi
- Group Manager → Modül Danışmanı

**Jüri (6 paydaş karakteri):** Müşteri Sponsor · Müşteri Operasyon · Modül Danışmanı · Solution Governance · Legal/Bütçe · Üst Yönetim

**Skor:** 600 puan + 60 bonus (gizli ipuçlarını açma)

---

İyi atölyeler 🎩
