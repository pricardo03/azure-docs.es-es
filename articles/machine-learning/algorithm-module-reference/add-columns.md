---
title: 'Agregar columnas: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo de agregar columnas en el servicio Azure Machine Learning para concatenar dos conjuntos de datos.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: f1e087e97007c6ba271651a9791c7c3b38a9b9b7
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029361"
---
# <a name="add-columns-module"></a>Agregar módulo de columnas

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para concatenar dos conjuntos de datos. Combinar todas las columnas de los dos conjuntos de datos que especifique como entradas para crear un único conjunto de datos. Si necesita concatenar más de dos conjuntos de datos, use varias instancias de **agregar columnas**.



## <a name="how-to-configure-add-columns"></a>Cómo configurar agregar columnas
1. Agregar el **agregar columnas** módulo al experimento.

2. Conecte los dos conjuntos de datos que desea concatenar. Si desea combinar más de dos conjuntos de datos, puede encadenar varias combinaciones de **agregar columnas**.

    - Es posible combinar dos columnas que tienen un número diferente de filas. El conjunto de datos de salida se rellena con los valores que faltan para cada fila de la columna de origen más pequeña.

    - No puede elegir las columnas individuales para agregar. Todas las columnas de cada conjunto de datos se concatenan al usar **agregar columnas**. Por lo tanto, si desea agregar solo un subconjunto de las columnas, utilice Seleccionar columnas de conjunto de datos para crear un conjunto de datos con las columnas que desea.

3. Ejecute el experimento.

### <a name="results"></a>Results
Después de ejecuta el experimento:

- Para ver las primeras filas del conjunto de datos nuevo, haga clic en la salida de **agregar columnas** y seleccione visualizar.

El número de columnas en el nuevo conjunto de datos es igual a la suma de las columnas de ambos conjuntos de datos de entrada.

Si hay dos columnas con el mismo nombre en los conjuntos de datos de entrada, se agrega un sufijo numérico al nombre de la columna. Por ejemplo, si hay dos instancias de una columna denominada TargetOutcome, podría cambiarse la columna izquierda TargetOutcome_1 y la columna derecha sería cambiar el nombre TargetOutcome_2.

## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 