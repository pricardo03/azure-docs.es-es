---
title: 'Exportación y eliminación de los datos: Custom Vision Service'
titlesuffix: Azure Cognitive Services
description: Obtenga información acerca de cómo exportar o eliminar los datos en Custom Vision Service.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: pafarley
ms.openlocfilehash: 4c395a062b1132710f888cc5a315529db082a805
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57850033"
---
# <a name="export-or-delete-user-data-in-custom-vision"></a>Exportación o eliminación de los datos del usuario en Custom Vision

Custom Vision recopila datos de usuario para que el servicio funcione, pero los clientes tienen control total sobre la visualización, exportar y eliminar sus datos con la visión personalizada [API entrenamiento](https://go.microsoft.com/fwlink/?linkid=865446).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Para obtener información sobre cómo exportar y eliminar datos de usuario de visión personalizada, consulte la tabla siguiente.

| Datos | Operación de exportación | Elimina una operación |
| ---- | ---------------- | ---------------- |
| Información de cuenta (claves de suscripción) | [GetAccountInfo](https://go.microsoft.com/fwlink/?linkid=865446) | Eliminación mediante Azure Portal (suscripciones de Azure). O bien, mediante el botón "Eliminar cuenta" en la página de configuración de CustomVision.ai (suscripciones de la cuenta de Microsoft). | 
| Detalles de la iteración | [GetIteration](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Detalles de rendimiento de la iteración | [GetIterationPerformance](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Lista de iteraciones | [GetIterations](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteIteration](https://go.microsoft.com/fwlink/?linkid=865446) |
| Proyectos y detalles del proyecto | [GetProject](https://go.microsoft.com/fwlink/?linkid=865446) y [GetProjects](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteProject](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Etiquetas de imagen | [GetTag](https://go.microsoft.com/fwlink/?linkid=865446) y [GetTags](https://go.microsoft.com/fwlink/?linkid=865446) | [DeleteTag](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Imágenes | [GetTaggedImages](https://go.microsoft.com/fwlink/?linkid=865446) (proporciona el URI para la descarga de la imagen) y [GetUntaggedImages](https://go.microsoft.com/fwlink/?linkid=865446) (proporciona el URI para la descarga de la imagen) | [DeleteImages](https://go.microsoft.com/fwlink/?linkid=865446) | 
| Modelos exportados | [GetExports](https://go.microsoft.com/fwlink/?linkid=865446) | Eliminación tras la eliminación de la cuenta |
