# e-NDP_HTR



HOME-ALCAR training experiments to fit aligned cartularies from HOME-ALCAR database into Kraken HTR core.

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
<img src="https://gitlab.com/magistermilitum/home_alcar_kraken/-/raw/main/images/val_acc_G1.jpg" width="546" height="400"> <img src="https://gitlab.com/magistermilitum/home_alcar_kraken/-/raw/main/images/val_acc_G2.jpg" width="546" height="400">
</p>





# Experiments

Testing Group 1 (Cursiva) against Nesle cartulary unseen during training (124 images, 228591 characters)

Testing Group 2 (Textualis) against Chartres_2 cartulary unseen during training (144 images, 271398 characters)


- **val_acc** = accuracy on validation set during training
- **test_acc** = accuracy on corpus test (Nesle or Chartres_2) after training
- **cer** = test character error rate
- **wer** = test word error rate

| model_name | Content | arch |val_acc | test_acc |cer | wer | logs |
| ------ | ------ |------ |------ |------ |------ |------ |------ |
| group1_test_1 | Navarre, ND_Roche, Clairmarais |arch_1 | 92.50% | 75.99% |22.68% |58.79% |[log_1](https://gitlab.com/magistermilitum/home_alcar_kraken/-/blob/main/logs/G1_test_1_evaluation) |
| group1_test_2 | Navarre, ND_Roche, Clairmarais, +Morchesne, |arch_1 | 93.47% | 76.33% |22.52% | 58.21% |[log_2](https://gitlab.com/magistermilitum/home_alcar_kraken/-/blob/main/logs/G1_test_2_evaluation) |
| group1_test_3 | Navarre, ND_Roche, Clairmarais, Sommereux |arch_1 |93.43% |77.32% |20.46% |54.34% | [log_3](https://gitlab.com/magistermilitum/home_alcar_kraken/-/blob/main/logs/G1_test_3_evaluation) |
| group1_test_4 | Navarre, ND_Roche, Clairmarais, +Morchesne, Sommereux, +10 pages Nesle (fine-tuning) |arch_1| 93.19% |86.74% |12.92% |37.01% |[log_4](https://gitlab.com/magistermilitum/home_alcar_kraken/-/blob/main/logs/G1_test_4_evaluation) |
| group1_test_5 | Navarre, ND_Roche, Clairmarais, +Morchesne, + e-NDP | arch_1 |94.22%| 82.24% |17.38% |48.61% |[log_5](https://gitlab.com/magistermilitum/home_alcar_kraken/-/blob/main/logs/G1_test_5_evaluation) |
||[all G1 test metrics](https://magistermilitum.gitlab.io/e-ndp_htr/)|
| group2_test_1 | S_Denis, Fervaques, S_Nicaise, Port_Royal 1-2 |arch_1| 91.90% |87.39% |10.81% |29.20%  |[log_6](https://gitlab.com/magistermilitum/home_alcar_kraken/-/blob/main/logs/G2_test_2b_evaluation) |
| group2_test_2 | S_Denis, Fervaques, S_Nicaise, Port_Royal 1-2, Vauluisant  |arch_1| 90.90% |88.17% |10.04% |26.63%  |[log_7](https://gitlab.com/magistermilitum/home_alcar_kraken/-/blob/main/logs/G2_test_1b_evaluation) |
| group2_test_3 | S_Denis, Fervaques, S_Nicaise, Port_Royal 1-2, Vauluisant, Molesme_1  |arch_1| 92.76% |88.66% |9.64% |25.20%  |[log_8](https://gitlab.com/magistermilitum/home_alcar_kraken/-/blob/main/logs/G2_test_3b_evaluation) |
| group2_test_3 | S_Denis, Fervaques, S_Nicaise, Port_Royal 1-2, Vauluisant, +10 pages Chartres_2 (fine-tuning)  |arch_1| 90.80% |89.20% |9.33% |24.83%  |[log_9] |
||[all G2 test metrics](https://gitlab.com/magistermilitum/home_alcar_kraken/-/blob/main/logs/G2_all_test_metrics.txt)|


