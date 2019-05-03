---
title: 'Aplicar transformación: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo aplicar transformación en el servicio de Azure Machine Learning para modificar un conjunto de datos de entrada en función de una transformación calculada previamente.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 810f375642af49814049589cb83ad17fea578b13
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028716"
---
# <a name="apply-transformation-module"></a>Módulo Aplicar transformación

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para modificar un conjunto de datos de entrada en función de una transformación calculada previamente.  
  
Por ejemplo, si usa puntuaciones z para normalizar los datos de entrenamiento utilizando el **Normalize Data** módulo, tendrá que usar el valor de puntuación z que se calculó para el entrenamiento durante la fase de puntuación también. En Azure Machine Learning, puede guardar el método de normalización como una transformación y, a continuación, usar **aplicar transformación** para aplicar la puntuación z para los datos de entrada antes de la puntuación.
  
Azure Machine Learning proporciona compatibilidad para crear y, a continuación, aplicar distintos tipos de transformaciones personalizadas. Por ejemplo, es posible que desee guardar y, a continuación, volver a usar las transformaciones de:  
  
- Quite o reemplace los valores que faltan, usando **limpiar datos que faltan**
- Normalizar datos, mediante **Normalizar datos**
  

## <a name="how-to-use-apply-transformation"></a>Cómo usar aplicar transformación  
  
1. Agregar el **aplicar transformación** módulo al experimento. Puede encontrar este módulo en **Machine Learning**, en el **puntuación** categoría. 
  
2. Busque una transformación existente para usarla como entrada.  Transformaciones guardadas anteriormente pueden encontrarse en el **transforma** grupo en el panel de navegación izquierdo.  
  
   
  
3. Conecte el conjunto de datos que desee transformar. El conjunto de datos debe tener exactamente el mismo esquema (número de columnas, nombres de columna, tipos de datos) que el conjunto de datos para el que se diseñó inicialmente la transformación.  
  
4. Ningún otro parámetro debe establecerse porque todas las personalizaciones se realizan al definir la transformación.  
  
5. Para aplicar una transformación al nuevo conjunto de datos, ejecute el experimento.  

## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 