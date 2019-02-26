---
title: 'Los SDK de Azure Media Services v3: Azure'
description: En este artículo se proporciona información general sobre cómo empezar a desarrollar con la API de Media Services v3, SDK y herramientas.
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
ms.date: 02/16/2019
ms.author: juliako
ms.custom: ''
ms.openlocfilehash: 2f0191bd181a8e10fa59f6d1d53da348e6440aba
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2019
ms.locfileid: "56343589"
---
# <a name="start-developing-with-media-services-v3-api-using-sdkstools"></a>Empiece a desarrollar con la API de Media Services v3, SDK y herramientas

Como desarrollador, puede usar la [API de REST](https://aka.ms/ams-v3-rest-sdk) de Media Services o bibliotecas de cliente que le permitan interactuar con la API de REST para crear, administrar y mantener fácilmente flujos de trabajo multimedia personalizados. La API de Media Services v3 se basa en la especificación de OpenAPI (anteriormente conocida como Swagger).

En este tema se proporcionan vínculos a los SDK, las herramientas y la documentación. También se proporciona información útil para distintos entornos de desarrollo.

## <a name="prerequisites"></a>Requisitos previos

Para empezar a desarrollar con Media Services, necesita:

- Una suscripción de Azure activa. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.
- [Creación de una cuenta de Media Services](create-account-cli-how-to.md)

## <a name="start-developing"></a>Comenzar a desarrollar

Las siguientes herramientas y SDK son compatibles con Azure Media Services 

- [CLI de Azure](https://aka.ms/ams-v3-cli) 
- [SDK de .NET](https://aka.ms/ams-v3-dotnet-sdk)
- [SDK de Java](https://aka.ms/ams-v3-java-sdk)
- [SDK de Node.js](https://aka.ms/ams-v3-nodejs-sdk)
- [SDK de Python](https://aka.ms/ams-v3-python-sdk)
- [Go SDK](https://aka.ms/ams-v3-go-sdk)
- [SDK de Ruby](https://aka.ms/ams-v3-ruby-sdk)

### <a name="azure-media-services-explorer"></a>Explorador de Azure Media Services

[Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE) es una herramienta disponible para los clientes de Windows que desean obtener información acerca de Media Services. AMSE es una aplicación de Winforms o C# que permite cargar, descargar, codificar, transmitir vídeo bajo demanda y contenido en directo con Media Services. Esta herramienta es para aquellos clientes que deseen probar Media Services sin escribir ningún código. El código AMSE se proporciona como un recurso para los clientes que desean desarrollar con Media Services.

AMSE es un proyecto de código abierto en el que el soporte técnico lo facilita la comunidad (se pueden notificar los problemas a https://github.com/Azure/Azure-Media-Services-Explorer/issues)). Este proyecto ha adoptado el [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Código de conducta de código abierto de Microsoft). Para más información, consulte las [preguntas más frecuentes del código de conducta](https://opensource.microsoft.com/codeofconduct/faq/) o póngase en contacto con opencode@microsoft.com si tiene cualquier otra pregunta o comentario.

## <a name="rest"></a>REST

Para empezar a desarrollar con las API REST de Media Services, instale un cliente de REST. Por ejemplo, **Postman**, **Visual Studio Code** con el complemento de REST o **Telerik Fiddler**. Estamos usando [Postman](media-rest-apis-with-postman.md) para los ejemplos de Media Services v3.

Explore la documentación de [referencia de la API REST](https://aka.ms/ams-v3-rest-ref) de Media Services y consulte estos ejemplos:

- [Tutorial: Carga, codificación y transmisión de vídeos con REST](stream-files-tutorial-with-rest.md)
- [Carga de archivos en una cuenta de Media Services: REST](upload-files-rest-how-to.md)
- [Creación de filtros con Media Services: REST](filters-dynamic-manifest-rest-howto.md)
- [API REST basada en Azure Resource Manager](https://github.com/Azure-Samples/media-services-v3-arm-templates)

<!-- ## CLI -->
[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

Explore la documentación de [referencia de la CLI de Azure](https://aka.ms/ams-v3-cli-ref) de Media Services y consulte estos ejemplos:

- [Escalado de unidades reservadas de multimedia: CLI](media-reserved-units-cli-how-to.md)
- [Creación de una cuenta de Media Services: CLI](./scripts/cli-create-account.md) 
- [Restablecimiento de las credenciales de la cuenta: CLI](./scripts/cli-reset-account-credentials.md)
- [Creación de recursos: CLI](./scripts/cli-create-asset.md)
- [Carga de un archivo: CLI](./scripts/cli-upload-file-asset.md)
- [Creación de transformaciones: CLI](./scripts/cli-create-transform.md)
- [Creación de trabajos: CLI](./scripts/cli-create-jobs.md)
- [Creación de EventGrid: CLI](./scripts/cli-create-event-grid.md)
- [Publicación de un recurso: CLI](./scripts/cli-publish-asset.md)
- [Filtrado: CLI](filters-dynamic-manifest-cli-howto.md)

## <a name="net"></a>.NET

Explore la documentación de [referencia de .NET](https://aka.ms/ams-v3-dotnet-ref) de Media Services y consulte estos ejemplos:

- [Tutorial: Carga, codificación y streaming de vídeos: .NET](stream-files-tutorial-with-api.md) 
- [Tutorial: Streaming en vivo con Media Services v3: .NET](stream-live-tutorial-with-api.md)
- [Tutorial: Análisis de vídeos con Media Services v3: .NET](analyze-videos-tutorial-with-api.md)
- [Creación de una entrada de trabajo a partir de un archivo local: .NET](job-input-from-local-file-how-to.md)
- [Creación de una entrada de trabajo a partir de una dirección URL de HTTPS: .NET](job-input-from-http-how-to.md)
- [Codificación con una transformación personalizada: .NET](customize-encoder-presets-how-to.md)
- [Uso del cifrado dinámico AES-128 y el servicio de entrega de claves: .NET](protect-with-aes128.md)
- [Uso del cifrado dinámico de DRM y el servicio de entrega de licencias: .NET](protect-with-drm.md)
- [Obtención de una clave de firma de la directiva existente: .NET](get-content-key-policy-dotnet-howto.md)
- [Creación de filtros con Media Services: .NET](filters-dynamic-manifest-dotnet-howto.md)

## <a name="java"></a>Java

Revise la documentación de [referencia de Java](https://aka.ms/ams-v3-java-ref) de Media Services.

## <a name="nodejs"></a>Node.js

Explore la documentación de [referencia de Node.js](https://aka.ms/ams-v3-nodejs-ref) y los [ejemplos](https://github.com/Azure-Samples/media-services-v3-node-tutorials) que muestran cómo usar Media Services API con node.js.

## <a name="python"></a>Python

Revise la documentación de [referencia de Python](https://aka.ms/ams-v3-python-ref) de Media Services.

## <a name="go"></a>Go

Revise la documentación de [referencia de Go](https://aka.ms/ams-v3-go-ref) de Media Services.

## <a name="next-steps"></a>Pasos siguientes

- [Creación de una cuenta: CLI](create-account-cli-how-to.md)
- [Acceso a las API: CLI](access-api-cli-how-to.md)

