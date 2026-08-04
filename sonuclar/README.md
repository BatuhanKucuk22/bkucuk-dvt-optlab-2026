# Karşılaştırmalı Performans Metrikleri (PSNR, SSIM, MOTA, FPS)

Bu dizin, projenin farklı aşamalarında ve farklı veri setleri (MOT17 Train/Test) üzerinde gerçekleştirilen deneylerin **resmi akademik çıktılarını, benchmark raporlarını ve performans analizlerini** barındırmak amacıyla oluşturulmuştur.

Yazılım mühendisliğindeki "Separation of Concerns" (Sorumlulukların Ayrılığı) prensibi gereği; kaynak kodlar ve mühendislik eforu `hafta3-4_iyilestirme` dizininde tutulurken, bu dizin tamamen sonuçların vitrini olarak görev yapar.

## 🏆 Güncel Rekorlar (Spikedge Pipeline)
Geliştirilen Ters İzdüşüm (Inverse Mapping) algoritması sayesinde sistemin MOT17 eğitim (train) seti üzerindeki güncel izleme başarımları orijinal SOTA değerlerini geride bırakmıştır:
* **MOTA (Takip Doğruluğu):** % 83.61
* **HOTA (Genel Başarı):** % 72.25
* **IDF1 (Kimlik Koruma):** % 81.09

## 📁 Dizin İçeriği
* `benchmark_train_inverse_mapping.md`: Ters izdüşüm algoritmasının sisteme entegre edilmesinden önceki hatalı durum ile güncel durumun ve orijinal HybridSORT referanslarının karşılaştırmalı analizi.
