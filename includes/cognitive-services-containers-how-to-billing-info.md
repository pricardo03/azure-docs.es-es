---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 04/16/2019
ms.openlocfilehash: e92d1c65d9601c23e7e785f07e2de3e43ea6612b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60598918"
---
Las consultas en el contenedor se facturan según el plan de tarifa del recurso de Azure utilizado para la `<ApiKey>`.

Los contenedores de servicios cognitivos no tienen licencia para ejecutarse sin estar conectados al punto de conexión de facturación para la medición. Los clientes tienen que habilitar los contenedores comunicar la información de facturación con la facturación del punto de conexión en todo momento. Los contenedores de Cognitive Services no envían datos de los clientes (por ejemplo, la imagen o el texto que se está analizando) a Microsoft. 

### <a name="connecting-to-azure"></a>Conectarse a Azure

El contenedor necesita los valores de argumento de facturación para ejecutar. Estos valores permiten que el contenedor para conectarse al extremo de facturación. El contenedor informa sobre el uso cada 10 a 15 minutos. Si no se conecta el contenedor dentro de la ventana de tiempo permitido para Azure, el contenedor continuará ejecutar pero no atender consultas hasta que se restaure el punto de conexión de facturación. 10 veces en el mismo intervalo de tiempo de 10 a 15 minutos, se intenta la conexión. Si no puede conectarse al punto de conexión facturación dentro de los 10 intentos, el contenedor dejará de ejecutarse. 

### <a name="billing-arguments"></a>Argumentos de facturación

Tres de las opciones siguientes deben especificarse con los valores válidos en orden para el `docker run` comando para iniciar el contenedor:

| Opción | DESCRIPCIÓN |
|--------|-------------|
| `ApiKey` | La clave de API del recurso de Cognitive Services usado para realizar un seguimiento de la información de facturación.<br/>El valor de esta opción debe establecerse en una clave de API del recurso aprovisionado especificado en `Billing`. |
| `Billing` | El punto de conexión del recurso de Cognitive Services usado para realizar el seguimiento de la información de facturación.<br/>El valor de esta opción debe establecerse en el punto de conexión URI de un recurso de Azure aprovisionado.|
| `Eula` | Indica que ha aceptado la licencia del contenedor.<br/>El valor de esta opción debe establecerse en `accept`. |


