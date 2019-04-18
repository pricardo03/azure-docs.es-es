---
title: Exportación y eliminación de datos
titleSuffix: Azure Cognitive Services
description: Eliminar datos del cliente para garantizar la privacidad y cumplimiento.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/02/2019
ms.author: diberry
ms.openlocfilehash: eb125133138c6de173fdeb90024a9e5d961a929d
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "58895163"
---
# <a name="export-and-delete-your-customer-data-in-language-understanding-luis-in-cognitive-services"></a>Exportación y eliminación de datos del cliente en Language Understanding (LUIS) en Cognitive Services

Eliminar datos del cliente para garantizar la privacidad y cumplimiento. 

## <a name="summary-of-customer-data-request-features"></a>Resumen de características de solicitud de datos del cliente
Language Understanding Intelligent Service (LUIS) conserva el contenido del cliente para que el servicio funcione, pero el usuario de LUIS tiene control total sobre la visualización, la exportación y la eliminación de sus datos. Esto puede hacerse a través de la web de LUIS [portal](luis-reference-regions.md) o [creación LUIS API (también conocida como programación)](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

El contenido del cliente se almacena cifrado en el almacenamiento regional de Microsoft Azure e incluye:

- El contenido de la cuenta de usuario recopilado en el registro.
- Datos de entrenamiento necesarios para compilar los modelos
- Registra las consultas de usuario utilizadas por [aprendizaje activo](luis-concept-review-endpoint-utterances.md) para ayudar a mejorar el modelo
  - Los usuarios pueden desactivar el registro de consultas mediante la anexión de `&log=false` a la solicitud. Puede obtener más información [aquí](troubleshooting.md#how-can-i-disable-the-logging-of-utterances).

## <a name="deleting-customer-data"></a>Eliminación de datos del cliente
Los usuarios de LUIS tienen control total para eliminar cualquier usuario contenido, ya sea a través del portal web de LUIS o las API de creación de LUIS (también conocida como mediante programación). En la siguiente tabla se muestran vínculos para proporcionar asistencia con ambos:

| | **Cuenta de usuario** | **Aplicación** | **Ejemplo Utterance(s)** | **Consultas de usuario final** |
| --- | --- | --- | --- | --- |
| **Portal** | [Vínculo](luis-concept-data-storage.md#delete-an-account) | [Vínculo](luis-how-to-start-new-app.md#delete-app) | [Vínculo](luis-concept-data-storage.md#utterances-in-an-intent) | [Declaraciones de aprendizaje activo](luis-how-to-review-endpoint-utterances.md#disable-active-learning)<br>[Grabaciones de voz registrados](luis-concept-data-storage.md#disable-logging-utterances) |
| **API** | [Vínculo](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c4c) | [Vínculo](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c39) | [Vínculo](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0b) | [Vínculo](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) |


## <a name="exporting-customer-data"></a>Exportación de datos del cliente
Los usuarios de LUIS tienen control total para ver los datos en el portal, sin embargo, se debe exportar mediante las API de creación de LUIS (también conocida como programación). En la tabla siguiente muestra los vínculos relacionados con exportaciones de datos a través de la API (también conocida como programación) LUIS de creación:

| | **Cuenta de usuario** | **Aplicación** | **Expresiones** | **Consultas de usuario final** |
| --- | --- | --- | --- | --- |
| **API** | [Vínculo](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c48) | [Vínculo](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) | [Vínculo](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) | [Vínculo](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36) |

## <a name="location-of-active-learning"></a>Ubicación del aprendizaje activo

Para habilitar [aprendizaje activo](luis-how-to-review-endpoint-utterances.md#enable-active-learning), registrados grabaciones de voz los usuarios, recibidos en los extremos publicados de LUIS, se almacenan en las siguientes ubicaciones geográficas de Azure:

* [Europa](#europe)
* [Australia](#australia)
* [Estados Unidos](#united-states)

A excepción de los datos de aprendizaje activo (que se detallan a continuación), LUIS sigue el [prácticas de almacenamiento de datos de servicios regionales](http://azuredatacentermap.azurewebsites.net/). 

### <a name="europe"></a>Europa

El [eu.luis.ai](https://eu.luis.ai) Europa creación (también conocido como API de programación) y el portal se hospedan en ubicación geográfica de Azure Europa. El portal de eu.luis.ai y creación de Europa (también conocido como API de programación) admiten la implementación de los puntos de conexión para las ubicaciones geográficas de Azure siguientes:

* Europa
* Francia
* Reino Unido

Al implementar en estas ubicaciones geográficas de Azure, las grabaciones de voz recibidos por el punto de conexión de los usuarios finales de la aplicación se almacenarán en la ubicación geográfica de Europa de Azure para el aprendizaje activo. Puede deshabilitar el aprendizaje activo, consulte [deshabilitar aprendizaje activo](luis-how-to-review-endpoint-utterances.md#disable-active-learning). Para administrar las grabaciones de voz almacenados, consulte [eliminar utterance](luis-how-to-review-endpoint-utterances.md#delete-utterance). 

### <a name="australia"></a>Australia

El [au.luis.ai](https://au.luis.ai) Australia creación (también conocido como API de programación) y el portal se hospedan en la ubicación geográfica de Azure Australia. El portal de au.luis.ai y creación de Australia (también conocido como API de programación) admiten la implementación de los puntos de conexión para las ubicaciones geográficas de Azure siguientes:

* Australia

Al implementar en estas ubicaciones geográficas de Azure, las grabaciones de voz recibidos por el punto de conexión de los usuarios finales de la aplicación se almacenarán en la ubicación geográfica de Australia de Azure para el aprendizaje activo. Puede deshabilitar el aprendizaje activo, consulte [deshabilitar aprendizaje activo](luis-how-to-review-endpoint-utterances.md#disable-active-learning). Para administrar las grabaciones de voz almacenados, consulte [eliminar utterance](luis-how-to-review-endpoint-utterances.md#delete-utterance). 

### <a name="united-states"></a>Estados Unidos

El [luis.ai](https://www.luis.ai) portal y la creación de Estados Unidos (también conocido como API de programación) se hospedan en la ubicación geográfica de Estados Unidos de Azure. El portal de luis.ai y creación de Estados Unidos (también conocido como API de programación) admiten la implementación de los puntos de conexión para las ubicaciones geográficas de Azure siguientes:

* Ubicaciones geográficas de Azure no admitidas la Europa o creación de las regiones de Australia

Al implementar en estas ubicaciones geográficas de Azure, las grabaciones de voz recibidos por el punto de conexión de los usuarios finales de la aplicación se almacenarán en la ubicación geográfica de Estados Unidos de Azure para el aprendizaje activo. Puede deshabilitar el aprendizaje activo, consulte [deshabilitar aprendizaje activo](luis-how-to-review-endpoint-utterances.md#disable-active-learning). Para administrar las grabaciones de voz almacenados, consulte [eliminar utterance](luis-how-to-review-endpoint-utterances.md#delete-utterance). 


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de las regiones de LUIS](./luis-reference-regions.md)
