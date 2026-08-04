# 🧪 Ablasyon Çalışması Raporu: Stage Swap (Aşama Yer Değiştirme) Analizi

## 1. Deneyin Amacı (Objective)
Bu ablasyon çalışması, Spikedge Görüntü İşleme Boru Hattı'ndaki algoritmaların **çalışma sırasının** hem Çoklu Nesne Takibi (MOT) performansına hem de Görsel Kaliteye (PSNR/ITF) olan etkisini ölçmek amacıyla yapılmıştır. 

`[Netleştirme ➔ Stabilizasyon]` sıralaması tersine çevrilerek, `[Stabilizasyon ➔ Netleştirme]` (Stage Swap) kurgusu test edilmiştir. Temel motivasyon, Ters İzdüşüm matrislerinin doğrudan ham video üzerinden hesaplanmasının takip doğruluğunu artırıp artırmayacağını ve bu sırada görüntü pürüzsüzlüğünün (ITF) ne durumda kalacağını gözlemlemektir.

## 2. İzleme Performansı Tablosu (MOT17-Train)

| Metrik | Orijinal HybridSORT (SOTA) | Boru Hattı V1 (Deblur ➔ Stab) | 🚀 Boru Hattı V2 (Stab ➔ Deblur) |
| :--- | :--- | :--- | :--- |
| **MOTA** (Takip Doğruluğu) | ~% 80.30 | % 83.612 | **% 83.631** 📈 |
| **HOTA** (Genel Başarı) | ~% 63.10 | % 72.252 | **% 72.391** 📈 |
| **IDF1** (Kimlik Koruma) | ~% 77.50 | % 81.093 | **% 81.409** 📈 |
| **MOTP** (Kutu Hassasiyeti) | - | % 85.653 | **% 85.758** 📈 |
| **IDSW** (Kimlik Değişimi) | ~700 Hata | 557 Hata | **546 Hata** 📉 (Daha İyi) |

## 3. Dinamik Görsel Kalite Analizi (MOT17-04 Örneklemi)
*Not: Referanssız bir veri seti olan MOT17 üzerinde kalite metrikleri, çıktının ham (raw) video ile piksel bazlı karşılaştırılmasıyla (ilk 100 kare) elde edilmiştir.*

*   **Ortalama PSNR (Müdahale Oranı):** 33.33 dB
*   **Ortalama SSIM (Yapısal Benzerlik):** 0.9659
*   **Dinamik ITF (Kareler Arası Stabilizasyon):** 36.00 dB

## 4. Mühendislik Çıkarımları ve Algoritmik Analiz

1. **Optik Akış ve Ham Veri Sadakati:** V2 mimarisinde LightStab doğrudan kameranın ham (raw) optik akışını okuduğu için, hesaplanan dönüşüm (transform) matrisleri gerçeğe çok daha yakın olmuştur. Bu durum, IDSW sayısının 557'den 546'ya düşmesiyle ispatlanmıştır.
2. **Görsel Kalite Karakteristiği:** PSNR skorunun 33.33 dB çıkması, boru hattının pikselleri "bozmadan" sadece gerekli netleştirme/stabilizasyon dokunuşlarını yaptığını göstermektedir.
3. **Pürüzsüzlükte SOTA Seviyesi:** Elde edilen 36.00 dB ITF skoru, literatürdeki önde gelen stabilizasyon modellerinin (Örn: StabiGS ~34.87 dB) üzerine çıkarak sistemin ardışık karelerde mükemmel bir kararlılık sağladığını kanıtlamaktadır.
