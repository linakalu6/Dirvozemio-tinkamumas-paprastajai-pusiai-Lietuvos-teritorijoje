# Paprastosios pušies (*Pinus sylvestris*) dirvožemio tinkamumas Lietuvoje

Grupinis semestro darbas (dirbtinio intelekto metodai, Lietuvos inžinerijos kolegija, Želdynų dizaino studijų programa). Duomenys: GBIF, SoilGrids. Modelis pagal dirvožemio savybes parodo, kiek vieta panaši į vietas, kuriose stebima paprastoji pušis. Išsami ataskaita pateikiama atskirai.

## Tikslas

Sukurti dirbtinio intelekto modelį, kuris pagal dirvožemio savybes (pH, smėlio ir molio dalis, organinė anglis trijuose gyliuose) įvertina, ar vieta panaši į vietas, kuriose stebima paprastoji pušis (*Pinus sylvestris*) Lietuvoje. Rezultatas skirtas želdynų dizaineriams ir želdintojams kaip **orientacinis patarimas**, o ne sodinimo rekomendacija.

## Užduoties tipas

Dvejetainė klasifikacija: `1` = pušis užregistruota (GBIF stebėjimas), `0` = pušies nėra (atsitiktinis taškas).

## Darbo pasidalijimas

Visos studentės tokio tipo projektą atliko pirmą kartą, todėl kiekviena pasidarė savo mokomąjį projektą (su Claude ir ChatGPT; sintetiniais ir realiais duomenimis). Grupė pasirinko Linos K. projektą kaip išsamiausią (Sigita B. su DI pagalba palygino savo darbą su juo). DI pasiūlytas darbų paskirstymas nepasiteisino, todėl darbus dalijomės pačios pagal situaciją.

| Studentė | Savo mokomasis projektas | Indėlis į bendrą projektą | DI įrankis |
| --- | --- | --- | --- |
| Lina K. | Pirmoji atliko darbą su DI; GBIF ir SoilGrids duomenys | Pasiūlė idėją ir pirmąjį duomenų surinkimą (GBIF 671 → 636 įrašai), sukūrė GitHub saugyklą ir README, pakvietė kitus dalyvius, įkėlė failus, paleido sąsiuvinius 01–03, pranešė klaidas; išbandė Dirv_DR10LT ir nusprendė jo nenaudoti | Claude Sonnet 5.5 |
| Svietlana M. | Sintetiniai duomenys (3000 mėginių) | Rašė ataskaitą: parsisiuntė failus iš GitHub, su DI pagalba suformavo ataskaitą, patikrino projektą pagal metodiką; pagal trūkumų sąrašą projektas papildytas ir ataskaita suformuota iš naujo | Claude Sonnet 5.5 (nemokama versija) |
| Mylita Ch. | Realūs duomenys: gbif.org (673 pušies radavietės) ir Dirv_DR10LT; ekologinė taisyklė (F1 0,667) ir Random Forest (F1 0,759) | Tikrino darbą ir paleido modelį, atrinko ir patikrino 3 sėkmės ir 3 nesėkmės pavyzdžius (7.4), sutvarkė literatūros sąrašą | Claude |
| Sigita B. | Sintetiniai duomenys (2 400 įrašų), bazinis ir gradientinio stiprinimo modeliai (ChatGPT, Colab) | Palygino savo projektą su Linos K. projektu, nagrinėjo 02 sąsiuvinį (Lietuvos riba, foniniai taškai, SoilGrids nuskaitymas), parengė pristatymą (pptx) | ChatGPT |

Išsamus individualus indėlis, naudotos užklausos („promptai“) ir refleksija pateikti ataskaitoje (X priedas, D–F).

## Duomenys

| Šaltinis | Kam naudojamas |
| --- | --- |
| [GBIF](https://www.gbif.org) *Pinus sylvestris* stebėjimai Lietuvoje | teigiami pavyzdžiai (671 įrašas, po valymo 636) |
| Atsitiktiniai taškai Lietuvos viduje | neigiami (pseudo-neigiami) pavyzdžiai (636) |
| [SoilGrids 2.0 (ISRIC)](https://docs.isric.org/globaldata/soilgrids/) | dirvožemio savybės: pH (phh2o), smėlis (sand), molis (clay), organinė anglis (soc); gyliai 0–5, 5–15, 15–30 cm; vidurkis; 250 m |
| [GADM 4.1](https://gadm.org) Lietuvos riba | neigiamų taškų generavimui ir žemėlapiui (vietinis failas `gadm41_LTU_0.json`) |

**GBIF citata:** GBIF.org (4 November 2026) GBIF Occurrence Download https://doi.org/10.15468/dl.2jbfvb (licencija CC BY-NC 4.0).

**Duomenų kiekis:** iš 1272 taškų (636 + 636) po SoilGrids žingsnio liko **1154 pavyzdžiai** (538 pušis, 616 atsitiktiniai taškai); mokymui 924, testavimui 230. Tai **vidutinis duomenų kiekis (500–5000)**. Neteko 98 teigiamų ir 20 neigiamų taškų (nėra SoilGrids duomenų).

**Privatumas:** iš GBIF failo pašalinti stulpeliai su stebėtojų vardais (`recordedBy`, `identifiedBy`, `rightsHolder`); skelbiamas tik `gbif_be_vardu.csv`. Pilnas failas pasiekiamas per nurodytą GBIF DOI.

**Dirv_DR10LT nenaudotas.** Rinkinio archyvą (10 apskričių `.shp` failų, apie 4,1 GB) atsisiuntėme ir išskleidėme, bet atributų lentelės neperskaitėme ir su savo taškais nesujungėme. Priežastys (dalis pagrįsta oficialia specifikacija, o ne mūsų patikrinimu): sluoksnis skirtas žemės ūkio naudmenų vertinimui, todėl miškų gali neapimti; didelė apimtis ir reikalinga biblioteka geopandas; LTDK-99 kodai ir galima LKS-94 koordinačių sistema; žemės ūkio balai nėra susiję su pušimi. Aprėpties patikra paliekama tolesniam darbui (ataskaitos 4.6 skyrius).

## Failų struktūra

| Failas | Aprašas |
| --- | --- |
| `pusis_01_duomenys.ipynb` | GBIF duomenų įkėlimas, valymas (671 → 636), taškų žemėlapis. Sukuria `gbif_be_vardu.csv` ir `pusis_isvalyta.csv` |
| `pusis_02_dirvozemis.ipynb` | Neigiami pavyzdžiai, Lietuvos riba, SoilGrids nuskaitymas, valymas, grafikai. Sukuria `pusis_su_dirvozemiu.csv` |
| `pusis_03_modelis.ipynb` | Baseline ir Proposed eksperimentai, metrikos, požymių svarba, SHAP, sėkmės ir nesėkmės pavyzdžiai, demonstracija |
| `pusis_04_zemelapis.ipynb` | Modelis pritaikytas visai Lietuvai (5 panašumo lygiai) ir interaktyvus žemėlapis pagal koordinates |
| `requirements.txt` | Naudojamos bibliotekos |
| `gbif_be_vardu.csv` | GBIF duomenys be stebėtojų vardų |
| `pusis_isvalyta.csv`, `pusis_su_dirvozemiu.csv` | Tarpiniai duomenų failai |
| `metrikos.csv`, `pavyzdziai.csv`, `*.png` | Eksperimentų rezultatai, pavyzdžiai ir paveikslai |

## Kaip paleisti

1. Įdiekite Python 3.10 ar naujesnį (rekomenduojama [Anaconda](https://www.anaconda.com)).
2. Įdiekite bibliotekas:

   ```
   pip install -r requirements.txt
   ```

3. Atsisiųskite šį repozitoriumą ir **visus failus padėkite viename aplanke savo naudotojo aplanke** (sąsiuviniai patys suranda reikiamus failus).
4. Paleiskite `jupyter notebook` ir atidarykite sąsiuvinius **eilės tvarka**: 01 → 02 → 03 → 04. Kiekviename paleiskite visus langelius (**Kernel → Restart & Run All**).

**Pastaba:** 02 ir 04 sąsiuviniai skaito SoilGrids duomenis internetu, todėl reikalingas ryšys; 02 gali užtrukti kelias minutes. Jei Lietuvos ribos atsisiuntimas nepavyksta, atsisiųskite `gadm41_LTU_0.json` ir padėkite į sąsiuvinio aplanką.

## Eksperimentai

Pagal metodikos 3 skyrių modelio tipas parenkamas pagal duomenų kiekį. Kadangi turime vidutinį duomenų kiekį (1154), naudojame XGBoost (rekomenduojamas šiai kategorijai) ir palyginame su RandomForest.

- **Baseline:** DummyClassifier (dažniausia klasė) ir LogisticRegression.
- **Proposed 1:** RandomForest su hiperparametrų paieška (GridSearchCV, 5-kartų GroupKFold).
- **Proposed 2:** XGBoost su hiperparametrų paieška (GridSearchCV, 5-kartų GroupKFold).
- **Duomenų skaidymas:** erdvinis (ištisi 0,5° stačiakampiai testavimui; 51 grupė), kad kaimyniniai taškai neatsidurtų ir mokyme, ir teste.
- **Metrikos:** Accuracy, Precision, Recall, F1, ROC AUC, konfuzijos matrica.
- **Paaiškinamumas:** permutation importance ir SHAP.
- **Žemėlapis:** modelis pritaikytas 1 km tinkleliui Lietuvoje (65 087 taškai); 5 panašumo lygiai (kvintiliai).

## Rezultatai

Testavimo aibė (230 taškų), skaičiai iš `metrikos.csv`:

| Modelis | Accuracy | Precision | Recall | F1 | ROC AUC |
| --- | --- | --- | --- | --- | --- |
| Baseline: Dummy | 0.600 | 0.000 | 0.000 | 0.000 | 0.500 |
| Baseline: LogisticRegression | 0.630 | 0.531 | 0.652 | 0.585 | 0.697 |
| Proposed 1: RandomForest | 0.657 | 0.561 | 0.652 | 0.603 | 0.709 |
| Proposed 2: XGBoost | 0.643 | 0.547 | 0.630 | 0.586 | 0.688 |

Metodikos tikslas (F1 ≥ 0,70 arba tikslumas ≥ 80 %) **nepasiektas**. Skirtumai tarp modelių maži ir greičiausiai neviršija atsitiktinės variacijos (230 testavimo taškų). Priežastys aprašytos skyriuje „Ribotumai“.

**Svarbiausi požymiai** (permutation importance, SHAP): pH, organinė anglis (5–15 cm), smėlis; kryptys atitinka pušies ekologiją, bet dalį ryšių gali lemti tai, kad pušis registruojama miškuose, o atsitiktiniai taškai patenka ir į dirbamus laukus.

### Sėkmės ir nesėkmės pavyzdžiai

Iš testavimo aibės atrinkta 3 teisingi ir 3 klaidingi sprendimai, kiekvienas patikrintas palydoviniame vaizde (išsamiai ataskaitos 7.4 skyriuje):

| Nr. | Rezultatas | Vieta | Pastaba |
| --- | --- | --- | --- |
| 1 | Sėkmė (pušis atpažinta, tikimybė 0,83) | Kuršių nerija | Spygliuočių miškas; rūgštus, smėlingas dirvožemis |
| 2 | Sėkmė (pušis atpažinta, 0,76) | Daumilai, Švenčionių r. | Spygliuočių miškas kitame regione |
| 3 | Sėkmė (teisingai „nėra“, 0,01) | Saugėlaukis, Joniškio r. | Ariami laukai; šarminis, molingas dirvožemis |
| 4 | Nesėkmė (pušis praleista, 0,09) | Salakas, Zarasų r. | Klaida dėl duomenų: tikėtina, pasodinta pušis želdiniuose |
| 5 | Nesėkmė (pušis praleista, 0,19) | Birštonas | Klaida dėl skyros: 250 m langelis sumaišo šlaitą, upę ir lauką |
| 6 | Nesėkmė (klaidingai „tinka“, 0,79) | Magūnai, Visagino sav. | Tikra modelio klaida: vandens telkinys miško viduje |

## Žemėlapis ir demonstracija

Modelis pritaikytas visai Lietuvai (1 km tinklelis); rezultatas – 5 lygių **panašumo į pušies stebėjimo vietas** žemėlapis. Patikra rodo, kad žemėlapis atskiria žemus lygius nuo aukštų (pušies stebėjimų dalis 0,11 → 0,63), bet 4 ir 5 lygiai beveik nesiskiria (0,611 ir 0,630). Žemėlapis yra ne sodinimo rekomendacija; lygių pavadinimuose žodis „tinkama“ reiškia tik panašumo laipsnį.

- **03 sąsiuvinis:** `prognoze(lat, lon)` pagal WGS 84 koordinates nuskaito SoilGrids ir pateikia modelio rezultatą; `interaktyvus_zemelapis(lat, lon)` parodo tašką žemėlapyje.
- **04 sąsiuvinis:** `zemelapis_taskui(lat, lon)` parodo tašką Lietuvos panašumo žemėlapyje su legenda ir riba.

Pavyzdys:

```python
m = zemelapis_taskui(54.69, 25.28)   # platuma, ilguma (WGS 84)
m
```

Jei taške SoilGrids duomenų nėra (pvz., užstatyta vieta), sistema tai pasako ir neprognozuoja.

## Ribotumai

- Neigiami pavyzdžiai sugeneruoti atsitiktinai (pseudo-neigiami), todėl kai kuriuose iš jų pušis iš tikrųjų gali augti.
- GBIF stebėjimai šališki: telkiasi prie kelių ir gyvenviečių, Vilniaus ir pajūrio apylinkėse. Koordinačių tikslumas skiriasi: po valymo 83 % taškų turi nurodytą netikslumą (mediana apie 10 m, didžiausia reikšmė apie 26 km), likusiems 17 % netikslumas nenurodytas. Dirvožemio duomenų skiriamoji geba yra 250 m (SoilGrids), todėl ji riboja tikslumą labiau nei GBIF koordinatės.
- Pušis dažnai sodinama, todėl jos buvimas ne visada rodo natūralų tinkamumą.
- SoilGrids yra modeliuoti duomenys (250 m), ne tiesioginiai matavimai; organinės anglies reikšmės didelės visame rinkinyje (mediana 94 g/kg) ir nepatikrintos su vietiniais matavimais.
- Dėl SoilGrids trūkstamų reikšmių neteko 98 teigiamų ir 20 neigiamų taškų (atrankos poslinkis).
- Naudoti tik dirvožemio požymiai (nėra klimato, žemės dangos, reljefo); Dirv_DR10LT nenaudotas.
- Vienas erdvinis skaidymas (230 testavimo taškų); modelių skirtumai neviršija atsitiktinės variacijos; nėra tikimybių kalibracijos; LightGBM ir neuroniniai tinklai neišbandyti.
- Žemėlapio 4 ir 5 lygiai beveik nesiskiria.
- Rezultatas yra orientacinė prognozė, o ne galutinis sprendimas.

## Dirbtinio intelekto naudojimas

Projekte naudoti DI asistentai (Claude, ChatGPT) kodui, klaidų taisymui, ataskaitos rengimui ir paaiškinimams. Rezultatus studentės paleido, patikrino ir interpretavo pačios; naudotos užklausos aprašytos ataskaitos X priede (E).
