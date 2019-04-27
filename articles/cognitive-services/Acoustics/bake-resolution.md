---
title: Resolución del proyecto acústica hornearlos
titlesuffix: Azure Cognitive Services
description: Esta información general conceptual describe la diferencia entre las resoluciones generales y bien al integrar acústica.
services: cognitive-services
author: KyleStorck
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: how-to
ms.date: 04/05/2019
ms.author: KyleStorck
ms.openlocfilehash: 7dbf63ba39c5dcdebb363cfc37a45f0216a07497
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61335479"
---
# <a name="project-acoustics-bake-resolution"></a>Resolución del proyecto acústica hornearlos
Esta información general conceptual describe la diferencia entre las resoluciones generales y bien al integrar acústica. Elija esta opción durante el paso de los sondeos de cocción el flujo de trabajo.

## <a name="Coarse-vs-Fine-Resolution"></a>Resolución gruesa frente a fina

La única diferencia entre la configuración de resolución gruesa y la fina es la frecuencia con la que se realiza la simulación. La fina usa una frecuencia el doble de alta que la gruesa. Esto tiene varias implicaciones en la simulación acústica:

* La longitud de onda de la gruesa es el doble de larga que la fina y, por lo tanto, los vóxeles son el doble de grandes.
* El tiempo de simulación está directamente relacionado con el tamaño de vóxel, lo que hace que la simulación mediante "bake" gruesa sea 16 veces más rápida que la simulación mediante "bake" fina.
* No se pueden simular portales (por ejemplo, las puertas o windows) menores que el tamaño de voxel. La configuración general puede hacer que algunos de estos portales más pequeños que no se simulan; por lo tanto, no pasan sonidos a través de en tiempo de ejecución. Para ver si sucede esto, puede visualizar los vóxeles.
* La frecuencia de simulación inferior da como resultado menos difracción en torno a los bordes y esquinas.
* Orígenes de sonido no se encuentra dentro de "rellena" voxels (es decir, voxels que contienen la geometría). Esto da como resultado ningún sonido. Es más difícil colocar los orígenes de sonido para que no estén dentro de la mayor voxels de general de lo que resulta cuando se usa el ajuste preciso.
* Los vóxeles más grandes se meten más en los portales, como se muestra a continuación. La primera imagen se creó con la configuración gruesa, mientras que la segunda es la misma entrada con resolución fina. Como indican las marcas de color rojo, hay mucha menos intrusión en la entrada cuando se usa la configuración fina. La línea azul es la entrada, como se define en la geometría, mientras que la línea roja es el portal acústico efectivo definido por el tamaño de vóxel. La evolución esta intrusión en una situación determinada dependerá completamente de cómo los vóxeles se alineen con la geometría del portal, que viene determinado por el tamaño y las ubicaciones de los objetos en la escena.

![Captura de pantalla de vóxeles gruesos rellenando una entrada en Unreal](media/unreal-coarse-bake.png)

![Captura de pantalla de vóxeles finos en una entrada en Unreal](media/unreal-fine-bake.png)

## <a name="next-steps"></a>Pasos siguientes

Pruebe usted mismo mediante la configuración de resolución gruesa y fina nuestro [Unreal](unreal-baking.md) o [Unity](unity-baking.md) complementos.