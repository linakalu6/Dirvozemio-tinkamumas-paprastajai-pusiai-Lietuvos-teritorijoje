# Paprastosios pušies (*Pinus sylvestris*) dirvožemio tinkamumas Lietuvoje

Semestro darbas (dirbtinio intelekto kursas).

## Tikslas
Sukurti dirbtinio intelekto modelį, kuris pagal dirvožemio savybes (pH, smėlio ir molio dalis, organinė anglis) nustato, ar vieta tinka augti paprastajai pušiai (*Pinus sylvestris*) Lietuvoje. Rezultatas skirtas želdynų dizaineriams ir želdintojams kaip orientacinis patarimas.

## Užduoties tipas
Dvejetainė klasifikacija: `1` = pušis auga, `0` = pušies nėra.

## Duomenys
| Šaltinis | Kam naudojamas |
|---|---|
| [GBIF](https://www.gbif.org) *Pinus sylvestris* stebėjimai Lietuvoje | teigiami pavyzdžiai |
| Atsitiktiniai taškai Lietuvos viduje | neigiami (pseudo-neigiami) pavyzdžiai |
| [SoilGrids 2.0 (ISRIC)](https://docs.isric.org/globaldata/soilgrids/) | dirvožemio savybės (phh2o, sand, clay, soc; gyliai 0–5, 5–15, 15–30 cm; vidurkis) |
| [GADM](https://gadm.org) Lietuvos riba | neigiamų taškų generavimui |

**GBIF citata:** GBIF.org (4 November 2026) GBIF Occurrence Download https://doi.org/10.15468/dl.2jbfvb (licencija CC BY-NC 4.0).

Duomenys yra vieši ir neturi asmens duomenų.

## Failų struktūra
| Failas | Aprašas |
|---|---|
| `pusis_01_duomenys.ipynb` | GBIF duomenų įkėlimas, valymas, taškų žemėlapis. Sukuria `pusis_isvalyta.csv` |
| `pusis_02_dirvozemis.ipynb` | Neigiamų pavyzdžių generavimas ir dirvožemio savybių nuskaitymas iš SoilGrids. Sukuria `pusis_su_dirvozemiu.csv` |
| `pusis_03_modelis.ipynb` | Baseline ir Proposed eksperimentai, metrikos, požymių svarba, SHAP |
| `requirements.txt` | Naudojamos bibliotekos |
| `gbif..csv` | GBIF atsisiųsti duomenys (neapdoroti) |
| `metrikos.csv`, `*.png` | Eksperimentų rezultatai |

## Kaip paleisti
1. Įdiekite Python 3.10 ar naujesnį (rekomenduojama [Anaconda](https://www.anaconda.com)).
2. Įdiekite bibliotekas:
   ```
   pip install -r requirements.txt
   ```
3. Atsisiųskite šį repozitoriumą ir **visus failus padėkite viename aplanke savo naudotojo aplanke** (sąsiuviniai patys suranda reikiamus failus).
4. Paleiskite `jupyter notebook` ir atidarykite sąsiuvinius **eilės tvarka**: 01 → 02 → 03. Kiekviename paleiskite visus langelius (**Run → Run All Cells**).

**Pastaba:** 2 sąsiuvinis skaito SoilGrids duomenis internetu, todėl reikalingas ryšys ir jis gali užtrukti kelias minutes ar ilgiau.

## Eksperimentai
- **Baseline:** DummyClassifier (dažniausia klasė) ir LogisticRegression.
- **Proposed:** RandomForest su hiperparametrų paieška (GridSearchCV).
- **Duomenų skaidymas:** erdvinis (ištisi 0,5° stačiakampiai testavimui), kad kaimyniniai taškai neatsidurtų ir mokyme, ir teste.
- **Metrikos:** Accuracy, Precision, Recall, F1, ROC AUC, konfuzijos matrica.
- **Paaiškinamumas:** permutation importance ir SHAP.

## Rezultatai
Įrašykite savo skaičius iš `metrikos.csv`:

| Modelis | Accuracy | Precision | Recall | F1 | ROC AUC |
|---|---|---|---|---|---|
| Baseline: Dummy | | | | | |
| Baseline: LogisticRegression | | | | | |
| Proposed: RandomForest | | | | | |

## Ribotumai
- Neigiami pavyzdžiai sugeneruoti atsitiktinai (pseudo-neigiami), todėl kai kuriuose iš jų pušis iš tikrųjų galėjo augti.
- GBIF stebėjimai šališki (telkiasi prie kelių ir gyvenviečių), dalis koordinačių suapvalinta (~1 km).
- Pušis dažnai sodinama, todėl jos buvimas ne visada rodo natūralų tinkamumą.
- SoilGrids yra modeliuoti duomenys (250 m skiriamoji geba), ne tiesioginiai matavimai.
- Rezultatas yra orientacinė rekomendacija, o ne galutinis sprendimas.

## Autorius
Įrašykite vardą, pavardę ir grupę.
