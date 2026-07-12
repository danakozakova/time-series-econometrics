# Analýza CGM glykémie — SARIMA, GARCH a Grangerova kauzalita 🩸

> 🇬🇧 *Time-series analysis of continuous glucose monitoring (CGM) data from the iDCL/DCLP3 clinical trial: STL decomposition, SARIMA forecasting with rolling 24-hour windows, structural-break detection around clinical interventions, GARCH volatility modeling, and Granger causality between daytime and nighttime glucose. **The clinical data cannot be redistributed** — see Data access below. Notebooks retain saved outputs, so all results are viewable without the data.*

Individuálny semestrálny projekt z predmetu Časové řady (BPE_CARA, Masarykova univerzita). Analyzujem hodinový glykemický časový rad pacienta s diabetom 1. typu z klinickej štúdie **iDCL Trial / DCLP3** (Brown et al., 2019) — **4 515 hodinových pozorovaní za 188 dní** (júl 2017 – február 2018), vrátane dvoch klinických intervencií.

Sledovanie glykémie poznám z každodenného života, a tak som chcela vedieť, čo o nej dokážu povedať nástroje časových radov.

## Čo projekt obsahuje

**[`01_select_patient.ipynb`](01_select_patient.ipynb)** — príprava dát: načítanie surových súborov štúdie, výpočet klinických metrík (GMI, TIR) pre všetkých pacientov, výber pacienta pre analýzu, agregácia CGM meraní na hodinový rad, napojenie termínov intervencií a kontrola chýbajúcich hodín. Výstupom je hodinový dataset jedného pacienta.

**[`02_analysis.ipynb`](02_analysis.ipynb)** — samotná analýza v piatich fázach:

1. **Explorácia a STL dekompozícia** — popisné štatistiky, denné profily, silná 24-hodinová sezónnosť
2. **SARIMA** — testy stacionarity, výber modelu, diagnostika, out-of-sample predikcia a **rolling 24-hodinová predikcia** (14 okien × 24 h) simulujúca reálne nasadenie; porovnanie s naivnými benchmarkmi cez Diebold-Marianov test
3. **Štrukturálne zlomy** — Chow test a CUSUM okolo intervencií T2 a T13
4. **GARCH(1,1)** — ARCH efekty a podmienená volatilita glykémie
5. **Grangerova kauzalita** — VAR model na denných a nočných radoch

Kompletný komentovaný výklad s interpretáciami je v **[reporte (PDF)](CARA_projekt_report.pdf)**.

## Hlavné zistenia

- Glykémia má výraznú dennú sezónnosť; SARIMA v rolling 24-hodinovej predikcii dosahuje RMSE 3,57 mg/dl (MAE 2,95) a poráža naivné benchmarky — **rozdiel však po Diebold-Marianovom teste nie je štatisticky signifikantný**. Poctivý záver: na hodinovej CGM predikcii sa jednoduchá persistencia prekonáva ťažko.
- Po intervencii v 13. týždni sa pacient **preukázateľne zhoršil** — čas v cieľovom pásme (TIR) klesol zo 74 % na 39 %, čo potvrdzujú štrukturálne testy aj porovnanie období.
- Grangerova kauzalita je **asymetrická**: denná glykémia predpovedá nočnú, opačný smer neplatí. Dáva to fyziologický zmysel — jedlo a aktivita počas dňa formujú nočný profil.

## Data access 🔐

**Dáta nie sú a nemôžu byť súčasťou tohto repozitára.** Ide o klinické dáta štúdie DCLP3, ktoré distribuuje Jaeb Center for Health Research pod vlastnými podmienkami použitia — prístup je individuálny a redistribúcia nie je povolená.

Ako dáta získať:

1. Otvorte stránku datasetu: **https://public.jaeb.org/dataset/573** (alternatívne cez NIDDK Central Repository: https://repository.niddk.nih.gov/studies/dclp3/)
2. Vyplňte žiadosť o prístup — uvádza sa **účel použitia** a kontaktný e-mail (osvedčil sa akademický)
3. Po schválení stiahnite dataset a rozbaľte súbory štúdie do priečinka `data/` v tomto projekte
4. Spustite `01_select_patient.ipynb`, ktorý z nich vygeneruje analytický dataset pre `02_analysis.ipynb`

Priečinok `data/` aj odvodené pacientské súbory sú v `.gitignore`, aby sa do repozitára nedostali ani omylom.

> Notebooky obsahujú uložené výstupy všetkých buniek — grafy, tabuľky aj výsledky testov si teda prezriete aj bez prístupu k dátam, priamo na GitHube alebo cez [nbviewer](https://nbviewer.org/github/danakozakova/time-series-econometrics/blob/main/03-cgm-glucose-analysis/02_analysis.ipynb).

## Atribúcia a poďakovanie

Táto analýza využíva dáta z klinickej štúdie *The International Diabetes Closed Loop (iDCL) Trial: Clinical Acceptance of the Artificial Pancreas (DCLP3)*, ktoré poskytuje Jaeb Center for Health Research (https://public.jaeb.org/dataset/573). Analýza, interpretácie a závery v tomto projekte sú výhradne moje a neboli revidované ani schválené študijnou skupinou DCLP3, Jaeb Center ani sponzorom štúdie. Analyzovaný pacient je v dátach plne anonymizovaný.

Referencia štúdie: Brown, S. A., et al. (2019). *Six-Month Randomized, Multicenter Trial of Closed-Loop Control in Type 1 Diabetes.* New England Journal of Medicine, 381(18), 1707–1717.

## Spustenie

```bash
pip install pandas numpy matplotlib statsmodels arch scipy jupyterlab
jupyter lab
```

Notebooky sú písané pre Python 3.10+. Bez dát (pozri Data access) je možné notebooky prezerať s uloženými výstupmi; na plnú replikáciu je potrebné najprv získať dataset a spustiť `01_select_patient.ipynb`.

## Obmedzenia a možné rozšírenia

MAPE nie je pre CGM dáta ideálna metrika (citlivosť na nízke glykémie) — vhodnejšia by bola klinicky orientovaná metrika, napr. založená na rizikových pásmach. Analýza pokrýva jedného pacienta; prirodzeným rozšírením je porovnanie naprieč pacientmi štúdie alebo hierarchické modelovanie.
