---
layout: post
title: Tutorial - Métricas de Avaliação Para Segmentação Semântica
subtitle: Como avaliar a qualidade da rede?  
tags: [AI, rms, Segmentação Semântica, Métricas de avaliação]
---


Os modelos de machine/deep learning em geral são avaliados com base em quatro métricas: positivos verdadeiros, 
falso positivos, negativos verdadeiros e falso negativos. Porém, em problemas de segmentação semântica estas métricas por si só não refletem a performance do modelo. Assim, este post tem como objetivo explorar as quatro métricas abaixo que são comumente utilizadas em problemas de segmentação:

- Pixel Accuracy

- Jaccard Index (Intersection-Over-Union(IoU))

- Dice Coefficient (F1 Score)


1. Pixel Accuracy

A acurácia de pixel indica o percentual de pixels classificados corretamente pelo modelo. Esta métrica, apesar de parecer
coerente para avaliar um problema de classificação semântica não é a melhor métrica. O Exemplo abaixo explica o motivo:

Imagem

Veja que o modelo apresentou um valor de 95% para a acurácia de pixel, porém, ao analisar o resultado, a única classe representadaé o background. Esta predição modelo não possui valor algum, uma vez que o background não é o objeto em que estamos interessados. Assim, esta métrica não é valida para nosso modelo, além disso, ela evidência uma das grande dificuldade durante o treinamentode redes de segmetação: Lidar com o desbalanço das classes. Por isso, métricas alternativas como IoU e dice coefficient são mais utilizadas.

2. IoU

O índice de jaccard, também conhecido como IoU, é a métrica mais utilizada em problemas de segmentação semântica, uma vez que seu cálculo é simples e efetivo. A imagem abaixo explica como é calculado o índice. Veja que a divisão da área de intersecção pela área de união tem como resultado uma valor de 0 a 1 onde o 0 representa nenhuma sobreposição e 1 um sobreposição perfeita, isto é, a máscara detectada pela rede é igual ao valor anotado.

Em problemas de segmentação binária calcula-se o IoU de cada classe e depois é retirado a média deste valor. Em geral os artigos expressam este índice como mIoU.

image

3. Dice Coefficient

O dice coefficent é calculado multiplicando a área de sobreposição por 2 e a dividindo pelo total pixels das imagens.





