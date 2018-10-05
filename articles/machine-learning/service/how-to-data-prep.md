---
title: Preparación de los datos con el SDK de preparación de datos de Machine Learning para Python en Azure
description: Obtenga información sobre cómo usar el SDK de preparación de datos para Python de Azure Machine Learning a fin de cargar datos de diversos formatos, transformarlos para que resulten más fáciles de usar, y escribir datos en una ubicación para que los modelos accedan a ellos.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 09/24/2018
ms.openlocfilehash: 673b8ddf3ed07507cbce4b54e9b8a488cb3ac689
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166612"
---
# <a name="prepare-data-for-modeling-with-azure-machine-learning"></a>Preparación de los datos para el modelado con Azure Machine Learning
 
La preparación de datos es una parte importante del flujo de trabajo de aprendizaje automático. Los modelos serán más precisos y eficientes si tienen acceso para limpiar los datos en un formato que sea más fácil de consumir. 

Puede preparar los datos en Python con el [SDK de preparación de datos de Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/dataprep?view=azure-dataprep-py). 

## <a name="data-preparation-pipeline"></a>Canalización de preparación de datos

Los pasos de preparación de datos principales son:

1. [Carga de los datos](how-to-load-data.md), que pueden estar en varios formatos
2. [Transformación](how-to-transform-data.md) en una estructura más fácil de usar
3. [Escritura](how-to-write-data.md) de esos datos en una ubicación accesible para los modelos

![Proceso de preparación de datos](./media/concept-data-preparation/data-prep-process.png)

## <a name="next-steps"></a>Pasos siguientes
Revise un [cuaderno de ejemplo](https://github.com/Microsoft/PendletonDocs/blob/master/Scenarios/GettingStarted/getting-started.ipynb) de preparación de datos mediante el SDK de preparación de datos de Azure Machine Learning.
