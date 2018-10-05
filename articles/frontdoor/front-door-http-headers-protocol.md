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
ms.openlocfilehash: 0dcb769627714be9da55faf2a8e82c8750789498
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47038857"
---
# <a name="azure-front-door-service---http-headers-protocol-support"></a>Azure Front Door Service: compatibilidad con el protocolo de encabezados HTTP
Este documento describe el protocolo que Azure Front Door Service admite con varias partes de la ruta de llamadas tal como se detalla en la imagen siguiente. Las secciones siguientes proporcionan más información sobre los encabezados HTTP que Front Door admite.

![Protocolo de encabezados HTTP de Azure Front Door Service][1]

>[!WARNING]
>Azure Front Door Service no proporciona garantías para todos los encabezados HTTP que no se documentan aquí.

## <a name="1-client-to-front-door"></a>1. Cliente para Front Door
Front Door acepta la mayoría de los encabezados de la solicitud entrante (sin tener que modificarlos); sin embargo, algunos encabezados reservados se quitarán de la solicitud entrante, si se envían. Esto incluye los encabezados con los siguientes prefijos:
 - X-FD*
 - X-MS*

## <a name="2-front-door-to-backend"></a>2. Front Door al back-end

Front Door incluirá los encabezados de la solicitud entrante, a menos que se quitaran debido a las restricciones mencionadas anteriormente. Front Door también agregará los encabezados siguientes:

| Encabezado  | Ejemplo y descripción |
| ------------- | ------------- |
| X-MS-Ref |  *X-MS-Ref: 0WrHgWgAAAACFupORp/8MS6vxhG/WUvawV1NURURHRTAzMjEARWRnZQ==* </br> Esta es una cadena de referencia única que identifica una solicitud atendida por Front Door. Es esencial para solucionar problemas, ya que se utiliza para buscar los registros de acceso.|
| X-MS-RequestChain |  *X-MS-RequestChain: hops=1* </br> Este es un encabezado que Front Door usa para detectar bucles de solicitudes y los usuarios no deben generar ninguna dependencia de él. |
| X-MS-Via |  *X-MS-Via: Azure* </br> Front Door lo agrega para indicar que Azure/Front Door era un destinatario intermedio de la solicitud entre el cliente y el servidor back-end. |

## <a name="3-front-door-to-client"></a>3. Front Door al cliente

Los siguientes son los encabezados que se envían desde Front Door a los clientes. Los encabezados enviados a Front Door desde el back-end se pasan también al cliente.

| Encabezado  | Ejemplo |
| ------------- | ------------- |
| X-MS-Ref |  *X-MS-Ref: 0WrHgWgAAAACFupORp/8MS6vxhG/WUvawV1NURURHRTAzMjEARWRnZQ==* </br> Esta es una cadena de referencia única que identifica una solicitud atendida por Front Door. Es esencial para solucionar problemas, ya que se utiliza para buscar los registros de acceso.|

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [crear una instancia de Front Door](quickstart-create-front-door.md).
- Más información acerca de cómo [funciona Front Door](front-door-routing-architecture.md).

<!--Image references-->
[1]: ./media/front-door-http-headers-protocol/front-door-protocol-summary.png