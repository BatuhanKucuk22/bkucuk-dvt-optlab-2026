# 🤖 AI Tools Review: Gemini vs. Claude (MOT Pipeline Architecture)

**Analist:** Batuhan Küçük (Lead Computer Vision Engineer)  
**Tarih:** 29.07.2026  
**İncelenen Dosyalar:** `Hybrid_Pipeline_car_person_gemini.ipynb` ve `Hybrid_Pipeline_car_person_claude.ipynb`  

Bu rapor, aynı bilgisayarlı görme (Computer Vision) ve Çoklu Nesne Takibi (MOT) problemi için iki farklı Büyük Dil Modeli (Gemini ve Claude) tarafından geliştirilen boru hatlarının (pipeline) mühendislik felsefelerini ve performans metriklerini kıyaslamak amacıyla hazırlanmıştır.

---

## 🏗️ 1. Mimari ve Algoritmik Felsefe Kıyaslaması

### 🔹 Gemini'nin Yaklaşımı: "Derin Öğrenme Odaklı & Sonradan Filtreleme"
Gemini tarafından oluşturulan pipeline, tespit (detection) aşamasında doğrudan SOTA derin öğrenme modellerine (YOLOX ve HybridSORT) güvenmeyi tercih etmiştir.
* **Kinematik Çapa Kalkanı (Kinematic Anchor Shield):** Gemini, sistemdeki gürültüleri tespit aşamasından önce değil, takip aşamasından sonra temizlemeyi seçmiştir. `max_displacement >= 35.0` ve `net_displacement >= 30.0` gibi katı kinematik kurallar koyarak titreşen veya hayalet nesneleri (3 adet gürültü) izleme listesinden başarıyla silmiştir. 

### 🔸 Claude'un Yaklaşımı: "Sahne Adaptif (Heuristic) & Klasik CV Entegrasyonu"
Claude tarafından oluşturulan pipeline ise çok daha esnek ve otonom bir "Scene-Adaptive" (Sahne Adaptif) yaklaşım sergilemiştir.
* **Otonom Dedektör Kararı:** Claude, videoyu doğrudan YOLOX'a sokmak yerine, önce sahnedeki kenar yoğunluğunu (edge density = 0.0156) ve hareket yoğunluğunu (motion density = 0.0749) otonom olarak analiz etmiştir. Bu analiz sonucunda ağır bir derin öğrenme modeli yerine klasik **MOG2 (Background Subtraction - Trafik Profili)** algoritmasını kullanmaya karar vermiştir.

---

## 📊 2. Kantitatif Performans ve Telemetri Kıyaslaması

Aşağıdaki tablo, iki modelin aynı girdilerle ürettiği **Stage 4 (Telemetri)** sonuçlarını yansıtmaktadır:

| Metrik | Gemini Pipeline | Claude Pipeline | Kazanan / Değerlendirme |
| :--- | :---: | :---: | :--- |
| **Restorasyon (PSNR)** | 35.88 dB | 38.42 dB | **Claude.** Sahne adaptif yaklaşım, görsel sadakati daha iyi korumuştur. |
| **Yapısal Benzerlik (SSIM)** | 0.9760 | 0.9779 | **Claude.** İki değer de mükemmele yakın olsa da Claude marjinal bir farkla öndedir. |
| **Sinyal Kalitesi (DSNR)** | 6.00 | 6.00 | **Berabere.** Her iki sistem de SOTA endüstri standartlarının (> 3.0) iki katına ulaşmıştır. |
| **Ortalama Takip Ömrü** | 44.8 Kare | 41.1 Kare | **Gemini.** Derin öğrenme tabanlı YOLOX modeli, nesnelerin kimliğini daha uzun süre hafızada tutmayı başarmıştır. |
| **Yörünge Düzgünlüğü (Jitter)**| 5.124 px/f² | 2.502 px/f² | **Claude.** Klasik MOG2 algoritması (Claude), derin öğrenme tabanlı YOLOX'a (Gemini) kıyasla çok daha pürüzsüz ve titreşimsiz bir yörünge sunmuştur. |
| **Kimlik Parçalanması** | %0.0 | %0.0 | **Berabere.** İki sistem de kusursuz kimlik bütünlüğü (ID koruma) sağlamıştır. |

---

## 🏆 3. Mühendislik Kararı: Gemini vs. Claude

Her iki model de **dünya standartlarında (SOTA)** mühendislik ürünleri ortaya koymuştur, ancak kullanım senaryolarına göre birbirlerine üstünlük sağlamaktadırlar:

1. **Gemini'nin Güçlü Yanı (Kompleks Sahneler):** Gemini'nin YOLOX'u merkeze alan ve arkasına kinematik kalkan ekleyen çözümü, kalabalık insan grupları, kamera hareketleri veya çoklu oklüzyon (kapanma) olan zorlu sahneler için kesinlikle daha iyi bir tercihtir. Derin öğrenmenin "hafıza" (Lifespan) avantajını çok iyi kullanmaktadır.
2. **Claude'un Güçlü Yanı (Statik kameralar ve Kaynak Optimizasyonu):** Claude'un donanım kaynaklarını koruyan (YOLOX yerine MOG2'ye geçiş yapabilen) sahne adaptif mimarisi ise mükemmel bir zekadır. Özellikle CCTV veya otoyol kameraları gibi statik açılı videolarda, bounding-box titreşimini (Jitter) yarı yarıya azaltarak donanımdan devasa bir tasarruf sağlar.

**Nihai Sonuç:** Endüstriyel bir projede (örneğin Palveo/Spikedge), **Claude'un sahne adaptif karar mekanizmasını (Auto-Probe), Gemini'nin Kinematik Çapa Kalkanı ile birleştirmek** ulaşılabilecek en üst düzey (Ultimate) mimariyi oluşturacaktır.
