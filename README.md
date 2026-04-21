# ⚽ La Liga Analysis & Prediction (2004–2025)

Analizë statistikore dhe probabilistike e kampionatit të La Liga
duke përdorur të dhëna historike nga 21 sezone (2004–2025).

---

## 🎯 Qëllimi i Projektit

- Analizë eksploruese e performancës së ekipeve nëpër 21 sezone
- Modelim i lëvizjes së ekipeve mes gjendjeve me **Zinxhirët Markov**
- Parashikim i kampionit të La Liga me **Monte Carlo Simulation**
- Reduktim dimensionesh me **PCA** dhe parashikim me **Linear Regression**
- Llogaritje e forcës relative të ekipeve me **Elo Rating System**
- Aplikim i shpërndarjeve probabilistike: **Poisson, Gjeometrik, Binomial**

---

## 📊 Dataset

Të dhënat janë mbledhur manualisht nga:
- **FBref.com** — statistika të avancuara

| Burimi | Sezone | Kolonat Kryesore |
|--------|--------|-----------------|
| La Liga Table | 21 (2004–2025) | Pts, W, D, L, GF, GA, GD |
| Squad Shooting | 9 (2016–2025) | Sh, SoT, SoT%, G/Sh |
| Squad Goalkeeping | 10 (2015–2025) | Save%, CS, CS%, GA90 |
| Squad Standard Stats | 11 (2014–2025) | Age, Poss, Ast, CrdY, CrdR |

---

## 🗂️ Struktura e Projektit

```
    laliga-analysis/
    │
    ├── data/
    │   ├── raw/
    │   │   ├── laliga_table/        ← 21 sezone standings
    │   │   ├── shooting/            ← 9 sezone shooting stats
    │   │   ├── goalkeeping/         ← 10 sezone goalkeeping stats
    │   │   └── squad_standart_stats/← 11 sezone standard stats
    │   │
    │   └── processed/
    │       ├── laliga_master.csv    ← dataset kryesor i pastruar
    │       ├── laliga_elo.csv       ← elo ratings historike
    │       └── laliga_pca.csv       ← komponentet PCA
    │
    ├── notebooks/
    │   ├── 01_data_cleaning.ipynb
    │   ├── 02_EDA.ipynb
    │   ├── 03_markov_chains.ipynb
    │   ├── 04_probability_models.ipynb
    │   ├── 05_elo_rating.ipynb
    │   ├── 06_PCA.ipynb
    │   └── 07_monte_carlo_final.ipynb
    │
    ├── outputs/
    │   └── figures/                 ← të gjitha grafiqet e eksportuara
    │
    └── README.md

```


---

## 📓 Përshkrimi i Notebook-eve

### 01 — Data Cleaning (ETL)
Pastrimi i të dhënave bruto:
- Lexim automatik i të gjitha CSV-ve me `glob`
- Rregullim i karaktereve të prishura (encoding UTF-8)
- Standardizim i emrave të ekipeve
- Heqja e kolonave të panevojshme
- Bashkim i 4 dataseteve në `laliga_master.csv`

### 02 — Exploratory Data Analysis (EDA)
- Statistika përshkruese për 420 observime
- Dominanca historike — Barcelona 12 tituj, Real Madrid 7, Atletico 2
- Trendi i pikëve të kampionit nëpër 21 sezone
- Shpërndarje normale e pikëve (μ=52, σ=16)
- Heatmap korrelacioni — GD (0.96) dhe GF (0.88) korrelojnë më shumë me Pts
- Scatter plot sulmi vs mbrojtja

### 03 — Zinxhirët Markov
Modelim i lëvizjes së ekipeve mes 4 gjendjeve:
Kampion → Top4 → MidTable → Fundtabele
- Matrica e tranzicionit nga të dhënat historike
- Matricë specifike për çdo ekip (Real Madrid dhe Barcelona kurrë nën Top4)
- Simulim i 20 sezoneve për çdo ekip
- Graf interaktiv me **Pyvis**

### 04 — Modelet Probabilistike
**Shpërndarja Poisson:**
- λ(Barcelona) = 2.445 gola/ndeshje
- λ(Real Madrid) = 2.323 gola/ndeshje

**Shpërndarja Gjeometrike:**
- Barcelona fiton titull çdo **1.75 sezone** mesatarisht
- Real Madrid fiton titull çdo **3.0 sezone** mesatarisht
- Atletico fiton titull çdo **10.5 sezone** mesatarisht

### 05 — Elo Rating System
- Sistem i bazuar në shahun klasik, adaptuar për futboll
- Elo finale (2024-2025): Barcelona 1916, Real Madrid 1894, Atletico 1735
- Probabiliteti i ndeshjes: Barcelona vs Real Madrid → 39.8% vs 35.2%
- Trendi historik i Elo nëpër 21 sezone

### 06 — Principal Component Analysis (PCA)
Reduktim nga 11 variabla → 3 komponente:

| Komponenti | Varianca | Interpretimi |
|-----------|----------|-------------|
| PC1 | 60.2% | Forca e Përgjithshme (GD, Pts, Elo) |
| PC2 | 13.6% | Stili i Lojës (Save% vs Sh, Poss) |
| PC3 | 9.1% | Eksperienca (Age) |

Kumulativisht: **82.9%** e informacionit ruhet me vetëm 3 komponente.

### 07 — Monte Carlo Final me PCA
Simulim i 10,000 sezoneve duke përdorur komponentet PCA:
- Model Linear Regression: **R² = 92.87%**
- Rezultatet finale:

| Ekipi | Probabiliteti |
|-------|--------------|
| Barcelona | **55.3%** |
| Real Madrid | **41.7%** |
| Atletico Madrid | **2.5%** |
| Sevilla | **0.2%** |
| Girona | **0.2%** |

---

## 🛠️ Teknologjitë e Përdorura

```python
Python 3.11
pandas        # manipulim të dhënash
numpy         # llogaritje numerike
matplotlib    # vizualizim
scipy         # shpërndarje probabilistike
scikit-learn  # PCA + Linear Regression
networkx      # grafe Markov
pyvis         # grafe interaktive
```

---

## 📈 Rezultatet Kryesore

> **Barcelona është favorit i qartë** me 55.3% probabilitet për titullin e La Liga, ndjekur nga Real Madrid me 41.7%. Atletico Madrid është i vetmi ekip tjetër me shanse reale (2.5%). Ekipet e tjera kanë probabilitete minimale për shkak të historikut të tyre.

> **Zinxhirët Markov** tregojnë se Barcelona dhe Real Madrid kurrë nuk kanë rënë nën pozicionin e 4-rt — fenomen unik në historinë e La Liga.

> **PCA** zbuloi se 60% e suksesit të një ekipi shpjegohet nga një faktor i vetëm: **Forca e Përgjithshme** (GD + Pts + Elo).

---

## ⚠️ Kufizimet e Studimit

- Të dhënat e avancuara (Shooting, Goalkeeping) disponohen vetëm nga 2015-2025
- xG (Expected Goals) nuk është përfshirë për mungesë të të dhënave historike
- Modeli nuk merr parasysh transfertat, lëndimet apo ndryshimet e trajnerit
- Zinxhirët Markov supozojnë që e ardhmja varet vetëm nga e tashmja

---
## 👤 Autori

**Ermir Haxhia**  
MSc Applied Mathematics — Universiteti i Tiranës  

🌐 [Portfolio](https://infrequent-network-348.notion.site/Ermir-Haxhia-Data-Analysis-Portfolio-32c61957cde1800ebff6d7c1190170ae) 
· [LinkedIn](https://www.linkedin.com/in/ermir-haxhia-b988212b5) 
· [GitHub](https://github.com/ermirhaxhia)

---
