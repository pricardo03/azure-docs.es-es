---
title: 'Agregar filas: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo de agregar filas en el servicio Azure Machine Learning para concatenar dos conjuntos de datos.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: ffd693ea3452ef48dc3e05e7bc4a6d3988a487b0
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028761"
---
# <a name="add-rows-module"></a>Agregar módulo de filas

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para concatenar dos conjuntos de datos. En la concatenación, las filas del segundo conjunto de datos se agregan al final del primer conjunto de datos.  
  
Concatenación de filas es útil en escenarios como los siguientes:  
  
+ Se ha generado una serie de estadísticas de evaluación, y desea combinarlas en una tabla para crear informes más fácilmente.  
  
+ Ha estado trabajando con diferentes conjuntos de datos, y desea combinar los conjuntos de datos para crear un conjunto de datos final.  

## <a name="how-to-use-add-rows"></a>Uso de agregar filas  

Para concatenar las filas de dos conjuntos de datos, las filas deben tener exactamente el mismo esquema. Esto significa, el mismo número de columnas y el mismo tipo de datos de las columnas.

1.  Arrastre el **agregar filas** módulo en el experimento, puede encontrar en **transformación de datos**, en el **manipular** categoría.

2. Conecte los conjuntos de datos a los dos puertos de entrada. El conjunto de datos que desea anexar debe estar conectado al segundo puerto (derecho). 
  
3.  Ejecute el experimento. El número de filas del conjunto de datos de salida debe ser igual a la suma de las filas de ambos conjuntos de datos de entrada.

    Si agrega el mismo conjunto de datos a las dos entradas de la **agregar filas** se duplica el módulo, el conjunto de datos. 

## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 