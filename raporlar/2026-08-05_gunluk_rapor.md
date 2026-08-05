# 📅 Günlük Staj İlerleme Raporu — 5 Ağustos 2026

**Stajyer:** Batuhan Küçük  
**Proje:** Spikedge Görüntü İşleme ve Çoklu Nesne Takibi (MOT) Boru Hattı  
**Çalışılan Odak:** Codabench MOT17 Benchmark Analizi, Affine Inverse Mapping (Ters İzdüşüm) Entegrasyonu, 2x2 Grid Görselleştirme Motoru ve Notebook Optimizasyonu  
**Tarih:** 05.08.2026  

---

### 🎯 Dünkü Hedeflerin Gerçekleşme Durumu
*   **Gerçekleşti - Test Seti Metrik Tescili:** TrackEval motoru ve Codabench platformu üzerinden resmi MOT17-Test seti "Kör Test" (Blind Test) sekansları değerlendirildi.
*   **Gerçekleşti - Karşılaştırmalı Test Raporlaması:** Codabench'ten elde edilen resmi HOTA, MOTA ve IDF1 metrikleri analiz edilerek SOTA (State-of-the-Art) kıyaslamasını içeren detaylı `BENCHMARK_ANALYSIS.md` dosyası GitHub reposuna eklendi.
*   **Kısmen Gerçekleşti - Proje Dokümantasyonu:** Rapor iskeleti oluşturuldu ancak test setindeki açısal kamera hareketlerinin (rotasyon) skorlarda yarattığı darboğaz tespit edildiğinden, dokümantasyondan ziyade bu sorunu çözecek mimari koda odaklanıldı.

---

### 🛠️ Günün Teknik Gelişmeleri ve Çözümleri

#### 1. Codabench Resmi Benchmark Analizi ve Darboğaz Tespiti
Günün ilk aşamasında MOT17 test seti sonuçları Codabench'e yüklendi. Kombine edilmiş kümülatif değerlendirmede **HOTA %58.55, MOTA %67.00 ve IDF1 %73.14** elde edildi. Bu sonuçlar yüksek kalitede olsa da, train setindeki iç değerlendirmelerimizin bir miktar altında kaldı.
Mühendislik analizi sonucunda temel darboğazın (bottleneck) "agresif kamera rotasyonları" olduğu saptandı. Geleneksel takip algoritmaları sadece yatay ve dikey ötelemeyi (`dx, dy`) hesaba kattığı için, kamera viraj aldığında takip kutuları (bounding box) nesnelerden kayarak Kimlik Değişimi (`IDSW: 1923`) hatalarına yol açmaktaydı.

#### 2. Çözüm: Affine Inverse Mapping (Ters İzdüşüm) Entegrasyonu
Tespit edilen rotasyon zafiyetini kökünden çözmek için sisteme **Ters İzdüşüm (Affine Inverse Mapping)** algoritması entegre edildi. OpenCV'nin matris dönüşüm matematiklerinden yararlanılarak, rotasyon açısı (`da`) da formüle dahil edildi ve `cv2.invertAffineTransform()` ile hedeflerin koordinatları stabilize edilmiş uzaydan raw (ham) uzaya milimetrik kilitlendi.
*   **Sonuç:** Train setinde yapılan ara testlerde **MOTA %86.936**, **HOTA %74.073** ve **IDF1 %83.022** seviyelerine fırladı. Özellikle IDSW hataları 494'e kadar düşürülerek modelin her türlü kamera açısına karşı devasa bir bağışıklık kazandığı kanıtlandı.

#### 3. 2x2 Grid Vision Engine (Görselleştirme Motoru) Geliştirmesi
Sadece matematiksel metriklerle yetinmeyip "anlamsal video performansını" (semantic performance) kanıtlamak amacıyla, FFmpeg tabanlı bir `filter_complex` ağacı kuruldu. Bu motor sayesinde sırasıyla; Raw Input, Stage 1 (DeepRFT), Stage 2 (LightStab) ve Final Output (HybridSORT) videoları 960x540 boyutlarına ölçeklenip eşzamanlı olarak 2x2 grid formunda tek bir ekranda birleştirildi. Bu sayede pipelinenın her bir aşamasının görüntüye katkısı akademik sunumlara hazır hale getirildi.

#### 4. GitHub Repository Senkronizasyonu
Yapılan tüm test seti analizleri ve mühendislik teşhisleri, `BENCHMARK_ANALYSIS.md` adıyla profesyonelce dokümante edildi. Bu dosya, `docs(benchmark): add MOT17 official Codabench evaluation results and markdown report` standart commit mesajı ile GitHub reposuna işlendi.

---

### 🔍 GitHub Repository İçin Notebook Analizleri (.ipynb)
GitHub reposuna eklenecek olan `MOT17_Test_dataset_train_affine_inverse_mapping_optimize.ipynb` dosyasının mühendislik incelemesi sonucunda şu üst düzey mimari detaylar öne çıkmaktadır:

*   **Akıllı Devam (Smart-Continuation) Mekanizması:** Sistem, `FORCE_CLEAN_RUN` bayrağı (flag) ile donatılmıştır. Bu sayede GPU/sunucu kesintilerinde bile Drive üzerindeki `intermediate` ve `output_tracks` dosyalarının sağlığı (`check_video_health`) kontrol edilmekte, halihazırda işlenmiş videolar atlanarak (bypass) sıfırdan hesaplama maliyetinin önüne geçilmektedir.
*   **Zırhlı Çekirdek ve Geçmişe Dönük Bağımlılık Yamaları (Backward Compatibility):** Modern NumPy 2.x sürümlerinde kullanımdan kaldırılan `np.float` ve `np.int` tipleri nedeniyle çöken TrackEval ve HybridSORT kütüphaneleri, çalışma zamanında regex (düzenli ifadeler) kullanılarak dinamik şekilde onarılmış ve sistemin tam kararlılıkla çalışması garanti altına alınmıştır. Ayrıca DeepRFT modeli doğrudan en stabil olduğu orijinal 5 Aralık 2021 commitine (`a88bf49`) geri döndürülerek yüklenmiştir.
*   **Otonom Telemetri Filtresi (Duplicate ID Sanitizer):** Takip sırasında oluşabilecek frame ve ID çakışmalarını engellemek için `seen_instances = set()` yapısı kullanılmış; aynı karede beliren mükerrer kimliklerin TrackEval motorunu çökertmesi kalıcı olarak engellenmiştir.

---

### 🎯 Yarınki Eylem Planı (Stratejik Plan)
1.  **Affine Inverse Test-Set Koşumu:** Bugün eğitim setinde devasa başarı sağlayan rotasyon destekli Ters İzdüşüm (Affine Inverse) algoritmasının, MOT17 Test Seti üzerinde çalıştırılması ve elde edilecek yeni çıktıların Codabench'e gönderilerek liderlik tablosunda (leaderboard) resmi SOTA HybridSORT modellerinin geçilmesi.
2.  **Görsel Medya Vitrini (Showcase):** 2x2 Grid Vision Engine çıktılarının izole edilip projenin README dosyasında GIF/Video olarak sunulmak üzere hazırlanması.
3.  **Proje Optimizasyonu ve Temizlik:** Geliştirme süreci boyunca biriken gereksiz test loglarının ve gereksiz ağırlık (weight) dosyalarının temizlenmesi; sistemin "Üretim (Production) Hazır" formata tam olarak sokulması.
