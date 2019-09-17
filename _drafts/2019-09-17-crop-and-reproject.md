---
layout: post
title: Como Cortar e Reprojetar um Raster com Python
subtitle: Dados disponíveis e como utilizá-los  
tags: [Python, rms, Sensoriamento Remoto]
---

## Objetivos

1. Aprender a Cortar e Reprojetar um Raster utilizando o Python


### Importar Bibliotecas do Python

``` python
import os
import numpy as np
import matplotlib.pyplot as plt
import rasterio as rio
from rasterio.plot import plotting_extent
from rasterio.mask import mask
from shapely.geometry import mapping
import geopandas as gpd
import earthpy as et
import earthpy.spatial as es
import earthpy.plot as ep
import seaborn as sns
from rasterio.warp import calculate_default_transform, reproject, Resampling
sns.set(font_scale=1.5)
```

## Carregar o shapefile e o raster que será cortado

``` python
#Carregar o shapefile
crop_extent = gpd.read_file("Users/../Shapefile.shp")

#Carregar o Raster
raster_data = rio.open("Users/.../arquivo_raster.tif")
```

## Verificar o sistema de coordenadas de ambos os arquivos

``` python
print("Sistema de Coordenadas do shapefile: ", crop_extent.crs)

print("Sistema de Coordenadas do Raster: ", raster_data.crs)
```

Se os sistemas forem **diferentes** é necessário reprojetar o raster ou o shapefile para que tenham o mesmo sistema. Neste tutorial, iremos reprojetar o raster.

## Reprojetar o Raster

``` python
#Definir uma função para reprojetar o raster
def reproject_et(inpath, outpath, new_crs):
    dst_crs = new_crs # CRS for web meractor 

    with rio.open(inpath) as src:
        transform, width, height = calculate_default_transform(
            src.crs, dst_crs, src.width, src.height, *src.bounds)
        kwargs = src.meta.copy()
        kwargs.update({
            'crs': dst_crs,
            'transform': transform,
            'width': width,
            'height': height
        })

        with rio.open(outpath, 'w', **kwargs) as dst:
            for i in range(1, src.count + 1):
                reproject(
                    source=rio.band(src, i),
                    destination=rio.band(dst, i),
                    src_transform=src.transform,
                    src_crs=src.crs,
                    dst_transform=transform,
                    dst_crs=dst_crs,
                    resampling=Resampling.nearest)
```

``` python
#Executar a função de reprojeção
reproject_et(
#Inpath = local onde se encontra a imagem que será reprojetada
inpath = '/Users/Neto/Desktop/Aprendizados/blog/notebook/landsat_empilhamento_de_bandas/nova_friburgo_stack.tif',
#Outpath = Local onde a imagem reprojetada será salva
outpath = '/Users/Neto/Desktop/Aprendizados/Mestrado/Materiais/meus_modelos/primeiro_teste/imagem/landsat_repoject.tif', 
# Novo Sistema de Coordenadas, neste caso utilizaremos o WGS 84 (EPSG:4326) como exemplo.
new_crs = 'EPSG:4326')
```







