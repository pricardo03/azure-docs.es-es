---
title: archivo de inclusión
description: archivo de inclusión
services: event-hubs
author: sethmanheim
ms.service: event-hubs
ms.topic: include
ms.date: 02/26/2018
ms.author: sethm
ms.custom: include file
ms.openlocfilehash: cd64bdabc2b7b34687296c855c27882925d80f63
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58124416"
---
En la tabla siguiente se enumeran las cuotas y los límites específicos de [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/). Para más información sobre los precios de Event Hubs, consulte los [precios de Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

| Límite | Ámbito | Notas | Valor |
| --- | --- | --- | --- |
| Número de espacios de nombres de Event Hubs por suscripción |Subscription |- |1000 |
| Número de centros de eventos por espacio de nombres |Espacio de nombres |Se rechazan las posteriores solicitudes de creación de un centro de eventos. |10 |
| Número de particiones por centro de eventos |Entidad |- |32 |
| Número de grupos de consumidores por centro de eventos |Entidad |- |20 |
| Número de conexiones de AMQP por espacio de nombres |Espacio de nombres |Se rechazan las posteriores solicitudes de conexiones adicionales y el código de llamada recibe una excepción. |5.000 |
| Tamaño máximo de evento de Event Hubs|Entidad |- |1 MB |
| Tamaño máximo del nombre de un centro de eventos |Entidad |- |50 caracteres |
| Número de destinatarios no de época por grupo de consumidores |Entidad |- |5 |
| Período de retención máximo de datos de eventos |Entidad |- |1-7 días |
| Unidades de rendimiento máximo |Espacio de nombres |Si se supera el límite de unidad de rendimiento, los datos se limitan y genera un [excepción de servidor ocupado](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception). Para solicitar un mayor número de unidades de rendimiento para el nivel estándar, archivo un [solicitud de soporte técnico](/azure/azure-supportability/how-to-create-azure-support-request). Las [unidades de rendimiento adicionales](../articles/event-hubs/event-hubs-auto-inflate.md) se encuentran disponibles en bloques de 20 y están sujetas a un compromiso de compra. |20 |
| Número de reglas de autorización por espacio de nombres |Espacio de nombres|Se rechazan las posteriores solicitudes de creación de reglas de autorización.|12 |
