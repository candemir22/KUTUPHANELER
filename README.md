# 🗄️ Hazine — Cephe Sistemleri Yerel Kütüphane Deposu

Bu klasör, rastgele CDN'lere (jsDelivr, docs.opencv.org vb.) bağımlı olmadan,
**kendi kontrolünüzdeki bir adresten** (GitHub Pages) çalışacak şekilde
hazırlanmış 4 temel kütüphaneyi içerir. Hepsi gerçek tarayıcı testinden
geçti, CDN'siz çalıştığı kanıtlandı.

## İçindekiler

| Klasör | Kütüphane | Ne işe yarar | Boyut |
|---|---|---|---|
| `kutuphaneler/tesseract/` | Tesseract.js v5 + Türkçe/İngilizce veri | Yazı/OCR okuma | ~29 MB |
| `kutuphaneler/pdfjs/` | PDF.js (resmi Mozilla) | PDF okuma | ~1.7 MB |
| `kutuphaneler/opencv/` | OpenCV.js (resmi derleme) | Çizgi/şekil tanıma | ~13 MB |
| `kutuphaneler/threejs/` | Three.js | 2D/3D görselleştirme | ~750 KB |
| `ornekler/hazine_test.html` | — | Hepsinin çalıştığını gösteren test sayfası | — |

## ⚠️ ÖNEMLİ: Neden GitHub Pages gerekiyor (sadece dosyaları açmak yetmez)

Three.js ve PDF.js gibi modern kütüphaneler "ES modül" formatında.
Tarayıcılar güvenlik gereği, bilgisayarınızda **çift tıklayarak açtığınız**
(`file://` ile başlayan) sayfalarda modül yüklemeyi engelliyor. Bu bir
hata değil, tarayıcının kuralı. Çözüm: dosyaları **gerçek bir web adresinden**
(`https://...`) sunmak — GitHub Pages tam olarak bunu ücretsiz yapıyor.

## 📤 GitHub'a Yükleme (git bilmeden, sadece tarayıcıdan)

1. **github.com**'da oturum açın
2. Sağ üstte **"+"** ikonuna basıp **"New repository"** deyin
3. İsim verin (örn: `cephe-hazine`), **"Public"** seçin, **"Create repository"**
4. Açılan sayfada **"uploading an existing file"** linkine tıklayın
   (veya **"Add file" → "Upload files"**)
5. İndirdiğiniz `hazine.zip` dosyasını **önce bilgisayarınızda çıkartın**
   (sağ tık → "Ayıkla/Extract"), sonra içindeki **`kutuphaneler` ve
   `ornekler` klasörlerini** o GitHub sayfasına **sürükleyip bırakın**
6. Altta **"Commit changes"** (yeşil buton) deyin — bu "push" demek,
   yani dosyalarınızı GitHub'a kalıcı olarak göndermek

## 🌐 GitHub Pages'i Açma (adresi canlıya alma)

1. Deponuzda üstteki **"Settings"** sekmesine girin
2. Sol menüden **"Pages"**'e tıklayın
3. **"Branch"** altında **"main"** seçin, **"Save"** deyin
4. 1-2 dakika sonra sayfa size bir adres verecek, örneğin:
   ```
   https://kullaniciadiniz.github.io/cephe-hazine/
   ```
5. Artık kütüphaneleriniz şu şekilde erişilebilir:
   ```
   https://kullaniciadiniz.github.io/cephe-hazine/kutuphaneler/tesseract/tesseract.min.js
   https://kullaniciadiniz.github.io/cephe-hazine/kutuphaneler/opencv/opencv.js
   https://kullaniciadiniz.github.io/cephe-hazine/kutuphaneler/pdfjs/pdf.min.mjs
   https://kullaniciadiniz.github.io/cephe-hazine/kutuphaneler/threejs/three.module.min.js
   ```

Bu adresleri, daha önce yaptığımız `kanat_sayac_v1.html` gibi
araçların içindeki CDN linklerinin (`cdn.jsdelivr.net/...`) yerine
koyacağız — o zaman hem kendi kontrolünüzde hem de kalıcı olacak.

## 🧪 Test Sayfası

`ornekler/hazine_test.html` dosyasını (GitHub Pages'e yükledikten sonra
o adresten) açarsanız, 4 kütüphanenin de yüklenip çalıştığını gösteren
✅ işaretli bir liste görürsünüz. Ben bunu gerçek tarayıcıda test ettim,
4/4 başarılı.

## 🗺️ Sırada Ne Var (bir sonraki aşamalar)

Bu ilk faz. Konuştuğumuz ama henüz eklemediğimiz kısımlar:

- **Ses tanıma (STT) / Metni sese çevirme (TTS)** — iyi haber: Chrome'un
  kendi yerleşik `Web Speech API`'si var, ekstra kütüphane indirmeye bile
  gerek yok, tarayıcıda hazır geliyor.
- **Yüz tanıma** — `face-api.js` gibi bir kütüphane + önceden eğitilmiş
  model dosyaları gerekiyor, ayrı bir faz olarak ekleyebiliriz.
- **Makine öğrenmesi** — TensorFlow.js tarayıcıda çalışan versiyon,
  bunu da bu hazineye ekleyebiliriz.

Şimdilik bu 4 kütüphaneyi GitHub'a yükleyip Pages'i açalım, çalıştığını
doğrulayalım — sonra üzerine ekleye ekleye gideriz.
