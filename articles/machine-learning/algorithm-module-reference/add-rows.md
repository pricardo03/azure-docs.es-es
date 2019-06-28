---
title: 'Adición de filas: Referencia para los módulos'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo Adición de filas en Azure Machine Learning Service para concatenar dos conjuntos de datos.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: ffd693ea3452ef48dc3e05e7bc4a6d3988a487b0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65028761"
---
# <a name="add-rows-module"></a>Módulo Adición de filas

En este artículo se describe un módulo de la interfaz visual (versión preliminar) de Azure Machine Learning Service.

Utilice este módulo para concatenar dos conjuntos de datos. En la concatenación, las filas del segundo conjunto de datos se agregan al final del primer conjunto de datos.  
  
La concatenación de filas es útil en escenarios como los siguientes:  
  
+ Se ha generado una serie de estadísticas de evaluación y desea combinarlas en una sola tabla para crear informes más fácilmente.  
  
+ Ha estado trabajando con diferentes conjuntos de datos y desea combinarlos para crear un conjunto de datos final.  

## <a name="how-to-use-add-rows"></a>Procedimiento para usar Adición de filas  

Para concatenar las filas de dos conjuntos de datos, las filas deben tener exactamente el mismo esquema. Esto significa, el mismo número de columnas y el mismo tipo de datos en las columnas.

1.  Arrastre el módulo **Adición de filas** al experimento. Puede encontrarlo en **Transformación de datos**, en la categoría **Manipulate** (Manipular).

2. Conecte los conjuntos de datos a los dos puertos de entrada. El conjunto de datos que desea anexar debe estar conectado al segundo puerto (derecho). 
  
3.  Ejecute el experimento. El número de filas del conjunto de datos de salida debe ser igual a la suma de las filas de ambos conjuntos de datos de entrada.

    Si agrega el mismo conjunto de datos a las dos entradas del módulo **Adición de filas**, se duplica el conjunto de datos. 

## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning Service. 