# 📅 Günlük Staj İlerleme Raporu — 17 Temmuz 2026

**Stajyer:** Batuhan Küçük  
**Proje:** BK-DVT-OptLab: Computer Vision Pipeline Optimization 2026    
**Çalışılan Odak:** Hafta 1 Kapanışı — Yeni Makalelerin Eklenmesi, SOTA (State-of-the-Art) Değerlendirmelerinin Yazılması ve Hafta 2'ye Geçiş  
**Tarih:** 17.07.2026  

### 🖼️ Alan 1: Görüntü Blur Giderme (Image Deblurring)
* **Durum:** UHD Görüntü Restorasyonu, NAFNet Ablasyon Çalışması, AIM 2025 Raporu ve DeepRFTv2 gibi önemli makaleler tabloya eklenerek toplam makale sayısı 20'ye ulaştırıldı.
* **Öne Çıkan Modeller (Yeni Eklenenler):** Özellikle Fourier uzayında kernel düzeyinde öğrenme yapan ve tersinir (reversible) yapısıyla VRAM tüketimini drastik şekilde düşüren *DeepRFTv2* (9.5/10), stajın donanım verimliliği hedefleri için mükemmel bir referans olarak sisteme dahil edildi. 
* **SOTA Değerlendirmesi:** Hazırlanan literatür tablosunun altına, GLOWDeblur'un 1.6B parametreli hafif difüzyon altyapısı ile referans noktasını oluşturduğu, DeepRFTv2'nin hız/VRAM verimliliği sağladığı ve NAFRepLocal'in yarışma lideri olarak öne çıktığı profesyonel SOTA değerlendirme paragrafı eklendi.

### 🎥 Alan 2: Gerçek Zamanlı Video Stabilizasyonu (Real-Time Video Stabilization)
* **Durum:** Daha önce tamamlanan literatür tablosu üzerinden, staj planındaki "Şu an en iyi yöntemler neler?" (SOTA) sorusuna yanıt veren sentez paragrafı oluşturuldu.
* **SOTA Değerlendirmesi:** *LightStab* modelinin (10/10) sıfır gecikmeli ve gözetimsiz mimarisiyle altın standart olduğu, SuperPoint tabanlı algoritmanın (~31.25 FPS) canlı sistemler için en uygulanabilir alternatif oluşturduğu ve sarsıntılı alanlara odaklanan meta-öğrenme modelinin (8.8/10) hız-doğruluk dengesindeki başarısı teknik bir özetle raporlandı.

### 🎯 Alan 3: STAPLE & YOLO Hibrit Hedef İzleme (Hybrid Target Tracking)
* **Durum:** Literatürdeki son güncel gelişmeler olan HybridTrack, Hybrid-SORT, MOT Review, Generative Semantic MOT ve DiffMOT makaleleri profesyonel bir şekilde tabloya entegre edildi. Bu alan da 20 makaleye ulaştı.
* **Öne Çıkan Modeller (Yeni Eklenenler):** Kalman Filtresini derin öğrenme ile öğrenilebilir hale getiren ve 112 FPS hızda çalışan *HybridTrack* (9.6/10) ile doğrusal olmayan (non-linear) hareketleri difüzyon modeliyle çözen ve 22.7 FPS hız sunan *DiffMOT* (9.4/10) yüksek öncelikli olarak işaretlendi.
* **SOTA Değerlendirmesi:** TBDQ-Net, HybridTrack ve DiffMOT'un hız-doğruluk ve karmaşık hareket modelleme yetenekleri üzerinden alanın en iyi yöntemlerini özetleyen sentez paragrafı tabloya eklendi.

### ⚙️ Proje Yönetimi ve Repo Güncellemeleri
* **Hafta 1 Hedeflerinin Aşılması:** Başlangıçta hedeflenen ≥45 makale sınırı aşılarak, her alan için 20 olmak üzere toplamda **60 makale** incelendi ve skorlandı.
* **GitHub Dokümantasyonu:** `hafta1_literatur` klasöründeki alt README dosyası ve projenin ana README dosyası, Conventional Commits (örn: `docs(readme): update roadmap to mark week 1 complete and week 2 in progress`) standartlarına uygun biçimde güncellendi.
* **Yol Haritası Durumu:** Hafta 1 (Literatür Taraması) durumu "Tamamlandı" (🟢) olarak işaretlenirken, Hafta 2 (Boru Hattı Kurulumu) hedefleri "Başlandı / Devam Ediyor" (🟡) statüsüne geçirildi.

---

## 🚀 Hafta 2'ye Dair Aksiyon Planı (Sonraki Adımlar)

1. **Hafta 2 (Pipeline Kurulumu) Başlangıcı:** Görüntü Blur Giderme, Video Stabilizasyonu ve Hibrit Hedef İzleme alanlarında belirlenen referans modellerden (örn. DeepRFTv2, LightStab, HybridTrack vb.) uygun olanların açık kaynaklı repolarının incelenmesi.
2. **Google Colab Ortamının Hazırlanması:** İlgili veri setlerine (GoPro, NUS, MOT17 vb.) erişim sağlanması ve seçilen 1-2 model için uçtan uca çalışacak (`veri → model → çıktı → metrik`) ilk Colab notebook taslaklarının oluşturulması.
3. **Baseline Metriklerin Loglanması:** Kurulacak pipeline'ların mevcut (baseline) PSNR/SSIM, stabilite skorları veya MOTA/IDF1 metriklerinin ölçülmesi ve Hafta 3'teki özgün iyileştirme adımı için kayıt altına alınması.
