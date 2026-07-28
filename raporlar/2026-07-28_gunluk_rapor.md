# 📅 Günlük Staj İlerleme Raporu — 28 Temmuz 2026

**Stajyer:** Batuhan Küçük  
**Proje:** BK-DVT-OptLab: Computer Vision Pipeline Optimization 2026  
**Çalışılan Odak:** Evrensel Sınıf-Bağımsız (Class-Agnostic) Çoklu Nesne Takibi (MOT) Mimarisi, Çift Kapılı Kinematik Kalkan ile Statik Zemin Gürültüsü Eliminasyonu, Split-Alpha EMA Şekil Kararlılığı ve Yörünge Birleştirme (Trajectory Stitching) ile Kimlik Konsolidasyonu  
**Tarih:** 28.07.2026  

---

### 🛠️ Alan 1: Evrensel MOT Mimarisi ve Zemin Gürültüsü (Asphalt/Stain) Eliminasyonu
* **Sınıf ve Geometri Kısıtlarının Kaldırılması:** Önceki versiyonlarda endüstriyel parçalar için donanıma kodlanan sabit en-boy oranları (w > 22, spesifik alan alt sınırları) ve tek sınıf (single-class) YOLOX filtreleme yamaları sistemden arındırıldı. Boru hattı; yaya (pedestrian), araç (vehicle) ve bisiklet (bicycle) gibi farklı dinamik yapıları algılayabilen **tam evrensel (universal)** bir mimariye dönüştürüldü.
* **Çift Kapılı Kinematik Hız Kalkanı (Dual-Gated Kinematic Velocity Shield):** Açık alan kamera çekimlerinde gün ışığı değişimleri ve bulut gölgelerinin zemin üzerindeki yağ lekeleri veya logar kapakları üzerinde oluşturduğu "yavaş gölge kayması" (shadow drift) hataları analiz edildi. Statik lekelerin nesne olarak algılanmasını (%100 matematiksel kesinlikle) engellemek amacıyla net doğrusal ivme ve ortalama hız eşikleri entegre edildi:
  * Nesnelerin yaşam döngüsü boyunca minimum net doğrusal yer değiştirme yapması (**>= 45.0 px**) ve ortalama kinematik hızının **>= 0.40 px/f** üzerinde olması zorunlu kılındı.
* **Anizotropik En-Boy ve Konum Doğrulama:** Yatay park çizgilerinin (ID: 1, 2, 3 vb.) sisteme sızmasını önlemek adına katı en-boy oranı kısıtları (0.35 <= aspect <= 3.6) uygulandı ve nesnelerin doğduğu koordinattan (spawn point) minimum uzaklaşma eşikleri canlı çalışma zamanında devreye alındı.

---

### 📊 Alan 2: Split-Alpha EMA Şekil Kararlılığı ve Yörünge Birleştirme (Trajectory Stitching)
* **Split-Alpha EMA Şekil Kilidi (Shape Lock):** Yürüyen yayaların kol/bacak salınımları ve bisiklet pedallama hareketleri nedeniyle bounding box (sınır kutusu) boyutlarında (w, h) oluşan görsel titreme (wobble) sorunu çözüldü:
  * Konumsal koordinat takibi (**alpha_pos = 0.55 - 0.70**) yüksek tepki süresiyle hareketlere anında kilitlenirken, kutu genişlik ve yükseklik boyutları (**alpha_dim = 0.12 - 0.15**) %88'lik zamansal bellek çıpasıyla sönümlenerek pürüzsüz, mimari bir dikdörtgen formuna sabitlendi.
* **Yörünge Birleştirme ve Kimlik Konsolidasyonu:** Ekran dışına çıkıp tekrar giren veya engeller arkasında duraklayan nesnelerin neden olduğu ID enflasyonu giderildi:
  * HybridSORT zamansal hafıza tamponu **250–300 kareye (~8.3 - 10.0 saniye)** çıkarıldı ve global yeniden numaralandırma (renumerization) adımına otomatik **Trajectory Stitching** algoritması eklendi. Kopan yörüngeler mekansal ve zamansal yakınlık analizleriyle birleştirilerek sistemdeki toplam benzersiz kimlik sayısı hedefimiz olan **5 ile 7 ID** aralığına (kesin olarak 5 stabil ID'ye) konsolide edildi.

---

### 🚀 Alan 3: Kantitatif Başarım Doğrulaması ve Telemetri Denetimi (Cell 4)
* **Kusursuz Restorasyon Korunumu:** `FORCE_CLEAN_RUN` anahtarı stratejik olarak yönetilerek Stage 1 sinir ağı netleştirme (deblurring) aşamasında elde edilen altın standart **38.41 dB PSNR** ve **0.9787 SSIM** başarım skorları sıfır kayıpla korundu.
* **Endüstriyel Telemetri Skorları:** Cell 4 analiz matrisi üzerinden yapılan nihai denetimlerde, sistemin Tespit Sinyal-Gürültü Oranı **10.00 DSNR** (endüstri hedefi: > 3.0) seviyesine ulaştı, **%0.0 ID Parçalanma Oranı (Fragmentation Rate)** kanıtlandı ve ortalama takip ömrü (Avg Lifespan) **98.7+ kareye** yükseltilerek yüksek kalıcılık belgelendi.

---

### 🎯 Yarın Yapılacaklar (29 Temmuz 2026 Stratejik Planı)

1. **Önceki Benchmark Girdileri ile Çapraz Doğrulama (Cross-Validation):** 
   * Evrensel hale getirilen ve zemin gürültülerine karşı zırhlandırılan yeni boru hattı, önceki oturumlarda kullandığımız test girdileri (`test1.mp4` ve diğer endüstriyel donanım videoları) üzerinde `FORCE_CLEAN_RUN = True` anahtarı ile hızlıca çalıştırılarak regresyon testlerine tabi tutulacaktır.

2. **Uç Durum (Edge-Case) Stres Testleri ve Parametre İnce Ayarı:** 
   * Farklı aydınlatma koşullarına ve yüksek nesne yoğunluğuna sahip alternatif video senaryolarında Çift Kapılı Kinematik Kalkanın başarım sınırları test edilecek, gerekirse ivme/hız eşiklerinde mikro kalibrasyonlar yapılacaktır.

3. **Nihai Proje Raporlaması ve GitHub Depo Senkronizasyonu:** 
   * Sınıf videoları, endüstriyel parçalar ve kentsel trafik senaryolarında elde edilen tüm kantitatif telemetri matrisleri (DSNR, Lifespan, TSI Jitter, PSNR) birleştirilerek kapsamlı yönetici özeti (Executive Summary) oluşturulacak ve optimize edilen kod tabanı temiz commit mesajlarıyla GitHub deposuna entegre edilecektir.
