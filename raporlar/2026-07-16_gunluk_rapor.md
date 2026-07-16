# 📅 Günlük Staj İlerleme Raporu — 16 Temmuz 2026

**Stajyer:** Batuhan Küçük  
**Proje:** BK-DVT-OptLab: Computer Vision Pipeline Optimization 2026  
**Çalışılan Odak:** Hafta 1 — Literatür Taramasının Tamamlanması, Altın Standart (Referans) Belirleme ve Profesyonel Skorlama Sisteminin Entegrasyonu  
**Tarih:** 16.07.2026  

### 🖼️ Alan 1: Görüntü Blur Giderme (Motion & Defocus Deblurring)
* **Durum:** Dün eksik olan makaleler tamamlanarak ilgili alanın literatürü 16 makaleye çıkarıldı ve 10 üzerinden skorlama sistemi tabloya entegre edildi.
* **Skorlama ve Referans:** 16 numaralı *GLOWDeblur* (Gao vd., 2026) makalesi, saniyede 1.7 kare (FPS) işleme hızı, 1.6B parametreli hafif difüzyon altyapısı ve gerçek dünya genellemesiyle 10/10 tam puan alarak altın standart referans olarak belirlendi.
* **Öne Çıkan Modeller:** Hafta 2 Colab testleri için referans modele ek olarak; işlem yükünü ~15 kat azaltan *DeMoE* (9.4/10) ve dikkat modülünde işlem yükünü 852.2G MACs'ten 2.9G MACs'e düşüren *PECA* (9.3/10) yüksek öncelikli adaylar olarak işaretlendi.

### 🎥 Alan 2: Gerçek Zamanlı Video Stabilizasyonu (Online & Causal Methods)
* **Durum:** Hazırlanan 20 makale için staj hedeflerine uygunluğu ölçen 10 üzerinden profesyonel skorlama sistemi başarıyla uygulandı.
* **Skorlama ve Referans:** 7 numaralı *LightStab* (Liu vd., 2026) makalesi; sıfır gecikme (zero-latency) ile çalışması, gelecek karelere bakmaması (no look-ahead) ve gözetimsiz (unsupervised) yapısıyla stajın çevrimiçi/nedensel çalışma önceliğine tam uyum sağlayarak 10/10 referans noktası seçildi.
* **Öne Çıkan Modeller:** Tam gerçek zamanlı (~31.25 FPS) çalışan ve SuperPoint ile özellikleri eşleştiren hibrit mimari (9.3/10) ile adaptasyon sürecini sarsıntılı alanlara hedefleyen *MetaVideoStab* varyantı (8.8/10), uygulanabilirlik açısından yüksek skor aldı.

### 🎯 Alan 3: STAPLE & YOLO Hibrit Hedef İzleme (Tracking-by-Detection)
* **Durum:** Listelenen 15 makale; gerçek zamanlı hız, kod erişilebilirliği ve ölçülebilir parametre optimizasyonu kriterleri doğrultusunda skorlandı.
* **Skorlama ve Referans:** 6 numaralı *TBDQ-Net* (Jia vd., 2026) çalışması, YOLO tabanlı hibrit izleme yapısı ve %80 oranında parametre azaltımı sunmasıyla 10/10 altın standart referans olarak konumlandırıldı.
* **Öne Çıkan Modeller:** YOLO ailesinin NMS (Non-Maximum Suppression) gecikmesini ortadan kaldıran 108 FPS hızındaki *RT-DETR* (9.4/10) ve sadece 1.5M parametreye sahip ultra hafif *DEIMv2* (9.2/10), potansiyel hibrit tespit omurgaları olarak belirlendi.

### ⚙️ Skorlama Metodolojisi
* Literatürdeki tüm çalışmalar; açık kaynaklı kod mevcudiyeti (Colab uyumu), hesaplama verimliliği (FLOPs/FPS optimizasyonları), yöntem özgünlüğü ve staj kapsamına (hız-doğruluk dengesi, nedensellik) uyum gibi 4 ana kritere göre değerlendirilmiştir.
* Tablolarda hiçbir orijinal veri değiştirilmemiş, yeni değerlendirmeler ayrı bir sütun olarak eklenmiş ve yapılan tüm güncellemeler *Conventional Commits* formatında GitHub reposuna aktarılmıştır.

---

## 🚀 Yarına Dair Aksiyon Planı (Sonraki Adımlar)

1. **Hafta 1 Kapanışı ve Depo Düzenlemesi:** Hafta 1 için belirlenen en az 45 makale, özetler, metrik analizleri ve referanslı skorlama hedeflerine eksiksiz ulaşıldığından; GitHub `README.md` dosyasının haftalık genel ilerleme özetini barındıracak şekilde düzenlenmesi.
2. **Alan Değerlendirmesi (State-of-the-Art) Sentezlerinin Yazılması:** Staj akış planında Hafta 1 gereksinimleri arasında zorunlu kılınan ancak henüz tamamlanmayan **"Alan Değerlendirmesi (State-of-the-Art)"** paragraflarının yazılması. Her 3 konu (Görüntü Blur Giderme, Video Stabilizasyonu ve Hibrit Hedef İzleme) için hazırlanan literatür tablolarının altına; güncel SOTA liderlerini, mimari darboğazları ve genel trendleri özetleyen 1'er paragraftan oluşan profesyonel teknik değerlendirme yazılarının entegre edilmesi.
