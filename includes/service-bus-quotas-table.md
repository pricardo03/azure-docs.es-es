---
title: archivo de inclusión
description: archivo de inclusión
services: service-bus-messaging
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 12/13/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 7add8c10fd3224b9c287ea4cc672191157f56a09
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58124398"
---
En la tabla siguiente se muestra información de cuota específica de mensajería de Azure Service Bus. Para obtener información sobre los precios y otras cuotas de Service Bus, consulte [precios de Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).

| Nombre de cuota | Ámbito | Notas | Valor |
| --- | --- | --- | --- |
| Número máximo de espacios de nombres básico o estándar por suscripción de Azure |Espacio de nombres |Se rechazan las solicitudes posteriores de espacios de nombres básico o estándar adicionales mediante el portal de Azure. |100|
| Número máximo de espacios de nombres Premium por suscripción de Azure |Espacio de nombres |El portal rechaza las solicitudes posteriores de espacios de nombres Premium adicionales. |25 |
| Tamaño de cola o tema |Entidad |Definido al crear la cola o tema. <br/><br/> Se rechazan los mensajes entrantes siguientes y el código de llamada recibe una excepción. |1, 2, 3, 4 o 5 GB.<br /><br />En la SKU Premium y la SKU estándar con [particiones](/azure/service-bus-messaging/service-bus-partitioning) habilitado, el tamaño máximo de cola o tema es 80 GB. |
| Número de conexiones simultáneas en un espacio de nombres |Espacio de nombres |Se rechazan las posteriores solicitudes de conexiones adicionales y el código de llamada recibe una excepción. Las operaciones de REST no cuentan para conexiones TCP simultáneas. |NetMessaging: 1,000.<br /><br />AMQP: 5,000. |
| Cantidad de solicitudes de recepción en una entidad de cola, tema o suscripción |Entidad |Recepción posteriores se rechazan las solicitudes y el código de llamada recibe una excepción. Esta cuota se aplica a un número combinado de operaciones de recepción simultáneas en todas las suscripciones de un tema. |5.000 |
| Número de temas o colas por espacio de nombres |Espacio de nombres |Se rechazan las posteriores solicitudes de creación colas o temas nuevos en el espacio de nombres. Como resultado, si se configuran mediante [Azure Portal][Azure portal], se genera un mensaje de error. Si se realiza una llamada desde la API de administración, el código de llamada recibe una excepción. |1.000 para el nivel básico o estándar. El número total de temas y colas en un espacio de nombres debe ser menor o igual a 1.000. <br/><br/>Para el nivel Premium, 1000 por unidad de mensajería (MU). Límite máximo es de 4.000. |
| Número de [temas o colas con particiones](/azure/service-bus-messaging/service-bus-partitioning) por espacio de nombres |Espacio de nombres |Se rechazan las posteriores solicitudes de creación colas o temas con particiones nuevos en el espacio de nombres. Como resultado, si se configuran mediante [Azure Portal][Azure portal], se genera un mensaje de error. Si se llama desde la API de administración, la excepción **QuotaExceededException** recibidos por el código de llamada. |Básica y los niveles estándar: 100.<br/><br/>Las entidades con particiones no son compatibles con el [Premium](../articles/service-bus-messaging/service-bus-premium-messaging.md) nivel.<br/><br />Cada cola o tema particionado cuenta en la cuota de 1.000 entidades por espacio de nombres. |
| Tamaño máximo de cualquier ruta de entidad de mensajería: cola o tema |Entidad |- |260 caracteres. |
| Tamaño máximo de cualquier nombre de entidad de mensajería: espacio de nombres, suscripción o regla de suscripción |Entidad |- |50 caracteres. |
| Tamaño máximo de un [identificador de mensaje](/dotnet/api/microsoft.azure.servicebus.message.messageid) | Entidad |- | 128 |
| Tamaño máximo de un mensaje [Id. de sesión](/dotnet/api/microsoft.azure.servicebus.message.sessionid) | Entidad |- | 128 |
| Tamaño del mensaje para una entidad de cola, tema o suscripción |Entidad |Los mensajes entrantes que superen estas cuotas se rechazan y el código de llamada recibe una excepción. |Tamaño de mensaje máximo: 256 KB para [nivel estándar](../articles/service-bus-messaging/service-bus-premium-messaging.md), 1 MB para [nivel Premium](../articles/service-bus-messaging/service-bus-premium-messaging.md). <br /><br />Debido a la sobrecarga del sistema, este límite es menor que estos valores.<br /><br />Tamaño de encabezado máximo: 64 KB.<br /><br />Número máximo de propiedades de encabezado en la bolsa de propiedades: **bytes/int. MaxValue**.<br /><br />Tamaño máximo de la propiedad en el contenedor de propiedades: sin límite explícito. Limitado por tamaño de encabezado máximo. |
| Tamaño de la propiedad de mensaje para una entidad de cola, tema o suscripción |Entidad | La excepción **SerializationException** se genera. |Tamaño de la propiedad de mensaje máximo para cada propiedad es 32.000. Tamaño acumulado de todas las propiedades no puede superar los 64 000. Este límite se aplica a todo el encabezado de la [BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage), que tiene las propiedades de usuario y las propiedades del sistema, como [SequenceNumber](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sequencenumber), [etiqueta](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label)y [ MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.messageid). |
| Número de suscripciones por tema |Entidad |Se rechazan las posteriores solicitudes de creación de suscripciones adicionales para el tema. Como resultado, si se configura a través del portal, se muestra un mensaje de error. Si se realiza una llamada desde la API de administración, el código de llamada recibe una excepción. |Nivel estándar: Cada suscripción cuenta para la cuota de 1.000 entidades, es decir, las colas, temas y suscripciones, por espacio de nombres. <br/> <br/> Nivel Premium: 2,000. |
| Número de filtros SQL por tema |Entidad |Se rechazan las posteriores solicitudes de creación de filtros adicionales en el tema y el código de llamada recibe una excepción. |2.000 |
| Número de filtros de correlación por tema |Entidad |Se rechazan las posteriores solicitudes de creación de filtros adicionales en el tema y el código de llamada recibe una excepción. |100 000 |
| Tamaño de filtros SQL o acciones |Espacio de nombres |Se rechazan las posteriores solicitudes de creación de filtros adicionales y el código de llamada recibe una excepción. |Longitud máxima de la cadena de condición de filtro: 1024 (1 KB).<br /><br />Longitud máxima de la cadena de acción de regla: 1024 (1 KB).<br /><br />Número máximo de expresiones por acción de regla: 32. |
| Número de reglas [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) por espacio de nombres, cola o tema |Entidad, espacio de nombres |Se rechazan las posteriores solicitudes de creación de reglas adicionales y el código de llamada recibe una excepción. |Número máximo de reglas: 12. <br /><br /> Las reglas que se configuran en un espacio de nombres de Service Bus se aplican a todas las colas y temas de dicho espacio de nombres. |
| Número de mensajes por transacción | Transacción | Se rechazan los mensajes entrantes adicionales y el código de llamada recibe una excepción que indica "no puede enviar más de 100 mensajes en una sola transacción". | 100 <br /><br /> Para ambas operaciones **Send()** y **SendAsync()**. |

[Azure portal]: https://portal.azure.com