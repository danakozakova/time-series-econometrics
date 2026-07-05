# ARMA predikcia kurzu EUR/USD 💱

> 🇬🇧 *Box-Jenkins methodology on the monthly EUR/USD exchange rate (FRED, 2000–2025): log-difference transformation, ARMA model selection and diagnostics, recursive 1–4 step forecasts with expanding vs. rolling windows, and a hand-rolled Diebold-Mariano test against the random-walk benchmark. Write-up in Slovak.*

Prvá skupinová úloha z predmetu Časové řady (riešená samostatne, v Pythone). Analyzujem mesačný kurz **EUR/USD** z databázy FRED (séria DEXUSEU, 2000–2025, 312 pozorovaní).

## Postup a zistenia

- **Transformácia na log-diferencie** — kurz je multiplikatívny proces, log-diferencie stabilizujú rozptyl a majú priamu interpretáciu ako mesačné percentuálne výnosy
- **Identifikácia cez ACF/PACF** — úroveň sa správa ako náhodná prechádzka, log-diferencie ukazujú jeden hrot na lagu 1 → kandidáti AR(1), MA(1), ARMA(1,1)
- **Diagnostika** — Ljung-Box (reziduá všetkých modelov sú biely šum), stabilita a invertibilita; podľa AIC/BIC vyhráva úsporný **MA(1)**
- **Rekurzívne predikcie 1–4 kroky dopredu** s expandujúcim aj rolling oknom (out-of-sample 60 mesiacov)
- **Diebold-Marianov test** (vlastná implementácia — v statsmodels chýba): MA(1) štatisticky signifikantne poráža naivný model (náhodnú prechádzku) na všetkých horizontoch aj oknách; RMSE naivného modelu je pri h ≥ 2 o ~40 % vyššie

Podrobný výklad s tabuľkami a grafmi je v **[reporte (PDF)](CARA_U01_report.pdf)**.

## Súbory a spustenie

- [`CARA_U01.ipynb`](CARA_U01.ipynb) — kompletná analýza, spustiteľná odhora nadol
- `data.csv` — stiahnutý časový rad (súčasť repa, analýza je plne replikovateľná)
- Bunky so sťahovaním z FRED API sú v notebooku ponechané ako neaktívne (`raw`) — dokumentujú presný pôvod dát (séria DEXUSEU) bez nutnosti vlastného API kľúča

```bash
pip install pandas numpy matplotlib statsmodels scipy jupyterlab
jupyter lab CARA_U01.ipynb
```

Zdroj dát: Federal Reserve Economic Data — https://fred.stlouisfed.org/series/DEXUSEU
