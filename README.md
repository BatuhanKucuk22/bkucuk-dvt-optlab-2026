# 🚀 BK-DVT-OptLab: Computer Vision Pipeline Optimization 2026

🇹🇷 **Özet:** Bu depo, Batuhan Küçük'ün 4 haftalık staj programı kapsamında kurguladığı derin öğrenme tabanlı bilgisayarlı görü boru hatlarını (pipeline) barındırır. Projenin temel amacı; **Görüntü Blur Giderme (Deblurring)**, **Gerçek Zamanlı Video Stabilizasyonu** ve **STAPLE & YOLO ile Hibrit Hedef İzleme** alanlarında uçtan uca altyapılar kurmak, bu yöntemleri literatür standartlarında (PSNR, SSIM, MOTA, FPS) ölçümlemek ve seçilen bir odakta sayısal kanıta dayalı mimari optimizasyon sağlamaktır.

🇬🇧 **Overview:** This repository hosts the deep learning-based Computer Vision pipelines developed by Batuhan Küçük during his 4-week internship program. The core objective is to establish end-to-end architectures in **Image Deblurring**, **Real-Time Video Stabilization**, and **STAPLE & YOLO Hybrid Target Tracking**, benchmark these methods using standard literature metrics (PSNR, SSIM, MOTA, FPS), and achieve a quantitative, metric-driven architectural optimization in a selected domain.

---

## 🎯 Proje Odak Alanları (Core Research Domains)

1. **Image Deblurring (Spatial Enhancement):** Motion and defocus blur restoration using modern architectures (e.g., DeblurGAN-v2, Restormer, NAFNet).
2. **Video Stabilization (Temporal Smoothness):** Online and causal video stabilization techniques targeting real-time performance and low latency.
3. **Hybrid Target Tracking:** Integrating STAPLE (complementary tracking) with YOLO tracking-by-detection paradigms for robust multi-object tracking.

---

## 📅 Yol Haritası & Durum Takibi (Roadmap & Status)

| Hafta | Odak (Focus) | Çıktı Hedefi (Deliverable) | Durum (Status) |
| :---: | :--- | :--- | :---: |
| **Hafta 1** | Literatür Taraması (Literature Survey) | 2025-2026 arası ≥45 makale analizi ve özet tablosu (`literature/`) | 🟢 *Tamamlandı (Completed)* |
| **Hafta 2** | Boru Hattı Kurulumu (Pipeline Setup) | Google Colab üzerinde 3 çalışan notebook & baseline metrikleri (`benchmarks/`) | 🟡 *Başlandı / Devam Ediyor (Started / In Progress)* |
| **Hafta 3** | Özgün İyileştirme (Optimization) | Seçilen alanda mimari/algoritmik katkı ve ablation testleri (`experiments/`) | ⚪ *Beklemede (Pending)* |
| **Hafta 4** | Kıyaslama & Raporlama (Final Evaluation) | Karşılaştırmalı metrik analiz raporu ve teknik blog/makale çıktısı (`results/`) | ⚪ *Beklemede (Pending)* |

---

## 📂 Güncel Depo Mimarisi (Repository Structure)

```text
├── hafta1_literatur/          # Literatür analizleri ve SOTA makale özetleri
│   ├── 01_deblurring_literatur.md
│   ├── 02_stabilization_literatur.md
│   ├── 03_tracking_literatur.md
│   └── README.md
├── hafta2_pipelines/          # Uygulamalı Baseline Colab Notebook'ları
│   ├── 01_deblurring_baseline.ipynb       # DeepRFT tabanlı görüntü bulanıklığı giderme
│   ├── 02_stabilization_baseline.ipynb    # LightStab tabanlı çevrimiçi video stabilizasyonu
│   ├── 03_hybrid_tracking_baseline.ipynb  # Hibrit nesne takibi ardışık düzeni
│   └── README.md
├── hafta3-4_iyilestirme/      # Mimari iyileştirmeler, ablation çalışmaları ve optimizasyonlar
└── raporlar/                  # Günlük staj raporları ve teknik ilerleme belgeleri
    ├── 2026-07-15_gunluk_rapor.md
    ├── 2026-07-16_gunluk_rapor.md
    ├── 2026-07-17_gunluk_rapor.md
    └── 2026-07-20_gunluk_rapor.md
