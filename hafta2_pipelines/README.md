# 🛠️ Hafta 2: Görüntü & Video İşleme Baseline Boru Hatları (Pipelines)

Bu dizin, bilgisayarla görü (Computer Vision) staj akışının 2. haftasında kurgulanan **Görüntü Bulanıklığı Giderme (Deblurring)**, **Çevrimiçi Video Stabilizasyonu (Online Stabilization)** ve **Hibrit Hedef İzleme (Hybrid Tracking)** baseline boru hatlarının Google Colab ortamındaki uçtan uca uygulamalarını içerir.

Boru hatlarının her biri bağımsız birer Jupyter Notebook (`.ipynb`) olarak tasarlanmıştır. Aşağıdaki kılavuz, tüm boru hatları için geçerli olan ortak bulut altyapısı ve donanım yapılandırmalarını açıklar.

---

## ⚙️ Ortak Sistem Gereksinimleri ve Çalışma Alanı Kurulumu

Tüm modeller derin öğrenme tabanlı olduğu için Google Colab üzerinde **GPU hızlandırıcısı** ile çalıştırılmalıdır. Kurulum adımlarını notebook dosyasını açmadan önce veya ilk hücrelerde uygulayınız.

### 1. Donanım Hızlandırıcısının (GPU) Aktif Edilmesi
Modellerin gerçek zamanlı ve hatasız çalışabilmesi için NVIDIA GPU altyapısı zorunludur:
1. Google Colab üzerinde ilgili `.ipynb` dosyasını açın.
2. Üst menüden **Çalışma Zamanı (Runtime) -> Çalışma zamanı türünü değiştir (Change runtime type)** yolunu izleyin.
3. **Donanım Hızlandırıcı (Hardware accelerator)** seçeneğini **T4 GPU** (veya mevcudiyetine göre üstü GPU'lar) olarak ayarlayıp kaydettiğinizden emin olun.

### 2. Google Drive Bulut Köprüsünün Bağlanması
Yüklenen test videolarının, önceden eğitilmiş model ağırlıklarının (`pretrained weights`) ve üretilen yörünge/stabilizasyon çıktılarının kaybolmaması için çalışma alanı Google Drive üzerine sabitlenir.

Her notebook'un 1. hücresi bulut köprüsünü kurmak için şu standart yapıyı kullanır:

```python
from google.colab import drive
import os

print("🔗 Google Drive bulut altyapısı bağlanıyor...")
drive.mount('/content/drive')

# 🎯 ÖNEMLİ: Projenizi Drive'ınızda hangi klasöre kaydettiyseniz 
# aşağıdaki değişkeni o klasörün yolu ile güncelleyiniz:
PROJECT_ROOT = "/content/drive/MyDrive/<PROJE_KLASORUNUZ>"

if os.path.exists(PROJECT_ROOT):
    os.chdir(PROJECT_ROOT)
    print(f"✅ Çalışma dizini sabitlendi: {os.getcwd()}")
else:
    print(f"⚠️ HATA: '{PROJECT_ROOT}' yolu bulunamadı! Lütfen yolu kontrol ediniz.")
```

> 💡 **Not:** Notebook'u ilk kez çalıştırdığınızda Google Colab sizden Drive erişim izni isteyecektir. İzin penceresini onaylayarak bağlama işlemini tamamlayabilirsiniz.

### 3. Tek Tıkla Çalıştırma ("Run All" Uyumluluğu)
Notebook'ların tamamı bağımlılıkları yükleme, dizin sabitleme, çıkarım yapma ve metrik raporlama adımlarını otonom bir sırayla icra edecek şekilde yazılmıştır. Ortak kurulumları tamamladıktan sonra üst menüden **Çalışma Zamanı -> Tümünü Çalıştır (Runtime -> Run all)** seçeneğine tıklayarak boru hatlarını sıfır insan müdahalesiyle çalıştırabilirsiniz.

---

## 📂 Dizin İçeriği ve Özel Rehberler
Her bir boru hattının kendine has model parametrelerini, test veri setlerini ve metrik çıktılarını incelemek için aşağıdaki kılavuz dosyalara göz atabilirsiniz:

* [📘 Pipeline 1: Görüntü Bulanıklığı Giderme Kurulum Notları](./01_deblurring_guide.md)
* [📙 Pipeline 2: Çevrimiçi Video Stabilizasyonu Kurulum Notları](./02_stabilization_guide.md)
* [📗 Pipeline 3: Hibrit Hedef İzleme Kurulum Notları](./03_tracking_guide.md)
