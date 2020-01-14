---
layout: post
title: Tutorial - Métricas de Avaliação Para Segmentação Semântica
subtitle: Como avaliar a qualidade da rede?  
tags: [AI, rms, Segmentação Semântica, Métricas de avaliação]
---


Os modelos de machine/deep learning em geral são avaliados com base em quatro métricas: positivos verdadeiros, 
falso positivos, negativos verdadeiros e falso negativos. Porém, em problemas de segmentação semântica estas métricas por si só não refletem a performance do modelo. Assim, este post tem como objetivo explorar as três métricas abaixo que são comumente utilizadas em problemas de segmentação semântica:

- Pixel Accuracy

- Jaccard Index (Intersection-Over-Union(IoU))

- Dice Coefficient (F1 Score)


1. Acurácia de Pixel (Pixel Accuracy)

A acurácia de pixel indica o percentual de pixels classificados corretamente pelo modelo. Esta métrica, apesar de parecer
coerente para avaliar um problema de classificação semântica não é a melhor métrica. O Exemplo abaixo explica o motivo:

Imagem ( RGB , Mascara e resultado)

Veja que o modelo apresenta um elevado valor (95%) para a acurácia de pixel. Porém, ao analisar o resultado, a única classe representada é o background. Esta predição modelo não possui valor algum, uma vez que o background não é o objeto em que estamos interessados. Esta métrica não é adequada pois comumente os problemas de segmentação semântica apresentam desbalanceamentos de classe. Assim, a classe correspondente ao background apresenta muito mais pixels nas imagens de treinamento em relação a classe de interesse. Assim, esta métrica não avalia corretamente o modelo, por isso, métricas alternativas como IoU e dice coefficient são mais utilizadas.

2. IoU

O índice de jaccard, também conhecido como IoU, é a métrica mais utilizada em problemas de segmentação semântica, uma vez que seu cálculo é simples e efetivo. A imagem abaixo explica como é calculado o índice. Veja que a divisão da área de intersecção pela área de união tem como resultado uma valor de 0 a 1 onde o 0 representa nenhuma sobreposição e 1 um sobreposição perfeita, isto é, a máscara detectada pela rede é igual ao valor anotado.

Imagem IoU

Em problemas de segmentação binária calcula-se o IoU de cada classe e depois é retirado a média deste valor. Em geral os artigos expressam este índice como mIoU. 


image explicando o mIoU

3. Dice Coefficient

O dice coefficent é calculado multiplicando a área de sobreposição por 2 e a dividindo pelo total pixels das imagens.





