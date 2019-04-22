---
title: Protocolo de soporte técnico para los encabezados HTTP en el servicio de puerta de entrada de Azure | Microsoft Docs
description: Este artículo describe los protocolos de encabezado HTTP que admite el servicio de puerta de entrada.
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
ms.openlocfilehash: 92e8435e4336c68982e4becc2a95f99b2c776c0e
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "58861849"
---
# <a name="protocol-support-for-http-headers-in-azure-front-door-service"></a>Soporte de protocolo para los encabezados HTTP en el servicio de puerta de entrada de Azure
Este artículo describe el protocolo que admite el servicio de puerta de entrada con partes de la ruta de acceso de llamada (consulte la imagen). Las secciones siguientes proporcionan más información acerca de los encabezados HTTP admitidos por el servicio de puerta de entrada.

![Protocolo de encabezados HTTP de Azure Front Door Service][1]

>[!IMPORTANT]
>Servicio de puerta de entrada no certificar todos los encabezados HTTP que no se documentan aquí.

## <a name="client-to-front-door-service"></a>Cliente de servicio de puerta de entrada
Servicio de puerta de entrada acepta la mayoría de los encabezados de la solicitud entrante sin tener que modificarlos. Algunos encabezados reservados se quitan de la solicitud entrante si envía, incluidos los encabezados de la X - FD-* prefijo.

## <a name="front-door-service-to-backend"></a>Servicio de puerta de entrada al back-end

Servicio de puerta de entrada incluye encabezados de una solicitud entrante a menos que quite debido a las restricciones. Puerta delantera también agrega los encabezados siguientes:

| Encabezado  | Ejemplo y descripción |
| ------------- | ------------- |
| Via |  Via: 1.1 azure </br> Puerta de entrada se agrega la versión del cliente HTTP seguida *Azure* como el valor de encabezado Via. Esto indica la versión del cliente HTTP y esa puerta de entrada fue un destinatario intermedio para la solicitud entre el cliente y el back-end.  |
| X-Azure-ClientIP | X-Azure-ClientIP: 127.0.0.1 </br> Representa la dirección IP de cliente asociada a la solicitud que se está procesando. Por ejemplo, una solicitud procedente de un servidor proxy puede agregar el encabezado X-Forwarded-For para indicar la dirección IP del llamador original. |
| X-Azure-SocketIP |  X-Azure-SocketIP: 127.0.0.1 </br> Representa la dirección IP del socket asociada con la conexión TCP que se originó la solicitud actual. Dirección IP del cliente de la solicitud no podría ser igual a su dirección IP de socket porque se puede sobrescribir arbitrariamente por un usuario.|
| X-Azure-Ref |  X-Azure-Ref: 0zxV+XAAAAABKMMOjBv2NT4TY6SQVjC0zV1NURURHRTA2MTkANDM3YzgyY2QtMzYwYS00YTU0LTk0YzMtNWZmNzA3NjQ3Nzgz </br> Una cadena de referencia exclusivo que identifica una solicitud atendida por puerta de entrada. Se utiliza para buscar los registros de acceso y críticos para solucionar problemas.|
| X-Azure-RequestChain |  X-Azure-RequestChain: saltos = 1 </br> Un encabezado que utiliza la puerta delantera para detectar bucles de solicitud y los usuarios no deben tomar una dependencia en él. |
| X-Forwarded-For | X-Forwarded-For: 127.0.0.1 </br> A menudo, el campo de encabezado x (XFF) HTTP identifica la dirección IP de origen de un cliente que se conecta a un servidor web a través de un equilibrador de carga o de proxy HTTP. Si hay un encabezado XFF existente, puerta delantera anexa la dirección IP de socket de cliente a ella o agrega el encabezado XFF con la dirección IP de socket de cliente. |
| X-Forwarded-Host | Host reenviados X: contoso.azurefd.net </br> El campo de encabezado HTTP X-reenviados-Host es un método común utilizado para identificar el host original solicitado por el cliente en el encabezado de solicitud HTTP del Host. Esto es porque el nombre de host de puerta de entrada puede diferir del servidor back-end que controla la solicitud. |
| X-Forwarded-Proto | X-Forwarded-Proto: http </br> El campo de encabezado HTTP X-Forwarded-Proto a menudo se usa para identificar el protocolo de origen de una solicitud HTTP porque la puerta de entrada, según la configuración, podría comunicarse con el back-end a través de HTTPS. Esto es cierto incluso si la solicitud para el proxy inverso es HTTP. |

## <a name="front-door-service-to-client"></a>Servicio de puerta de entrada al cliente

Los encabezados enviados a la puerta de entrada desde el back-end también se pasan al cliente. Estos son los encabezados enviados desde la puerta de entrada a los clientes.

| Encabezado  | Ejemplo |
| ------------- | ------------- |
| X-Azure-Ref |  *X-Azure-Ref: 0zxV+XAAAAABKMMOjBv2NT4TY6SQVjC0zV1NURURHRTA2MTkANDM3YzgyY2QtMzYwYS00YTU0LTk0YzMtNWZmNzA3NjQ3Nzgz* </br> Esta es una cadena de referencia única que identifica una solicitud atendida por Front Door. Esto es fundamental para solucionar problemas ya que se utiliza para buscar los registros de acceso.|

## <a name="next-steps"></a>Pasos siguientes

- [Crear una puerta de entrada](quickstart-create-front-door.md)
- [Cómo funciona la puerta de entrada](front-door-routing-architecture.md)

<!--Image references-->
[1]: ./media/front-door-http-headers-protocol/front-door-protocol-summary.png