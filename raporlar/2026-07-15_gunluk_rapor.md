# 📅 Günlük Staj İlerleme Raporu — 15 Temmuz 2026

**Stajyer:** Batuhan Küçük  
**Proje:** BK-DVT-OptLab: Computer Vision Pipeline Optimization 2026  
**Çalışılan Odak:** Hafta 1 — Literatür Taramasının Tamamlanması, Depo Standardizasyonu ve Metot Seçim Hazırlığı  
**Tarih:** 15.07.2026  

### 🖼️ Alan 1: Görüntü Blur Giderme (Motion & Defocus Deblurring)
* **Durum:** 7 makale eklendi (Hedefe kalan: 8 makale).
* **Öne Çıkan Bulgular:** Dar alıcı alan sorununu çözen meta-tuning destekli **SFSNet**, IMU/Jiroskop sensör verisini sinyal işleme ile birleştiren donanım destekli **GAMD**, ve gerçek dünya nesne hareketlerine odaklanan **OMDNet** mimarileri incelenmiştir.
* **Kod Erişilebilirliği:** Eklenen 7 makaleden 3'ünün resmi GitHub reposu (M2AENet, OMDNet, Reblurring-guided JDRL) mevcuttur; bu durum Hafta 2 (Pipeline Kurulumu) için güçlü bir temel oluşturmaktadır.

### 🎥 Alan 2: Gerçek Zamanlı Video Stabilizasyonu (Online & Causal Methods)
* **Durum:** 11 makale eklendi (Hedefe kalan: 4 makale).
* **Öne Çıkan Bulgular:** Özellikle canlı yayın ve düşük gecikme (low-latency) gerektiren sistemler için meta-öğrenme tabanlı **MetaVideoStab** (2024 ve 2025 versiyonları), sıfır gecikmeli gözetimsiz **LightStab**, ve 3D Gaussian Splatting (3DGS) tabanlı **GaVS / StabiGS** yaklaşımları literatüre eklenmiştir.
* **Kod Erişilebilirliği:** 11 makalenin 6'sında aktif açık kaynak kod deposu veya proje sayfası saptanmıştır.

### 🎯 Alan 3: STAPLE & YOLO Hibrit Hedef İzleme (Tracking-by-Detection)
* **Durum:** 15 makale eklendi (**Haftalık hedefe ulaşıldı!**).
* **Öne Çıkan Bulgular:** YOLO serisi ile SORT/DeepSORT/ByteTrack ailesinin entegrasyonları sistematik olarak incelenmiştir. Özellikle NMS gecikmesini ortadan kaldıran uçtan uca **RT-DETR** modeli, YOLO ailesine güçlü bir rakip olarak öne çıkmış; İHA'lar için gradient kılavuzlu **YOLO-Ro-KCF** ve çok işlevli **Rex-Omni** modelleri kaydedilmiştir.
* **Kod Erişilebilirliği:** İncelemesi tamamlanan 15 makalenin 4'ünde (DEIMv2, RF-DETR, TBDQ-Net, RT-DETR) yüksek yıldızlı ve üretime hazır GitHub repoları tespit edilmiştir.

---

## 🚀 4. Yarına Dair Aksiyon Planı (Sonraki Adımlar)

1. **Literatür Sayısını Tamamlama:**
   * **Alan 1 (Deblurring):** 8 yeni makale (özellikle Restormer, NAFNet, DeblurGAN-v2 türevleri) eklenerek 15'e tamamlanacak.
   * **Alan 2 (Stabilizasyon):** 4 yeni makale (MeshFlow, DIFRINT veya StabNet varyasyonları) eklenerek 15'e tamamlanacak.
2. **State-of-the-Art (SOTA) Değerlendirme Yazıları:** Her üç dosyanın en altında bulunan *"🧠 Alan Değerlendirmesi"* kısımlarına, mevcut yöntemlerin avantaj ve darboğazlarını özetleyen 1'er paragraflık alan sentezleri yazılacak.
