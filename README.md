# Section 1 : Handwritten text recognition for e-NDP



e-NDP_HTR training experiments to fit transcribed pages from AN, LL105-126 registers into Kraken HTR core.


## Definitions

Stats for each register volume (LL105 - LL126, 26 volumes)


|      Volume     |    Dates   |  Type | N.Pages |
|:---------------:|:----------:|:-----:|:-------:|
|       105       | 1326-1352. | Train |    21   |
|       106A      | 1356-1361. | Train |    20   |
|       106B      | 1362-1364. | Train |    11   |
|       107       | 1367-1370. | Train |    10   |
|       108A      | 1392-1394. | Train |    27   |
|       108B      | 1397-1399. | Train |    14   |
|       109A      | 1399-1401. |  Test |    4    |
|       109B      | 1401-1405. | Train |    10   |
|       109C      | 1405-1407. | Train |    12   |
|       110       | 1407-1411. | Train |    17   |
|       111       | 1412-1414. |  Test |    3    |
|       112       | 1414-1424. | Train |    17   |
|       113       | 1425-1432. | Train |    17   |
|       114       |  1433-1437 | Train |    11   |
|       115       | 1440-1444. |  Test |    4    |
|       116       | 1445-1459. |  Test |    4    |
|       117       | 1450-1454. | Train |    60   |
|  118 (minutes)  | 1453-1456. | Train |    13   |
|       119       | 1456-1460. | Train |    18   |
|       120       | 1460-1465. | Train |     3   |
|       121       | 1465-1474. |  Test |     3   |
|       122       | 1474-1481. | Train |    21   |
|     123-124     | 1481-1489. |  Test |    4    |
|       125       |  1489-1493 | Train |   20+16 |
|       126       |  1493-1497 | Train |    18   |
| Bnf Latin 17740 |  1430-1444 |  Test |   687   |

references:
[Projet e-NDP – Notre-Dame de Paris et son cloître : archives des séances du séminaire](https://lamop.hypotheses.org/files/2020/11/e-NDP_seance1_20201020-compresse-1.pdf)



[SÉRIE LL MONUMENTS ECCLÉSIASTIQUES REGISTRES](http://www.archivesnationales.culture.gouv.fr/chan/chan/fonds/EGF/SA/InvSAPDF/Ll.pdf)

# Architecture

### Architecure 1: 


![kraken architecture 1](https://gitlab.com/magistermilitum/home_alcar_kraken/-/raw/main/images/kraken_arch_1.drawio.png)

hyper_params': {'pad': 24, 'freq': 1.0, 'batch_size': 1, 'lag': 5, 'min_delta': None, 'optimizer': 'Adam', 'lrate': 0.0002, 'momentum': 0.9, 'weight_decay': 0, 'schedule': 'reduceonplateau', 'normalization': None, 'normalize_whitespace': True, 'augment': False, 'step_size': 10, 'gamma': 0.1, 'rop_patience': 3, 'cos_t_max': 50}}

> Trained on Kraken (https://github.com/mittagessen/kraken). Training command: 
> 
> kraken 3.0 : ketos train -N 70 -q dumb -f page --threads 32 -r 0.0001 --schedule reduceonplateau --sched-patience 3 -d cuda:0 --preload --pad 24 -s '[1,128,0,1 Cr4,16,32 Do0.1,2 Mp2,2 Cr4,16,32 Do0.1,2 Mp2,2 Cr3,8,64 Do0.1,2 Mp2,2 Cr3,8,64 Do0.1,2 S1(1x0)1,3 Lbx256 Do0.3,2 Lbx256 Do0.3,2 Lbx256 Do0.3]' --augment training_folder/*.xml
>
> kraken 4.0 : ketos train -N 70 -q dumb -f page --workers 32 -r 0.0001 --schedule reduceonplateau --sched-patience 3 -d cuda:0 --pad 24 -s '[1,128,0,1 Cr4,16,32 Do0.1,2 Mp2,2 Cr4,16,32 Do0.1,2 Mp2,2 Cr3,8,64 Do0.1,2 Mp2,2 Cr3,8,64 Do0.1,2 S1(1x0)1,3 Lbx256 Do0.3,2 Lbx256 Do0.3,2 Lbx256 Do0.3]' --augment  training_folder/*.xml


### Architecure 2:

# Training


training board accuracy on validation set

<p float="left">
<img src="https://gitlab.com/magistermilitum/e-ndp_htr/-/raw/main/images_train/endp_models.jpg" width="546" height="400"> 
</p>





# HTR Experiments

## Training and testing data-sets 

- F. Odart de Morchesne:  189 images (1427, 378 pages): https://gallica.bnf.fr/ark:/12148/btv1b9059518w.image  /  http://elec.enc.sorbonne.fr/morchesne/html/morchesne.html
- Cart. ND de Clairmarais :  134 images (1220 - 1460, 268 pages) : https://bvmm.irht.cnrs.fr/mirador/index.php?manifest=https://bvmm.irht.cnrs.fr/iiif/32117/manifest  /  https://doi.org/10.5281/zenodo.5600884
- Livre Rouge ( Châtelet de Paris. Y//3 ) :  34 images (1223 - 1474) : https://www.siv.archives-nationales.culture.gouv.fr/siv/UD/FRAN_IR_056373/c-2xp1c58jd-1w19uv1g8sus6  /  https://gitlab.huma-num.fr/lamop/htr/-/tree/master/Livre_Rouge_Y__3
- e-NDP: LL 108a (27 images) + LL 125 (20) : https://e-ndp-beta.lamop.fr/public/E-NdP/temp/JPEG/
         
         + 1º group : volumes LL 106b-126 (64 images)
         + 2º group : volumes LL 106b-126 (82 images) 
         + 3º group : volumes LL 106b-126 (76 images) 
         + 4º group : volumes LL 106b-126 (78 images) 
         + 5º group : volumes LL 105, 106A, 111, 107, 118, 119, 120, 123-124, 127-128 (82 pages)
         = 429 images (TRAIN)

         + 28 images (semi-external TEST) : LL 109A, 111, 115, 116, 121, 123-124 (volumes not used in training)

Total : 
- TRAIN: 786 images --> 554 folios -> 1109 pages
- TEST: 28 images --> 28 pages

### External test dataset
- Bnf Latin 17740 (1430 - 1444, 687 images): https://gallica.bnf.fr/ark:/12148/btv1b525092040
- Cartulary of Charles II of Navarre (Navarre_Pau_AD_E513, 1297 - 1372, 209 images) : http://earchives.le64.fr/archives-en-ligne/ark:/81221/r13615z7dvnv8k/f1 / https://doi.org/10.5281/zenodo.5600884

## Multilingual 

- Odart de Morchesne : 274 formules --> 94 lat + 180 fro (35%-40% lat)
- Clairmarais : 178 actes ---> 168 lat + 10 fro (92% lat)
- Livre Rouge (35% - 40% latin)
- e-ndp (almost all in latin)
- Total: ± 77% latin / 23% french


## Model versions

Training HTR versions using varied data:

- 19/10/2021: V1 core --> Formulaire Odart de Morchesne + Cartulaire de Clairmarais + Livre Rouge + LL 108a (e-dnp_V1) : 
- 16/11/2021: V2 core --> V1 core + 84 pages (1º e-ndp transcription group)
- 11/01/2022: V3 core --> V1 core + V2 core + 82 pages (2º e-ndp transcription group)
- 10/02/2022: V4 core --> V1 core + V2 core + V3 core + 76 pages (3º e-ndp transcription group)
- 30/06/2022: V5 core ---> all V4 core + 78 pages (4º e-ndp transcription group)
- 17/07/2022: V6 core ---> all V5 core + 40 pages coming for new digitized volumes: 105, 106A, 111, 107, 118, 119, 120, 123-124, 127-128)
- 16/08/2022: V7 core ---> all V6 core + 42 pages coming for new digitized volumes: 105, 106A, 111, 107, 118, 119, 120, 123-124, 127-128)


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
| V4_test | V1 core, V2 core, V3 core +3º e-ndp group |arch_1 |93.52% |88.55% |11.43% |32.47% | [log_5](https://gitlab.com/magistermilitum/e-ndp_htr/-/raw/main/Logs/endp_V4_evaluation) |
| V5_test | V1, V2, V4 cores + 4º e-ndp group |arch_1 |94.48% |90.31% |9.73% |27.61% | [log_6](https://gitlab.com/magistermilitum/e-ndp_htr/-/blob/main/Logs/endp_V4_2_evaluation) |
||[all G1 test metrics](https://magistermilitum.gitlab.io/e-ndp_htr/)|
| V3_Latin_17740 | V3 tested on Latin 17740 manuscrit |arch_1| - |89.25% |11.21% |30.28% |[log_7](https://gitlab.com/magistermilitum/e-ndp_htr/-/raw/main/Logs/V3_latin_17740_evaluation) |
| V3b_Latin_17740 | V3b tested on Latin 17740 manuscrit |arch_1| - |82.59% |18.78% |48.17% |[log_8](https://gitlab.com/magistermilitum/e-ndp_htr/-/raw/main/Logs/V3b_latin_17740_evaluation) |
| V7_Latin_17740 | V7 tested on Latin 17740 manuscrit |arch_1| - |91.52% |9.27% |28.28% |[log_9](https://github.com/chartes/e-NDP_HTR/raw/main/Logs/V7_latin_17740_evaluation) |
| V3_Navarre | V3 tested on Charles II of Navarre manuscrit |arch_1| - |82.82% |14.36% |44.42% |[log_10](https://gitlab.com/magistermilitum/e-ndp_htr/-/raw/main/Logs/V3_Navarre_evaluation) |
| V3b_Navarre | V3b tested on Charles II of Navarre manuscrit |arch_1| - |67.81% |29.02% |69.80% |[log_11](https://gitlab.com/magistermilitum/e-ndp_htr/-/raw/main/Logs/V3b_Navarre_evaluation) |
| V7_Navarre | V7 tested on Charles II of Navarre manuscrit |arch_1| - |85.42% |12.52% |37.78% |[log_12](https://github.com/chartes/e-NDP_HTR/raw/main/Logs/V7_Navarre_evaluation) |



# Section 2: Layout Segmentation

Layout segmentation is a compulsory step before HTR recognition in order to distinguish sections inside a document. This process intend to separate interdependant page zones to produce a recognition in a section-sequence order and not in a line-sequence order which mix textual and peri-textual content.

For e-NDP we contemplate 5 sections to englobe the page distribution in all the 26 volumes: 
1. **Block** : All the central text blocks, that normally corresponds to the main content called "conclusions" in registers.
2. **Liste** : List of names of the canons who were present during the meeting. Normally located before the "conclusions".
3. **Entrée** : Marginal notes or entries to inform about the content of "conclusions".
4. **Date** : Paragraph contending the date. Normally at the head of a "conclusion", but separate of the main body.
5. **Numérotation** : Page numbers in roman or arabic. Usually appear in the corners of the pages.


<img align="center" src="https://gitlab.com/magistermilitum/e-NDP/-/raw/main/images/automatic_sections.jpg" width="600" height="455" />
<p align="center">Automatic layout segmentation in a e-NDP page. </p>

## Layout segmentation experiments

We annotate 376 transcribed pages from the e-NDP ground-truth (V1 core to V6 core) and we experiment using a classical CNN+BiLSTM architecture.

Order to replicate the training in Kraken 3: 
> ketos segtrain -f page -o seg_model -d cuda:0 -bl --threads 32 --epochs 50 --schedule reduceonplateau -s '[1,1200,0,3 Cr7,7,64,2,2 Gn32 Cr3,3,128,2,2 Gn32 Cr3,3,128 Gn32 Cr3,3,256 Gn32 Cr3,3,256 Gn32 Lbx32 Lby32 Cr1,1,32 Gn32 Lby32 Lbx32]' training_folder/*xml

Another training option is the fine-tuning on the default blla.mlmodel (https://github.com/mittagessen/kraken/blob/master/kraken/blla.mlmodel):
> ketos segtrain -i blla.mlmodel -f page -o seg_model -d cuda:0 -bl --threads 32  --resize add --epochs 50 --schedule reduceonplateau -s '[1,1200,0,3 Cr7,7,64,2,2 Gn32 Cr3,3,128,2,2 Gn32 Cr3,3,128 Gn32 Cr3,3,256 Gn32 Cr3,3,256 Gn32 Lbx32 Lby32 Cr1,1,32 Gn32 Lby32 Lbx32]' training_folder/*xml

## Layout segmentation model versions


- **mean_iu** = Mean intersection over union (IU)
- **freq_iu** = Frequency intersection over union (IU)
- **mean_acc** = Mean accuracy (average of the prediction accuracy over all categories)
- **IU** is the overlap ratio between the candidate bound and the ground truth bound.

| model_name | Content | mean_iu | freq_iu |mean_acc |
| ------ | ------ |------ |------ |------ |
| V1_layout | endp V1-V2 cores | 0.6508| 0.7918 |0.9552 |
| V2_layout | endp V1-V4 cores  | 0.6744 | 0.8366 |0.9648 |
| V3_layout | endp V1-V6 cores  | 0.6936 | 0.8455 |0.9673 |


references:
[Long, J., Shelhamer, E., & Darrell, T. (2015). Fully convolutional networks for semantic segmentation. In Proceedings of the IEEE conference on computer vision and pattern recognition (pp. 3431-3440).](https://openaccess.thecvf.com/content_cvpr_2015/papers/Long_Fully_Convolutional_Networks_2015_CVPR_paper.pdf)
