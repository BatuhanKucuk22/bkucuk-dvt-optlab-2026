# 🚀 BK-DVT-OptLab: Computer Vision Pipeline Optimization 2026

🇹🇷 **Özet:** Bu depo, Batuhan Küçük'ün 4 haftalık staj programı kapsamında kurguladığı derin öğrenme tabanlı bilgisayarlı görü boru hatlarını (pipeline) barındırır. Projenin temel amacı; **Görüntü Blur Giderme (Deblurring)**, **Gerçek Zamanlı Video Stabilizasyonu** ve **STAPLE & YOLO ile Hibrit Hedef İzleme** alanlarında uçtan uca altyapılar kurmak, bu yöntemleri literatür standartlarında (PSNR, SSIM, MOTA, FPS) ölçümlemek ve seçilen bir odakta sayısal kanıta dayalı mimari optimizasyon sağlamaktır.

🇬🇧 **Overview:** This repository hosts the deep learning-based Computer Vision pipelines developed by Batuhan Küçük during his 4-week internship program. The core objective is to establish end-to-end architectures in **Image Deblurring**, **Real-Time Video Stabilization**, and **STAPLE & YOLO Hybrid Target Tracking**, benchmark these methods using standard literature metrics (PSNR, SSIM, MOTA, FPS), and achieve a quantitative, metric-driven architectural optimization in a selected domain.

---

## 🎯 Proje Odak Alanları (Core Research Domains)

1. **Image Deblurring (Spatial Enhancement):** Motion and defocus blur restoration using modern architectures (e.g., DeepRFT, DeblurGAN-v2).
2. **Video Stabilization (Temporal Smoothness):** Online and causal video stabilization techniques targeting real-time performance and low latency (e.g., LightStab).
3. **Hybrid Target Tracking:** Integrating SOTA detection (YOLOX) with motion prediction (HybridSORT) and Classical CV (MOG2) for robust, scene-adaptive multi-object tracking.

---

## 📅 Yol Haritası & Durum Takibi (Roadmap & Status)

| Hafta | Odak (Focus) | Çıktı Hedefi (Deliverable) | Durum (Status) |
| :---: | :--- | :--- | :---: |
| **Hafta 1** | Literatür Taraması (Literature Survey) | 2025-2026 arası ≥45 makale analizi ve özet tablosu (`hafta1_literatur/`) | 🟢 *Tamamlandı (Completed)* |
| **Hafta 2** | Boru Hattı Kurulumu (Pipeline Setup) | Google Colab üzerinde tam otonom çalışan notebooklar & zırhlı ortam kurulumları (`hafta2_pipelines/`) | 🟢 *Tamamlandı (Completed)* |
| **Hafta 3** | Özgün İyileştirme (Optimization) | Baseline metriklerinin ölçümü, SOTA kıyaslamaları ve mimari optimizasyonlar (`hafta3-4_iyilestirme/`) | 🟡 *Başlandı / Devam Ediyor (Started / In Progress)* |
| **Hafta 4** | Kıyaslama & Raporlama (Final Evaluation) | Karşılaştırmalı metrik analiz raporu ve teknik blog/makale çıktısı (`sonuclar/`) | ⚪ *Beklemede (Pending)* |

---

## 📂 Güncel Depo Mimarisi (Repository Structure)

```text
├── hafta1_literatur/          # Literatür analizleri ve SOTA makale özetleri
│   ├── 01_deblurring_literatur.md
│   ├── 02_stabilization_literatur.md
│   ├── 03_tracking_literatur.md
│   └── README.md
├── hafta2_pipelines/          # Uygulamalı Baseline Colab Notebook'ları ve Otonom Mimari
│   ├── MOT17_Test.ipynb                       # Otonom GT simülatörü ve Native HOTA metrik motoru
│   ├── Hybrid_Pipeline_car_person_gemini.ipynb  # Kinematik zırhlı, derin öğrenme (YOLOX) odaklı MOT mimarisi
│   ├── Hybrid_Pipeline_car_person_claude.ipynb  # Otonom karar mekanizmalı, sahne adaptif (MOG2/YOLO) MOT mimarisi
│   ├── 01_deblurring_baseline.ipynb           # DeepRFT tabanlı görüntü bulanıklığı giderme
│   ├── 02_stabilization_baseline.ipynb        # LightStab tabanlı çevrimiçi video stabilizasyonu
│   ├── 03_hybrid_tracking_baseline.ipynb      # Hibrit nesne takibi ardışık düzeni
│   └── README.md
├── hafta3-4_iyilestirme/      # Mimari iyileştirmeler, ablation çalışmaları ve optimizasyonlar
│   └── README.md
├── raporlar/                  # Günlük staj raporları ve teknik ilerleme belgeleri
│   ├── 2026-07-28_gunluk_rapor.md             # Split-Alpha EMA ve Evrensel Mimari Entegrasyon Raporu
│   ├── 2026-07-29_gunluk_rapor.md             # HOTA Entegrasyonu, Kırık Link Kalkanı ve PyTorch/NumPy Zırhlaması
│   ├── AI_Tools_Review.md                     # Gemini ve Claude algoritmik felsefelerinin kantitatif çapraz analizi
│   └── (Önceki günlere ait günlük raporlar...)
└── sonuclar/                  # Karşılaştırmalı Performans Metrikleri ve Final Çıktılar
    └── README.md
