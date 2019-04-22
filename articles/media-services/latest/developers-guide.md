---
title: 'Los SDK de Azure Media Services v3: Azure'
description: En este artículo se proporciona información general sobre cómo empezar a desarrollar con la API de Media Services v3 mediante SDK.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: ''
ms.service: media-services
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 04/11/2019
ms.author: juliako
ms.custom: ''
ms.openlocfilehash: 9fb4d1561a661387f759aada9e776d43a95aa5c7
ms.sourcegitcommit: b8a8d29fdf199158d96736fbbb0c3773502a092d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/15/2019
ms.locfileid: "59564516"
---
# <a name="develop-against-media-services-v3-api-using-sdks"></a>Desarrollo con la API de Media Services v3 con SDK

Como desarrollador, puede usar la [API de REST](https://aka.ms/ams-v3-rest-ref) de Media Services o bibliotecas de cliente que le permitan interactuar con la API de REST para crear, administrar y mantener fácilmente flujos de trabajo multimedia personalizados. La API de [Media Services v3](https://aka.ms/ams-v3-rest-sdk) se basa en la especificación OpenAPI (anteriormente conocida como Swagger).

> [!NOTE]
> No se garantiza que los SDK de Azure Media Services v3 sean seguros para subprocesos. Al desarrollar una aplicación multiproceso, debe agregar su propia lógica de sincronización de subprocesos para proteger el cliente o usar un nuevo objeto AzureMediaServicesClient por subproceso. También debe tener cuidado con los problemas de subprocesos múltiples introducidos por objetos opcionales que proporciona el código al cliente (por ejemplo, una instancia de HttpClient en .NET).

En este tema se proporcionan vínculos a los SDK, las herramientas y las guías paso a paso.

## <a name="prerequisites"></a>Requisitos previos

Para empezar a desarrollar con Media Services, necesita:

- Una suscripción de Azure activa. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.
- [Información sobre conceptos fundamentales](concepts-overview.md)
- Revise [Desarrollo con Media Services API v3](media-services-apis-overview.md)
- [Creación de una cuenta de Media Services: CLI](create-account-cli-how-to.md)

## <a name="start-developing-with-sdks"></a>Comenzar a desarrollar con los SDK

### <a name="net"></a>.NET

Use el [SDK de .NET](https://aka.ms/ams-v3-dotnet-sdk) para [conectarse a Media Services](configure-connect-dotnet-howto.md).

Explore la documentación de [referencia de .NET](https://aka.ms/ams-v3-dotnet-ref) de Media Services.

### <a name="java"></a>Java

Use el [SDK de Java](https://aka.ms/ams-v3-java-sdk) para [conectarse a Media Services](configure-connect-java-howto.md).

Revise la documentación de [referencia de Java](https://aka.ms/ams-v3-java-ref) de Media Services.

### <a name="nodejs"></a>Node.js

Use el [SDK de Node.js](https://aka.ms/ams-v3-nodejs-sdk) para [conectarse a Media Services](configure-connect-nodejs-howto.md).

Explore la documentación de [referencia de Node.js](https://aka.ms/ams-v3-nodejs-ref) y los [ejemplos](https://github.com/Azure-Samples/media-services-v3-node-tutorials) que muestran cómo usar Media Services API con node.js.

### <a name="python"></a>Python

Use el [SDK de Python](https://aka.ms/ams-v3-python-sdk).

Revise la documentación de [referencia de Python](https://aka.ms/ams-v3-python-ref) de Media Services.

### <a name="go"></a>Go

Use el [SDK de Go](https://aka.ms/ams-v3-go-sdk).

Revise la documentación de [referencia de Go](https://aka.ms/ams-v3-go-ref) de Media Services.

### <a name="ruby"></a>Ruby

Use el [SDK de Ruby](https://aka.ms/ams-v3-ruby-sdk).

## <a name="azure-media-services-explorer"></a>Explorador de Azure Media Services

[Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE) es una herramienta disponible para los clientes de Windows que desean obtener información acerca de Media Services. AMSE es una aplicación de Winforms o C# que permite cargar, descargar, codificar, transmitir vídeo bajo demanda y contenido en directo con Media Services. Esta herramienta es para aquellos clientes que deseen probar Media Services sin escribir ningún código. El código AMSE se proporciona como un recurso para los clientes que desean desarrollar con Media Services.

AMSE es un proyecto de código abierto en el que el soporte técnico lo facilita la comunidad (se pueden notificar los problemas a https://github.com/Azure/Azure-Media-Services-Explorer/issues)). Este proyecto ha adoptado el [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Código de conducta de código abierto de Microsoft). Para más información, consulte las [preguntas más frecuentes del código de conducta](https://opensource.microsoft.com/codeofconduct/faq/) o póngase en contacto con opencode@microsoft.com si tiene cualquier otra pregunta o comentario.

## <a name="next-steps"></a>Pasos siguientes

[Información general](media-services-overview.md)
