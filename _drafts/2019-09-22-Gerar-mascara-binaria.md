---
layout: post
title: Como Gerar Máscaras (Raster) Binário com Python
subtitle: Convertendo Shapefile e GeoJson Em Raster Binário 
tags: [AI, Python, rms, Sensoriamento Remoto, Máscara Binária]
---

Em problemas de segmentação semântica, é comum o uso de rasteres binários, isto é, imagens que possuem o valor de 0 para 
o background e 1 para a classe de interesse, no treinamento das redes neurais. Estas máscaras servem como o output que se 
espera da rede em treinamento. Neste tutorial, a classe de interesse são cicatrizes de deslizamento de terra retiradas de 
imagens do Satélite RapidEye.

### Workflow

- 0. Importar as Bibliotecas;
- 1. Carregar a Imagem .tif;
- 2. Carregar o Shapefile ou GeoJson;
- 3. Verificar se os Sistemas de Cordenadas de Referência (CRS) são os mesmos;
- 4. Gerar a Máscara Binária;
- 5. Salvar;
- 5. Definir uma Função que gera máscaras binárias.

#### 0. Importar as bibliotecas

``` python

import os

import rasterio
from rasterio.plot import reshape_as_image
import rasterio.mask
from rasterio.features import rasterize

import pandas as pd
import geopandas as gpd
from shapely.geometry import mapping, Point, Polygon
from shapely.ops import cascaded_union

import numpy as np
import cv2
import matplotlib.pyplot as plt

```

#### 1. Carregar a Imagem .tif

Para carregar a imagem .tif defina a variável raster_path como o caminho para a imagem sobre a qual será gerada a máscara binária.

``` python

raster_path = "/Users/...../imagem.tif"
with rasterio.open(raster_path, "r") as src:
    raster_img = src.read()
    raster_meta = src.meta

```

#### 2. Carregar o Shapefile ou GeoJson .tif


``` python

shape_path = /Users/.../poligono.geojson
train_df = gpd.read_file(shape_path)

```

#### 3. Verificar se os Sistemas de Cordenadas de Referência (CRS) são os mesmos;


Caso os Valores sejam diferentes, é necessário reprojetar o Vetor (Shapefile ou GeoJson) ou a imagem (raster). Para isso, pode-se utilizar o software QGIS.  

```python
 
 print("CRS do Raster: {}, CRS do Vetor {}".format(train_df.crs, src.crs))

```

#### 4. Gerar a Máscara Binária;





