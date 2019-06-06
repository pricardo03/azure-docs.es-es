---
title: archivo de inclusión
description: archivo de inclusión
services: event-hubs
author: sethmanheim
ms.service: event-hubs
ms.topic: include
ms.date: 05/22/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: fa6b4d6d0db09f8c4955430d6dc227356416d915
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66736014"
---
En la tabla siguiente se enumeran las cuotas y los límites específicos de [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/). Para más información sobre los precios de Event Hubs, consulte los [precios de Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

| Límite | Scope | Notas | Valor |
| --- | --- | --- | --- |
| Número de espacios de nombres de Event Hubs por suscripción |Subscription |- |100 |
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
| Número de llamadas a GetRuntimeInformation (método) | Entidad | - | 50 por segundo | 

### <a name="event-hubs-dedicated---quotas-and-limits"></a>Event Hubs dedicado: cuotas y límites
La oferta de Event Hubs dedicado se factura a un precio mensual fijo, con un mínimo de 4 horas de uso. El nivel dedicado ofrece todas las características del plan estándar, pero con capacidad de escala de empresa y los límites para los clientes con cargas de trabajo exigentes. 

| Característica | límites |
| --- | ---|
| Ancho de banda |  20 CUs |
| Espacios de nombres | 50 por unidad de capacidad |
| Event Hubs |  1000 por espacio de nombres |
| Eventos de entrada | Se incluye |
| Tamaño de los mensajes | 1 millón de Bytes |
| Particiones | 2000 por unidad de capacidad |
| Grupos de consumidores | No hay límite por unidad de capacidad, 1000 por centro de eventos |
| Conexiones asincrónicas | 100 000 incluidos |
| Retención de mensajes | Seguridad de hasta 7 días (próximamente la retención de 90 días), 10 TB incluido por unidad de capacidad |
| Capture | Se incluye |
