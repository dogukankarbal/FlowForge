# FlowForge v1.0

**Centrifugal Pump Preliminary Design Tool · Santrifüj Pompa Ön Tasarım Aracı**

1D Meanline · Multi-Case · Performance Curves · Browser-based · EN / TR

---

> 🇬🇧 [English](#english) · 🇹🇷 [Türkçe](#türkçe)

---

<a name="english"></a>
# 🇬🇧 English

## Overview

FlowForge v1.0 is a browser-based 1D meanline solver for the preliminary hydraulic design of centrifugal pump impellers. Written in Python, then ported to a self-contained HTML/JavaScript application — no installation required. Open `FlowForge_v1.0.html` in any modern web browser and run all calculations locally.

Given three operating-point inputs — flow rate *Q*, target head *H*, and rotational speed *n* — the solver iterates on impeller outer diameter *D₂* until the delivered net head converges to the target within the specified tolerance. After the design point is solved, the Performance Curve module plots *H(Q)*, *η(Q)*, *P(Q)*, and *Incidence(Q)* over any flow-rate range.

> **Scope:** Preliminary design only. FlowForge uses 1D meanline assumptions and does not replace 3D CFD or detailed mechanical design. Results should be validated against pump test data or higher-fidelity models before manufacturing.

---

## Key Capabilities

- Up to 8 simultaneous design cases with tab navigation and side-by-side comparison table
- Four slip-factor models: **Stodola**, **Wiesner (1967)**, **Pfleiderer (1961)**, and **user-specified constant σ**
- Automatic Wiesner validity check with fallback to Stodola when φ ≥ φ\_lim
- Dual specific-speed reporting: *ns*\_design (from inputs) and *ns*\_delivered (from converged geometry)
- Geometry-sensitive hydraulic loss model: friction (*k*h), incidence (*k*i), and disk-friction (*k*df)
- Inlet velocity triangle: auto or manual *D₁* and *b₁*, pre-swirl *Vu₁* support
- *k*m sensitivity analysis: d*D₂*/d*k*m at constant *H*, *n*, *Q*
- *k*m vs *ns* consistency check against Gülich (2020) regime table
- Performance Curve module: *H(Q)*, *η(Q)*, *P(Q)*, *Incidence(Q)* with BEP marker
- CSV export of all converged cases
- Bilingual interface: **English / Turkish** toggle
- Integrated fluid property tool: water (NIST polynomial fit) and EG/water mixtures (30 %, 50 %, 70 % by volume, 0–100 °C)

---

## File Structure

```
FlowForge_v1.0.html                    — Main application (self-contained, no dependencies)
FlowForge_v1.0_User_Manual.pdf        — Full user manual (English)
FlowForge_v1_0_Kullanici_Kilavuzu.pdf — Full user manual (Turkish)
FlowForge_v1.0_Validation_Report.pdf  — Step-by-step validation report
```

---

## Quick Start

1. Download or clone the repository.
2. Open `FlowForge_v1.0.html` in **Chrome**, **Edge**, or **Safari** by double-clicking the file.
3. Enter the operating point (*Q*, *H*, *n*), select a slip model, configure design parameters, and click **Run Case**.
4. After convergence, click **Compute Curve** to generate *H(Q)*, *η(Q)*, *P(Q)*, and *Incidence(Q)* over the specified flow-rate range.
5. Use the **Case Comparison** table to review all solved cases side by side, and **Export CSV** to save results.

> **Firefox note:** Firefox may block certain JavaScript features when loading via the `file://` protocol. If the interface does not respond, serve the file through a local web server:
> ```
> python3 -m http.server 8080
> ```
> Then navigate to `http://localhost:8080/FlowForge_v1.0.html`.

---

## Physics & Methods

| Component | Method |
|---|---|
| Euler head | Euler turbomachinery equation |
| Slip factor | Stodola / Wiesner (1967) / Pfleiderer (1961) / constant σ |
| Hydraulic friction loss | *H*f = *k*h · *H*Euler |
| Incidence loss | *H*i = *k*i · (*Vm₁*²/2g) · *i*² |
| Disk friction | *P*df = *k*df · ρ · ω³ · *D₂*⁵ · (1 + 5·*b₂*/*D₂*) |
| Water density | Polynomial fit to NIST data (0–100 °C, max error < 0.05 kg/m³) |
| EG/water density | Direct table lookup — no interpolation |

---

## Validation

Solver outputs for three independent scenarios were compared against step-by-step analytical calculations based on Gülich (2020), Dixon & Hall (2014), Wiesner (1967), and Pfleiderer (1961).

| # | Scenario | Slip Model | H\_net Error |
|---|---|---|---|
| 1 | Industrial water pump, 2900 rpm | Wiesner (1967) | 0.695 % ✓ |
| 2 | HVAC cooling pump, 50 % EG/water, 1450 rpm | Pfleiderer (1961) | 0.692 % ✓ |
| 3 | Fire-fighting pump, ns < 10 (boundary test) | Stodola | Hard-val. FAIL correctly flagged ✓ |

Full step-by-step calculations are available in `FlowForge_v1.0_Validation_Report.pdf`.

---

## Limitations

FlowForge v1.0 is **not appropriate** for:

- Strong off-design operation (*Q*/*Q*design < 0.5 or > 1.5)
- Cavitation / NPSH-critical applications
- Complex inlet conditions (pre-swirl from upstream components, non-uniform velocity distributions)
- Detailed loss breakdown (impeller–volute interaction, secondary flows, leakage paths)
- High-viscosity fluids (μ > ~5 mPa·s) where Reynolds-number corrections are non-trivial

---

## References

1. Wiesner, F.J. (1967). "A Review of Slip Factors for Centrifugal Impellers." *ASME J. Engineering for Power*, 89(4), pp. 558–572.
2. Pfleiderer, C. (1961). *Die Kreiselpumpen für Flüssigkeiten und Gase* (5th ed.). Springer-Verlag.
3. Dixon, S.L. & Hall, C.A. (2014). *Fluid Mechanics and Thermodynamics of Turbomachinery* (7th ed.). Butterworth-Heinemann.
4. Gülich, J.F. (2020). *Centrifugal Pumps* (4th ed.). Springer. — *k*m/*ns* regime table; disk friction model (§3.6).
5. Stepanoff, A.J. (1957). *Centrifugal and Axial Flow Pumps* (2nd ed.). Wiley.
6. NIST / IAPWS (2018). Water density data. https://webbook.nist.gov

---

## Author

Developed by **Doğukan Karbal** · Aeronautical - Astronautical Eng. 

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/dogukankarbal/)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white)](https://github.com/dogukankarbal)

---
---

<a name="türkçe"></a>
# 🇹🇷 Türkçe

## Genel Bakış

FlowForge v1.0, santrifüj pompa çarklarının ön hidrolik tasarımı için geliştirilmiş tarayıcı tabanlı bir 1D meanline çözücüdür. Önce Python ile yazılmış, ardından kurulum gerektirmeyen bağımsız bir HTML/JavaScript uygulamasına dönüştürülmüştür. `FlowForge_v1.0.html` dosyasını herhangi bir modern web tarayıcısında açın; tüm hesaplamalar yerel olarak çalışır.

Üç çalışma noktası girdisinden — debi *Q*, hedef basma yüksekliği *H* ve dönme hızı *n* — hareketle çözücü, iletilen net basma yüksekliği belirtilen tolerans içinde hedefe yakınsayana kadar çark dış çapı *D₂* üzerinde iterasyon yapar. Tasarım noktası çözüldükten sonra Performans Eğrisi modülü herhangi bir debi aralığında *H(Q)*, *η(Q)*, *P(Q)* ve *Hücum Açısı(Q)* eğrilerini çizer.

> **Kapsam:** Yalnızca ön tasarım aşaması içindir. FlowForge 1D meanline kabullerini kullanır; 3D CFD veya ayrıntılı mekanik tasarımın yerini almaz. Üretime geçmeden önce tüm sonuçlar pompa test verileri veya daha yüksek doğruluklu modellerle doğrulanmalıdır.

---

## Temel Yetenekler

- Sekize kadar eş zamanlı tasarım durumu; sekme tabanlı gezinme ve yan yana karşılaştırma tablosu
- Dört kayma faktörü modeli: **Stodola**, **Wiesner (1967)**, **Pfleiderer (1961)** ve **kullanıcı tanımlı sabit σ**
- Otomatik Wiesner geçerlilik denetimi; φ ≥ φ\_lim durumunda Stodola'ya geri dönüş
- İkili özgül hız raporlaması: *ns*\_tasarım (girdilerden) ve *ns*\_iletilen (yakınsanan geometriden)
- Geometriye duyarlı hidrolik kayıp modeli: sürtünme (*k*h), hücum açısı (*k*i) ve disk sürtünmesi (*k*df)
- Giriş hız üçgeni: otomatik veya elle *D₁* ve *b₁*, ön girdap *Vu₁* desteği
- *k*m hassasiyet analizi: sabit *H*, *n*, *Q*'da d*D₂*/d*k*m
- *k*m / *ns* tutarlılık denetimi (Gülich 2020 rejim tablosuna göre)
- Performans Eğrisi modülü: *H(Q)*, *η(Q)*, *P(Q)*, *Hücum Açısı(Q)* — BEP işaretçisiyle
- Tüm yakınsanmış durumlar için CSV dışa aktarımı
- İki dilli arayüz: **Türkçe / İngilizce** geçiş
- Entegre akışkan özelliği aracı: su (NIST polinom fit) ve EG/su karışımları (hacimce %30, %50, %70; 0–100 °C)

---

## Dosya Yapısı

```
FlowForge_v1.0.html                    — Ana uygulama (bağımsız, harici bağımlılık yok)
FlowForge_v1.0_User_Manual.pdf        — Tam kullanım kılavuzu (İngilizce)
FlowForge_v1_0_Kullanici_Kilavuzu.pdf — Tam kullanım kılavuzu (Türkçe)
FlowForge_v1.0_Validation_Report.pdf  — Adım adım validasyon raporu
```

---

## Hızlı Başlangıç

1. Depoyu indirin veya klonlayın.
2. `FlowForge_v1.0.html` dosyasını **Chrome**, **Edge** veya **Safari**'de çift tıklayarak açın.
3. Çalışma noktasını (*Q*, *H*, *n*) girin, kayma faktörü modelini seçin, tasarım parametrelerini yapılandırın ve **Run Case** düğmesine tıklayın.
4. Yakınsama sonrasında **Compute Curve** düğmesine tıklayarak *H(Q)*, *η(Q)*, *P(Q)* ve *Hücum Açısı(Q)* eğrilerini hesaplayın.
5. Durum Karşılaştırma tablosunu kullanarak tüm çözümlü durumları yan yana inceleyin; sonuçları **CSV Dışa Aktar** ile kaydedin.

> **Firefox notu:** Firefox, `file://` protokolü üzerinden açıldığında bazı JavaScript özelliklerini engelleyebilir. Arayüz yanıt vermiyorsa dosyayı yerel bir web sunucusu üzerinden açın:
> ```
> python3 -m http.server 8080
> ```
> Ardından `http://localhost:8080/FlowForge_v1.0.html` adresine gidin.

---

## Fizik & Yöntemler

| Bileşen | Yöntem |
|---|---|
| Euler basma yüksekliği | Euler turbomakina denklemi |
| Kayma faktörü | Stodola / Wiesner (1967) / Pfleiderer (1961) / sabit σ |
| Hidrolik sürtünme kaybı | *H*f = *k*h · *H*Euler |
| Hücum açısı kaybı | *H*i = *k*i · (*Vm₁*²/2g) · *i*² |
| Disk sürtünmesi | *P*df = *k*df · ρ · ω³ · *D₂*⁵ · (1 + 5·*b₂*/*D₂*) |
| Su yoğunluğu | NIST verilerine polinom fit (0–100 °C, maks. hata < 0.05 kg/m³) |
| EG/su yoğunluğu | Doğrudan tablo arama — interpolasyon yok |

---

## Doğrulama

Üç bağımsız senaryo için solver çıktıları, Gülich (2020), Dixon & Hall (2014), Wiesner (1967) ve Pfleiderer (1961)'e dayalı adım adım analitik hesaplarla karşılaştırıldı.

| # | Senaryo | Kayma Modeli | H\_net Hatası |
|---|---|---|---|
| 1 | Endüstriyel su pompası, 2900 rpm | Wiesner (1967) | %0.695 ✓ |
| 2 | HVAC soğutma pompası, %50 EG/su, 1450 rpm | Pfleiderer (1961) | %0.692 ✓ |
| 3 | Yangın söndürme pompası, ns < 10 (sınır testi) | Stodola | Hard-val. FAIL doğru işaretlendi ✓ |

Adım adım hesaplar `FlowForge_v1.0_Validation_Report.pdf` dosyasında mevcuttur.

---

## Kısıtlamalar

FlowForge v1.0 aşağıdaki durumlar için **uygun değildir**:

- Güçlü tasarım dışı çalışma (*Q*/*Q*tasarım < 0.5 veya > 1.5)
- Kavitasyon / NPSH-kritik uygulamalar
- Karmaşık giriş koşulları (yukarı akıştaki bileşenlerden ön girdap, düzgün olmayan hız dağılımları)
- Ayrıntılı kayıp dökümü gerektiren durumlar (çark–salyangoz etkileşimi, ikincil akışlar, sızıntı yolları)
- Yüksek viskoziteli akışkanlar (μ > ~5 mPa·s) — Reynolds sayısı düzeltmeleri bu aralıkta önemsiz değildir

---

## Kaynaklar

1. Wiesner, F.J. (1967). "A Review of Slip Factors for Centrifugal Impellers." *ASME J. Engineering for Power*, 89(4), ss. 558–572.
2. Pfleiderer, C. (1961). *Die Kreiselpumpen für Flüssigkeiten und Gase* (5. baskı). Springer-Verlag.
3. Dixon, S.L. & Hall, C.A. (2014). *Fluid Mechanics and Thermodynamics of Turbomachinery* (7. baskı). Butterworth-Heinemann.
4. Gülich, J.F. (2020). *Centrifugal Pumps* (4. baskı). Springer. — *k*m/*ns* rejim tablosu; disk sürtünme modeli (§3.6).
5. Stepanoff, A.J. (1957). *Centrifugal and Axial Flow Pumps* (2. baskı). Wiley.
6. NIST / IAPWS (2018). Su yoğunluk verileri. https://webbook.nist.gov

---

## Geliştirici

Geliştiren: **Doğukan Karbal** · Uçak ve Uzay Müh.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/dogukankarbal/)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white)](https://github.com/dogukankarbal)

---

*FlowForge v1.0 · Santrifüj Pompa Çözücü · 1D Meanline Aracı · Tarayıcı Tabanlı*
