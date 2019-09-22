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

#### 2. Carregar o Shapefile ou GeoJson 

Para carregar o arquivo shapefile (.shp) ou GeoJson (.geojson) defina a variável shape_path com o local onde está localizado o arquivo.


``` python

shape_path = /Users/.../poligono.geojson
train_df = gpd.read_file(shape_path)

```

#### 3. Verificar se os Sistemas de Cordenadas de Referência (CRS) são os mesmos;


Caso os Valores sejam diferentes, é necessário reprojetar o Vetor (Shapefile ou GeoJson) ou a imagem (raster). Para isso, pode-se utilizar o software QGIS.  

```python
 
 print("CRS do Raster: {}, CRS do Vetor {}".format(train_df.crs, src.crs))

```

#### 4. Gerar a Máscara Binária

Para gerar a máscara binária basta rodar o código abaixo. Ao final, a máscara gerada será plotada.

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

#### 5. Salvar

Para salvar, deve-se converter o arquivo para 'uint16' e definir o local que será salvo o arquivo.

```python
mask = mask.astype("uint16")
arquivo_salvar = "/Users/.../mascaras/train.tif"
bin_mask_meta = src.meta.copy()
bin_mask_meta.update({'count': 1})
with rasterio.open(arquivo_salvar, 'w', **bin_mask_meta) as dst:
    dst.write(mask * 255, 1)

```

#### 6. Definir uma Função que gera máscaras binárias.

A função generate_mask(), tem como entrada os parâmentros abaixo:

**raster_path** = local onde a imagem .tif esta localizada;

**shape_path** = local onde o Shapefile ou GeoJson está localizado.

**output_path** = local onde será salva a máscara binária gerada.

**file_name** = nome do aquivo que será gerado.


```python

def generate_mask(raster_path, shape_path, output_path, file_name):
    
    """crs MUST be the same in order to generate the mask"""
    
    #load_raster
    #raster_path = folder where the image is located
    
    with rasterio.open(raster_path, "r") as src:
        raster_img = src.read()
        raster_meta = src.meta
    
    #load shapefile or GeoJson
    #shape_path = folder where the shapefile is located
    train_df = gpd.read_file(shape_path)
    
    if train_df.crs != src.crs:
        print(" Raster has crs: {} and Vector has crs: {}.\n Convert to the same Coordinate Reference System and try again!".format(src.crs,train_df.crs))
        
        
    def poly_from_utm(polygon, transform):
        poly_pts = []

        # make a polygon from multipolygon
        poly = cascaded_union(polygon)
        for i in np.array(poly.exterior.coords):

            # transfrom polygon to image crs, using raster meta
            poly_pts.append(~transform * tuple(i))

        # make a shapely Polygon object
        new_poly = Polygon(poly_pts)
        return new_poly
    
    
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
    
    mask = mask.astype("uint16")
    
    bin_mask_meta = src.meta.copy()
    bin_mask_meta.update({'count': 1})
    os.chdir(output_path)
    with rasterio.open(file_name, 'w', **bin_mask_meta) as dst:
        dst.write(mask * 255, 1)


```


#### Referências

- https://medium.com/datadriveninvestor/preparing-aerial-imagery-for-crop-classification-ce05d3601c68

- https://rasterio.readthedocs.io/en/stable/api/rasterio.mask.html







