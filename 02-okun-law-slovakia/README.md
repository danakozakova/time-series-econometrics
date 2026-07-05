# Hospodársky cyklus a Okunov zákon — Slovensko 🇸🇰

> 🇬🇧 *Business-cycle analysis of Slovak quarterly real GDP and unemployment (1998–2025, covering both the 2008–09 crisis and COVID): stationarity and structural-break testing (ADF, KPSS, Zivot-Andrews, Chow, CUSUM), trend decomposition via HP filter and polynomial trends with breaks, and Okun's law estimation across decomposition methods. Write-up in Slovak.*

Druhá skupinová úloha z predmetu Časové řady (riešená samostatne, v Pythone). Analyzujem štvrťročný **reálny HDP a mieru nezamestnanosti Slovenska** (FRED, 1998–2025) — obdobie pokrývajúce finančnú krízu 2008–09 aj covid.

## Postup a zistenia

- **Stacionarita** — vizuálna analýza, ACF/PACF, ADF a KPSS testy na úrovniach aj diferenciách
- **Štrukturálne zlomy** — Chow test (vlastná implementácia), CUSUM na reziduách AR modelov a **Zivot-Andrewsov test** jednotkového koreňa s endogénnym zlomom
- **Dekompozícia trendu tromi metódami** — Hodrick-Prescottov filter (λ = 1600), polynomiálny trend a polynomiálny trend so štrukturálnym zlomom; porovnanie cyklických zložiek cez korelačnú maticu a robustnosť identifikovaných recesií naprieč metódami
- **Okunov koeficient** — regresia medzery nezamestnanosti na medzeru výstupu, odhad zvlášť pre každú metódu dekompozície; stabilita vzťahu testovaná Chow testom a modelom s umelými premennými pre zlom

Podrobný výklad s tabuľkami a grafmi je v **[reporte (PDF)](CARA_U02_report.pdf)**.

## Poznámky z dátovej reality

- Sezónne očistená séria HDP bola vo FREDe dostupná len do roku 2020 — pracovala som preto so sezónne neočistenou sériou a očistenie som riešila a porovnávala sama
- `zivot_andrews` v statsmodels **modifikuje vstupné dáta** (mutuje pole namiesto práce s kópiou) — v kóde ošetrené volaním na `y.copy()`. Drobnosť, ktorá vie potichu pokaziť nadväzujúce výpočty, ak o nej neviete

## Súbory a spustenie

- [`CARA_U02.ipynb`](CARA_U02.ipynb) — kompletná analýza, spustiteľná odhora nadol
- `data.csv` — stiahnuté časové rady (súčasť repa, analýza je plne replikovateľná)
- Bunky so sťahovaním z FRED API (série CLVMNACSAB1GQSK, LRHUTTTTSKQ156S) sú ponechané ako neaktívne (`raw`) — dokumentujú pôvod dát bez nutnosti API kľúča

```bash
pip install pandas numpy matplotlib statsmodels scipy jupyterlab
jupyter lab CARA_U02.ipynb
```

Zdroj dát: Federal Reserve Economic Data — https://fred.stlouisfed.org
