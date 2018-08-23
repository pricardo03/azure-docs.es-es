---
title: Niveles de zoom y cuadrícula de mosaico en Azure Maps | Microsoft Docs
description: Obtenga información acerca de los niveles de zoom y la cuadrícula de mosaico en Azure Maps
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: c6d38dbb7ee292172fe081c2b77a49db61856d5c
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2018
ms.locfileid: "42146008"
---
# <a name="zoom-levels-and-tile-grid"></a>Niveles de zoom y cuadrícula de mosaico
Azure Maps usa el sistema de coordenadas de la proyección de Mercator esférica (EPSG: 3857).

El mundo se divide en mosaicos cuadrados. Render (Raster) tiene 21 niveles de zoom, numerados de 0 a 20. Render (Vector) tiene 23 niveles de zoom, numerados de 0 a 22. En el nivel de zoom 0, todo el mundo cabe en un solo mosaico:

![Mosaico del mundo](./media/zoom-levels-and-tile-grid/world0.png)

El nivel de zoom 1 utiliza cuatro mosaicos para representar el mundo: un cuadrado de 2 x 2

![Parte superior izquierda del mosaico del mundo](./media/zoom-levels-and-tile-grid/world1a.png)     ![Parte superior derecha del mosaico del mundo](./media/zoom-levels-and-tile-grid/world1c.png) 

![Parte inferior izquierda del mosaico del mundo](./media/zoom-levels-and-tile-grid/world1b.png)     ![Parte inferior derecha del mosaico del mundo](./media/zoom-levels-and-tile-grid/world1d.png) 


Cada nivel de zoom posterior divide por cuatro los mosaicos de la anterior, creando una cuadrícula de 2<sup>zoom</sup> x 2<sup>zoom</sup>. El nivel de zoom 22 es una cuadrícula de 2<sup>22</sup> x 2<sup>22</sup> o 4 194 304 x 4 194 304 mosaicos (17 592 186 044 416 mosaicos en total).

La tabla siguiente proporciona los valores de la lista completa de los niveles de zoom:

|Nivel de zoom|Metros/píxel|Metros/lado de mosaico|
|--- |--- |--- |
|0|156543|40075008|
|1|78271,5|20037504|
|2|39135,8|10018764,8|
|3|19567,9|5009382,4|
|4|9783,9|2504678,4|
|5|4892|1252352|
|6|2446|626176|
|7|1223|313088|
|8|611,5|156544|
|9|305,7|78259,2|
|10|152,9|39142,4|
|11|76,4|19558,4|
|12|38,2|9779,2|
|13|19,1|4889,6|
|14|9,6|2457,6|
|15|4.8|1228,8|
|16|2.4|614,4|
|17|1.2|307,2|
|18|0.6|152,8|
|19|0,3|76,4|
|20|0,15|38,2|
|21|0,075|19,1|
|22|0,0375|9,55|

Los mosaicos se denominan según las coordenadas x e y, y el nivel de zoom, correspondientes a la posición del mosaico en la cuadrícula para ese nivel de zoom.

Al determinar qué nivel de zoom se usará, recuerde que cada ubicación está en una posición fija en su mosaico. Esto significa que el número de mosaicos necesarios para mostrar una extensión determinada del territorio depende de la ubicación específica de la cuadrícula de zoom en el mundo. Por ejemplo, si hay dos puntos separados 900 metros, *puede* que al mostrar una ruta entre ellos en el nivel de zoom 17, solo ocupen tres mosaicos. Sin embargo, si el punto occidental está a la derecha de su mosaico y el punto oriental a la izquierda, puede ocupar cuatro mosaicos:

![Escala de demostración del zoom](./media/zoom-levels-and-tile-grid/zoomdemo_scaled.png) 

Una vez determinado el nivel de zoom, se pueden calcular los valores x e y. El mosaico superior izquierdo de cada cuadrícula de zoom es x=0, y=0; el icono de la esquina inferior derecha está en x=2<sup>zoom -1</sup>, y = 2<sup>zoom-1</sup>.

Esta es la cuadrícula de zoom para el nivel de zoom 1:

![Cuadrícula de zoom para el nivel de zoom 1](./media/zoom-levels-and-tile-grid/api_x_y.png)
