# 📘 Pipeline 1: Görüntü Bulanıklığı Giderme (Image Deblurring) — Kurulum ve Çalıştırma Kılavuzu

**Notebook Dosyası:** [🔗 01_deblurring_baseline.ipynb](hafta2_pipelines/01_deblurring_baseline.ipynb)  
**Odak Alanı:** Hareket (Motion Blur) ve Odak (Defocus Blur) Bulanıklığı Restorasyonu  
**Referans Veri Seti:** GoPro / RealBlur Benchmark Test Kümesi  

Bu boru hattı, kameraların hızlı hareketi, sarsıntısı veya uzun pozlama nedeniyle bozulmuş görüntülerin derin öğrenme tabanlı restorasyon mimarileri ile netleştirilmesini ve kantitatif geri kazanım kalitesinin ölçülmesini sağlar. 

Notebook dosyamız repoda uçtan uca çalışır durumda mevcuttur. Aşağıdaki kılavuz, boru hattının çalışma mantığını, özelleştirilebilir temel yapılandırma noktalarını ve örnek kullanım parçalarını (snippets) tek bir akışta özetlemektedir.

---

## ⚙️ 1. Çalıştırma Akışı ve Temel Kurulum

Boru hattı; ortam hazırlığı, veri denetimi, model çıkarımı (inference) ve kantitatif metrik analizi olmak üzere 4 temel fazdan oluşur. Kendi görsellerinizi veya farklı bir dizini kullanmak istediğinizde, notebook içerisindeki şu temel blokları kendinize göre özelleştirebilirsiniz:

### 🔹 Ortam ve Dizin Yapılandırması (Örnek Şablon)
Notebook'un ilk aşamasında bulut sürücüsü bağlanır ve girdi/çıktı klasörleri tanımlanır. Farklı bir çalışma alanı veya veri seti kullanmak için sadece yol değişkenlerini değiştirmeniz yeterlidir:

```python
from google.colab import drive
import os

# 1. Bulut altyapısı bağlanır
drive.mount('/content/drive')

# 🎯 KİŞİSELLEŞTİRME: Ana proje dizini ve veri yolları
PROJECT_ROOT = "/content/drive/MyDrive/<PROJE_KLASORUNUZ>"
INPUT_DIR = os.path.join(PROJECT_ROOT, "Deblurring/input")
OUTPUT_DIR = os.path.join(PROJECT_ROOT, "Deblurring/output")
WEIGHTS_DIR = os.path.join(PROJECT_ROOT, "Deblurring/weights")

os.makedirs(INPUT_DIR, exist_ok=True)
os.makedirs(OUTPUT_DIR, exist_ok=True)
```

---

## 🚀 2. Model Çıkarımı (Inference) ve Örnek Kullanım

Bağımlılıklar (`torch`, `opencv-python`, `scikit-image`) yüklendikten ve model ağırlıkları doğrulandıktan sonra, sistem belirlediğiniz klasördeki tüm bulanık görselleri tarayarak NVIDIA T4 GPU üzerinde **FP16 (Yarı Hassasiyetli)** çıkarım uygular.

### 🔹 Çıkarım ve Metrik Analizi Özet Bloğu
Notebook içinde çalışan temel çıkarım döngüsünün ve restorasyon kalitesini ölçen metrik motorunun sadeleştirilmiş mantığı şu şekildedir:

```python
import glob
import cv2
import torch
from skimage.metrics import peak_signal_noise_ratio as psnr
from skimage.metrics import structural_similarity as ssim

# 1. Bulanık görseller tarayarak kuyruğa alınır
image_files = sorted(glob.glob(os.path.join(INPUT_DIR, "*.png")))
print(f"🚀 {len(image_files)} görsel için GPU çıkarımı başlatılıyor...")

for img_path in image_files:
    file_name = os.path.basename(img_path)
    
    # 🎯 ÖRNEK AKIŞ: Görsel tensöre çevrilir, model(input) çalıştırılır
    # restored_tensor = model(input_tensor)
    
    # Netleştirilen çıktı hedef klasöre kaydedilir
    save_path = os.path.join(OUTPUT_DIR, file_name)
    print(f"   └── ✔️ Netleştirildi ve kaydedildi: {file_name}")

# 2. Referans (Ground-Truth) görsellerle kantitatif kıyaslama
# psnr_val = psnr(gt_image, restored_image)
# ssim_val = ssim(gt_image, restored_image, channel_axis=2)
```

---

## 📊 3. Kantitatif Başarım ve Beklenen Metrikler

Boru hattının son aşaması, netleştirilen çıktıları orijinal (ground-truth) referans görsellerle karşılaştırarak iki evrensel metriği raporlar:

* **PSNR (Tepe Sinyal Gürültü Oranı - Peak Signal-to-Noise Ratio):** Görüntüdeki piksel gürültüsünün oranını logaritmik olarak ölçer. Birimi desibeldir (dB) ve değer yükseldikçe kalite artar.
* **SSIM (Yapısal Benzerlik İndeksi - Structural Similarity Index):** İnsan algısına yakın şekilde parlaklık, kontrast ve yapısal tutarlılığı 0 ile 1 arasında değerlendirir.

### 🎯 T4 GPU / GoPro Test Seti Referans Değerleri:
Aşağıdaki tablo, repoda mevcut olan `01_deblurring_baseline.ipynb` notebook'u standart GoPro test kümesinde çalıştırıldığında elde edilmesi gereken referans baseline aralıklarını gösterir:

| Metrik | Beklenen Başarım Aralığı | Hedef / Anlamı |
| :--- | :---: | :--- |
| **Ortalama PSNR** | `31.0 dB — 32.5 dB` | Yüksek sinyal geri kazanımı ve minimum piksel gürültüsü |
| **Ortalama SSIM** | `0.910 — 0.940` | İnsan gözü algısına %90+ oranında yakın yapısal netlik |

---

## 💡 Hızlı Çalıştırma İpuçları

* **Tek Tıkla Çalıştırma:** Boru hattını sıfırdan son metrik raporuna kadar kesintisiz çalıştırmak için Google Colab üst menüsünden **Çalışma Zamanı -> Tümünü Çalıştır (Runtime -> Run all)** komutunu kullanabilirsiniz.
* **GPU Kontrolü:** İşlemlerin gerçek zamanlı hızda tamamlanması için sağ üst menüden çalışma alanının **T4 GPU** donanımına bağlı olduğundan emin olunuz. CPU üzerinde çıkarım süreleri görsel başına 10-15 saniyeye kadar uzayabilir.
