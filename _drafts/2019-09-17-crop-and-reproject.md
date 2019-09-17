---
layout: post
title: Como Cortar e Reprojetar um Raster com Python
subtitle: Dados disponíveis e como utilizá-los  
tags: [Python, rms, Sensoriamento Remoto]
---

## Objetivos

1. Aprender a Cortar e Reprojetar um Raster utilizando o Python


### Bibliotecas do Python

```
{
  import os
import numpy as np
import rasterio as rio
import earthpy as et
from rasterio.warp import calculate_default_transform, reproject, Resampling
}
```








