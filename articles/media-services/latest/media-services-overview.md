---
title: Introducción a Azure Media Services v3 | Microsoft Docs
description: En este artículo se proporciona información general de alto nivel sobre Media Services y vínculos a artículos para obtener más información.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: azure media services, transmisión, difusión, en vivo, sin conexión
ms.service: media-services
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 02/07/2019
ms.author: juliako
ms.custom: mvc
ms.openlocfilehash: 1911b851e4e219ec4c6d2d4872b75e9c18706feb
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2019
ms.locfileid: "55893330"
---
# <a name="what-is-azure-media-services-v3"></a>¿Qué es Azure Media Services v3?

Azure Media Services es una plataforma basada en la nube que le permite compilar soluciones que consiguen un streaming de vídeo con calidad de difusión, mejoran la accesibilidad y la distribución, analizan el contenido, etc. Tanto si es un desarrollador de aplicaciones, un centro de llamadas, una autoridad gubernamental o una empresa de entretenimiento, Media Services le ayuda a crear aplicaciones que ofrecen experiencias multimedia de alta calidad a grandes audiencias de los navegadores y dispositivos móviles más populares en la actualidad. 

## <a name="what-can-i-do-with-media-services"></a>¿Qué puedo hacer con Media Services?

Media Services le permite generar una variedad de flujos de trabajo multimedia en la nube. A continuación, se indican algunos ejemplos de lo que se puede lograr con Media Services.  

* Entregue vídeos en varios formatos para que se puedan reproducir en una amplia variedad de navegadores y dispositivos. Para la entrega de streaming en vivo y bajo petición a varios clientes (dispositivos móviles, televisión, equipos, etc.), el contenido de audio y vídeo debe codificarse y comprimirse adecuadamente. Para ver cómo entregar y hacer streaming de este tipo de contenido, consulte [Quickstart: Encode and stream files](stream-files-dotnet-quickstart.md) (Inicio rápido: codificación y streaming de archivos).
* Haga streaming de eventos deportivos para una gran audiencia en línea, como fútbol, béisbol, deportes universitarios o de instituto, etc. 
* Retransmita reuniones y eventos públicos, como plenos de ayuntamientos, juntas municipales y reuniones de órganos legislativos.
* Analice contenido de audio o vídeos grabados. Por ejemplo, para lograr una mayor satisfacción del cliente, las organizaciones pueden extraer texto a voz y crear paneles e índices de búsqueda. A continuación, pueden extraer inteligencia de las quejas habituales, los orígenes de las quejas y otros datos relevantes.
* Cree un servicio de suscripción de vídeo y haga streaming de contenido protegido por DRM cuando un cliente (por ejemplo, un estudio cinematográfico) necesite restringir el acceso y el uso de una obra con derechos de autor.
* Entregue contenido sin conexión para su reproducción en aviones, trenes y automóviles. Es posible que un cliente tenga que descargar el contenido en su teléfono o tableta para reproducirlo cuando tenga previsto desconectarse de la red.
* Implemente una plataforma de vídeo educativa con Azure Media Services y [Azure Cognitive Services APIs](https://docs.microsoft.com/azure/#pivot=products&panel=ai) para subtítulos de voz a texto, traducción a varios idiomas, etc. 
* Use Azure Media Services en conjunción con [Azure Cognitive Services APIs](https://docs.microsoft.com/azure/#pivot=products&panel=ai) para agregar subtítulos y leyendas a los vídeos. con el fin de llegar a más público (por ejemplo, personas con discapacidades auditivas o usuarios que quieren leer el contenido en un idioma diferente).
* Habilite Azure CDN para lograr un gran escalado y mejorar el control de picos de carga puntuales (por ejemplo, al inicio de un evento de lanzamiento de un producto). 

## <a name="v3-capabilities"></a>Funcionalidades de v3

v3 se basa en una superficie de API unificada que expone la funcionalidad de administración y operaciones creada en Azure Resource Manager. 

Esta versión ofrece las siguientes funcionalidades:  

* **Transformaciones** que le ayudan a definir flujos de trabajo básicos de tareas de análisis o procesamiento multimedia. La transformación es una receta para procesar archivos de audio y vídeo. A continuación, puede aplicarla varias veces para procesar todos los archivos en la biblioteca de contenido si envía trabajos a la transformación.
* **Trabajos** para procesar (codificar o analizar) vídeos. Se puede especificar contenido de entrada en un trabajo con direcciones URL de HTTPS, direcciones URL de SAS o rutas de acceso a archivos ubicados en Azure Blob Storage. Actualmente, AMS v3 no admite la codificación de transferencia fragmentada a través de direcciones URL HTTPS.
* **Notificaciones** que supervisan el progreso o estado del trabajo y eventos de error, inicio o detención del canal en vivo. Las notificaciones están integradas en el sistema de notificaciones de Azure Event Grid. Puede suscribirse fácilmente a eventos de varios recursos en Azure Media Services. 
* Las plantillas de **Azure Resource Management** se pueden usar para crear e implementar transformaciones, puntos de conexión de streaming, canales, etc.
* **El control de acceso basado en rol** se puede establecer en recursos, lo que le permite bloquear el acceso a recursos específicos, como transformaciones, canales, etc.
* **SDK de cliente** en varios lenguajes: .NET, .NET Core, Python, Go, Java y Node.js.

## <a name="naming-conventions"></a>Convenciones de nomenclatura

Los nombres de recursos de Azure Media Services v3 (por ejemplo, recursos, trabajos y transformaciones) están sujetos a las restricciones de nomenclatura de Azure Resource Manager. De acuerdo con Azure Resource Manager, los nombres de los recursos siempre son únicos. Por lo tanto, puede usar cualquier cadena de identificador único (por ejemplo, GUID) para los nombres de recursos. 

Los nombres de recursos de Media Services no pueden incluir: '<', '>', '%', '&', ':', '&#92;', '?', '/', '*', '+', '.', el carácter de comilla simple o cualquier carácter de control. Todos los demás caracteres se permiten. La longitud máxima de un nombre de recurso es de 260 caracteres. 

Para más información sobre la nomenclatura de Azure Resource Manager, consulte: [Requisitos de nomenclatura](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/resource-api-reference.md#arguments-for-crud-on-resource) y [Convenciones de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).

## <a name="media-services-v3-api-design-principles"></a>Principios de diseño de API de Media Services v3

Uno de los principales principios de diseño de la versión v3 de la API es hacerla más segura. Las API de la versión v3 no devuelven secretos o credenciales en una operación **Get** o **List**. Las claves se hacen siempre NULL, se vacían o se sanean de la respuesta. Debe llamar a un método de acción independiente para obtener los secretos o las credenciales. Acciones independientes le permiten establecer distintos permisos de seguridad RBAC en caso de que algunas API recuperen o muestren secretos y otras no. Para más información acerca de cómo administrar el acceso mediante RBAC, consulte [Uso de RBAC para administrar el acceso](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest).

Estos son algunos ejemplos 

* no devolver los valores de ContentKey en la operación Get del objeto StreamingLocator, 
* no devolver las claves de restricción en la operación Get de ContentKeyPolicy, 
* no devolver la parte de la cadena de consulta de la dirección URL (para quitar la firma) de direcciones URL de entrada HTTP de los trabajos.

Consulte el ejemplo de [Get content key policy: .NET](get-content-key-policy-dotnet-howto.md).

## <a name="how-can-i-get-started-with-v3"></a>¿Cómo puedo comenzar con v3?

Como desarrollador, puede usar la [API de REST](https://go.microsoft.com/fwlink/p/?linkid=873030) de Media Services o bibliotecas de cliente que le permitan interactuar con la API de REST para crear, administrar y mantener fácilmente flujos de trabajo multimedia personalizados. Media Services v3 API se basa en la [especificación de OpenAPI](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media) (anteriormente conocida como Swagger).

[Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE) es una herramienta disponible para los clientes de Windows que desean obtener información acerca de Media Services. AMSE es una aplicación de Winforms o C# que permite cargar, descargar, codificar, transmitir vídeo bajo demanda y contenido en directo con Media Services. Esta herramienta es para aquellos clientes que deseen probar Media Services sin escribir ningún código. El código AMSE se proporciona como un recurso para los clientes que desean desarrollar con Media Services.

AMSE es un proyecto de código abierto en el que el soporte técnico lo facilita la comunidad (se pueden notificar los problemas a https://github.com/Azure/Azure-Media-Services-Explorer/issues)). Este proyecto ha adoptado el [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Código de conducta de código abierto de Microsoft). Para más información, consulte las [preguntas más frecuentes del código de conducta](https://opensource.microsoft.com/codeofconduct/faq/) o póngase en contacto con opencode@microsoft.com si tiene cualquier otra pregunta o comentario.
 
Las siguientes bibliotecas de cliente son compatibles con Azure Media Services: 

|Referencias de API|SDK y herramientas|Ejemplos|
|---|---|---|---|
|[Referencia de REST](https://aka.ms/ams-v3-rest-ref)|[SDK DE REST](https://aka.ms/ams-v3-rest-sdk)|[Ejemplos de REST Postman](https://github.com/Azure-Samples/media-services-v3-rest-postman)<br/>[API REST basada en Azure Resource Manager](https://github.com/Azure-Samples/media-services-v3-arm-templates)|
|[Referencia de la CLI de Azure](https://aka.ms/ams-v3-cli-ref)|[CLI de Azure](https://aka.ms/ams-v3-cli)|[Ejemplos de la CLI de Azure](https://github.com/Azure/azure-docs-cli-python-samples/tree/master/media-services)||
|[Referencia de .NET](https://aka.ms/ams-v3-dotnet-ref)|[SDK de .NET](https://aka.ms/ams-v3-dotnet-sdk)|[Ejemplos de .NET](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials)||
||[SDK de .NET Core](https://aka.ms/ams-v3-dotnet-sdk) (elija la pestaña **CLI de .NET**)|[Ejemplos de .NET Core](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials)||
|[Referencia de Java](https://aka.ms/ams-v3-java-ref)|[SDK de Java](https://aka.ms/ams-v3-java-sdk)||
|[Referencia de Node.js](https://aka.ms/ams-v3-nodejs-ref)|[SDK de Node.js](https://aka.ms/ams-v3-nodejs-sdk)|[Ejemplos de Node.js](https://github.com/Azure-Samples/media-services-v3-node-tutorials)||
|[Referencia de Python](https://aka.ms/ams-v3-python-ref)|[SDK de Python](https://aka.ms/ams-v3-python-sdk)||
|[Referencia de Go](https://aka.ms/ams-v3-go-ref)|[Go SDK](https://aka.ms/ams-v3-go-sdk)||
|Ruby|[SDK de Ruby](https://aka.ms/ams-v3-ruby-sdk)||

## <a name="next-steps"></a>Pasos siguientes

Para ver lo fácil que es empezar a codificar y hacer streaming de archivos de vídeo, consulte [Stream files](stream-files-dotnet-quickstart.md) (Streaming de vídeos). 

