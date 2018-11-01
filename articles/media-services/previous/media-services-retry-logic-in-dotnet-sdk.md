---
title: Lógica de reintento en el SDK de Media Services para .NET | Microsoft Docs
description: El tema proporciona una descripción de la lógica de reintento en el SDK de Media Services para .NET.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 527b61a6-c862-4bd8-bcbc-b9aea1ffdee3
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2018
ms.author: juliako
ms.openlocfilehash: 0a4c9db8da046e901241bc383098013b2acc6bb2
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50242269"
---
# <a name="retry-logic-in-the-media-services-sdk-for-net"></a>Lógica de reintento en el SDK de Media Services para .NET
Cuando se trabaja con servicios de Microsoft Azure, pueden producirse errores transitorios. Si se produce un error transitorio, en la mayoría de los casos, el funcionamiento se produce después de algunos reintentos. El SDK de Media Services para .NET implementa la lógica de reintento para administrar los errores transitorios asociados a las excepciones y los errores causados por las solicitudes web, que ejecutan consultas, guardan cambios y operaciones de almacenamiento.  De forma predeterminada, el SDK de Media Services para .NET ejecuta cuatro reintentos antes de volver a producir la excepción en la aplicación. El código de la aplicación debe controlar esta excepción correctamente.  

 La siguiente es una breve directriz de las directivas de solicitud de servicio web, almacenamiento, consulta y SaveChanges:  

* La directiva de almacenamiento se usa para las operaciones de Blob Storage (cargas o descarga de archivos de recursos).  
* La directiva de solicitud web se utiliza para las solicitudes web genéricas (por ejemplo, para obtener un token de autenticación y resolver el punto de conexión del clúster de los usuarios).  
* La directiva de consulta se usa para consultar entidades de REST (por ejemplo, mediaContext.Assets.Where(...)).  
* La directiva de SaveChanges se utiliza para realizar cualquier acción que cambia los datos en el servicio (por ejemplo, la creación de una entidad que actualiza una entidad, o una llamada a una función de servicio para una operación).  
  
  En este tema se enumeran los tipos de excepción y los códigos de error que administra el SDK de Media Services para la lógica de reintento de .NET.  

## <a name="exception-types"></a>Tipos de excepciones
En la tabla siguiente se describen las excepciones que administra el SDK de Media Services para .NET o no administra algunas operaciones que pueden provocar errores transitorios.  

| Excepción | Solicitud web | Storage | Consultar | SaveChanges |
| --- | --- | --- | --- | --- |
| WebException<br/>Para más información, vea la sección [Códigos de estado de WebException](media-services-retry-logic-in-dotnet-sdk.md#WebExceptionStatus). |SÍ |Sí |Sí |SÍ |
| DataServiceClientException<br/> Para obtener más información, consulte [Códigos de estado de error HTTP](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Sin  |SÍ |Sí |SÍ |
| DataServiceQueryException<br/> Para obtener más información, consulte [Códigos de estado de error HTTP](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Sin  |SÍ |Sí |SÍ |
| DataServiceRequestException<br/> Para obtener más información, consulte [Códigos de estado de error HTTP](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Sin  |SÍ |Sí |SÍ |
| DataServiceTransportException |Sin  |No |SÍ |SÍ |
| TimeoutException |SÍ |Sí |SÍ |Sin  |
| SocketException |SÍ |Sí |Sí |SÍ |
| StorageException |Sin  |Sí |No |Sin  |
| IOException |Sin  |Sí |No |Sin  |

### <a name="WebExceptionStatus"></a> Códigos de estado WebException
La tabla siguiente se muestra para qué códigos de error de WebException se implementa la lógica de reintento. La enumeración [WebExceptionStatus](http://msdn.microsoft.com/library/system.net.webexceptionstatus.aspx) define los códigos de estado.  

| Status | Solicitud web | Storage | Consultar | SaveChanges |
| --- | --- | --- | --- | --- |
| ConnectFailure |SÍ |Sí |Sí |SÍ |
| NameResolutionFailure |SÍ |Sí |Sí |SÍ |
| ProxyNameResolutionFailure |SÍ |Sí |Sí |SÍ |
| SendFailure |SÍ |Sí |Sí |SÍ |
| PipelineFailure |SÍ |Sí |SÍ |Sin  |
| ConnectionClosed |SÍ |Sí |SÍ |Sin  |
| KeepAliveFailure |SÍ |Sí |SÍ |Sin  |
| UnknownError |SÍ |Sí |SÍ |Sin  |
| ReceiveFailure |SÍ |Sí |SÍ |Sin  |
| RequestCanceled |SÍ |Sí |SÍ |Sin  |
| Tiempo de espera |SÍ |Sí |SÍ |Sin  |
| ProtocolError <br/>El reintento de ProtocolError se controla mediante la administración del código de estado HTTP. Para obtener más información, consulte [Códigos de estado de error HTTP](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |SÍ |Sí |Sí |SÍ |

### <a name="HTTPStatusCode"></a> Códigos de estado de error HTTP
Cuando las operaciones Query y SaveChanges producen DataServiceClientException, DataServiceQueryException o DataServiceQueryException, se devuelve el código de estado de error HTTP en la propiedad StatusCode.  La tabla siguiente se muestra para qué códigos de error se implementa la lógica de reintento.  

| Status | Solicitud web | Storage | Consultar | SaveChanges |
| --- | --- | --- | --- | --- |
| 401 |Sin  |Sí |No |Sin  |
| 403 |Sin  |SÍ<br/>Administración de reintentos con esperas más prolongadas. |Sin  |Sin  |
| 408 |SÍ |Sí |Sí |SÍ |
| 429 |SÍ |Sí |Sí |SÍ |
| 500 |SÍ |Sí |SÍ |Sin  |
| 502 |SÍ |Sí |SÍ |Sin  |
| 503 |SÍ |Sí |Sí |SÍ |
| 504 |SÍ |Sí |SÍ |Sin  |

Si desea echar un vistazo a la implementación real del SDK de Media Services para la lógica de reintento. NET, consulte [azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services/tree/dev/src/net/Client/TransientFaultHandling).

## <a name="next-steps"></a>Pasos siguientes
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Envío de comentarios
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

