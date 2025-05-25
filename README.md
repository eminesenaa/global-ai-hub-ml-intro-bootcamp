# 🧠 Gözetimli Öğrenme ile Formula 1 DNF Tahmini (Random Forest)

Bu proje, **Akbank Global AI Hub Makine Öğrenmesine Giriş Bootcamp** kapsamında geliştirilmiştir.  
Amaç, Formula 1 yarışlarında bir sürücünün yarışı tamamlayıp tamamlamayacağını (DNF – Did Not Finish) makine öğrenmesi kullanarak tahmin etmektir.

Veri seti, 1950–2024 yılları arasındaki gerçek Formula 1 yarış sonuçlarını içermektedir.  
Model, yarışa ait bazı temel bilgiler kullanarak DNF olup olmayacağını öngörmeye çalışır.

---

## 🎯 1. Proje Amacı

Bu projede hedef, geçmiş yarış verilerine dayanarak bir sürücünün yarışı tamamlayıp tamamlamayacağını tahmin eden bir makine öğrenmesi modeli geliştirmektir.

Tahmin için kullanılan bazı temel öznitelikler şunlardır:
- Başlangıç sırası (grid)
- Tamamlanan tur sayısı (laps)
- Yarış süresi (milliseconds)
- Kazanılan puan (points)
- Yarış sonucu (`dnf`: 0 → Bitirdi, 1 → Bitiremedi)

---

## 📊 2. Kullanılan Veri Seti

Bu projede kullanılan veri seti, [Formula 1 World Championship (1950–2022)](https://www.kaggle.com/datasets/rohanrao/formula-1-world-championship-1950-2020) verilerine dayanmaktadır.  
Veri seti Kaggle üzerinden temin edilmiştir ve toplamda **100.000'den fazla yarış sonucu** içermektedir.

Bu proje kapsamında özellikle `results.csv`, `drivers.csv` ve `races.csv` dosyalarından yararlanılmıştır.  
Veriler birleştirilerek her sürücünün yarış bazlı geçmiş performansına ait aşağıdaki öznitelikler kullanılmıştır:

### 📌 Öznitelikler (Features):
- `grid`: Sürücünün yarışa başladığı sıra
- `laps`: Tamamlanan tur sayısı
- `milliseconds`: Yarışı tamamlama süresi (sadece bitirenler için)
- `points`: Yarıştan kazanılan puan
- `position`: Sürücünün yarışı bitirdiği sıradaki pozisyon (DNF'ler için eksik olabilir)

### 🎯 Hedef Değişken (Target):
- `dnf` (Did Not Finish):  
  - `0`: Sürücü yarışı tamamladı  
  - `1`: Sürücü yarışı tamamlayamadı (DNF)

Veri ön işleme aşamasında, eksik veriler `-1` ile doldurularak modelin bu durumları öğrenmesine izin verilmiştir.

---

## 🤖 3. Kullanılan Yöntemler ve Algoritmalar

Bu projede **gözetimli öğrenme (supervised learning)** yöntemi kullanılmıştır.  
Hedef değişken (`dnf`) sadece iki değer alabildiği için bu bir **ikili sınıflandırma (binary classification)** problemidir.

Model, geçmiş yarışlara ait öznitelikleri kullanarak yeni bir sürücünün yarış dışı kalma (DNF) ihtimalini tahmin etmeyi amaçlar.

### 🔎 Denenen Makine Öğrenmesi Algoritmaları:
Projede çeşitli sınıflandırma algoritmaları uygulanarak performansları karşılaştırılmıştır:

- `Logistic Regression`
- `Decision Tree Classifier`
- `Random Forest Classifier`
- `K-Nearest Neighbors (KNN)`

Her algoritma **5 katlı çapraz doğrulama (cross-validation)** ile değerlendirilmiştir.  
Karşılaştırmalar F1 skoruna göre yapılmış, çünkü:
- Veri seti dengesizdir (DNF olan ve olmayan sayısı eşit değil)
- F1 skoru, precision ve recall arasında denge kurduğu için uygundur

### 🏆 En Başarılı Model: Random Forest

Karşılaştırma sonucunda en yüksek F1 skorunu veren model **Random Forest** olmuştur.  
Bu model:
- Dengesiz veri ile iyi çalışır
- Aşırı öğrenmeye karşı dayanıklıdır
- Özellik önemini çıkarmaya olanak tanır

Bu nedenle proje kapsamında en uygun model olarak Random Forest tercih edilmiştir.

---

## 📈 4. Model Başarımı ve Değerlendirme

Eğitilen modeller, test verisi üzerinde aşağıdaki metriklerle değerlendirilmiştir:

- **Accuracy (Doğruluk)**: Modelin genel başarı oranı
- **Precision (Kesinlik)**: DNF tahmini yapılanların ne kadarının gerçekten DNF olduğu
- **Recall (Duyarlılık)**: Gerçek DNF vakalarının ne kadarının doğru tahmin edildiği
- **F1 Skoru**: Precision ve Recall’un dengeli ortalaması

### ✅ Random Forest Sonuçları (Test Verisinde)

| Metrik      | Değer     |
|-------------|-----------|
| Accuracy    | ~%99.5    |
| Precision   | ~%99.9    |
| Recall      | ~%99.6    |
| F1 Score    | **~0.9989** |

Model ayrıca **karışıklık matrisi (confusion matrix)** ile görselleştirilmiş ve modelin DNF'leri başarıyla ayırt ettiği gözlemlenmiştir.

### 📌 Özellik Önemi Analizi

Random Forest modelinde kullanılan özniteliklerin önem düzeyleri de hesaplanmıştır.  
Modelin kararlarında en etkili olan öznitelikler:

1. `laps` (tamamlanan tur sayısı)  
2. `milliseconds` (yarış süresi)  
3. `grid` (başlangıç sırası)

Bu analiz, modelin gerçek dünya mantığıyla tutarlı bir şekilde karar verdiğini göstermektedir.

---

## 🧾5. Sonuç ve Çıkarımlar

Bu proje kapsamında Formula 1 yarışlarındaki sürücülerin yarışı tamamlayıp tamamlamayacağı (DNF) başarılı bir şekilde tahmin edilmiştir.  
Makine öğrenmesi teknikleriyle geçmiş verilere dayalı olarak gelecekteki yarış senaryoları için anlamlı tahminler yapılabileceği gösterilmiştir.

### 🎯 Elde Edilen Başlıca Sonuçlar:

- `Random Forest` algoritması, hem yüksek doğruluk hem de güçlü genelleme yeteneği sayesinde en başarılı model olmuştur.
- F1 skoru yaklaşık **0.9989** gibi oldukça yüksek bir değere ulaşmıştır.
- Veri setindeki eksik bilgiler model performansını bozmadan `-1` ile temsil edilerek başarı korunmuştur.
- Modelin kararları, öznitelik önemi (feature importance) analiziyle anlaşılır ve güvenilir şekilde yorumlanabilmiştir.

### 💡 Öğrenilenler ve Gelecek Adımlar:

- Gerçek dünya verileriyle çalışan sınıflandırma problemlerinde **veri ön işleme ve dengeleme tekniklerinin** model başarısında kritik rol oynadığı görülmüştür.
- `SHAP` gibi açıklanabilirlik yöntemleriyle model kararları daha detaylı analiz edilebilir.
- Aynı veri seti, farklı problem tanımlarıyla (örneğin sıralama tahmini, puan klasifikasyonu) başka projelere dönüştürülebilir.

------

## 🧪 Ek: Modelin Overfitting Durumu

Bu projede kullanılan Random Forest modeli üzerinde yapılan test ve çapraz doğrulama sonuçlarına göre **overfitting (aşırı öğrenme)** durumu gözlemlenmemiştir.

### 🔍 Overfitting Olmadığını Gösteren Bulgular:

- **Test verisinde yüksek başarı**:  
  Model daha önce görmediği test verisinde de %99.5 üzeri doğruluk ve 0.9989 F1 skoru ile güçlü performans göstermiştir.

- **Çapraz doğrulama ile desteklenmiş başarı**:  
  5 katlı cross-validation sonucunda F1 skorları tutarlı çıkmış ve ortalama skor **0.9738** olarak ölçülmüştür.

- **Random Forest algoritmasının yapısı**:  
  Bu algoritma, çok sayıda karar ağacının birleşimiyle çalışır ve genelleme kabiliyeti yüksek olduğu için overfitting’e karşı dirençlidir.

- **Dengeli ve büyük veri seti**:  
  27.000+ gözlem içeren veri seti, eksik değerlerin anlamlı şekilde temsil edilmesi (`fillna(-1)`) ve sınıf dengesinin korunması (`stratify=y`) ile doğru biçimde işlenmiştir.

### ✅ Sonuç:
Model hem eğitim hem test verilerinde başarılı sonuçlar verdiği, hem de veri bölünmeleri arasında tutarlı davrandığı için bu projede **overfitting riski düşük** olarak değerlendirilmiştir.

---

## 🔗 Projeye Erişim

Bu projeye ait Jupyter Notebook dosyasına **Kaggle hesabım üzerinden** aşağıdaki bağlantıdan ulaşabilirsiniz:

👉 [Kaggle Üzerinde Görüntüle](https://www.kaggle.com/code/eminesena/supervised-random-forest-f1-dnf)

Notebook, veri ön işleme, model karşılaştırması, Random Forest eğitimi, değerlendirme metrikleri ve görselleştirme adımlarını detaylı biçimde içermektedir.


