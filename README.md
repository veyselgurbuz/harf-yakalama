# 🎹 Harf Yakalama — Klavye Öğrenme Oyunu

8 yaşındaki çocuklar için, klavyedeki harflerin yerini eğlenerek öğreten tek dosyalık bir tarayıcı oyunu.
Kurulum yok, bağımlılık yok, internet gerekmez: **`index.html` dosyasına çift tıkla, oyun açılır.**

![HTML](https://img.shields.io/badge/HTML-5-orange) ![CSS](https://img.shields.io/badge/CSS-3-blue) ![JavaScript](https://img.shields.io/badge/JavaScript-vanilla-yellow)

---

## 🎯 Nasıl oynanır?

Gökyüzündeki bulutlardan büyük harfler aşağı düşer. **Mavi çerçeveli ve zıplayan** harf hedeftir —
klavyede o harfin tuşuna bas. Aynı anda en fazla 3 harf düşer ve sıra her zaman en eski harftedir.

| Durum | Puan | Hata |
|---|---|---|
| ✅ Doğru tuş | **+50** | — |
| ❌ Yanlış tuş | **−50** | +1 |
| ⬇️ Harf zemine değerse | **−50** | +1 |

Puan hiçbir zaman 0'ın altına inmez. Harfler zamanla kademeli olarak hızlanır.

**Oyun iki şekilde biter:**

- 🎉 **15000 puan** → oyunu kazanırsın: ekranda konfeti patlar, alkış sesi çalar.
- 💀 **8 hata** → sağdaki adam asmaca tamamlanır ve oyun biter.

---

## ✨ Özellikler

- **Adam asmaca:** Her hatada bir parça eklenir (zemin → direk → üst çubuk → ip → kafa → gövde → kollar → bacaklar). 8. parçada oyun biter.
- **En İyi 5 skor tablosu:** Oyun sonunda puanın 0'dan büyük ve listedeki 5. sıradan yüksekse en fazla 5 harflik bir isim girip kaydedersin. İsim, puan ve tarih birlikte saklanır. Kayıtlar tarayıcının `localStorage`'ında tutulur, oyun kapatılsa da kaybolmaz.
- **Duraklatma (kötüye kullanıma kapalı):** Duraklatınca harfler tamamen gizlenir; devam edince ekrandaki harfler **değişir**, 3'ten geri sayılır ve bir süre tekrar duraklatılamaz. Böylece duraklatıp tuşu rahatça arama avantajı ortadan kalkar. Puan, hata ve hız aynen korunur.
- **Sesler — hiç ses dosyası yok:** Hepsi Web Audio API ile anlık üretilir. Doğru harfte "ding", yanlışta "buzz", duraklatmada iki notalı ton, geri sayımda bip, kaybedince Mario tarzı hüzünlü ezgi, kazanınca sentezlenmiş alkış + zafer arpeji.
- **Yardım penceresi:** Sağ alttaki **i** düğmesi oyunun tüm kurallarını anlatır; oyun ilk kez açıldığında kendiliğinden gelir.
- **Duyarlı tasarım:** 1400 px masaüstünden 375 px telefona kadar taşma olmadan çalışır.

---

## 🎛️ Düğmeler

| Düğme | İşlevi |
|---|---|
| ▶ Başla | Yeni oyun başlatır |
| ⏸ Duraklat / ▶ Devam Et | Oyunu dondurur ve geri sayımla sürdürür |
| ↻ Yeniden Başlat | Her şeyi sıfırlayıp baştan başlar |
| 🏆 En İyi 5 | Skor tablosunu açar |
| i | Yardım penceresini açar |

`Esc` tuşu açık pencereyi kapatır.

---

## 🚀 Çalıştırma

```bash
git clone https://github.com/veyselgurbuz/harf-yakalama.git
```

Ardından `index.html` dosyasına çift tıklaman yeterli. Derleme, sunucu veya paket kurulumu gerekmez.

> **Not:** Tüm CSS ve JavaScript tek dosyaya gömülüdür. Bunun nedeni, tarayıcıların `file://`
> üzerinden açılan sayfalarda harici `.css` / `.js` dosyalarını güvenlik gereği engelleyebilmesidir;
> tek dosya sayesinde oyun her yerde çift tıklamayla çalışır.

---

## ⚙️ Ayarlar

Oyunun dengesi `index.html` içindeki `AYAR` nesnesinden değiştirilebilir:

```js
var AYAR = {
  MAKS_HARF: 3,            // aynı anda ekrandaki harf sayısı
  DOGRU_PUAN: 50,          // doğru tuş puanı
  HATA_PUAN: 50,           // hata başına düşen puan
  MAKS_HATA: 8,            // kaç hatada oyun biter
  MAKS_PUAN: 15000,        // kazanmak için gereken puan
  GERI_SAYIM: 3,           // devam ederken geri sayım
  DURAKLAT_BEKLEME: 10,    // tekrar duraklatma bekleme süresi (sn)
  BASLANGIC_HIZ: 0.10,     // düşme hızı (alan yüksekliğinin oranı/sn)
  HIZ_ARTISI: 0.005,       // saniye başına hızlanma
  MAKS_HIZ: 0.32           // en yüksek hız
};
```

---

## 🧰 Teknik

Tek bir `index.html`: HTML + CSS + vanilla JavaScript. Harici kütüphane yok.

- Düşme animasyonu `requestAnimationFrame` ile delta zaman tabanlıdır — hız, ekran tazeleme oranından bağımsızdır.
- Hız, oyun alanı yüksekliğinin yüzdesi olarak hesaplanır; farklı ekran boyutlarında aynı hissedilir.
- Adam asmaca satır içi SVG'dir, parçalar hata sayısına göre görünür olur.
- Konfeti, Web Animations API ile üretilir.
