---
title: Configuración de puerto de Azure Relay | Microsoft Docs
description: Obtenga información sobre los valores de puerto de Azure Relay.
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/26/2018
ms.author: spelluru
ms.openlocfilehash: 9d11179a8518ebf48f68f8607f94e0253d4edb80
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60789933"
---
# <a name="azure-relay-port-settings"></a>Configuración de puerto de Relay de Azure

En la tabla siguiente se describe la configuración que se requiere para los valores de los puerto de Azure Relay.

## <a name="hybrid-connections"></a>conexiones híbridas

Las conexiones híbridas emplean protocolos WebSocket en el puerto 443 con SSL como mecanismo de transporte subyacente, utilizando solo **HTTPS**. 

## <a name="wcf-relays"></a>Relés de WCF
  
|Enlace|Seguridad de transporte|Port|  
|-------------|------------------------|----------|  
|[Clase BasicHttpRelayBinding](/dotnet/api/microsoft.servicebus.basichttprelaybinding) (cliente)|Sí|HTTPS| 
|" |Sin |HTTP|  
|[Clase BasicHttpRelayBinding](/dotnet/api/microsoft.servicebus.basichttprelaybinding) (servicio)|Es posible usar el|9351/HTTP|  
|[Clase NetEventRelayBinding](/dotnet/api/microsoft.servicebus.neteventrelaybinding) (cliente)|Sí|9351/HTTPS|  
|" |Sin |9350/HTTP|  
|[Clase NetEventRelayBinding](/dotnet/api/microsoft.servicebus.neteventrelaybinding) (servicio)|Es posible usar el|9351/HTTP|  
|[Clase NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) (cliente o servicio)|Es posible usar el|5671/9352/HTTP (9352/9353 si se usa el modo híbrido)|  
|[Clase NetOnewayRelayBinding](/dotnet/api/microsoft.servicebus.netonewayrelaybinding) (cliente)|Sí|9351/HTTPS|  
|" |Sin |9350/HTTP|  
|[Clase NetOnewayRelayBinding](/dotnet/api/microsoft.servicebus.netonewayrelaybinding) (servicio)|Es posible usar el|9351/HTTP|  
|[Clase WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) (cliente)|Sí|HTTPS|  
|" |Sin |HTTP|  
|[Clase WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) (servicio)|Es posible usar el|9351/HTTP|  
|[Clase WS2007HttpRelayBinding](/dotnet/api/microsoft.servicebus.ws2007httprelaybinding) (cliente)|Sí|HTTPS|  
|" |Sin |HTTP|  
|[Clase WS2007HttpRelayBinding](/dotnet/api/microsoft.servicebus.ws2007httprelaybinding) (servicio)|Es posible usar el|9351/HTTP|

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre Azure Relay, visite estos vínculos:
* [¿Qué es Relay de Azure?](relay-what-is-it.md)
* [Preguntas más frecuentes acerca de Relay](relay-faq.md)