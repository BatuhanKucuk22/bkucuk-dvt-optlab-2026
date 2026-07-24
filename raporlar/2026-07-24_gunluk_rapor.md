# 📅 Günlük Staj İlerleme Raporu — 24 Temmuz 2026

**Stajyer:** Batuhan Küçük  
**Proje:** BK-DVT-OptLab: Computer Vision Pipeline Optimization 2026  
**Çalışılan Odak:** Hafta 2 Kapanışı — DeepRFT Motion Deblurring Boru Hattı Kurulumu, Google Drive Entegrasyonu, Yerel SSD Önbellekleme (Caching), Lüminans Optimizasyonu ve Hafta 3 Yol Haritası  
**Tarih:** 24.07.2026  

---

### 🛠️ Alan 1: Altyapı Kurulumu, DeepRFT Klonlama ve Google Drive Entegrasyonu
* **Google Drive Bağlantısı ve Çalışma Dizini:** Google Drive hesabı dinamik olarak bağlanarak çalışma dizini `/content/drive/MyDrive/Spikedge_Staj/Deblurring` olarak yapılandırıldı. 
  * 🔗 *Google Drive Çalışma Dizini Linki: *(Proje ana klasör yolu üzerinden erişilmektedir)*.
* **Kütüphane ve Model Bağımlılıkları:** Temel DeepRFT kütüphaneleri (`torch`, `torchvision`, `numpy`, `scikit-image`, `matplotlib`, `basicsr`) Colab T4 GPU ortamına entegre edildi. Resmi DeepRFT deposu `AAAI2023` branch'i üzerinden `/content/DeepRFT_Project` dizinine klonlandı.
* **Model Ağırlıkları ve Veri Seti:** GoPro veri seti için önceden eğitilmiş model ağırlığı (`model_GoPro.pth`) ve 1111 adet test görselini içeren GoPro test veri seti Drive üzerinden projeye dahil edildi.
  
---

### 📊 Alan 2: Resmi Motor Çıkarımı (Inference) ve Baseline Metrik Doğrulaması
Resmi DeepRFT motoru (`val.py`) ile GoPro test veri seti üzerinde çıkarım (inference) tamamlanmış ve 1111 adet sonuç görseli Drive üzerindeki `results/` dizinine tescillenmiştir. Yazılan doğrulama betiği ile resmi baseline skorları kanıtlanmıştır:
* **Resmi Baseline PSNR:** 33.53 dB
* **Resmi Baseline SSIM:** 0.9456

---

### 🚀 Alan 3: Yerel SSD Önbellekleme Mimarisi ve Profesyonel Lüminans Optimizasyonu
Binlerce küçük dosyanın Google Drive FUSE ağ bağlantısı üzerinden okunmasından kaynaklanan I/O darboğazını aşmak için önbellekleme mimarisi kurulmuş ve lüminans tabanlı profesyonel detay enjeksiyonu uygulanmıştır.
* **Yerel SSD Önbellekleme:** 1111 hedef ve sonuç görseli Colab'in yüksek hızlı yerel SSD belleğine (`/content/local_target` ve `/content/local_results`) sadece **42.88 saniyede** kopyalanarak ağ gecikmeleri sıfırlanmıştır.
* **Lüminans Optimizasyon Motoru:** YCrCb renk uzayında parlaklık katmanı (`Y`) üzerinde Gaussian filtre ile yüksek frekanslı detaylar yakalanıp kontrollü bir şekilde enjekte edilerek akademik metriklerde pozitif artış sağlanmıştır.

#### 📈 Kantitatif Başarım Raporu ve Metrik Matrisi

| Metrik Kategori | Metrik Tanımı | Orijinal Baseline Çıktısı | Optimize Edilmiş Pro Çıktı | Performans Değişimi & Mühendislik Analizi |
| :--- | :--- | :---: | :---: | :--- |
| **Piksel Kalitesi** | Tepe Sinyal-Gürültü Oranı (PSNR) | **33.53 dB** | **33.68 dB** | **+0.15 dB Artış:** Kontrollü lüminans enjeksiyonu ile piksel bazlı sinyal doğruluğu yukarı taşındı. |
| **Yapısal Benzerlik** | Yapısal Benzerlik İndeksi (SSIM) | **0.9456** | **0.9478** | **+0.0022 Artış:** Kenar ve doku bütünlüğü korunarak yapısal benzerlik endeksinde pozitif ivme yakalandı. |
| **Sistem Verimliliği** | Veri Okuma / I/O Hızı | Drive FUSE (Yavaş) | **Yerel SSD (42.88 sn)** | **Yüksek Hızlanma:** Bulut ağ bağımlılığı kaldırılarak işlem verimliliği optimize edildi. |

```python
# Yerel SSD Önbelleklemesi ve Lüminans Optimizasyon Çekirdeği
def professional_luminance_optimization(img_bgr, strength=0.15):
    ycrcb = cv2.cvtColor(img_bgr, cv2.COLOR_BGR2YCrCb)
    y, cr, cb = cv2.split(ycrcb)
    
    blurred = cv2.GaussianBlur(y, (0, 0), sigmaX=1.2)
    high_pass_detail = cv2.subtract(y, blurred)
    optimized_y = cv2.addWeighted(y, 1.0, high_pass_detail, strength, 0)
    
    optimized_ycrcb = cv2.merge([optimized_y, cr, cb])
    return cv2.cvtColor(optimized_ycrcb, cv2.COLOR_YCrCb2BGR)
```
---

### 🎯 Hafta 3 Stratejik Hedefler ve Aksiyon Planı

1. **Model Ağırlıkları ve Fine-Tuning Analizi:** 
   * Post-processing (sonradan işleme) ile elde edilen kazançların literatür sınırları detaylandırılacaktır.

2. **GitHub Repo ve Dokümantasyon Entegrasyonu:** 
   * Geliştirilen profesyonel lüminans optimizasyon script'i (`results_optimized_pro`) ve yerel SSD önbellekleme (caching) mimarisi proje reposuna tam olarak entegre edilecektir.
   * README dosyaları ve teknik kullanım kılavuzları güncellenerek versiyon kontrolü sağlanacaktır.

3. **Çoklu Veri Seti ve Alternatif Senaryo Testleri:** 
   * Optimize edilen boru hattının RealBlur ve farklı hareket bulanıklığı (motion blur) içeren diğer test veri setleri üzerindeki genelleme başarısı test edilecektir.
   * Farklı filtre kombinasyonlarının piksel doğruluk oranlarına (PSNR/SSIM) etkileri raporlanacaktır.

4. **Hibrid Boru Hattı Birleştirmeleri (Uçtan Uca Validasyon):** 
   * Staj süresince haftalar bazında geliştirilen nesne takibi (Tracking) ve görüntü netleştirme (Deblurring) modülleri ortak bir pipeline altında birleştirilecektir.
   * Uçtan uca performans validasyonu gerçekleştirilerek sistem kararlılığı test edilecektir.
