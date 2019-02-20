---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 002/08/2019
ms.openlocfilehash: ce7d8628c28a4a202a05aeea60e71655b4b7f1a2
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2019
ms.locfileid: "55984926"
---
Los contenedores de Cognitive Services no tienen licencia para ejecutarse sin estar conectados a Azure para realizar mediciones. Los clientes tienen que habilitar los contenedores para comunicar la información de facturación con el servicio de medición en todo momento. Los contenedores de Cognitive Services no envían los datos del cliente (la expresión) a Microsoft. El contenedor informa sobre el uso cada 10 a 15 minutos.

`docker run` utiliza los siguientes argumentos para la facturación:

| Opción | DESCRIPCIÓN |
|--------|-------------|
| `ApiKey` | La clave de API del recurso de Cognitive Services usado para realizar un seguimiento de la información de facturación.<br/>El valor de esta opción debe establecerse en una clave de API del recurso aprovisionado especificado en `Billing`. |
| `Billing` | El punto de conexión del recurso de Cognitive Services usado para realizar el seguimiento de la información de facturación.<br/>El valor de esta opción debe establecerse en el URI del punto de conexión de un recurso aprovisionado de Azure para LUIS.|
| `Eula` | Indica que ha aceptado la licencia del contenedor.<br/>El valor de esta opción debe establecerse en `accept`. |

> [!IMPORTANT]
> Las tres opciones deben especificarse con valores válidos; en caso contrario, no se inicia el contenedor.
