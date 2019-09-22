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

- 0 Importar as Bibliotecas;
- 1 Carregar a Imagem .tif;
- 2 Carregar o Shapefile ou GeoJson;
- 3 Verificar se os Sistemas de Cordenadas de Referência (CRS) são os mesmos;
- 4 Gerar a Máscara Binária;
- 5 Salvar;
- 6 Definir uma Função que gera máscaras binárias.

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

```python
 
 #Função que 
 def poly_from_utm(polygon, transform):
    poly_pts = []
    
    # Gerar um polígono a partir de multipolígonos.
    poly = cascaded_union(polygon)
    for i in np.array(poly.exterior.coords):
        
        # Transformar os polígonos para o CRS da imagem, utilizando os metadados do raster.
        poly_pts.append(~transform * tuple(i))
        
    # Gerar um objeto de polígono
    new_poly = Polygon(poly_pts)
    return new_poly

# Gerar a máscara binária

poly_shp = []
im_size = (src.meta['height'], src.meta['width'])
for num, row in train_df.iterrows():
    if row['geometry'].geom_type == 'Polygon':
        poly = poly_from_utm(row['geometry'], src.meta['transform'])
        poly_shp.append(poly)
    else:
        for p in row['geometry']:
            poly = poly_from_utm(p, src.meta['transform'])
            poly_shp.append(poly)

mask = rasterize(shapes=poly_shp,
                 out_shape=im_size)

# Plotar a máscara gerada

plt.figure(figsize=(15,15))
plt.imshow(mask)

```





