# Hafta 3-4: Özgün Mimari İyileştirmeler & Ablation Çalışmaları

Bu dizin, Spikedge Görüntü İşleme Boru Hattı'nın (DeepRFT + LightStab + HybridSORT) üzerine inşa edilen özgün algoritmik iyileştirmeleri ve ablasyon (ablation) deneylerini barındırır. 

Çalışmaların odak noktası, farklı görüntü işleme modellerinin birbiri ardına (pipeline) çalıştırılması sırasında ortaya çıkan geometrik kaymaları ve veri uyumsuzluklarını otonom olarak çözen ara katmanlar (middleware) geliştirmektir.

## 🚀 Öne Çıkan Geliştirmeler
* **Ters İzdüşüm (Inverse Affine Mapping):** Video stabilizasyonu (LightStab) aşamasında piksellerde meydana gelen kaymanın, nesne takibi (MOT) bounding box koordinatlarına ters afin matris dönüşümü ile geri yansıtılması işlemi.
* **Duplicate ID Sanitizer:** İzleme motorundan gelen ve TrackEval değerlendirme aracının çökmesine neden olan aynı karedeki (frame) kopya kimlikleri (ID) otonom olarak temizleyen zırhlı filtreleme sistemi.
* **Aşama Yer Değiştirme (Stage Swap) Deneyleri:** Modellerin çalışma sırasının (Örn: Stabilizasyon ➔ Netleştirme vs. Netleştirme ➔ Stabilizasyon) izleme metriklerine olan etkisinin incelenmesi.

## 📁 Dizin İçeriği
* `MOT17_Test_dataset_train.ipynb`: Ters izdüşüm, veri temizleme ve TrackEval kümülatif motorunun entegre edildiği, MOT17 eğitim seti üzerinde çalışan uçtan uca zırhlı test motoru.
