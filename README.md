# Analýza časových radov — ekonometria v Pythone 📈

> 🇬🇧 *Three time-series projects from a graduate econometrics course (Masaryk University): SARIMA/GARCH analysis of continuous glucose monitoring data from a clinical trial, ARMA forecasting of the EUR/USD exchange rate, and business-cycle decomposition with Okun's law for Slovakia. All in Python (statsmodels, arch). Write-ups are in Slovak.*

Tri projekty z predmetu Časové řady (BPE_CARA, Masarykova univerzita), všetky v Pythone. Zvyšok kurzu pracoval v Gretli a R — Python bola moja voľba, vrátane samostatnej implementácie testov, ktoré v statsmodels chýbajú (Diebold-Mariano, Chow).

---

## ⭐ [Analýza CGM glykémie — SARIMA, GARCH, Grangerova kauzalita](03-cgm-glucose-analysis/)

**Vlajkový projekt** (individuálna semestrálna práca, voľná téma): kompletná analýza glykemického časového radu pacienta s diabetom 1. typu z klinickej štúdie **iDCL/DCLP3** — 4 515 hodinových CGM pozorovaní za 188 dní.

Téma nie je náhodná: sledovanie glykémie poznám z každodenného života, a tak som chcela vedieť, čo o nej dokážu povedať nástroje časových radov.

- **STL dekompozícia a denné profily** — silná 24-hodinová sezónnosť
- **SARIMA modelovanie** s rolling 24-hodinovou predikciou (simulácia reálneho použitia v pumpe) — a záver: SARIMA poráža naivné benchmarky, ale po Diebold-Marianovom teste rozdiel **nie je štatisticky signifikantný**
- **Štrukturálne zlomy** (Chow, CUSUM) okolo klinických intervencií — po 13. týždni sa pacient preukázateľne zhoršil (TIR 74 % → 39 %)
- **GARCH(1,1)** — dynamika podmienenej volatility glykémie
- **Grangerova kauzalita** deň vs. noc: denná glykémia predpovedá nočnú, opačne nie

📄 [Report](03-cgm-glucose-analysis/CARA_projekt_report.pdf) · 📓 notebooky: [výber pacienta](03-cgm-glucose-analysis/01_select_patient.ipynb), [analýza](03-cgm-glucose-analysis/02_analysis.ipynb)

> ⚠️ **Dáta nie sú súčasťou repa.** Klinické dáta DCLP3 podliehajú podmienkam Jaeb Center for Health Research a nemožno ich redistribuovať. Návod na získanie prístupu je v README projektu. Notebooky obsahujú uložené výstupy, takže výsledky si prezriete aj bez dát.

---

## [ARMA predikcia kurzu EUR/USD](01-arma-forecasting-eurusd/)

Box-Jenkinsova metodológia na mesačnom kurze EUR/USD (FRED, 2000–2025, 312 pozorovaní):

- transformácia na log-diferencie, identifikácia cez ACF/PACF
- AR(1), MA(1), ARMA(1,1) — diagnostika (Ljung-Box, stabilita, invertibilita), výber modelu cez AIC/BIC
- rekurzívne predikcie 1–4 kroky dopredu, **expanding vs. rolling okno**
- vlastná implementácia **Diebold-Marianovho testu**: MA(1) štatisticky signifikantne poráža naivný model (náhodnú prechádzku) na všetkých horizontoch

📄 [Report](01-arma-forecasting-eurusd/CARA_U01_report.pdf) · 📓 [notebook](01-arma-forecasting-eurusd/CARA_U01.ipynb)

## [Hospodársky cyklus a Okunov zákon — Slovensko](02-okun-law-slovakia/)

Štvrťročný reálny HDP a nezamestnanosť SR (1998–2025), cez finančnú krízu aj covid:

- testy stacionarity (ADF, KPSS) a jednotkového koreňa so štrukturálnym zlomom (**Zivot-Andrews**)
- dekompozícia trendu tromi metódami (HP filter, polynomiálny trend, polynóm so zlomom) a robustnosť identifikovaných recesií naprieč metódami
- **Okunov koeficient** podľa metódy dekompozície + stabilita cez Chow test a umelé premenné
- bonus z dátovej reality: sezónne očistený HDP bol vo FREDe dostupný len do 2020, takže očistenie bolo treba vyriešiť vlastnoručne

📄 [Report](02-okun-law-slovakia/CARA_U02_report.pdf) · 📓 [notebook](02-okun-law-slovakia/CARA_U02.ipynb)

---

## Použité technológie

`Python` · `pandas` · `statsmodels` (ARIMA/SARIMA, STL, HP filter, ADF/KPSS/Zivot-Andrews, VAR) · `arch` (GARCH) · `matplotlib` · dáta cez `fredapi`

Každý projekt má vlastný priečinok s notebookom, reportom (PDF) a dátami alebo návodom na ich získanie. Notebooky sú spustiteľné odhora nadol; pri veľkých notebookoch odporúčam [nbviewer](https://nbviewer.org/).

## Autorka

Mgr. Dana Kozáková — štatistička a lektorka matematiky a štatistiky. 
