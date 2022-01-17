# e-NDP_HTR



e-NDP_HTR training experiments to fit transcribed pages from AN, LL105-126 registers into Kraken HTR core.


# Definitions

Stats for each register volume (LL105 - LL126, 26 volumes)


|      Volume     |    Dates   |  Type | N.Pages |
|:---------------:|:----------:|:-----:|:-------:|
|       105       | 1326-1352. |       |         |
|       106A      | 1356-1361. |       |         |
|       106B      | 1362-1364. | Train |    8    |
|       107       | 1367-1370. |       |         |
|       108A      | 1392-1394. | Train |    27   |
|       108B      | 1397-1399. | Train |    8    |
|       109A      | 1399-1401. |  Test |    4    |
|       109B      | 1401-1405. | Train |    7    |
|       109C      | 1405-1407. | Train |    8    |
|       110       | 1407-1411. | Train |    7    |
|       111       | 1412-1414. |  Test |    3    |
|       112       | 1414-1424. | Train |    7    |
|       113       | 1425-1432. | Train |    10   |
|       114       |  1433-1437 | Train |    8    |
|       115       | 1440-1444. |  Test |    4    |
|       116       | 1445-1459. |  Test |    4    |
|       117       | 1450-1454. | Train |    60   |
|  118 (minutes)  | 1453-1456. |       |         |
|       119       | 1456-1460. |       |         |
|       120       | 1460-1465. |       |         |
|       121       | 1465-1474. |  Test |    3    |
|       122       | 1474-1481. | Train |    7    |
|     123-124     | 1481-1489. |  Test |    4    |
|       125       |  1489-1493 | Train |   20+8  |
|       126       |  1493-1497 | Train |    8    |
| Bnf Latin 17740 |  1430-1444 |  Test |   687   |

references:
[Projet e-NDP – Notre-Dame de Paris et son cloître : archives des séances du séminaire](https://lamop.hypotheses.org/files/2020/11/e-NDP_seance1_20201020-compresse-1.pdf)


[SÉRIE LL MONUMENTS ECCLÉSIASTIQUES REGISTRES](http://www.archivesnationales.culture.gouv.fr/chan/chan/fonds/EGF/SA/InvSAPDF/Ll.pdf)

# Architecture

### Architecure 1: 


![alt text for screen readers](https://gitlab.com/magistermilitum/home_alcar_kraken/-/raw/main/images/kraken_arch_1.drawio.png)

hyper_params': {'pad': 16, 'freq': 1.0, 'batch_size': 1, 'lag': 5, 'min_delta': None, 'optimizer': 'Adam', 'lrate': 0.0001, 'momentum': 0.9, 'weight_decay': 0, 'schedule': 'reduceonplateau', 'normalization': None, 'normalize_whitespace': True, 'augment': False, 'step_size': 10, 'gamma': 0.1, 'rop_patience': 3, 'cos_t_max': 50}}

### Architecure 2:

# Training



training board accuracy on validation set

<p float="left">
<img src="https://gitlab.com/magistermilitum/home_alcar_kraken/-/raw/main/images/val_acc_G1.jpg" width="546" height="400"> 
</p>





# Experiments

## Training and testing data-sets 

- F. Odart de Morchesne:  189 images (378 pages): https://gallica.bnf.fr/ark:/12148/btv1b9059518w.image  /  http://elec.enc.sorbonne.fr/morchesne/html/morchesne.html
- cart. ND de Clairmarais :  134 images (268 pages) : https://bvmm.irht.cnrs.fr/mirador/index.php?manifest=https://bvmm.irht.cnrs.fr/iiif/32117/manifest  /  https://doi.org/10.5281/zenodo.5600884
- Livre Rouge ( Châtelet de Paris. Y//3 ) :  34 images : https://www.siv.archives-nationales.culture.gouv.fr/siv/UD/FRAN_IR_056373/c-2xp1c58jd-1w19uv1g8sus6  /  https://gitlab.huma-num.fr/lamop/htr/-/tree/master/Livre_Rouge_Y__3
- e-NDP: LL 108a (27 images) + LL 125 (20) : https://e-ndp-beta.lamop.fr/public/E-NdP/temp/JPEG/
         
         + 1º group : volumes LL 106b-126 (64 images)
         + 2º group : volumes LL 106b-126 (82 images) 
         = 193 images (TRAIN)

         + 22 images (semi-external TEST) : LL 109A, 111, 115, 116, 121, 123-124 (volumes not used on training)

Total : 
- TRAIN: 550 images --> 450 folios -> 900 pages
- TEST: 22 images --> 22 pages

### External test dataset
- Bnf Latin 1770 (687 images): https://gallica.bnf.fr/ark:/12148/btv1b525092040
- Cartulary of Charles II of Navarre (Navarre_Pau_AD_E513, 209 images) : http://earchives.le64.fr/archives-en-ligne/ark:/81221/r13615z7dvnv8k/f1 / https://doi.org/10.5281/zenodo.5600884

## Multilingual 

- Odart de Morchesne : 274 actes --> 94 lat + 180 fro (35%-40% lat)
- Clairmarais : 178 actes ---> 176 lat + 2 fro (99% lat)
- Livre Rouge (35% - 40% latin)
- e-ndp (almost all in latin)
- Total: ± 72% latin / 28% french


## Model versions

Training HTR versions using varied data:

- 19/10/2021: V1 core --> Formulaire Odart de Morchesne + Cartulaire de Clairmarais + Livre Rouge + LL 108a (e-dnp_V1) : 
- 16/11/2021: V2 core --> V1 core + 84 pages (1º e-ndp transcription group)
- 11/01/2022: V3 core --> V1 core + V2 core + 82 pages (2º e-ndp transcription group)


- **val_acc** = accuracy on validation set _during_ training
- **test_acc** = accuracy on corpus test _after_ training
- **cer** = test character error rate
- **wer** = test word error rate

| model_name | Content | arch |val_acc | test_acc |cer | wer | logs |
| ------ | ------ |------ |------ |------ |------ |------ |------ |
| V1_test | Morchesne, Clairmarais, Livre Rouge, 108a |arch_1 | 92.50% | 69.75% |34.88% |71.50% |[log_1](https://gitlab.com/magistermilitum/e-ndp_htr/-/raw/main/Logs/endp_V1_evaluation) |
| V2_test | V1 core, +LL115 (20 pages), +1º e-ndp group |arch_1 | 94.71% | 83.57% |18.97% | 48.23% |[log_2](https://gitlab.com/magistermilitum/e-ndp_htr/-/raw/main/Logs/endp_V2_evaluation) |
| V3_test | V1 core, V2 core, +2º e-ndp group |arch_1 |93.90% |86.92% |13.25% |36.24% | [log_3](https://gitlab.com/magistermilitum/e-ndp_htr/-/raw/main/Logs/endp_V3_evaluation) |
| V3b_test | Only e-ndp transcriptions (193 images) |arch_1| 91.19% |81.90% |18.44% |46.94% |[log_4](https://gitlab.com/magistermilitum/e-ndp_htr/-/raw/main/Logs/endp_V3b_evaluation) |
||[all G1 test metrics](https://magistermilitum.gitlab.io/e-ndp_htr/)|
| V3_Latin_17740 | V3 tested on Latin 17740 manuscrit |arch_1| - |89.25% |11.21% |30.28% |[log_5](https://gitlab.com/magistermilitum/e-ndp_htr/-/raw/main/Logs/V3_latin_17740_evaluation) |
| V3b_Latin_17740 | V3b tested on Latin 17740 manuscrit |arch_1| - |82.59% |18.78% |48.17% |[log_6](https://gitlab.com/magistermilitum/e-ndp_htr/-/raw/main/Logs/V3b_latin_17740_evaluation) |
| V3_Navarre | V3 tested on Charles II of Navarre manuscrit |arch_1| - |82.82% |14.36% |44.42% |[log_7](https://gitlab.com/magistermilitum/e-ndp_htr/-/raw/main/Logs/V3_Navarre_evaluation) |
| V3b_Navarre | V3b tested on Charles II of Navarre manuscrit |arch_1| - |67.81% |29.02% |69.80% |[log_8](https://gitlab.com/magistermilitum/e-ndp_htr/-/raw/main/Logs/V3b_Navarre_evaluation) |

