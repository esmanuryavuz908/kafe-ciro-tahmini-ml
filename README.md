# ☕ Kafe Günlük Ciro Tahmini ve Doğrusal Regresyon Analizi

60 günlük sentetik kafe operasyon verileri üzerinde veri sızıntısını önleyen ön işleme, özellik mühendisliği ve basit/çoklu doğrusal regresyon modellerini karşılaştırarak günlük ciro tahmini yapan makine öğrenmesi projesi.

![Python](https://img.shields.io/badge/Python-3.13-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=for-the-badge&logo=jupyter&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-Veri_Analizi-150458?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-Sayısal_Hesaplama-013243?style=for-the-badge&logo=numpy&logoColor=white)
![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-Regresyon-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Görselleştirme-11557C?style=for-the-badge)
![Seaborn](https://img.shields.io/badge/Seaborn-Isı_Haritası-4C72B0?style=for-the-badge)

---

## 🎯 Projenin Amacı ve Çözdüğü Problem

Kafe ve perakende işletmelerinde günlük ciro; yalnızca müşteri yoğunluğuna değil, hava sıcaklığı, hafta sonu trafiği ve aktif kampanyalar gibi operasyonel faktörlere de bağlıdır. Bu proje, söz konusu değişkenlerin günlük ciro üzerindeki etkisini nicel olarak ölçmeyi ve geleceğe yönelik senaryolar için güvenilir gelir tahminleri üretmeyi amaçlar.

Teknik açıdan proje iki temel makine öğrenmesi problemine odaklanır:
1. **Veri Sızıntısını (Data Leakage) Önleme:** Eksik değerlerin (`NaN`) doldurulması işlemini veri setini bölmeden önce değil, `%80 eğitim / %20 test` ayrımından sonra yalnızca eğitim seti ortalamalarıyla gerçekleştirir.
2. **Model Karmaşıklığı ve Özellik Etkisi Karşılaştırması:** Sadece müşteri sayısını kullanan basit doğrusal regresyon modeli ile hava durumu, kampanya, hafta sonu ve türetilen yeni özellikleri içeren çoklu doğrusal regresyon modelinin performansını somut metriklerle kıyaslar.

---

## ✨ Öne Çıkan Özellikler ve Gerçek Proje Bulguları

### Uygulanan Veri Bilimi Adımları
- **Tekrarlanabilir Sentetik Veri Üretimi:** `np.random.seed(42)` ile 60 günlük sıcaklık (`10–37°C`), hafta sonu, müşteri sayısı (`50–199`), kampanya durumu ve gürültü bileşenli ciro verisi oluşturulması.
- **Eksik Veri Simülasyonu ve Güvenli Doldurma:** Sıcaklık sütununda 3, Müşteri sayısı sütununda 2 olmak üzere toplam 5 eksik değerin simüle edilmesi ve `X_train` ortalamalarıyla hem eğitim hem test setinin doldurulması.
- **Kategorik Kodlama (Encoding):** `Hafta sonu` (`Evet: 1`, `Hayır: 0`) ve `Kampanya` (`Var: 1`, `Yok: 0`) değişkenlerinin sayısallaştırılması.
- **Özellik Mühendisliği (Feature Engineering):** Sıcaklığın `25°C` ve üzerinde olduğu günleri işaretleyen ikili `Sıcak Gün` değişkeninin türetilmesi.
- **Senaryo Bazlı Tahmin:** Eğitilen çoklu regresyon modeli ile yeni bir iş günü senaryosu için anlık ciro tahmini yapılması.

### 📊 Temel İstatistiksel Bulgular (60 Günlük Veri)
| Metrik | Gerçek Çıktı |
| :--- | :---: |
| **Günlük Ortalama Ciro** | `13.633,92 TL` |
| **Maksimum Günlük Ciro** | `20.578,23 TL` |
| **60 Günlük Toplam Müşteri Sayısı** *(eksik kayıtlar hariç)* | `7.443 kişi` |

### 🏆 Model Karşılaştırma Sonuçları
| Model | Kullanılan Değişkenler | $R^2$ Skoru | MSE (Hata Kareler Ortalaması) |
| :--- | :--- | :---: | :---: |
| **Model 1 (Basit Doğrusal Regresyon)** | Sadece `Müşteri sayısı` | `0.9312` | `1.095.218,95` |
| **Model 2 (Çoklu Doğrusal Regresyon)** | `Sıcaklık`, `Hafta sonu`, `Müşteri sayısı`, `Kampanya`, `Sıcak Gün` | **`0.9780`** | **`350.572,60`** |

> **Öne Çıkan Sonuç:** Operasyonel değişkenler ve türetilen `Sıcak Gün` özelliği modele dahil edildiğinde açıklayıcılık oranı ($R^2$) **`0.9312`'den `0.9780`'e** yükselmiş, Hata Kareler Ortalaması (MSE) ise yaklaşık **%68 oranında** düşmüştür.

### 🧮 Çoklu Regresyon Modeli Öğrenilen Katsayıları
- **Sabit Terim (Intercept):** `906,17`
- **Hafta sonu:** `+1.772,71`
- **Kampanya:** `+1.532,26`
- **Sıcak Gün ($\ge 25^\circ\text{C}$):** `+1.045,01`
- **Müşteri sayısı:** `+87,62`
- **Sıcaklık:** `-2,96`

### 🔮 Örnek Yeni Senaryo Tahmini
- **Girdi:** Hava Sıcaklığı `28°C` | Müşteri Sayısı `160` | Hafta Sonu: `Evet` | Kampanya: `Var` | Sıcak Gün: `Evet`
- **Modelin Öngördüğü Tahmini Ciro:** **`19.191,82 TL`**

---

## 🛠️ Kullanılan Teknolojiler ve Kütüphaneler

- **Dil:** Python 3.13
- **Geliştirme Ortamı:** Jupyter Notebook (`ipykernel`)
- **Veri Manipülasyonu ve Sayısal İşlemler:** `pandas`, `numpy`
- **Veri Görselleştirme:** `matplotlib`, `seaborn`
- **Makine Öğrenmesi ve Değerlendirme:** `scikit-learn` (`train_test_split`, `LinearRegression`, `mean_squared_error`, `r2_score`)

---

## 📁 Proje Klasör ve Dosya Yapısı

> **Not:** Veri seti kod içerisinde `NumPy` ve `Pandas` ile dinamik olarak üretildiği için harici bir `.csv` dosyasına ihtiyaç duyulmaz.

```text
kafe-ciro-tahmini-ml/
├── kafe_ciro_tahmini.ipynb   # Veri üretimi, EDA, ön işleme, modelleme ve değerlendirme notebook'u                                 
├── .gitignore                                 # Gereksiz geçici dosya ve sanal ortamları hariç tutan yapılandırma
└── README.md                                  # Proje dokümantasyonu
```
---

## 🚀 Kurulum ve Çalıştırma

Projeyi yerel bilgisayarınızda çalıştırmak için aşağıdaki adımları takip edin:

### 1. Depoyu Klonlayın
```bash
git clone https://github.com/esmanuryavuz908/kafe-ciro-tahmini-ml.git
cd kafe-ciro-tahmini-ml
```

### 2. Sanal Ortam Oluşturun (Önerilen)
```bash
python -m venv venv

# Windows için:
venv\Scripts\activate

# macOS / Linux için:
source venv/bin/activate
```

### 3. Gerekli Kütüphaneleri Yükleyin
```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

### 4. Notebook'u Başlatın
```bash
jupyter notebook
```
Açılan tarayıcı arayüzünden `kafe_ciro_tahmini.ipynb` dosyasını seçin ve **Cell > Run All** (veya **Run > Run All Cells**) seçeneğiyle tüm hücreleri sırasıyla çalıştırın.

---
