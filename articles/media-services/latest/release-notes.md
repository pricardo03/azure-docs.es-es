---
title: Notas de la versión de Azure Media Services v3 | Microsoft Docs
description: Para mantenerse al día con los últimos desarrollos, en este artículo se proporcionan las actualizaciones más reciente en Azure Media Services v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/22/2018
ms.author: juliako
ms.openlocfilehash: db68f979239a5783338d99360209ae231a75c936
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49945042"
---
# <a name="azure-media-services-v3-release-notes"></a>Notas de la versión de Azure Media Services v3 

Para mantenerse al día con los avances más recientes, este artículo proporciona información acerca de los elementos siguientes:

* Versiones más recientes
* Problemas conocidos
* Corrección de errores
* Funciones obsoletas
* Planes de cambios

## <a name="october-2018---ga"></a>Octubre de 2018: disponibilidad general

En esta sección se describen las actualizaciones de octubre de Azure Media Services (AMS).

### <a name="rest-v3-ga-release"></a>Versión de disponibilidad general de REST v3

La [versión de disponibilidad general de REST v3](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01) incluye otras API de Live, filtros de manifiesto de nivel de cuenta y recurso y compatibilidad con DRM.

#### <a name="azure-resource-management"></a>Administración de recursos de Azure 

La compatibilidad con la administración de recursos de Azure habilita la API de operaciones y administración unificada (ahora todo en un solo lugar).

A partir de esta versión, puede usar plantillas de Resource Manager para crear eventos en vivo.

#### <a name="improvement-of-asset-operations"></a>Mejora de las operaciones de recurso 

Se han introducido las siguientes mejoras:

- Ingesta de direcciones URL de HTTP o direcciones URL de SAS de Azure Blob Storage.
- Especifique sus propios nombres de contenedor para los recursos. 
- Compatibilidad con la salida más sencilla para crear flujos de trabajo personalizados con Azure Functions.

#### <a name="new-transform-object"></a>Nuevo objeto Transform

El nuevo objeto **Transform** simplifica el modelo de codificación. El nuevo objeto facilita la creación y el uso compartido de la codificación de plantillas y valores preestablecidos de Resource Manager. 

#### <a name="azure-active-directory-authentication-and-rbac"></a>Autenticación y RBAC de Azure Active Directory

El control de acceso basado en rol (RBAC) y la autenticación de Azure AD habilitan transformaciones seguras, objetos LiveEvent, directivas de clave de contenido o recursos por rol o usuarios en Azure AD.

#### <a name="client-sdks"></a>SDK de cliente  

Idiomas admitidos en Media Services v3: .NET Core, Java, Node.js, Ruby, Typescript, Python y Go.

#### <a name="live-encoding-updates"></a>Actualizaciones de Live Encoding

Se presentan las siguientes actualizaciones de Live Encoding:

- Nuevo modo de latencia baja de Live (10 segundos de principio a fin).
- Compatibilidad mejorada de RTMP (mayor estabilidad y compatibilidad con más codificadores de origen).
- Ingesta segura de RTMPS.

    Al crear un objeto LiveEvent, ahora obtiene cuatro direcciones URL de ingesta. Las cuatro direcciones URL de ingesta son casi idénticas, tienen el mismo token de streaming (AppId) y solo se diferencian en componente de número de puerto. Dos de las direcciones URL son principales y de copia de seguridad para RTMPS. 
- Soporte técnico de transcodificación las 24 horas. 
- Soporte técnico de señalización de anuncios mejorado en RTMP a través de SCTE35.

#### <a name="improved-event-grid-support"></a>Soporte técnico mejorado de Event Grid

Puede ver las siguientes mejoras de soporte técnico de Event Grid:

- Integración de Azure Event Grid para facilitar el desarrollo con Logic Apps y Azure Functions. 
- Suscríbase a eventos sobre codificación, canales en vivo y mucho más.

### <a name="cmaf-support"></a>Compatibilidad con CMAF

Compatibilidad con el cifrado de CMAF y "cbcs" para reproductores Apple HLS (iOS 11+) y MPEG-DASH que admiten CMAF.

### <a name="video-indexer"></a>Video Indexer

La versión de disponibilidad general Video Indexer se anunció en agosto. Para información reciente sobre las características admitidas actualmente, vea [Novedades de Video Indexer](../../cognitive-services/video-indexer/video-indexer-overview.md?toc=/azure/media-services/video-indexer/toc.json&bc=/azure/media-services/video-indexer/breadcrumb/toc.json). 

### <a name="plans-for-changes"></a>Planes de cambios

#### <a name="azure-cli-20"></a>CLI de Azure 2.0
 
Próximamente estará disponible el módulo de la CLI de Azure 2.0 que incluye operaciones en todas las características (como Live, directivas de clave de contenido, filtros de cuenta/recurso, directivas de streaming). 

### <a name="known-issues"></a>Problemas conocidos

Solo los clientes que usan la API de versión preliminar con filtros de cuenta o recurso se ven afectados por el problema siguiente.

Si creó filtros de cuenta o recurso entre el 28/09 y el 12/10 con las API o la CLI de Media Services v3, debe quitar todos los filtros de cuenta y recurso y volver a crearlos debido a un conflicto de versiones. 

## <a name="may-2018---preview"></a>Mayo de 2018: versión preliminar

### <a name="net-sdk"></a>SDK de .NET

Las características siguiente están disponibles en el SDK de .NET:

* **Transformaciones** y **trabajos** para codificar o analizar el contenido multimedia. Para ver ejemplos, consulte los artículos sobre [streaming de archivos](stream-files-tutorial-with-api.md) y [análisis](analyze-videos-tutorial-with-api.md).
* **StreamingLocators** para publicar y transmitir contenido a los dispositivos de usuarios finales.
* **StreamingPolicies** y **ContentKeyPolicies** para configurar la entrega de claves y la protección de contenido (DRM) al entregar el contenido.
* **LiveEvents** y **LiveOutputs** para configurar la ingesta y el archivo de contenido de streaming en vivo.
* **Recursos** para almacenar y publicado contenido multimedia en Azure Storage. 
* **StreamingEndpoints** para configurar y escalar el empaquetado dinámico, cifrado y streaming tanto de contenido multimedia en vivo como a petición.

### <a name="known-issues"></a>Problemas conocidos

* Al enviar un trabajo, puede especificar que se ingiera el vídeo de origen mediante direcciones URL HTTPS, URL SAS o rutas de acceso a archivos ubicados en Azure Blob Storage. Actualmente, AMS v3 no admite la codificación de transferencia fragmentada a través de direcciones URL HTTPS.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Información general](media-services-overview.md)
