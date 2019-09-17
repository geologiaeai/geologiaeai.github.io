---
layout: post
title: Como Cortar e Reprojetar um Raster com Python
subtitle: Dados disponíveis e como utilizá-los  
tags: [Python, rms, Sensoriamento Remoto]
---

## Objetivos

1. Cortar e Reprojetar um Raster utilizando o Python


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
inpath = '/Users/.../raster.tif',
#Outpath = Local onde a imagem reprojetada será salva
outpath = '/Users/../raster_repoject.tif', 
# Novo Sistema de Coordenadas, neste caso utilizaremos o WGS 84 (EPSG:4326) como exemplo.
new_crs = 'EPSG:4326')
```

## Verificar se o arquivo foi reprojetado
``` python
raster_reproject = rio.open('/Users/.../raster_repoject.tif')
print(landsat_data.crs)
# Se o valor exibido for EPSG: 4326 o arquivo foi corretamente reprojetado
```

## Cortar a Imagem na dimensões do shapefile
``` python
with rio.open("/Users/.../raster_repoject.tif") as landsat_image:
    # Cortar a imagem na dimensão do shapefile
    # A variável raster_crop conterá a imagem cortada
    # A variável raster_crop_metadata conterá os metados da imagem que serão atualizados posteriormente
    raster_crop, raster_crop_metadata = es.crop_image(raster_image, crop_extent)
```

## Plotar a imagem cortada

``` python
# obter as dimnesões da imagem recortada
raster_crop_affine = raster_crop_metadata["transform"]
# Determinar a extensão do plot
plot_extent = plotting_extent(landsat_crop[0], raster_crop_affine)
# Plotar a imagem
ep.plot_bands(raster_crop[0],
              extent=plot_extent,
              cmap='Greys',
              title="Raster Recortado",
              scale=False)
plt.show()
```

## Atualizar os metados para ficar registrado as novas dimensões do raster

``` python
raster_crop_metadata.update({'transform': raster_crop_affine,
                       'height': landsat_crop.shape[1],
                       'width': landsat_crop.shape[2],
                         'nodata': 0})
landsat_crop_meta

```

## Exportar a imagem cortada

``` python
path_out = "/Users/.../landsat_cropped.tif"
with rio.open(path_out, 'w', **landsat_crop_metadata) as ff:
    ff.write(raster_crop[0], 1)

```




