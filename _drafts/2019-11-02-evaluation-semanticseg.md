---
layout: post
title: Tutorial - Métricas de Avaliação Para Segmentação Semântica
subtitle: Como avaliar a qualidade da rede?  
tags: [AI, rms, Segmentação Semântica, Métricas de avaliação]
---


Os modelos de machine/deep learning em geral são avaliados com base em quatro métricas: positivos verdadeiros, 
falso positivos, negativos verdadeiros e falso negativos. Porém, em problemas de segmentação semântica estas métricas não
são muito utilizadas, uma vez que não refletem a performance da rede. Assim, existem algumas métricas mais utilizadas na 
literatura tais como: 

- Pixel Accuracy

- Jaccard Index (Intersection-Over-Union(IoU))

- Dice Coefficient (F1 Score)


1. Pixel Accuracy

A acurácia de pixel indica o percentual de pixels classificados corretamente pelo modelo. Esta métrica, apesar de parecer
coerente para avaliar um problema de classificação semântica não é a melhor métrica. O Exemplo abaixo explica o motivo:

Imagem

Veja que o modelo apresentou um valor de 95% para a acurácia de pixel, porém, ao analisar o resultado, a única classe representada
é o background. Esta predição modelo não possui valor algum, uma vez que o background não é o objeto em que estamos interessados.
Assim, esta métrica não é valida para nosso modelo, além disso, ela evidência uma das grande dificuldade durante o treinamento
de redes de segmetação: Lidar com o desbalanço das classes. Por isso, métricas alternativas como IoU e dice coefficient foram
desenvolvidas.

2. Dice Coefficient





