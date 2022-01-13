# e-NDP_HTR



e-NDP_HTR training experiments to fit transcribed pages from AN, LL105-126 registers into Kraken HTR core.


# Definitions

Detected script types for each cartulary


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
[Kestemont, M., Christlein, V., & Stutzmann, D. (2017). Artificial Paleography: Computational Approaches to Identifying Script Types in Medieval Manuscripts. Speculum, 92(S1), S86-S109.](https://hal.archives-ouvertes.fr/hal-01854939/document)

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

         + 22 images (TEST) : LL 109A, 111, 115, 116, 121, 123-124
         + Bnf Latin 1770 (687): 

- Total : 
- TRAIN: 550 images -> 450 folios -> 900 pages
- TEST: 22 images -> 22 pages

## Multilingual 

- Odart de Morchesne : 274 actes --> 94 lat + 180 fro (35%-40% lat)
- Clairmarais : 178 actes ---> 176 lat + 2 fro (99% lat)
- Livre Rouge (35%-40% latin)
- e-ndp (almost all in latin)
- Total: ± 72% latin / 28% français


## Model versions

Training HTR versions using varied data:

- 19/10/2021: V1 core -> Formulaire Odart de Morchesne + Cartulaire de Clairmarais + Livre Rouge + LL 108a (e-dnp_V1) : 
- 16/11/2021: V2 core -> V1 core + 84 pages (1º e-ndp transcription group)
- 11/01/2022: V3 core -> V1 core + V2 core + 82 pages (2º e-ndp transcription group)


- **val_acc** = accuracy on validation set during training
- **test_acc** = accuracy on corpus test (Nesle or Chartres_2) after training
- **cer** = test character error rate
- **wer** = test word error rate

| model_name | Content | arch |val_acc | test_acc |cer | wer | logs |
| ------ | ------ |------ |------ |------ |------ |------ |------ |
| V1_test | Morchesne, Clairmarais, Livre Rouge, 108a |arch_1 | 92.50% | 69.79% |22.68% |58.79% |[log_1](https://gitlab.com/magistermilitum/e-ndp_htr/-/raw/main/Logs/endp_V1_evaluation) |
| V2_test | V1 core, +LL115 (20 pages), +1º e-ndp group |arch_1 | 93.47% | 83.33% |22.52% | 58.21% |[log_2](https://gitlab.com/magistermilitum/e-ndp_htr/-/raw/main/Logs/endp_V2_evaluation) |
| V3_test | V1 core, V2 core, +2º e-ndp group |arch_1 |93.43% |77.32% |20.46% |86.94% | [log_3](https://gitlab.com/magistermilitum/e-ndp_htr/-/raw/main/Logs/endp_V3_evaluation) |
| V3b_test | Only e-ndp transcriptions (193 images) |arch_1| 93.19% |86.74% |81.52% |37.01% |[log_4](hhttps://gitlab.com/magistermilitum/e-ndp_htr/-/raw/main/Logs/endp_V3b_evaluation) |
||[all G1 test metrics](https://magistermilitum.gitlab.io/e-ndp_htr/)|


