---
title: 'Azure Front Door Service: compatibilidad con el protocolo de encabezados HTTP | Microsoft Docs'
description: Este artículo le ayudará a comprender los protocolos de encabezados HTTP admitidos por Front Door
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: efb3ffeb2e5fdde79fe3741377b8a06074671fea
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46951141"
---
# <a name="azure-front-door-service---http-headers-protocol-support"></a>Azure Front Door Service: compatibilidad con el protocolo de encabezados HTTP
Este documento describe el protocolo que Azure Front Door Service admite con varias partes de la ruta de llamadas tal como se detalla en la imagen siguiente. Las secciones siguientes proporcionan más información sobre los encabezados HTTP que Front Door admite.

![Protocolo de encabezados HTTP de Azure Front Door Service][1]

>[!WARNING]
>Azure Front Door Service no proporciona garantías para todos los encabezados HTTP que no se documentan aquí.

## <a name="1-client-to-front-door"></a>1. Cliente para Front Door

Front Door analizará los encabezados siguientes. Para cualquier otro encabezado, Front Door actuará como un paso a través y los enviará a la aplicación de back-end.

| Encabezado  | Ejemplo y descripción |
| ------------- | ------------- |
| X-MSEdge-IG  | *X-MSEdge-IG: 2BC2B67F49ED47DFA47FBE2E962AC81F* </br> GUID que identifica una o varias solicitudes HTTP realizadas para representar una única página web. Front Door reenviará el GUID al back-end con el encabezado X-FD-ImpressionGuid. </br> Si el cliente no envía este encabezado, Front Door generará automáticamente un nuevo GUID y lo enviará al back-end. |

## <a name="2-front-door-to-backend"></a>2. Front Door al back-end

Los encabezados siguientes se enviarán desde Front Door al back-end de la aplicación.

| Encabezado  | Ejemplo y descripción |
| ------------- | ------------- |
| X-MS-Ref |  *X-FD-Ref: 0WrHgWgAAAACFupORp/8MS6vxhG/WUvawV1NURURHRTAzMjEARWRnZQ==* </br> El identificador de una solicitud atendida por Front Door. Este valor de encabezado es la información más importante para solucionar problemas. Este identificador puede usarse para buscar los registros de acceso. Front Door enviará el mismo encabezado tanto al cliente como al back-end. |
| X-FD-ClientHttpVersion | *X-FD-ClientHttpVersion: 1.1* </br>Versión del protocolo HTTP entre el cliente y Front Door. Los valores posibles son 1.1, 2.0, etc. |
| X-FD-ClientIP | *X-FD-ClientIP: 131.107.192.35* </br>Dirección de protocolo de Internet del cliente asociada con la solicitud que se está procesando |
| X-FD-EdgeEnvironment | *X-FD-EdgeEnvironment: Edge-Prod-WSTr3* </br>Nodo específico de Front Door que controla la solicitud. Este encabezado puede ser registrado por el back-end para ayudar a depurar el trabajo. Sin embargo, no se debe adoptar ninguna dependencia de las convenciones de nomenclatura de Front Door, la distribución del tráfico entre los nodos de Front Door o la disponibilidad de un nodo individual específico. |
| X-FD-EventID | *X-FD-EventId: FB805145C0124400915BE0E180F3A159* </br>Identificador único (GUID) creado al principio del procesamiento de la solicitud. Cualquier registro del acceso creado al procesar esta solicitud se asociará con el mismo GUID. |
| X-FD-ImpressionGuid | *X-FD-ImpressionGuid: 2BC2B67F49ED47DFA47FBE2E962AC81F* </br>GUID que identifica una o varias solicitudes HTTP realizadas para representar una única página web. El lado del cliente puede especificar este GUID: </br>- Enviándolo el encabezado de solicitud *X-MSEdge-IG*. Por ejemplo, X-MSEdge-IG: 2BC2B67F49ED47DFA47FBE2E962AC81F </br>- Enviándolo en la cadena de consulta "ig". Por ejemplo, *?ig=2BC2B67F49ED47DFA47FBE2E962AC81F*. Si el cliente no envía un GUID, Front Door generará uno nuevo. |
| X-FD-OriginalURL | *X-FD-OriginalURL: http://www.contoso.com:80/* </br> Dirección URL de la solicitud original realizada por el cliente. |
| X-FD-Partner | *X-FD-Partner: ARM-contoso.azurefd.net_Default* </br>Identificador del perfil de Front Door para el que la solicitud se controla. Pueden usarlo los back-ends que hospedan varias aplicaciones para diferentes Front Door como una manera de determinar para qué configuración de Front Door a la que atienden es la actual solicitud. |
| X-FD-RequestHadClientId | Reserved |
| X-FD-ResponseHasClientId | Reserved |
| X-FD-SocketIP | *X-FD-SocketIP: 131.107.192.35* </br>El mismo que el valor de X-FD-ClientIP anterior |

## <a name="3-front-door-to-client"></a>3. Front Door al cliente

Los siguientes son los encabezados que se envían desde Front Door a los clientes. Los encabezados enviados a Front Door desde el back-end se pasan también al cliente.

| Encabezado  | Ejemplo |
| ------------- | ------------- |
| X-MS-Ref  | *X-FD-Ref: 0WrHgWgAAAACFupORp/8MS6vxhG/WUvawV1NURURHRTAzMjEARWRnZQ==* </br>Identificador de una solicitud atendida por Front Door. Este valor de encabezado es la información más importante para solucionar problemas. Este identificador puede usarse para buscar los registros de acceso. Front Door enviará el mismo encabezado tanto al cliente como al back-end. |

## <a name="next-steps"></a>Pasos siguientes

- Más información acerca de cómo [crear una instancia de Front Door](quickstart-create-front-door.md).
- Más información acerca de cómo [funciona Front Door](front-door-routing-architecture.md).

<!--Image references-->
[1]: ./media/front-door-http-headers-protocol/front-door-protocol-summary.png