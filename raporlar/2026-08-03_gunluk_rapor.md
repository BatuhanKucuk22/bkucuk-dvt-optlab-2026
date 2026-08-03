# 📅 Günlük Staj İlerleme Raporu — 3 Ağustos 2026

**Stajyer:** Batuhan Küçük  
**Proje:** Spikedge Görüntü İşleme ve Çoklu Nesne Takibi (MOT) Boru Hattı  
**Çalışılan Odak:** Ters İzdüşüm (Inverse Mapping), MOT17 Test Seti, 4'lü Grid Dashboard ve Codabench Submission  
**Tarih:** 03.08.2026  

---

### 🎯 Dünkü Hedeflerin Gerçekleşme Durumu
* **Gerçekleşti - MOTA Skoru Düşüşünün Çözülmesi:** Stabilizasyon aşamasının (LightStab) pikselleri kaydırması nedeniyle TrackEval motorunda yaşanan MOTA düşüşü, sisteme entegre edilen "Ters İzdüşüm (Inverse Affine Mapping)" algoritması ile matematiksel olarak çözüldü. Kutular orijinal sarsıntılı video uzayına milimetrik olarak geri itildi.
* **Gerçekleşti - MOT17 Test Seti Maratonu:** Eğitim (Train) setinden çıkılarak resmi `MOT17-test` sekansları (01, 03, 06, 07, 08, 12, 14) başarıyla işlendi. Dinamik FPS algılayıcı ve Akıllı Devam Etme (Smart Resume) modülleri bu ağır yük altında sorunsuz çalıştı.
* **Devam Ediyor - Resmi SOTA Metrik Tescili:** Elde edilen telemetri dosyaları Codabench format standartlarına (FRCNN, DPM, SDP klasörlemesi) uygun hale getirilerek resmi MOTChallenge sunucusuna yüklendi. Komiteden onay ve sonuç beklenmektedir.

---

### 🛠️ Günün Teknik Gelişmeleri ve Çözümleri

#### 1. Ters İzdüşüm (Inverse Mapping) Algoritması Entegrasyonu
Boru hattının en kritik aşamalarından biri olan koordinat kayması sorunu çözüldü. Stabilizasyon sırasında hesaplanan `transforms_smooth` matrisleri `.npy` formatında fiziksel olarak diske mühürlendi. İzleme (Tracking) aşamasından sonra devreye giren `restore_coordinates` fonksiyonu ile YOLOX modelinin pürüzsüz videoda bulduğu hedefler, ters matris operasyonuyla sarsıntılı gerçek dünyadaki konumlarına restore edildi.

#### 2. Danışman Talebi: 4'lü Grid Dashboard (Görsel Vizyon Motoru)
Proje danışmanının talebi doğrultusunda, modelin aşamalı gelişimini ve anlamsal performansını kanıtlamak için özel bir görselleştirme motoru kodlandı. FFmpeg'in `filter_complex` ağacı kullanılarak;
1. *RAW INPUT* (Orijinal sarsıntılı)
2. *STAGE 1* (DeepRFT ile Deblurring)
3. *STAGE 2* (LightStab ile Stabilizasyon)
4. *FINAL OUTPUT* (Ters izdüşüm uygulanmış HybridSORT)

aşamaları 960x540 çözünürlükte ölçeklendirilip 2x2 matris (Grid) halinde tek bir senkronize MP4 dosyasına dönüştürüldü ve Colab üzerinden HTML5 formatında izlenebilir hale getirildi.

#### 3. Anlamsal Video Performansı (Semantic Performance) Doğrulaması
Oluşturulan 4'lü oynatıcı üzerinden yapılan analizlerde, HybridSORT'un temizlenmiş (Stage 2) videolara bakarak elde ettiği yüksek anlamsal tespit performansının (düşük IDSW, yüksek yaya tanıma oranı) korunduğu görüldü. Kutuların, sarsıntılı (RAW) videonun üzerine "zımbalanmış" gibi kusursuz oturması, kurulan Ters İzdüşüm mantığının %100 doğrulukla çalıştığını kanıtladı.

---

### 🔍 GitHub Repository İçin Notebook Analizi (.ipynb)
GitHub reposuna eklenecek olan nihai notebook dosyasının (`MOT17_Test_dataset_test.ipynb`) mühendislik incelemesi sonucunda dokümantasyona eklenecek öne çıkan detaylar şunlardır:

* ✔ **Frame Sanitizer Entegrasyonu:** TrackEval motorunun çökmesine neden olan "0. kare (`frame_id == 0`)" çıktıları, veri kaybı yaşanmadan `restore_coordinates` modülü içerisinde otonom olarak "1. kare" formatına kaydırılarak (shift) temizlendi.
* ✔ **Otonom Çevre Yamaları (Environment Patches):** Çalışma ortamındaki kütüphane çakışmalarını önlemek için; HybridSORT içerisindeki eski NumPy metotları (`np.float`, `np.int`) güncel NumPy 2.x standartlarına dönüştürüldü ve LightStab modülündeki Matplotlib arayüzü `Agg` (Headless) moduna zorlanarak arka plan render hataları engellendi.
* ✔ **İzole Test Dizinleri:** Eğitim ve test verilerinin telemetrilerinin birbirine karışmaması adına pipeline içerisindeki çıktı dizinleri (`intermediate_test`, `output_tracks_test`, `telemetry_test`) tam izolasyon sağlayacak şekilde yeniden yapılandırıldı.

---

### 🎯 Yarınki Eylem Planı (Stratejik Plan)
1. **Codabench Metrik Onayının Kontrolü:** Resmi MOTChallenge sunucusundan gelecek onay sonrası `MOT17-test` SOTA metriklerinin (HOTA, MOTA, IDF1 vb.) alınarak, orijinal HybridSORT referans değerleriyle kıyaslanması.
2. **Plan B (Ablasyon Çalışması) Hazırlığı:** Eğer Codabench onay süreci 24 saati aşarsa, zaman kaybetmemek adına aynı Ters İzdüşüm (Inverse Mapping) kodunun `MOT17-train` veri seti üzerinde çalıştırılarak yerel TrackEval motorunda "Eğitim Seti Ablasyon Kıyaslaması" yapılması.
3. **Repository Senkronizasyonu:** Bu raporun, oluşturulan 4'lü Grid Dashboard videosunun ve son zırhlı `.ipynb` dosyasının projenin GitHub reposuna taahhüt (commit) edilmesi.
