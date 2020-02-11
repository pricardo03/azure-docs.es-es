---
title: Introducción a Azure Media Services v3
titleSuffix: Azure Media Services
description: Información general de alto nivel de Azure Media Services v3 con vínculos a inicios rápidos, tutoriales y ejemplos de código.
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
ms.date: 02/03/2020
ms.author: juliako
ms.custom: mvc
ms.openlocfilehash: 559050d11b6b85a9842c679b94e0e5b2fd5dd0f8
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2020
ms.locfileid: "76987720"
---
# <a name="azure-media-services-v3-overview"></a>Introducción a Azure Media Services v3

Azure Media Services es una plataforma basada en la nube que le permite compilar soluciones que consiguen un streaming de vídeo con calidad de difusión, mejoran la accesibilidad y la distribución, analizan el contenido, etc. Tanto si es un desarrollador de aplicaciones, un centro de llamadas, una autoridad gubernamental o una empresa de entretenimiento, Media Services le ayuda a crear aplicaciones que ofrecen experiencias multimedia de alta calidad a grandes audiencias de los navegadores y dispositivos móviles más populares en la actualidad.

Los SDK de Media Services v3 se basan en la [especificación OpenAPI (Swagger) de Media Services v3](https://aka.ms/ams-v3-rest-sdk).

> [!NOTE]
> Actualmente, no puede usar Azure Portal para administrar recursos de v3. Use la [API REST](https://aka.ms/ams-v3-rest-ref), la [CLI](https://aka.ms/ams-v3-cli-ref) o uno de los [SDK](media-services-apis-overview.md#sdks) admitidos.

## <a name="compliance-privacy-and-security"></a>Cumplimiento, privacidad y seguridad
 
Como recordatorio importante, debe cumplir todas las leyes aplicables, en el uso de Azure Media Services, y no puede usar Azure Media Services ni ningún otro servicio de Azure de forma que infrinja los derechos de otros o que puedan ser perjudiciales para otros usuarios. 

Antes de cargar los vídeos e imágenes, incluidos los datos biométricos, en Media Services para el procesamiento y el almacenamiento, debe (1) tener todos los derechos adecuados para los vídeos o imágenes; y (2) tener todos los consentimientos correspondientes de los distintos vídeos, lo que permite el uso y el almacenamiento de los datos biométricos en Media Services. 

Para más información sobre el cumplimiento, la privacidad y el modo en que Microsoft protege los datos en Azure Media Services, visite el [Centro de confianza de Azure](https://www.microsoft.com/trust-center/?rtc=1). En lo que respecta a las obligaciones de privacidad de Microsoft y al control de los datos, consulte la [declaración de privacidad](https://privacy.microsoft.com/PrivacyStatement), los [términos de Online Services](https://www.microsoft.com/licensing/product-licensing/products) ("OST") y el [anexo de procesamiento de datos](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=67) ("DPA"). La información sobre la eliminación y retención de datos de los clientes está disponible en DPA. Mediante el uso de Azure Media Services, acepta estar obligado por el OST, el DPA y la declaración de privacidad.

## <a name="what-can-i-do-with-media-services"></a>¿Qué puedo hacer con Media Services?

Media Services le permite crear una variedad de flujos de trabajo multimedia en la nube. Estos son algunos ejemplos de lo que puede hacer con Media Services:

* Entregue vídeos en varios formatos para que se puedan reproducir en una amplia variedad de navegadores y dispositivos. Para la entrega de streaming en vivo y bajo petición a varios clientes (dispositivos móviles, televisión, equipos, etc.), el contenido de audio y vídeo debe codificarse y comprimirse adecuadamente. Para ver cómo entregar y hacer streaming de este tipo de contenido, consulte [Quickstart: Encode and stream files](stream-files-dotnet-quickstart.md) (Inicio rápido: codificación y streaming de archivos).
* Haga streaming de eventos deportivos para una gran audiencia en línea, como fútbol, béisbol, deportes universitarios o de instituto, etc.
* Retransmita reuniones y eventos públicos, como plenos de ayuntamientos, juntas municipales y reuniones de órganos legislativos.
* Analice contenido de audio o vídeos grabados. Por ejemplo, para lograr una mayor satisfacción del cliente, las organizaciones pueden extraer texto a voz y crear paneles e índices de búsqueda. A continuación, pueden extraer inteligencia de las quejas habituales, los orígenes de las quejas y otros datos relevantes.
* Cree un servicio de suscripción de vídeo y haga streaming de contenido protegido por DRM cuando un cliente (por ejemplo, un estudio cinematográfico) necesite restringir el acceso y el uso de una obra con derechos de autor.
* Entregue contenido sin conexión para su reproducción en aviones, trenes y automóviles. Es posible que un cliente tenga que descargar el contenido en su teléfono o tableta para reproducirlo cuando tenga previsto desconectarse de la red.
* Implemente una plataforma de vídeo educativa con Azure Media Services y [Azure Cognitive Services APIs](https://docs.microsoft.com/azure/#pivot=products&panel=ai) para subtítulos de voz a texto, traducción a varios idiomas, etc.
* Use Azure Media Services en conjunción con [Azure Cognitive Services APIs](https://docs.microsoft.com/azure/#pivot=products&panel=ai) para agregar subtítulos y leyendas a los vídeos. con el fin de llegar a más público (por ejemplo, personas con discapacidades auditivas o usuarios que quieren leer el contenido en un idioma diferente).
* Habilite Azure CDN para lograr un gran escalado y mejorar el control de picos de carga puntuales (por ejemplo, al inicio de un evento de lanzamiento de un producto).

## <a name="how-can-i-get-started-with-v3"></a>¿Cómo puedo comenzar con v3? 

Aprenda a codificar y a empaquetar contenido, a hacer streaming de vídeos a petición, a realizar difusión en directo y a analizar vídeos con Media Services v3. En los tutoriales, las referencias de API y otra documentación, se muestra cómo entregar de forma segura secuencias de audio o vídeo, a petición y en directo, que se escalan a millones de usuarios.

> [!TIP]
> Antes de empezar a desarrollar, revise:<br/>* [Conceptos fundamentales](concepts-overview.md) (incluye conceptos importantes, como empaquetado, codificación y protección)<br/>* [Desarrollo con las API de Media Services v3](media-services-apis-overview.md) (incluye información acerca de cómo acceder a las API, convenciones de nomenclatura, etc.)

### <a name="sdks"></a>SDK

Empiece a desarrollar con los [SDK de cliente de Azure Media Services v3](media-services-apis-overview.md#sdks)

### <a name="quickstarts"></a>Guías de inicio rápido  

Los inicios rápidos muestran instrucciones básicas para que los nuevos clientes prueben rápidamente Media Services.

* [Transmitir en secuencias archivos de vídeo (.NET)](stream-files-dotnet-quickstart.md)
* [Hacer streaming de archivos de vídeo: CLI](stream-files-cli-quickstart.md)
* [Hacer streaming de archivos de vídeo: Node.js](stream-files-nodejs-quickstart.md)

### <a name="tutorials"></a>Tutoriales

Los tutoriales muestran procedimientos basados en un escenario para algunas de las principales tareas de Media Services.

* [Codificación de archivos remotos y streaming de vídeo: REST](stream-files-tutorial-with-rest.md)
* [Codificación de archivos cargados y streaming de vídeo: .NET](stream-files-tutorial-with-api.md)
* [Streaming en vivo: .NET](stream-live-tutorial-with-api.md)
* [Análisis del vídeo: .NET](analyze-videos-tutorial-with-api.md)
* [Cifrado dinámico AES-128: .NET](protect-with-aes128.md)

### <a name="samples"></a>Ejemplos

Use [este explorador de ejemplos](https://docs.microsoft.com/samples/browse/?products=azure-media-services) para examinar ejemplos de código de Azure Media Services.

### <a name="how-to-guides"></a>Guías paso a paso

Las guías paso a paso contienen ejemplos de código que muestran cómo realizar una tarea. En esta sección, encontrará muchos ejemplos. Estas son algunos de ellos:

* [Creación de una cuenta: CLI](create-account-cli-how-to.md)
* [Acceso a las API: CLI](access-api-cli-how-to.md)
* [Codificación con HTTPS como entrada de trabajo: .NET](job-input-from-http-how-to.md)  
* [Supervisión de eventos: Portal](monitor-events-portal-how-to.md)
* [Cifrado dinámico con varias soluciones de DRM: .NET](protect-with-drm.md) 
* [Procedimiento de codificación con una transformación personalizada: CLI](custom-preset-cli-howto.md)

## <a name="ask-questions-give-feedback-get-updates"></a>Formule preguntas, realice comentarios y obtenga actualizaciones

Consulte el artículo [Comunidad de Azure Media Services](media-services-community.md) para ver diferentes formas de formular preguntas, enviar comentarios y obtener actualizaciones de Media Services.

## <a name="next-steps"></a>Pasos siguientes

[Información sobre conceptos fundamentales](concepts-overview.md)

