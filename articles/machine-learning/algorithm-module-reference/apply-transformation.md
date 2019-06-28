---
title: 'Aplicar la transformación: referencia para los módulos'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo Aplicar la transformación en Azure Machine Learning Service para modificar un conjunto de datos de entrada basado en una transformación calculada previamente.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 810f375642af49814049589cb83ad17fea578b13
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65028716"
---
# <a name="apply-transformation-module"></a>Módulo Aplicar transformación

En este artículo se describe un módulo de la interfaz visual (versión preliminar) de Azure Machine Learning Service.

Utilice este módulo para modificar un conjunto de datos de entrada basado en una transformación calculada previamente.  
  
Por ejemplo, si usa puntuaciones z para normalizar los datos de aprendizaje utilizando el nódulo **Normalizar datos**, tendrá que usar el valor de puntuación z que se calculó para el aprendizaje durante la fase de puntuación. En Azure Machine Learning, puede guardar el método de normalización como una transformación y después usar **Aplicar transformación** para aplicar la puntuación z a los datos de entrada antes de la puntuación.
  
Azure Machine Learning proporciona compatibilidad para crear y después aplicar distintos tipos de transformaciones personalizadas. Por ejemplo, es posible que desee guardar las transformaciones y volverlas a usar para:  
  
- Quitar o reemplazar los valores que faltan usando **Limpiar los datos que faltan**.
- Normalizar los datos usando **Normalizar datos**.
  

## <a name="how-to-use-apply-transformation"></a>Cómo se usa Aplicar transformación  
  
1. Agregue el módulo **Aplicar transformación** al experimento. Puede encontrar este módulo en la categoría **Machine Learning**, en la categoría **Puntuación**. 
  
2. Busque una transformación existente para usarla como entrada.  Las transformaciones guardadas anteriormente se pueden encontrar en el grupo **Transformaciones** del panel de navegación izquierdo.  
  
   
  
3. Conecte el conjunto de datos que quiera transformar. El conjunto de datos debe tener exactamente el mismo esquema (número de columnas, nombres de columna, tipos de datos) que el conjunto de datos para el que la transformación se ha diseñó inicialmente.  
  
4. No hay que establecer ningún parámetro adicional, puesto que toda la personalización se realiza al definir la transformación.  
  
5. Para aplicar una transformación al nuevo conjunto de datos, ejecute el experimento.  

## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning Service. 