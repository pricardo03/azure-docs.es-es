---
title: 'Aplicar la transformación: referencia para los módulos'
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo usar el módulo Aplicar la transformación en Azure Machine Learning para modificar un conjunto de datos de entrada basado en una transformación calculada previamente.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/11/2020
ms.openlocfilehash: 443b021b266a202775e94e44acac3a91a2b70617
ms.sourcegitcommit: b95983c3735233d2163ef2a81d19a67376bfaf15
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2020
ms.locfileid: "77137577"
---
# <a name="apply-transformation-module"></a>Módulo Aplicar transformación

En este artículo se describe un módulo del diseñador de Azure Machine Learning (versión preliminar).

Utilice este módulo para modificar un conjunto de datos de entrada basado en una transformación calculada previamente.  
  
Por ejemplo, si usa puntuaciones z para normalizar los datos de aprendizaje utilizando el nódulo **Normalizar datos**, tendrá que usar el valor de puntuación z que se calculó para el aprendizaje durante la fase de puntuación. En Azure Machine Learning, puede guardar el método de normalización como una transformación y después usar **Aplicar transformación** para aplicar la puntuación z a los datos de entrada antes de la puntuación.
  
Azure Machine Learning proporciona compatibilidad para crear y después aplicar distintos tipos de transformaciones personalizadas. Por ejemplo, es posible que desee guardar las transformaciones y volverlas a usar para:  
  
- Quitar o reemplazar los valores que faltan usando **Limpiar los datos que faltan**.
- Normalizar los datos usando **Normalizar datos**.
  

## <a name="how-to-use-apply-transformation"></a>Cómo se usa Aplicar transformación  
  
1. Agregue el módulo **Apply Transformation** (Aplicar transformación) a la canalización. Puede encontrar este módulo en la categoría **Evaluación y puntuación del modelo**. 
  
2. Busque una transformación existente para usarla como entrada. Las transformaciones guardadas anteriormente se pueden encontrar en el grupo **My Datasets** (Mis conjuntos de datos) en la categoría **Conjuntos de datos** en el árbol del módulo de la izquierda.  
  
   
  
3. Conecte el conjunto de datos que quiera transformar. El conjunto de datos debe tener exactamente el mismo esquema (número de columnas, nombres de columna, tipos de datos) que el conjunto de datos para el que la transformación se ha diseñó inicialmente.  
  
4. No hay que establecer ningún parámetro adicional, puesto que toda la personalización se realiza al definir la transformación.  
  
5. Para aplicar una transformación al nuevo conjunto de datos, ejecute la canalización.  

## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning. 