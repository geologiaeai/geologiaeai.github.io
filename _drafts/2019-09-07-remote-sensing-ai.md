---
layout: post
title: Bases de dados de Sensoriamento Remoto Disponíveis para o uso em Machine e Deep Learning
subtitle: Dados disponíveis e como utilizá-los  
tags: [AI, Datasets,]
---

O aprendizado de máquina, em especial o deep learning, é uma ferramenta com grande potêncial para auxiliar na resolução de problemas de sensoriamento remoto (SR) relacionados à classificação de imagens, segmentação de instâncias e semântica, detecção de objetos e classificação de cenas. Porém, em geral, os resultados só são bons quando se utilizam grandes bancos de dados para o treinamento end-to-end das redes. Assim, com intuito de auxiliar e facilitar a busca por datasets de SR para serem utilizados em deep learning, elaborei esta lista que apresenta as principais fontes de imagens de sensoriamento remoto.

### Classificação de Cenas ###

| Nome | Fonte | Descrição | Resolução Espacial | 
| ------------- | ------------- | --------------------------- | ------------- |
| [UC Merced Land Use Dataset](http://weegee.vision.ucmerced.edu/datasets/landuse.html) |[Yang & Newsam 2010](https://www.researchgate.net/publication/221589425_Bag-of-visual-words_and_spatial_extensions_for_land-use_classification)  | 21 classes de uso/ocupação do solo com 100 imagens (256x256px) por categoria. | 0.30m | 
| [SAT-4 and SAT-6 airborne datasets](https://csc.lsu.edu/~saikat/deepsat/) | [Basu et al. 2015](https://arxiv.org/abs/1509.03602)  | 6 classes de uso/ocupação do solo com 400 mil imagens (28x28px) por categoria com 4 bandas (RGBNIR) extraídas de *National Agriculture Imagery Program (NAIP) em 2009*. | 1.0m | 
| [Brazilian Cerrado-Savanna Scenes Dataset](http://www.patreo.dcc.ufmg.br/2017/11/12/brazilian-cerrado-savanna-scenes-dataset/) | [K.Nogueira et al. 2016](https://homepages.dcc.ufmg.br/~keiller.nogueira/pdf/prrs2016.pdf)  | 4 classes de vegetação presente na região da Serra do Cipó, MG com 1311 imagens (64x64px) por categoria com 3 bandas (RGNIR) obtidas pelo sensor RapidEye. | 5.0m |
| [EuroSat](http://madm.dfki.de/downloads) | [Helber et al. 2017](https://arxiv.org/abs/1709.00029)  | 10 classes de uso/ocupação do solo com 27 mil imagens (64x64px) com 3 ou 16 bandas do satélite Sentinel 2. | 10m |
| [Brazilian Coffee Scene Dataset](http://www.patreo.dcc.ufmg.br/2017/11/12/brazilian-coffee-scenes-dataset/) | [O. A. B. Penatti et al. 2017](https://www.cv-foundation.org/openaccess/content_cvpr_workshops_2015/W13/papers/Penatti_Do_Deep_Features_2015_CVPR_paper.pdf)  | 2876 imagens (64x64px) de platações de café no estado de Minas Gerais obtidas pelo sensor SPOT em 2005. | 10m |
| [NWPU-RESISC45 dataset](http://www.escience.cn/people/JunweiHan/NWPU-RESISC45.html) | [ Cheng et al. 2017](https://arxiv.org/abs/1703.00121)  |  45 classes de cenas com 700 imagens em cada classe, somando um total de  31,500 imagens (256x256px) que foram retiradas do Google Earth.| - |
| [So2Sat LCZ42](https://dataserv.ub.tum.de/index.php/s/m1454690) | [Zhu et al. 2018](https://mediatum.ub.tum.de/1454690)  |Imagens de SAR e sensores multiespectrais correspondendo as zonas climáticas locais distribuidas por 42 cidades de diferentes países, obtidas pelos satélites Sentinel-1 e 2. | - |
| [Functional Map of the World Challenge](https://github.com/fMoW/dataset) | [Christie et al. 2018](https://arxiv.org/abs/1711.07846)  |  63 classes que somam mais de 1 milhão de imagens (32x32px). Existem 2 dataset, *fMoW-full* e *fMoW-rgb*. *fMoW-full* está no formato TIFF e contêm imagens multiespectrais de 4 e 8 bandas (3.5 TB de dados) *fMoW-rgb* está em JPEG format, todas as bandas foram convertidas para RGB (200gb). | 0.3m |
| [AID dataset](https://captain-whu.github.io/AID/) | [Xia et al. 2017](https://ieeexplore.ieee.org/document/7907303)  |30 classes com 10000 imagens (600x600px) por classe. As imagens foram obtidas do Google Earth. | - |
| [WHU-RS19](http://www.xinhua-fluid.com/people/yangwen/WHU-RS19.html) | [Yen](http://www.xinhua-fluid.com/people/yangwen/WHU-RS19.html)  |19 classes com 50 imagens (600x600) em cada classe.| - |
| [RSI-CB](https://github.com/lehaifeng/RSI-CB) | [Haifeng et al. 2017](https://arxiv.org/abs/1705.10450)  | Existem dois datasets - O Primeiro possui 6 categorias com 35 subclasses e mais de 24000 imagens (256x256px) por classe. O segundo possui 6 categorias com 45 subclasses com mais de 36000 imagens (128x128px) por classe.| - |
| [SIRI-WHU](http://www.lmars.whu.edu.cn/prof_web/zhongyanfei/Num/Google.html) | [Zhao et al. 2016](https://pdfs.semanticscholar.org/bcba/ed61bdc02707a0c69a421ad5744980c6dbd2.pdf)  | 12 classes com mais de 200 imagens (200x200px) por classe. As imagens foram obtidas do Google Earth.| 2m |
| [BigEarthNet: Large-Scale Sentinel-2 Benchmark](http://bigearth.net/) | [Sumbul et al. 2019](https://arxiv.org/abs/1902.06148)  | Diversas classes baseadas no *CORINE Land Cover Database (CLC 2018)* que somam 590326 imagens do Satélite Sentinel-2 | - |
| [**Competição**- Planet: Understanding the Amazon from Space](https://www.kaggle.com/c/planet-understanding-the-amazon-from-space) | Planet, 2017  | 13 classes de uso/ocupação do solo, 4 classes de condições de nuvens. As imagens possuem 4 bandas (RGB-NIR) e recobrem a Bacia Amazônica. | 5m |
| [**Competição**- Statoil/C-CORE Iceberg Classifier Challenge](https://www.kaggle.com/c/statoil-iceberg-classifier-challenge) | Statoil/C-CORE, 2018 | 2 clsses (barcos e icebergs) com imagens (75X75px) de 2 bandas (HH/HV) polarizadas SAR | 5m |
| [**Competição**- WiDS Datathon 2019 : Detection of Oil Palm Plantations](https://www.kaggle.com/c/widsdatathon2019) | Global WiDS Team & West Big Data Innovation Hub, 2019 | 2 classes de imagens (256x256px) que representam platações de óleo de palma que somam mais de 20000 imagens | 3m |


### Segmentação Semântica ###

| Nome | Fonte | Descrição | Resolução Espacial | 
| ------------- | ------------- | --------------------------- | ------------- |
| [ISPRS Potsdam 2D Semantic Labeling Contest ](http://www2.isprs.org/commissions/comm3/wg4/2d-sem-label-potsdam.html) |ISPRS| 6 classes com rasters de máscaras anotadas destas classes. As imagems apresentam 4 bandas (RGB-IR) | 0.05m | 
| Nome | Fonte | Descrição | Resolução Espacial | 
| [Inria Aerial Image Labeling Dataset](https://project.inria.fr/aerialimagelabeling/contest/) |inria.fr| Máscaras e imagens RGB (5000x5000px) que representam construções em 5 diferentes cidades.) | 0.3m | 



