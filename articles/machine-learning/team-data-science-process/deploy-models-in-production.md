---
title: 'Implementación de modelos en producción: proceso de ciencia de datos en equipos'
description: La implementación de modelos en producción les permite desempeñar un papel activo en la toma de decisiones empresariales.
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/30/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 1a75c842989cfbaf7bb1880831fda2bc6994622b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60238342"
---
# <a name="deploy-models-to-production-to-play-an-active-role-in-making-business-decisions"></a>Implementación de modelos en producción para desempeñar un papel activo en la toma de decisiones empresariales

La implementación de producción permite que un modelo desempeñe un rol activo en una empresa. Las predicciones a partir de un modelo implementado pueden usarse en las decisiones empresariales.

## <a name="production-platforms"></a>Plataformas de producción

Hay varios enfoques y plataformas para poner los modelos en producción. Estas son algunas opciones:

- [Implementación de modelos con el servicio Azure Machine Learning](../service/how-to-deploy-and-where.md)
- [Implementación de un modelo en SQL-Server](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-py6-operationalize-the-model)
- [Servidor de Microsoft Machine Learning](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)

>[!NOTE]
>Antes de la implementación, uno debe asegurarse de que la latencia de la puntuación de los modelos es lo suficientemente baja para usarse en producción.
>

>[!NOTE]
>Para implementar mediante Azure Machine Learning Studio, consulte [Implementar un servicio web de Azure Machine Learning](../studio/publish-a-machine-learning-web-service.md).
>

## <a name="ab-testing"></a>Pruebas A/B

Cuando hay varios modelos en producción, puede resultar útil realizar [pruebas A/B](https://en.wikipedia.org/wiki/A/B_testing) para comparar el rendimiento de los modelos. 
 
## <a name="next-steps"></a>Pasos siguientes

También se proporcionan tutoriales que muestran todos los pasos del proceso en **escenarios concretos**. Se enumeran y enlazan con descripciones en miniatura en los [tutoriales de ejemplo](walkthroughs.md). En ellos se ilustra cómo combinar herramientas y servicios locales y en la nube en un flujo de trabajo o canalización para crear una aplicación inteligente. 
