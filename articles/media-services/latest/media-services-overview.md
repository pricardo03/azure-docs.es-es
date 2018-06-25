---
title: Introducción a Azure Media Services v3 | Microsoft Docs
description: En este artículo se proporciona información general de alto nivel sobre Media Services y vínculos a artículos para obtener más información.
services: media-services
documentationcenter: na
author: Juliako
manager: cfowler
editor: ''
tags: ''
keywords: azure media services, transmisión, difusión, en vivo, sin conexión
ms.service: media-services
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 06/14/2018
ms.author: juliako
ms.custom: mvc
ms.openlocfilehash: 489801852202163ef40d57da0082e39793196d85
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264101"
---
# <a name="what-is-azure-media-services-v3"></a>¿Qué es Azure Media Services v3?

> [!div class="op_single_selector" title1="Select the version of Media Services that you are using:"]
> * [Versión 2: disponibilidad general](../previous/media-services-overview.md)
> * [Versión 3: versión preliminar](media-services-overview.md)

> [!NOTE]
> La versión más reciente de Azure Media Services está en versión preliminar y se denomina v3.

Azure Media Services es una plataforma basada en la nube que le permite compilar soluciones que consiguen un streaming de vídeo con calidad de difusión, mejoran la accesibilidad y la distribución, analizan el contenido, etc. Tanto si es un desarrollador de aplicaciones, un centro de llamadas, una autoridad gubernamental o una empresa de entretenimiento, Media Services le ayuda a crear aplicaciones que ofrecen experiencias multimedia de alta calidad a grandes audiencias de los navegadores y dispositivos móviles más populares en la actualidad. 

## <a name="what-can-i-do-with-media-services"></a>¿Qué puedo hacer con Media Services?

Media Services le permite generar una variedad de flujos de trabajo multimedia en la nube. A continuación, se indican algunos ejemplos de lo que se puede lograr con Media Services.  

* Entregue vídeos en varios formatos para que se puedan reproducir en una amplia variedad de navegadores y dispositivos. Para la entrega de streaming en vivo y bajo petición a varios clientes (dispositivos móviles, televisión, equipos, etc.), el contenido de audio y vídeo debe codificarse y comprimirse adecuadamente. Para ver cómo entregar y hacer streaming de este tipo de contenido, consulte [Quickstart: Encode and stream files](stream-files-dotnet-quickstart.md) (Inicio rápido: codificación y streaming de archivos).
* Haga streaming de eventos deportivos para una gran audiencia en línea, como fútbol, béisbol, deportes universitarios o de instituto, etc. 
* Retransmita reuniones y eventos públicos, como plenos de ayuntamientos, juntas municipales y reuniones de órganos legislativos.
* Analice contenido de audio o vídeos grabados. Por ejemplo, para lograr una mayor satisfacción del cliente, las organizaciones pueden extraer texto a voz y crear paneles e índices de búsqueda. A continuación, pueden extraer inteligencia de las quejas habituales, los orígenes de las quejas y otros datos relevantes. 
* Cree un servicio de suscripción de vídeo y haga streaming de contenido protegido por DRM cuando un cliente (por ejemplo, un estudio cinematográfico) necesite restringir el acceso y el uso de una obra con derechos de autor.
* Entregue contenido sin conexión para su reproducción en aviones, trenes y automóviles. Es posible que un cliente tenga que descargar el contenido en su teléfono o tableta para reproducirlo cuando tenga previsto desconectarse de la red.
* Agregue subtítulos y leyendas a los vídeos para llegar a un público más amplio (por ejemplo, personas con discapacidades auditivas o usuarios que quieren leer el contenido en un idioma diferente). 
* Implemente una plataforma de vídeo educativa con Azure Media Services y [Azure Cognitive Services APIs](https://docs.microsoft.com/en-us/azure/#pivot=products&panel=ai) para subtítulos de voz a texto, traducción a varios idiomas, etc.
* Habilite Azure CDN para lograr un gran escalado y mejorar la administración de cargas instantáneas pesadas (por ejemplo, al inicio de un evento de lanzamiento de un producto). 

## <a name="v3-capabilities"></a>Funcionalidades de v3

v3 se basa en una superficie de API unificada que expone la funcionalidad de administración y operaciones creada en Azure Resource Manager. 

Esta versión ofrece las siguientes funcionalidades:  

* **Transformaciones** que le ayudan a definir flujos de trabajo básicos de tareas de análisis o procesamiento multimedia. La transformación es una receta para procesar archivos de audio y vídeo. A continuación, puede aplicarla varias veces para procesar todos los archivos en la biblioteca de contenido si envía trabajos a la transformación.
* **Trabajos** para procesar (codificar o analizar) vídeos. Se puede especificar contenido de entrada en un trabajo con direcciones URL de HTTP, direcciones URL de SAS o rutas de acceso a archivos ubicados en Azure Blob Storage. 
* **Notificaciones** que supervisan el progreso o estado del trabajo y eventos de error, inicio o detención del canal en vivo. Las notificaciones están integradas en el sistema de notificaciones de Azure Event Grid. Puede suscribirse fácilmente a eventos de varios recursos en Azure Media Services. 
* Las plantillas de **Azure Resource Management** se pueden usar para crear e implementar transformaciones, puntos de conexión de streaming, canales, etc.
* **El control de acceso basado en rol** se puede establecer en recursos, lo que le permite bloquear el acceso a recursos específicos, como transformaciones, canales, etc.
* **SDK de cliente** en varios lenguajes: .NET, .NET Core, Python, Go, Java y Node.js.

## <a name="naming-conventions"></a>Convenciones de nomenclatura

Los nombres de recursos de Azure Media Services v3 (por ejemplo, recursos, trabajos y transformaciones) están sujetos a las restricciones de nomenclatura de Azure Resource Manager. De acuerdo con Azure Resource Manager, los nombres de los recursos siempre son únicos. Por lo tanto, puede usar cualquier cadena de identificador único (por ejemplo, GUID) para los nombres de recursos. 

Los nombres de recursos de Media Services no pueden incluir: '<', '>', '%', '&', ':', '&#92;', '?', '/', '*', '+', '.', el carácter de comilla simple o cualquier carácter de control. Todos los demás caracteres se permiten. La longitud máxima de un nombre de recurso es de 260 caracteres. 

Para más información sobre la nomenclatura de Azure Resource Manager, consulte: [Convenciones de nomenclatura](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/resource-api-reference.md#arguments-for-crud-on-resource) y [Convenciones de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).

## <a name="how-can-i-get-started-with-v3"></a>¿Cómo puedo comenzar con v3?

Como desarrollador, puede usar la [API de REST](https://go.microsoft.com/fwlink/p/?linkid=873030) de Media Services o bibliotecas de cliente que le permitan interactuar con la API de REST para crear, administrar y mantener fácilmente flujos de trabajo multimedia personalizados. Puede encontrar el ejemplo de Postman de REST [aquí](https://github.com/Azure-Samples/media-services-v3-rest-postman). También puede usar la [API REST basada en Azure Resource Manager](https://github.com/Azure-Samples/media-services-v3-arm-templates).

Microsoft genera y admite las bibliotecas de cliente siguientes: 

|Biblioteca de cliente|Ejemplos|
|---|---|
|[SDK de CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)|[Ejemplos de la CLI de Azure](https://github.com/Azure/azure-docs-cli-python-samples/tree/master/media-services)|
|[.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.Media/1.0.0)|[Ejemplos de .NET](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials)|
|[SDK de .NET Core](https://www.nuget.org/packages/Microsoft.Azure.Management.Media/1.0.0) (elija la pestaña **CLI de .NET**)|[Ejemplos de .NET Core](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials)|
|[SDK de Java](https://docs.microsoft.com/java/api/overview/azure/mediaservices)||
|[SDK de Node.js](https://docs.microsoft.com/javascript/api/azure-arm-mediaservices/index?view=azure-node-latest)|[Ejemplos de Node.js](https://github.com/Azure-Samples/media-services-v3-node-tutorials)|
|[SDK de Python](https://pypi.org/project/azure-mgmt-media/1.0.0rc1/)||
|[Go SDK](https://github.com/Azure/azure-sdk-for-go/tree/master/services/preview/mediaservices/mgmt/2018-03-30-preview/media)||

Media Services proporciona [archivos de Swagger](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media) que puede usar para generar SDK para el lenguaje o la tecnología que prefiera.  

## <a name="next-steps"></a>Pasos siguientes

Para ver lo fácil que es empezar a codificar y hacer streaming de archivos de vídeo, consulte [Stream files](stream-files-dotnet-quickstart.md) (Streaming de vídeos). 

