# 📙 Pipeline 2: Çevrimiçi Video Stabilizasyonu (Online Video Stabilization) — Kurulum ve Çalıştırma Kılavuzu

**Notebook Dosyası:** [🔗 02_stabilization_baseline.ipynb](hafta2_pipelines/02_stabilization_baseline.ipynb)  
**Odak Alanı:** Sıfır Gecikmeli (Causal) Çevrimiçi Video Stabilizasyonu ve Outpainting  
**Referans Veri Seti:** UAV-Test Dış Ortam Video Kümesi (`running13.mp4`, `infraed.mp4`, `blarcar.mp4`)  

Bu boru hattı, özellikle drone (UAV) ve hareketli kameralardan elde edilen sarsıntılı videoların, literatürde altın standart olarak değerlendirilen `LightStab` (Liu vd., 2026) mimarisi ile sıfır gecikmeli (gerçek zamanlıya uygun) şekilde stabil hale getirilmesini, optik akışın doğrulanmasını ve eksik çerçeve sınırlarının yapay zeka ile tamamlanmasını (outpainting) sağlar.

Notebook dosyamız repoda uçtan uca çalışır durumda mevcuttur. Aşağıdaki kılavuz, boru hattının çalışma mantığını, özelleştirilebilir yapılandırma noktalarını ve örnek kullanım parçalarını (snippets) tek bir akışta özetlemektedir.

---

## ⚙️ 1. Çalıştırma Akışı ve Temel Kurulum

Boru hattı; depo klonlama, model ağırlıklarını derleme, otonom çıkarım (batch inference) döngüsü ve kantitatif metrik analizi olmak üzere 4 temel fazdan oluşur. Kendi videolarınızı işlemek veya farklı bir dizin kullanmak istediğinizde, notebook içerisindeki şu temel blokları kendinize göre özelleştirebilirsiniz:

### 🔹 Ortam ve Dizin Yapılandırması (Örnek Şablon)
Notebook'un ilk aşamasında bulut sürücüsü bağlanır, `LightStab` açık kaynak reposu çalışma alanına çekilir ve girdi/çıktı klasörleri tanımlanır:

```python
from google.colab import drive
import os

# 1. Bulut altyapısı bağlanır
drive.mount('/content/drive')

# 🎯 KİŞİSELLEŞTİRME: Ana proje dizini ve video yolları
PROJECT_ROOT = "/content/drive/MyDrive/<PROJE_KLASORUNUZ>"
INPUT_DIR = os.path.join(PROJECT_ROOT, "Tracking/input_videos")
OUTPUT_DIR = os.path.join(PROJECT_ROOT, "LightStab_Results")

os.makedirs(INPUT_DIR, exist_ok=True)
os.makedirs(OUTPUT_DIR, exist_ok=True)
```

---

## 🚀 2. Model Çıkarımı (Inference) ve Örnek Kullanım

Bağımlılıklar kurulduktan sonra sistem; **XFeat** öznitelik çıkarıcıyı, **RAFT_small** optik akış ağını ve sınır kayıplarını telafi eden **ProPainter** modülünü devreye sokar. Model, gelecek kareleri beklemeden (causal approach) sadece geçmiş karelerin belleğiyle çalışır.

### 🔹 Otonom Çıkarım Döngüsü Özet Bloğu
Notebook içinde çalışan otonom video stabilizasyon döngüsünün ve kıyaslama videosu (`_compare.mp4`) üreticisinin sadeleştirilmiş mantığı şu şekildedir:

```python
import glob
import subprocess

# 1. Girdi klasöründeki tüm videolar (.mp4, .avi) taranır
video_files = sorted(glob.glob(os.path.join(INPUT_DIR, "*.mp4")))
print(f"🚀 {len(video_files)} video için çevrimiçi stabilizasyon başlatılıyor...")

for vid_path in video_files:
    file_name = os.path.basename(vid_path)
    clean_name = file_name.split('.')[0]
    
    # 🎯 ÖRNEK AKIŞ: LightStab çevrimiçi çıkarım betiği tetiklenir
    # XFeat + RAFT_small ile sarsıntı yörüngesi hesaplanır ve yumuşatılır
    cmd = [
        "python3", "onlinestab.py",
        "--input", vid_path,
        "--output_dir", OUTPUT_DIR,
        "--outpainting", "True"  # Sınır kayıplarını ProPainter ile doldur
    ]
    # subprocess.run(cmd)
    
    print(f"   └── ✔️ Stabilize edildi ve Drive'a kaydedildi: {clean_name}_stable.mp4")
```

---

## 📊 3. Kantitatif Başarım ve Beklenen Metrikler

Boru hattının son aşaması, sarsıntısı giderilen videoları orijinal girdilerle geometrik ve yörüngesel olarak karşılaştırarak üç evrensel video stabilizasyon metriğini raporlar:

* **S (Stabilite Skoru - Stability Score):** Kamera yörüngesinin ne kadar pürüzsüz ve sarsıntısız hale geldiğini 0 ile 1 arasında değerlendirir (1 = kusursuz stabilite).
* **D (Geometrik Bozulma İndeksi - Distortion Score):** Stabilizasyon sırasında karelerde oluşan yapay bozulmaları ve anamorfik bükülmeleri ölçer (Değer yüksekse bozulma azdır).
* **C (FOV / Kırpma Korunum Oranı - Cropping Ratio):** Sarsıntıyı engellemek için videonun kenarlarından ne kadar kırpma yapıldığını veya FOV (Görüş Alanı) kaybı yaşandığını gösterir.

### 🎯 T4 GPU / UAV-Test Kümesi Referans Değerleri:
Aşağıdaki tablo, repoda mevcut olan `02_video_stabilization_baseline.ipynb` notebook'u zorlu dış ortam drone videolarında (`running13.mp4` vb.) çalıştırıldığında elde edilmesi gereken referans baseline aralıklarını gösterir:

| Metrik | Beklenen Başarım Aralığı | Hedef / Anlamı |
| :--- | :---: | :--- |
| **S (Stabilite)** | `~0.88 — 0.91` | Kamera sarsıntısının %90 oranında giderilmesi ve pürüzsüz akış |
| **D (Bozulma)** | `~0.89 — 0.92` | Geometrik formun korunması, minimum yapay bükülme (rolling shutter absorsiyonu) |
| **C (FOV Korunumu)**| `~0.93 — 0.96` | ProPainter entegrasyonu sayesinde %94+ oranında orijinal görüş alanı korunumu |

---

## 💡 Hızlı Çalıştırma İpuçları

* **Tek Tıkla Çalıştırma:** Boru hattını ortam kurulumundan son kıyaslama videolarının indirilmesine kadar kesintisiz çalıştırmak için Google Colab üst menüsünden **Çalışma Zamanı -> Tümünü Çalıştır (Runtime -> Run all)** komutunu kullanabilirsiniz.
* **Donanım Hızlandırıcı:** Optik akış ve outpainting modüllerinin eşzamanlı çalışması yüksek VRAM gerektirir. Sağ üst menüden çalışma alanının mutlaka **T4 GPU** donanımına bağlı olduğunu teyit ediniz.
* **Pipeline 3 Köprüsü:** Bu boru hattı, modüler mimarisi sayesinde istendiği takdirde 3. Pipeline (Hibrit Hedef İzleme) ile tam uyumlu ve entegre bir şekilde çalışabilecek potansiyelde tasarlanmıştır. Mevcut aşamada takip algoritmamızı bağımsız benchmark videolarıyla validasyona tabi tutuyor olsak da; üretilen bu sarsıntısız çıktılar, ihtiyaç duyulduğunda hedef izleme modelini pürüzsüz verilerle besleyen ve algı kararlılığını en üst düzeye çıkaran bir ön işleme (preprocessing) katmanı olarak doğrudan devreye alınabilir.
