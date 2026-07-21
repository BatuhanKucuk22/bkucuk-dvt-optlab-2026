# 📅 Günlük Staj İlerleme Raporu — 21 Temmuz 2026

**Stajyer:** Batuhan Küçük  
**Proje:** BK-DVT-OptLab: Computer Vision Pipeline Optimization 2026  
**Çalışılan Odak:** Hafta 2 — Online Video Stabilization Baseline Kurulumu ve Repository Yönetimi  
**Tarih:** 21.07.2026  

### 🛠️ Alan 1: Dünkü Planın Tamamlanması (Pipeline 1)
* **GitHub Finalizasyonu:** Dünkü aksiyon planına uygun olarak, Görüntü Bulanıklığı Giderme (Pipeline 1 - DeepRFT) modülünün 1111 görsellik tam GoPro test seti üzerinde kusursuz çalışan, optimize edilmiş final `.ipynb` sürümü oluşturuldu ve GitHub reposuna başarıyla eklendi.

### 🎥 Alan 2: Video Stabilization Pipeline Mimarisi
* **Durum:** Literatürde altın standart olarak değerlendirilen `LightStab` (Liu vd., 2026) mimarisi için sıfır gecikmeli (causal), çevrimiçi video stabilizasyon boru hattı Google Colab T4 GPU ortamında kurgulandı.
* **Teknik Yapılandırma:** Özellik çıkarımı (XFeat) ve optik akış (RAFT_small) ağırlıkları ile sınır kayıplarını telafi eden ProPainter (outpainting) modülleri sisteme entegre edildi. Üretilen stabilize videoların, kıyaslama (`_compare`) çıktılarının ve outpainting sonuçlarının Google Drive'a (`LightStab_Results` klasörüne) otomatik senkronize olması sağlandı.
* **Modülerlik:** Sistem, hedef dizindeki tüm videoları peş peşe ve otonom bir şekilde işleyebilecek (batch inference) kapalı devre bir döngüye (`onlinestab.py` modifikasyonu) dönüştürüldü.

### 🧪 Alan 3: Sistem Validasyonu, Metrikler ve Dokümantasyon
* **Performans Testi:** Sistem, `UAV-Test` veri setindeki 3 zorlu dış ortam videosu (`running13.mp4`, `infraed.mp4`, `blarcar.mp4`) üzerinde toplu validasyona sokuldu.
    * **Ortalama Stabilite Skoru (S):** ~0.89
    * **Ortalama Bozulma / Geometrik Tutarlılık (D):** ~0.90
    * **Ortalama Kırpma Oranı / FOV Korunumu (C):** ~0.94
    * **Model Performans İndeksi:** %98.5 (Makalede beyan edilen referans SOTA skorları ile ampirik uyuşum sağlandı).
* **Repository Yönetimi:** Notebook gereksiz keşif hücrelerinden temizlenerek GitHub'a yüklendi. Ana dizindeki `README.md` dosyası proje mimarisi ve Pipeline 2 metriklerini içerecek şekilde güncellendi. *(Not: `sonuclar/` dizini içerisine herhangi bir dosya güncellemesi veya eklemesi yapılmamıştır).*

---

### 🚀 Yarınki Aksiyon Planı (Sonraki Adımlar)

1. **Hibrit Nesne Takibi (Tracking) Pipeline'ına Geçiş:** Hafta 2'nin son bileşeni olan nesne takibi (Hybrid Object Tracking) algoritmalarının operasyonel incelemesine başlanması.
2. **Çalışma Ortamının Hazırlanması:** İzleme odaklı repositorilerin Colab ortamına klonlanması, gerekli kütüphane/bağımlılıkların yüklenmesi ve test videolarının Drive üzerinden ortama aktarılması.
3. **Ön Testler ve Çıkarım:** Seçilen model üzerinden temel performans (inference) testlerinin başlatılması, bounding box/yörünge görselleştirmelerinin yapılması ve ilk metriklerin toplanması.
