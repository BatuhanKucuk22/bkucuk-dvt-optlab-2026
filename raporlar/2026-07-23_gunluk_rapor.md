# 📅 Günlük Staj İlerleme Raporu — 23 Temmuz 2026

**Stajyer:** Batuhan Küçük  
**Proje:** BK-DVT-OptLab: Computer Vision Pipeline Optimization 2026  
**Çalışılan Odak:** Hafta 2 — Ortak Kurulum Dokümantasyonu, Kullanım Rehberi Entegrasyonu, SSIM Kaynaklı Performans Düşüşü Analizi ve Birleşik Boru Hattı Zırhlandırılması  
**Tarih:** 23.07.2026  

---

### 🛠️ Alan 1: Ortak Kurulum Dokümantasyonu, Kullanım Rehberi ve Altyapı Zırhlandırması
* **Ortak Kurulum Dokümanı ve Kullanım Rehberi Entegrasyonu:** Günün ilk aşamasında, Hafta 2 boyunca başarıyla geliştirilen 3 ayrı boru hattının (Netleştirme, Geometrik Stabilizasyon ve Nesne Takibi) standart ve hatasız koşturulabilmesi için **Ortak Kurulum Dokümantasyonu ve Teknik Kullanım Rehberi** hazırlanarak `hafta2_pipelines` dizinine eklendi. Bu rehber; PyTorch, OpenCV ve FastReID kütüphanelerinin VRAM dostu yapılandırma adımlarını, Google Colab T4 GPU ortam gereksinimlerini ve modüllerin ardışık çalıştırma sırasını standartlaştırarak proje reposuna tam entegre edildi.
* **Birleşik Boru Hattı (`UnifiedVisionPipelinePro`) ve Özerk Render:** Üç temel modül tek bir nesne yönelimli mimaride birleştirildi. Harici render betiklerindeki dosya kilitlenme hatalarını ortadan kaldırmak adına, YOLOX-X omurgasından çıkan koordinat verilerini (`.txt`) belleğe alıp doğrudan **Native OpenCV** üzerinden yörünge çizen hatasız bir akış inşa edildi.
* **Ortam Zırhlandırması:** Colab oturum kopmalarına karşı otomatik Drive bağlama, dinamik klasör arama zırhı ve dosya kilitlenmelerini engelleyen zaman damgalı (`datetime`) kayıt mekanizması koda entegre edildi:
  ```python
  class UnifiedVisionPipelinePro:
      def __init__(self):
          # 🛡️ Zırh 1: Google Drive Bağlantısını Otomatik Denetle ve Bağla
          if not os.path.exists("/content/drive/MyDrive"):
              drive.mount('/content/drive')
          
          # 🛡️ Zırh 2: Dinamik Repo Yolu Doğrulama (İsim Değişikliklerine Karşı Zırh)
          self.tracking_repo = self._resolve_tracking_repo()
          os.makedirs("/content/drive/MyDrive/.../final_outputs", exist_ok=True)



---

### 📊 Alan 2: `classroom.mp4` — Kantitatif Kıyaslama ve SSIM Kaynaklı Performans Düşüşü Analizi
Dünkü saf takipçi (Baseline Pipeline 3) testlerinde `classroom.mp4` videosu üzerinde 983 kare boyunca 11.348 tespit ve 26 benzersiz kimlik (Unique ID) elde edilmişti. Bugün video, ön işleme katmanlarını da barındıran **Birleşik Boru Hattından** geçirilmiş ve 360° metrik motoru ile analiz edilmiştir. 

Yapılan kantitatif incelemede; ön işlemenin kenar keskinliğini ciddi oranda artırmasına rağmen **dokusal yapı koruma oranında (SSIM) neden olduğu bozunmanın, YOLOX-X modelinin öznitelik çıkarım performansını negatif etkilediği** tespit edilmiştir. GitHub üzerinde kusursuz bir şekilde görüntülenmesi için standartlara uygun hazırlanan karşılaştırmalı metrik matrisi aşağıdadır:

| Metrik Kategori | Metrik Tanımı | Dünkü Baseline (Saf Pipeline 3) | Bugünkü Birleşik Pipeline Çıktısı | Performans Değişimi & Mühendislik Analizi |
| :--- | :--- | :---: | :---: | :--- |
| **Yapısal Koruma** | Dokusal Benzerlik Oranı (SSIM) | **1.0000** (Referans) | **0.8399** | **-%16.0 Dokusal Bozunma:** Agresif kontrast filtreleri görüntünün doğal piksel haritasını saptırmıştır. |
| **Görüntü Netliği** | Kenar Keskinliği (Laplacian Var.) | 7.26 (Ham Video) | **54.98** | **+%656.9 Keskinlik Artışı:** Kenarlar belirginleşmiş, ancak bu durum görüntüde yapay bir gren (gürültü) oluşturmuştur. |
| **Geometrik Stabilizasyon** | Titreme Varyansı (Jitter Var.) | 0.0036 (Ham Video) | **0.0021** | **-%41.4 Titreme Azalması:** Kamera sarsıntıları fiziksel olarak başarılı şekilde bastırılmıştır. |
| **Yörünge Takibi** | Toplam Tespit Sayısı | **11.348 Kutu** | **11.043 Kutu** | **-305 Tespit Kaybı:** SSIM düşüşüne bağlı dokusal değişim, YOLOX modelinin bazı hedefleri kaçırmasına yol açmıştır. |
| **Takip Kararlılığı** | Benzersiz Kimlik (Unique ID) | **26 Farklı ID** | **30 Farklı ID** | **+4 ID Kopması (Performans Düşüşü):** Ön işleme kaynaklı gürültü, Hybrid-SORT oklüzyon çözücüsünde geçici kimlik değişimlerine sebep olmuştur. |

#### 🔍 SSIM Düşüşünün Takipçiye Olumsuz Etkisinin Kanıtlanması (Kod Tabanlı Teşhis)
Ön işleme katmanımızın uyguladığı agresif CLAHE ve Bilateral filtreleme, görüntüdeki kenarları güçlendirirken (**Laplacian +%656.9**), orijinal görüntünün piksel istatistiklerini bozarak **SSIM değerini 0.8399 seviyesine çekmiştir**. YOLOX-X modeli COCO veri setinin doğal dokularıyla eğitildiği için, bu yapay dokusal sapma modelin güven skorunu anlık olarak düşürmüş ve saf Pipeline 3'e kıyasla **daha fazla ID kopması (26 yerine 30 ID)** yaşanmasına neden olmuştur.

```python
# Görüntü Kalitesi (SSIM Dokusal Bozunma ve Laplacian Keskinliği) Analiz Motoru
def analyze_image_quality(raw_path, proc_path):
    cap_r, cap_p = cv2.VideoCapture(raw_path), cv2.VideoCapture(proc_path)
    laplacian_proc, ssim_scores = [], []
    
    while cap_r.isOpened() and cap_p.isOpened():
        ret_r, frame_r = cap_r.read()
        ret_p, frame_p = cap_p.read()
        if not ret_r or not ret_p: break
        
        gray_r = cv2.cvtColor(frame_r, cv2.COLOR_BGR2GRAY)
        gray_p = cv2.cvtColor(frame_p, cv2.COLOR_BGR2GRAY)
        
        # Keskinlik artışı yüksek olsa bile SSIM skorundaki düşüş modelin öznitelik haritasını bozuyor
        laplacian_proc.append(cv2.Laplacian(gray_p, cv2.CV_64F).var())
        score, _ = ssim(gray_r, gray_p, full=True)
        ssim_scores.append(score) # Ortalama ~0.8399 seviyesinde kalarak dokusal sapmayı kanıtlıyor
        
    return np.mean(laplacian_proc), np.mean(ssim_scores)

---

### 🧠 Alan 3: Endüstriyel Uç Senaryo (`bolt-detection.mp4`) ve Hiper-Parametre Denemeleri

Konveyör bant üzerindeki hızlı vidalarda yaşanan yörünge kopmalarını gidermek amacıyla boru hattına iki farklı optimizasyon katmanı entegre edilerek sınır testleri gerçekleştirilmiştir:

* **Takipçi Hafıza Tamponu (Tracker Tuning):** Hızlı hareket eden nesnelerin kaybolma süresini telafi etmek amacıyla `track_buffer` hiper-parametresi 30'dan 60'a çıkarılmış, hareket bulanıklığı (motion blur) nedeniyle güven skoru düşen vidaları yakalamak için `--conf` eşigi **0.05** seviyesine indirilmiştir.
* **Temporal Çözünürlük ve Kare İnterpolasyonu (Faz 0):** Ardışık kareler arasındaki fiziksel yer değiştirmeyi (displacement) yumuşatmak adına OpenCV tabanlı doğrusal bir ara kare (`mid_frame`) üretici yazılmış ve video kare hızı 2 katına (29 $\rightarrow$ 58 FPS) çıkarılmıştır:
```python
def run_frame_interpolation_stage(self, video_name):
    # Ardışık iki kare arasındaki piksel sıçramasını yumuşatan ara katman
    while cap.isOpened():
        ret, curr_frame = cap.read()
        if not ret: break
        out.write(prev_frame)
        mid_frame = cv2.addWeighted(prev_frame, 0.5, curr_frame, 0.5, 0) # Motion blur kırıcı
        out.write(mid_frame)
        prev_frame = curr_frame

```



---

### 🚀 Yarınki Aksiyon Planı (Sonraki Adımlar)

1. **SSIM Bozunumunu Giderecek Hibrit Ön İşleme Tasarımı:** Bugün kanıtlanan SSIM düşüşünün nesne takibine verdiği zararı ortadan kaldırmak için; yarın ön işleme katmanındaki CLAHE ve Bilateral filtre eşikleri yumuşatılacak, orijinal dokuyu koruyan (yüksek SSIM hedefli) yeni bir filtreleme mimarisi test edilecektir.
2. **Birleşik Boru Hattı Tam Performans Validasyonu:** Ön işleme katmanı optimize edildikten sonra `classroom.mp4` videosu yeniden işlenecek ve saf Pipeline 3'ün elde ettiği **26 benzersiz ID** skoru geride bırakılana kadar boru hattı iyileştirmelerine devam edilecektir.
3. **Seçici Uygulama (Selective Preprocessing) Mimarisi:** Görüntü netleştirmeyi tüm videoya körü körüne uygulamak yerine, yalnızca modelin güven skorunun düştüğü bulanık karelerde devreye sokan adaptif bir akış kontrolcüsü planlanacaktır.
4. **Çoklu Veri Seti ve Alternatif Metotlarla Performans Validasyonu:** Performans artırımı sağlamak amacıyla GitHub reposunda bulunan 3 farklı boru hattı ve alternatif görüntü işleme metotları kullanılarak farklı veri setlerindeki videolar üzerinde kapsamlı optimizasyon denemeleri gerçekleştirilecektir. Yapılan testler sonucunda belirgin bir performans artışı ve pozitif metrik kazanımı sağlayan en başarılı boru hattı mimarisi seçilerek GitHub reposuna entegre edilecektir.
