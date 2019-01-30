---
title: 'Introducción a las entidades de Azure Media Services: Azure | Microsoft Docs'
description: Este artículo proporciona información general de la entidades de Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 12/20/2018
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 902014ff3c242a18b3872ba490845eb0923f39a4
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54451194"
---
# <a name="azure-media-services-entities-overview"></a>Introducción a las entidades de Azure Media Services

En este artículo ofrece una breve introducción a las entidades de Azure Media Services y redirige a un artículo para obtener más información acerca de cada entidad. 

| Tema | DESCRIPCIÓN |
|---|---|
| [Filtros de cuenta y de recurso](filters-dynamic-manifest-overview.md)|Al entregar su contenido a los clientes (streaming de eventos en directo o vídeo bajo demanda), es posible que el cliente necesite más flexibilidad que la descrita en el archivo de manifiesto del recurso predeterminado. Azure Media Services le permite definir [filtros de cuenta](https://docs.microsoft.com/rest/api/media/accountfilters) y [filtros de recurso](https://docs.microsoft.com/rest/api/media/assetfilters). A continuación, puede usar **manifiestos dinámicos** basados en los filtros predefinidos. |
| [Recursos](assets-concept.md)|Una entidad de [recurso](https://docs.microsoft.com/rest/api/media/assets) contiene archivos digitales (como vídeos, audio, imágenes, colecciones de miniaturas, pistas de texto y subtítulos) y metadatos de estos archivos. Una vez que los archivos digitales se cargan en un recurso, se pueden utilizar en los flujos de trabajo de codificación, streaming y análisis de contenido de Media Services.|
| [Directivas de claves de contenido](content-key-policy-concept.md)|Puede usar Media Services para proteger su contenido multimedia desde el momento en que este deja el equipo y pasa a través del almacenamiento, el procesamiento y la entrega. Con Media Services puede entregar el contenido cifrado de forma dinámica en vivo y a petición con el Estándar de cifrado avanzado (AES-128) o cualquiera de los tres sistemas de administración de derechos digitales (DRM) principales: Microsoft PlayReady, Google Widevine y Apple FairPlay. Media Services también proporciona un servicio para entregar claves AES y licencias de DMR (PlayReady, Widevine y FairPlay) a los clientes autorizados.|
| [LiveEvents y LiveOutputs](live-events-outputs-concept.md)|Media Services permite entregar eventos en directo a sus clientes en la nube de Azure. Para configurar los eventos de streaming en directo en Media Services v3, debe estar familiarizado con los conceptos de [LiveEvents](https://docs.microsoft.com/rest/api/media/liveevents) y [LiveOutputs](https://docs.microsoft.com/rest/api/media/liveoutputs).|
| [Puntos de conexión de streaming](streaming-endpoint-concept.md)|Un [punto de conexión de streaming](https://docs.microsoft.com/rest/api/media/streamingendpoints) representa un servicio de streaming que puede entregar contenido directamente a una aplicación de reproducción de cliente o a Content Delivery Network (CDN) para su posterior distribución. La secuencia de salida del servicio de punto de conexión de streaming puede ser streaming en vivo o un recurso de vídeo a petición en la cuenta de Media Services. Cuando se crea una cuenta de Media Services, se genera automáticamente un punto de conexión de streaming **predeterminado** en estado detenido. No se puede eliminar el punto de conexión de streaming **predeterminado**. Es posible crear puntos de conexión de streaming adicionales en la cuenta. Para iniciar el streaming de vídeos, debe iniciar el punto de conexión de streaming desde el cual desea transmitir el vídeo. |
| [Localizadores de streaming](streaming-locators-concept.md)|Debe proporcionar a los clientes una dirección URL que pueden usar para reproducir archivos de audio o vídeo codificados, debe crear un [localizador de streaming](https://docs.microsoft.com/rest/api/media/streaminglocators) y compilar las direcciones URL de streaming.|
| [Directivas de streaming](streaming-policy-concept.md)| Las [directivas de streaming](https://docs.microsoft.com/rest/api/media/streamingpolicies) permiten definir los protocolos de streaming y las opciones de cifrado de los localizadores de streaming. Puede especificar el nombre de una directiva de streaming personalizada que creó o usar una de las directivas de streaming predefinidas que ofrece Media Services. <br/><br/>Al utilizar una directiva de streaming personalizada, debe diseñar un conjunto limitado de dichas directivas para su cuenta de Media Service y reutilizarlas para sus localizadores de streaming siempre que se necesiten las mismas opciones y protocolos de cifrado. No debe crear una nueva directiva de streaming para cada localizador de streaming.|
| [Transformaciones y trabajos](transforms-jobs-concept.md)|Use [transformaciones](https://docs.microsoft.com/rest/api/media/transforms) para configurar tareas comunes para codificar o analizar vídeos. Cada **Transformación** describe una receta, o un flujo de trabajo simple de tareas para procesar los archivos de vídeo o audio.<br/><br/>Un [trabajo](https://docs.microsoft.com/rest/api/media/jobs) es la solicitud real a Azure Media Services para aplicar la **transformación** a un contenido de vídeo o audio determinado. El **trabajo** especifica información como la ubicación del vídeo de entrada y la ubicación de la salida. Puede especificar la ubicación del vídeo de entrada mediante: direcciones URL HTTPS, direcciones URL SAS o un recurso.|

## <a name="next-steps"></a>Pasos siguientes

[Streaming de un archivo](stream-files-dotnet-quickstart.md)
