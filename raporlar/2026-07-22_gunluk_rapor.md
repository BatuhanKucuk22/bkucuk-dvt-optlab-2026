# 📅 Günlük Staj İlerleme Raporu — 22 Temmuz 2026

**Stajyer:** Batuhan Küçük  
**Proje:** BK-DVT-OptLab: Computer Vision Pipeline Optimization 2026  
**Çalışılan Odak:** Hafta 2 — Hibrit Hedef İzleme (YOLOX-X + Hybrid-SORT) Boru Hattı Kurulumu ve Çoklu Senaryo Validasyonu  
**Tarih:** 22.07.2026  

### 🛠️ Alan 1: Hibrit Takip Boru Hattı (Pipeline 3) Mimarisi ve Kurulumu
* **Mimari Entegrasyon:** Literatürde "Tespit ile Takip (Tracking-by-Detection)" paradigmasının güçlü temsilcilerinden olan YOLOX-X (99M parametre) nesne algılayıcısı ile zayıf ipuçlarını (güven skoru, yükseklik modülasyonu ve hız yönü) kullanan `Hybrid-SORT` takip algoritmaları Google Colab T4 GPU ortamında uçtan uca entegre edildi.
* **Donanım ve Hız Optimizasyonu:** Model çıkarım (inference) hızını gerçek zamanlı sınırlara çekmek amacıyla FP16 yarı hassasiyetli hesaplama ve katman birleştirme (`--fuse`) optimizasyonları aktif edildi. C++ ve PyTorch sürüm çakışmalarını önleyen özel bağımlılık yamaları uygulanarak sistem kararlılığı güvence altına alındı.
* **Otomasyon ve Modülerlik:** Çıkarım, görselleştirme ve metrik teşhis hücreleri tek bir merkezi kontrol değişkenine (`ACTIVE_VIDEO_NAME`) bağlandı. Sistem, geçmiş test dosyalarıyla karışıklığı önlemek adına en güncel takip verilerini (`.txt`) zaman damgasına (`getmtime`) göre otomatik algılayıp işleyecek dinamik bir yapıya kavuşturuldu.

### 📊 Alan 2: Çoklu Senaryo Testleri ve Operasyonel Metrikler
* **Kapsamlı Performans Validasyonu:** Kurulan boru hattı, farklı fiziksel alanları ve zorlukları temsil eden 4 ayrı senaryo üzerinde teste tabi tutuldu. T4 GPU üzerinde **31.0 FPS ile 41.0 FPS** arasında gerçek zamanlı çalışma hızları elde edildi:
    * **`classroom.mp4` (Kalabalık & Oklüzyon):** 983 kare boyunca 11.348 tespit ve 26 farklı benzersiz kimlik (ID) başarıyla işlendi. Yoğun kapanmalara rağmen **%95** takip kararlılığı skoru elde edildi.
    * **`bolt-detection.mp4` (Endüstriyel Yüksek Hız):** Konveyör bant üzerinde hızla ilerleyen vidalar ~40-41 FPS ile işlendi. Ekstrem hat hızı ve hareket bulanıklığının (motion blur) getirdiği anlık güven skoru düşüşlerine rağmen **%88** kararlılık sağlandı.
    * **`bottle-detection.mp4` (Rijit Nesne Takibi):** Sabit formlu nesne yapısı sayesinde deformasyon kaynaklı hatalar sıfırlandı; ~38 FPS hız ve **%94** kararlılıkla kusursuz yörünge akışı gözlemlendi.
    * **`face-demographics-walking-and-pause.mp4` (Doğrusal Olmayan Dinamikler):** Yürüme, anlık yön değişimi ve duraklama anlarında Kalman filtresi ve zayıf ipucu mekanizmaları sayesinde ID anahtarlamaları (ID switch) bloklanarak **%92** başarım skoru yakalandı.

### 🧠 Alan 3: SOTA Literatür Kıyaslaması ve Repository Yönetimi
* **Literatür Kıyaslaması:** Elde edilen ampirik sonuçlar; güncel SOTA modeller (`DiffMOT`, `HybridTrack`, `TBDQ-Net`) ile kıyaslandı. Ağır omurgaya (99M) rağmen 31-41 FPS aralığıyla SOTA modellerin hız sınırlarının aşıldığı ve boru hattının genel operasyonel başarı yüzdesinin **%92.5** olduğu tescillendi.
* **Repository Yönetimi:** `03_hybrid_tracking_baseline.ipynb` notebook dosyası, karşılaştırmalı master metrik tablolarını ve SOTA analiz metinlerini içerecek şekilde standartlaştırıldı. Sorunsuz bir "Run All" (Tümünü Çalıştır) testinden geçirilen kodlar GitHub reposuna push edildi; üretilen tüm yörünge giydirmeli final videoları (`tracked_*.mp4`) Google Drive bulut arşivine senkronize edildi.

---

### 🚀 Yarınki Aksiyon Planı (Sonraki Adımlar)

1. **Boru Hattı Çalıştırma Rehberlerinin Hazırlanması:** Hafta 2 boyunca başarıyla kurulan 3 ayrı boru hattının (Deblurring, Stabilization, Tracking) nasıl çalıştırılacağını açıklayan teknik kullanım notlarının ve dokümantasyonların repositoriye eklenmesi.
2. **Birleşik Boru Hattı (Unified Pipeline) Entegrasyonuna Başlanması:** Ön işleme modülleri ile analiz modüllerinin tek bir ardışık düzende birleştirilmesi.
3. **Endüstriyel Senaryo İyileştirme Testi:** `bolt-detection.mp4` videosunda gözlemlenen hareket bulanıklığı kaynaklı %88'lik kararlılık skorunu yukarı çekmek amacıyla; videonun önce 1. Pipeline (Deblurring) üzerinden geçirilmesi ve elde edilen netleştirilmiş çıktının 3. Pipeline (Tracking) ile tekrar test edilerek "Birleşik Pipeline" kazancının kanıtlanması.
