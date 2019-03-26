---
title: 'Servicio de puerta de entrada de Azure: los encabezados HTTP admiten | Microsoft Docs'
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
ms.openlocfilehash: 40bfdfc3837da12f62864433508482a65def291c
ms.sourcegitcommit: 280d9348b53b16e068cf8615a15b958fccad366a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58407324"
---
# <a name="azure-front-door-service---http-headers-support"></a>Servicio de puerta delantera Azure - compatibilidad con encabezados HTTP
Este documento describe el protocolo que Azure Front Door Service admite con varias partes de la ruta de llamadas tal como se detalla en la imagen siguiente. Las secciones siguientes proporcionan más información sobre los encabezados HTTP que Front Door admite.

![Protocolo de encabezados HTTP de Azure Front Door Service][1]

>[!WARNING]
>Azure Front Door Service no proporciona garantías para todos los encabezados HTTP que no se documentan aquí.

## <a name="1-client-to-front-door"></a>1. Cliente para Front Door
Front Door acepta la mayoría de los encabezados de la solicitud entrante (sin tener que modificarlos); sin embargo, algunos encabezados reservados se quitarán de la solicitud entrante, si se envían. Esto incluye los encabezados con los siguientes prefijos:
 - X-FD-*

## <a name="2-front-door-to-backend"></a>2. Front Door al back-end

Front Door incluirá los encabezados de la solicitud entrante, a menos que se quitaran debido a las restricciones mencionadas anteriormente. Front Door también agregará los encabezados siguientes:

| Encabezado  | Ejemplo y descripción |
| ------------- | ------------- |
| Via |  *Via: 1.1 Azure* </br> Puerta de entrada se agrega la versión del cliente HTTP seguida por "Azure" como el valor de encabezado Via. Se agrega para indicar la versión del cliente HTTP y esa puerta de Azure era un destinatario intermedio para la solicitud entre el cliente y el back-end.  |
| X-Azure-ClientIP | *X-Azure-ClientIP: 127.0.0.1* </br> Representa la dirección de protocolo de Internet "cliente" asociada con la solicitud que se está procesando. Por ejemplo, una solicitud procedente de un servidor proxy puede agregar el encabezado X-Forwarded-For para indicar la dirección IP del llamador original. |
| X-Azure-SocketIP | *X-Azure-SocketIP: 127.0.0.1* </br> Representa la dirección de protocolo de Internet del socket asociada con la conexión TCP, la solicitud actual se ha originado. Dirección IP del cliente de una solicitud no puede ser igual a su dirección IP de Socket porque se puede sobrescribir arbitrariamente un usuario final.|
| X-Azure-Ref | *X-Azure-Ref: 0zxV+XAAAAABKMMOjBv2NT4TY6SQVjC0zV1NURURHRTA2MTkANDM3YzgyY2QtMzYwYS00YTU0LTk0YzMtNWZmNzA3NjQ3Nzgz* </br> Esta es una cadena de referencia única que identifica una solicitud atendida por Front Door. Es esencial para solucionar problemas, ya que se utiliza para buscar los registros de acceso.|
| X-Azure-RequestChain |  *X-Azure-RequestChain: hops=1* </br> Este es un encabezado que Front Door usa para detectar bucles de solicitudes y los usuarios no deben generar ninguna dependencia de él. |
| X-Forwarded-For | *X-Forwarded-For: 127.0.0.1* </br> El campo de encabezado de x (XFF) HTTP es un método común para identificar la dirección IP de origen de un cliente que se conecta a un servidor web a través de un equilibrador de carga o de proxy HTTP. Si se ha producido un encabezado XFF existente, a continuación, la puerta delantera anexa la dirección IP de socket de cliente a la otra parte agrega el encabezado XFF con la dirección IP de socket de cliente. |
| X-Forwarded-Host | *Host reenviados X: contoso.azurefd.net* </br> El campo de encabezado HTTP X-reenviados-Host es un método común para identificar el host original solicitado por el cliente en el encabezado de solicitud HTTP de Host, puesto que puede variar el nombre de host de puerta de entrada para el servidor back-end que controla la solicitud. |
| X-Forwarded-Proto | *X-Forwarded-Proto: http* </br> El campo de encabezado HTTP X-Forwarded-Proto es un método común para identificar el protocolo de origen de una solicitud HTTP, ya que según la configuración de puerta de entrada pueden comunicarse con el back-end mediante HTTPS, incluso si la solicitud para el proxy inverso es HTTP. |

## <a name="3-front-door-to-client"></a>3. Front Door al cliente

Los siguientes son los encabezados que se envían desde Front Door a los clientes. Los encabezados enviados a Front Door desde el back-end se pasan también al cliente.

| Encabezado  | Ejemplo |
| ------------- | ------------- |
| X-Azure-Ref |  *X-Azure-Ref: 0zxV+XAAAAABKMMOjBv2NT4TY6SQVjC0zV1NURURHRTA2MTkANDM3YzgyY2QtMzYwYS00YTU0LTk0YzMtNWZmNzA3NjQ3Nzgz* </br> Esta es una cadena de referencia única que identifica una solicitud atendida por Front Door. Es esencial para solucionar problemas, ya que se utiliza para buscar los registros de acceso.|

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [crear una instancia de Front Door](quickstart-create-front-door.md).
- Más información acerca de cómo [funciona Front Door](front-door-routing-architecture.md).

<!--Image references-->
[1]: ./media/front-door-http-headers-protocol/front-door-protocol-summary.png