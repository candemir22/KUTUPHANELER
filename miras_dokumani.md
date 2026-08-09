# PROJE MİRAS DOKÜMANI — Cephe Sistemleri Dijital Arşiv & Analiz Sistemi

> Bu metni yeni bir Claude sohbetinin EN BAŞINA yapıştır. Claude'a önceki
> oturumlarda nereye kadar geldiğimizi ve nasıl kodlaması gerektiğini anlatır.

## Kullanıcı Profili
- Alüminyum giydirme cephe sistemleri ile uğraşıyor (kataloglar, DXF çizimler, PDF teknik dökümanlar)
- Python/HTML/JS öğreniyor, aktif ve meraklı, git/GitHub'a yeni başladı (komut satırı değil, GitHub web arayüzünden sürükle-bırak ile yükleme yapıyor)
- Türkçe konuşuyor — kod içindeki fonksiyon/değişken isimleri de TÜRKÇE tutuluyor (örn: `veri_tabanini_yukle`, `dikmeVeYatayTespitEt`)
- GitHub kullanıcı adı: **candemir22**, kütüphane deposu: **github.com/candemir22/sait**

## Şu Ana Kadar Kurulanlar

### 1) Python/Streamlit uygulaması (`APP.py`)
Yerel çalışan masaüstü arşiv sistemi:
- PDF/resim/DXF dosyalarını tarar (önce metin katmanı, yoksa Tesseract OCR)
- Tesseract yolunu otomatik bulur (PATH'e güvenmez, bilinen kurulum yollarını dener)
- Her sayfanın görselini de kaydeder (`sayfa_gorselleri/` klasörü)
- "Detay Bazlı Analiz": DETAY N sayfası seçince görsel + malzeme/ağırlık tablosu yan yana gösterir, PDF olarak dışa aktarır
- Sonuçları `sistemler_veritabani.json`'a kaydeder
- **Versiyonlanıyor**: `versiyonlar/APP_v1.py` gibi, eskiler bozulmadan yeni versiyon eklenir

### 2) Canlı Python↔HTML köprüsü (`tarama_motoru.py` + `sunucu.py`)
- `tarama_motoru.py`: OCR/PDF/DXF tarama mantığının PAYLAŞILAN tek kopyası (Streamlit'e bağımlı değil, `log` callback kullanır)
- `sunucu.py`: Flask ile `localhost:5000` üzerinden çalışan API, HTML'den sürüklenen dosyayı anında tarar

### 3) Bağımsız HTML+JS araçları (hepsi "blueprint/teknik çizim" tasarım dilinde)
**Tasarım dili:** Navy mavi zemin (`#0B3D5C`/`#062639`), cyan çizgiler (`#7FD8F5`/`#4C96AE`), kağıt rengi paneller (`#F0EDE4`), rust turuncu vurgu (`#C1592A`), yeşil (`#5FBE8A`). Fontlar: **Big Shoulders Display** (başlıklar), **IBM Plex Sans** (gövde metni), **IBM Plex Mono** (kod/veri/sayılar). İnce cetvel/grid arka plan deseni, köşe crop-mark'ları (teknik çizim föyü hissi).

- `analiz_paneli_v1.html`: Kaydedilmiş `sistemler_veritabani.json`'u sürükle-bırak ile yükleyip görselleştiren panel (arama, malzeme dağılım grafiği, DETAY kartları)
- `canli_analiz_v1.html`: `sunucu.py`'ye canlı bağlanan, büyük sürükle-bırak alanlı anlık analiz ekranı
- `cephe_hesaplayici_v1.html`: DXF dosyasından cephe eni/boyu + dikme/yatay SAYISINI hesaplar — **tamamen JavaScript, Python'a hiç ihtiyaç yok** (DXF zaten düz metin/koordinat)
- `kanat_sayac_v1.html`: OpenCV.js ile çizgi SAYAR (dikme/yatay), Tesseract.js ile yazıdan Eni/Boyu OKUR — ikisi ayrı kaynak, kalibrasyon/ölçme YOK, sadece sayma + okuma

### 4) "Hazine" — Kendi Kontrolündeki Kütüphane Deposu
**Neden:** CDN'lere (jsDelivr, docs.opencv.org) bağımlı kalmak yerine, kütüphaneleri `github.com/candemir22/sait` deposunda kalıcı barındırıyoruz, GitHub Pages ile yayınlıyoruz.

`kutuphaneler/` klasöründeki içerik:
- `tesseract/` — Tesseract.js v5 + tur/eng dil verisi (OCR)
- `pdfjs/` — PDF.js resmi derleme (PDF okuma)
- `opencv/` — OpenCV.js resmi derleme (çizgi/şekil tanıma)
- `threejs/` — Three.js (2D/3D görselleştirme)
- `tensorflow/` — TensorFlow.js (makine öğrenmesi)
- `yuz-tanima/` — face-api.js + model ağırlıkları (yüz TESPİTİ, henüz "tanıma" değil)
- `ses-tanima-offline/` — vosk-browser (`vosk.js`), Türkçe model dosyası (`model.tar.gz`) kullanıcı tarafından elle ekleniyor (35MB, alphacephei.com'dan)

`ornekler/` klasöründe her kütüphanenin çalıştığını kanıtlayan test sayfaları var (`hazine_test.html`, `ses_demo.html`, `ses_tanima_offline_demo.html`).

### GitHub Pages Adresi (ÖNEMLİ — bunu arama, doğrudan kullan)

Hazine deposu **Public** olduğu için özel bir API anahtarı/token/bağlantı GEREKMİYOR — GitHub Pages sadece sabit, herkese açık bir HTTPS adresi veriyor. Adres formatı:

```
https://candemir22.github.io/sait/<dosya-yolu>
```

Örnekler:
```
https://candemir22.github.io/sait/kutuphaneler/opencv/opencv.js
https://candemir22.github.io/sait/kutuphaneler/tesseract/tesseract.min.js
https://candemir22.github.io/sait/kutuphaneler/pdfjs/pdf.min.mjs
https://candemir22.github.io/sait/kutuphaneler/threejs/three.module.min.js
https://candemir22.github.io/sait/kutuphaneler/tensorflow/tf.min.js
https://candemir22.github.io/sait/kutuphaneler/yuz-tanima/face-api.min.js
```

**Doğrulama:** Kullanıcıya sor / repo Settings → Pages sekmesinde "Your site is live at ..." kutusundaki TAM adresi teyit et — kullanıcı adı veya depo adı değişmiş olabilir. Adres 404 veriyorsa Pages henüz aktifleşmemiş olabilir (birkaç dakika sürebilir) veya branch/klasör ayarı yanlış olabilir.

✅ **TEYİT EDİLDİ (bu oturumda):** `https://candemir22.github.io/sait/` doğru adres, kullanıcı tarafından onaylandı.

⚠️ Not: Bu adresin **kökü** (`/sait/`) doğrudan açıldığında sadece ham/süssüz README görünür veya boş gibi hisseder — bu normaldir, endişelenme. Sistemin gerçekten çalıştığını görmek için doğrudan bir alt sayfa aç, örn:
```
https://candemir22.github.io/sait/ornekler/hazine_test.html
```
Bu sayfa 6 kütüphanenin de yüklendiğini gösteren yeşil ✅ satırları göstermeli.

Bu adres herhangi bir platformdan (Blogger dahil) şifre/login gerektirmeden `<script src="...">` ile doğrudan çağrılabilir — normal bir dosya linki gibi davranır, özel bir "bağlantı kurma" işlemi yoktur.

## Öğrenilen Kritik Teknik Dersler (TEKRARLANMASIN)

1. **Tesseract PSM sorunu**: Teknik çizimlerdeki izole/dağınık metin etiketleri (örn. çizginin uzağındaki "BOYU 1500") varsayılan otomatik sayfa modunda GÖZDEN KAÇIYOR. Çözüm: `worker.setParameters({ tessedit_pageseg_mode: '11' })` (sparse text modu). Bu OLMADAN OCR güvenilmez.

2. **ES modül + `file://` sorunu**: Three.js ve PDF.js gibi modern kütüphaneler ES modül formatında. Tarayıcılar `file://` (çift tıklayarak açılan) sayfalarda modül yüklemeyi GÜVENLİK GEREĞİ engelliyor. Çözüm: gerçek bir `http(s)://` adresinden sunmak — GitHub Pages bunu çözüyor. Yerel test için `python -m http.server` yeterli.

3. **OpenCV.js başlatma**: `@techstark/opencv-js` paketi `cv`'yi bir **Promise** olarak döndürüyor (13MB WASM senkron yüklenemez). `cv.onRuntimeInitialized` YETMİYOR, `if (cv instanceof Promise) { cv.then(gercekCv => {...}) }` gerekiyor.

4. **Three.js parça bağımlılığı**: `three.module.min.js` içeriden `three.core.min.js`'i import ediyor — ikisi birden kopyalanmalı.

5. **Sandbox ağ kısıtlamaları**: Claude'un kod çalıştırma ortamı sadece belirli domainlere erişebiliyor (npm registry, GitHub, PyPI gibi) — `alphacephei.com`, `docs.opencv.org`, `cdn.jsdelivr.net` gibi adreslere ERİŞEMİYOR. Bu yüzden bazı dosyalar (büyük model dosyaları) kullanıcının kendi tarayıcısından indirilmesi gerekiyor.

6. **GitHub Pages önbellek gecikmesi**: Bir dosyayı GitHub'da güncelleyip hemen test edince ESKİ hali görünebilir (tarayıcı önbelleği veya GitHub'ın CDN'i henüz güncellenmemiş olabilir). Çözüm: gizli/InPrivate pencerede aç, ya da adresin sonuna `?v=2` gibi rastgele bir parametre ekle.

✅ **DOĞRULANDI (bu oturumda):** `https://candemir22.github.io/sait/ornekler/hazine_test.html` gizli modda açıldı, 6/6 kütüphane yeşil (Tesseract dahil, gerçek OCR ile "ENİ 2000 / BOYU 1500" doğru okundu). Sistem CANLI ve ÇALIŞIYOR.

## Kullanıcı Tercihleri / Kısıtlar

- CDN bağımlılığından hoşlanmıyor, "kendi hazinesinde" barındırmayı tercih ediyor
- Kod içi isimlendirme Türkçe olsun istiyor
- Yeni bir şey kurmadan önce "araştırma" yapılmasını, aceleyle kod yazılmamasını seviyor — ama netleştikten sonra hızlı ilerlemek istiyor
- Kart/tablo tarzı sıradan dashboard'lar yerine özel tasarlanmış "blueprint" estetiği bekliyor
- Para/kredi kartı gerektiren şeylere (Anthropic API gibi) şimdilik hazır değil — ücretsiz/yerel çözümler tercih ediliyor

## Claude'a Talimat (yeni oturumda bunu oku, ona göre davran)

1. Yeni bir HTML aracı istenirse: önce yukarıdaki "hazine" kütüphanelerini CDN yerine kullanmayı düşün (`https://candemir22.github.io/sait/kutuphaneler/...` adresleri)
2. Blueprint tasarım diline sadık kal (renkler/fontlar yukarıda yazılı)
3. Türkçe değişken/fonksiyon isimleri kullan
4. Kod üretmeden önce mimariyle ilgili belirsizlik varsa kısa bir soru sormaktan çekinme
5. Yukarıdaki "Kritik Teknik Dersler" bölümündeki hataları TEKRARLAMA
6. Kullanıcı git/GitHub konusunda yeni — komut satırı değil, GitHub web arayüzü üzerinden basit adımlarla anlat
