# 📅 Günlük Staj İlerleme Raporu — 4 Ağustos 2026

**Stajyer:** Batuhan Küçük  
**Proje:** Spikedge Görüntü İşleme ve Çoklu Nesne Takibi (MOT) Boru Hattı  
**Çalışılan Odak:** Ters İzdüşüm (Inverse Mapping) Baseline Testi, Stage Swap (Aşama Yer Değiştirme) Ablasyon Deneyi, Dinamik Görsel Kalite Analizi (ITF) ve GitHub Mimari Senkronizasyonu  
**Tarih:** 04.08.2026  

---

### 🎯 Dünkü Hedeflerin Gerçekleşme Durumu
*   **Gerçekleşti - Plan B (Eğitim Seti Ablasyon Kıyaslaması):** Codabench onayını beklemeden zamanı optimize etmek adına, Ters İzdüşüm (Inverse Mapping) algoritması yerel MOT17-Train seti üzerinde çalıştırıldı. Beklendiği gibi metriklerde devasa bir sıçrama yakalandı.
*   **Gerçekleşti - Repository Senkronizasyonu:** Yazılım mühendisliğindeki "Separation of Concerns" (Sorumlulukların Ayrılığı) prensibi benimsenerek GitHub reposu `hafta3-4_iyilestirme` (motor kodları) ve `sonuclar` (vitrin ve benchmark) olarak ikiye ayrıldı. İlgili Markdown dosyaları (README ve Benchmark) profesyonel commit mesajlarıyla repoya işlendi.
*   **İleri Taşındı - Test Seti Metrik Tescili:** Gün içerisinde sunucu (server) tarafında yaşanan anlık bağlantı kopmaları ve ağır işlem yükü kaynaklı GPU (T4) limit aşımı / kota problemleri nedeniyle, resmi MOT17-Test seti metriklerinin TrackEval üzerinden hesaplanması süreci mecburi olarak yarına ertelenmiştir.

---

### 🛠️ Günün Teknik Gelişmeleri ve Çözümleri

#### 1. Baseline Ters İzdüşüm Başarısı (V1)
Günün ilk aşamasında, orijinal `[Netleştirme ➔ Stabilizasyon]` sırası korunarak Ters İzdüşüm (Inverse Mapping) algoritması sisteme entegre edildi ve MOT17-Train seti üzerinde uçtan uca test edildi. Piksellerdeki kaymanın geriye yansıtılmasıyla sistem **MOTA %83.612** ve **HOTA %72.252** skorlarına ulaştı. Ayrıca takip algoritmalarının en büyük handikabı olan Kimlik Kopması (IDSW) sayısı 883'ten 557'ye düşürüldü.

#### 2. Stage Swap (Aşama Yer Değiştirme) Mimari Optimizasyonu (V2)
Algoritmaların çalışma sırasının takip performansına olan matematiksel etkisi araştırıldı. Orijinal sıralama tersine çevrilerek `[Stabilizasyon ➔ Netleştirme]` (Stage Swap) kurgusuna geçildi. Bu sayede LightStab algoritmasının, yapay zekanın müdahale ettiği pikseller yerine **doğrudan ham videodaki (raw) optik akışı** okuması sağlandı.

#### 3. Mimari Kıyaslama (V1 vs V2) ve Kümülatif Metriklerde Zirve
Yeni Swap (V2) mimarisinin ham video üzerinden çıkardığı saf izdüşüm matrisleri, günün ilk testine (V1) kıyasla nesne takibi metriklerinde kıl payı da olsa daha yüksek ve kararlı bir zirveye ulaşılmasını sağladı:
*   **MOTA (Takip Doğruluğu):** %83.612'den **%83.631** seviyesine yükseldi.
*   **HOTA (Genel Başarı):** %72.252'den **%72.391**'e çıktı.
*   **Kimlik Değişimi (IDSW):** Sistemdeki kopmalar 557'den **546'ya** düşerek, hedef kutularının (bounding box) sarsıntılı uzaya V2 mimarisinde çok daha iyi zımbalandığını matematiksel olarak kanıtladı.

#### 4. Dinamik Görsel Kalite ve Pürüzsüzlük (ITF) Analizi
Sabit (hardcoded) kalite referansları terk edilerek, doğrudan Python çekirdeğine entegre edilen dinamik bir görüntü analiz motoru kodlandı. MOT17-04 sekansı üzerinde yapılan gerçek zamanlı ölçümlerde;
*   Ham görüntüye müdahale oranını temsil eden **PSNR** ortalama 33.33 dB olarak ölçüldü.
*   Kareler arası ardışık pürüzsüzlüğü ve stabilizasyon kararlılığını ölçen **ITF (Interframe Transformation Fidelity)** skoru **36.00 dB** çıkarak, literatürdeki StabiGS (~34.87 dB) gibi SOTA modellerin üzerine çıkmayı başardı.

---

### 🔍 GitHub Repository İçin Notebook Analizleri (.ipynb)
GitHub reposuna gün içerisinde eklenen iki farklı `.ipynb` dosyasının mühendislik incelemesi sonucunda öne çıkan yapısal detaylar şunlardır:

*   **`MOT17_Test_dataset_train.ipynb` (V1 Baseline):** TrackEval motorunun çökmesine neden olan aynı karedeki (frame) mükerrer kimlik tahminleri, telemetri okuma döngüsüne entegre edilen zırhlı kalkan (`seen_instances = set()`) ile otonom olarak filtrelenerek veri temizleme (Duplicate ID Sanitization) işlemi başarıyla kod tabanına işlenmiştir.
*   **`MOT17_Test_swap_dataset_train.ipynb` (V2 Swap):** Modüler kalite metrikleri entegrasyonu için `skimage.metrics` kütüphanesi kod tabanına dahil edilmiş ve RAM darboğazını (OOM) önlemek adına hesaplama döngüsü ilk 100 kare (`frame_limit = 100`) ile sınırlandırılmıştır. Ayrıca önceki ablasyon deneyine ait 4 saatlik verilerin ezilmemesi için sistemin çıktı dizinleri `intermediate_swap`, `output_tracks_swap` ve `telemetry_swap` olarak izole edilmiş, girdi videoları ise depolama maliyetini minimize etmek amacıyla ortak `input_videos_train` klasöründen okunmuştur.

---

### 🎯 Yarınki Eylem Planı (Stratejik Plan)
1.  **Sunucu Hatalarının Aşılması ve Test Seti Metrikleri:** Bugün yaşanan GPU limit ve server kopması sorunlarının ardından, yeni ve temiz bir ortamda TrackEval motorunun doğrudan "Kör Test" (MOT17-Test Seti) sekanslarına uygulanarak nihai resmi metriklerin (HOTA, MOTA, IDF1) elde edilmesi.
2.  **Karşılaştırmalı Test Raporlaması:** Test veri setinden alınacak kümülatif sonuçların, orijinal HybridSORT/ByteTrack referanslarıyla kıyaslandığı nihai bir benchmark Markdown dosyasının hazırlanıp GitHub vitrinine (`sonuclar` klasörüne) işlenmesi.
3.  **Proje Dokümantasyonu:** Elde edilen bu güçlü çıktılarla, staj sonu teknik raporunun / sunumunun iskelet yapısının oluşturulmasına devam edilmesi.
