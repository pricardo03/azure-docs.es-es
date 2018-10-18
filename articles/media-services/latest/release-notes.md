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
ms.date: 09/25/2018
ms.author: juliako
ms.openlocfilehash: ed2550c1df4645933fb968c54ee536995c810136
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47219336"
---
# <a name="azure-media-services-v3-preview-release-notes"></a>Notas de la versión de Azure Media Services v3 (versión preliminar) 

Para mantenerse al día con los avances más recientes, este artículo proporciona información acerca de los elementos siguientes:

* Versiones más recientes
* Problemas conocidos
* Corrección de errores
* Funciones obsoletas
* Planes de cambios

## <a name="may-07-2018"></a>07 de mayo de 2018

### <a name="net-sdk"></a>SDK de .NET

Las características siguiente están disponibles en el SDK de .NET:

1. **Transformaciones** y **trabajos** para codificar o analizar el contenido multimedia. Para ver ejemplos, consulte los artículos sobre [streaming de archivos](stream-files-tutorial-with-api.md) y [análisis](analyze-videos-tutorial-with-api.md).
2. **StreamingLocators** para publicar y transmitir contenido a los dispositivos de usuarios finales.
3. **StreamingPolicies** y **ContentKeyPolicies** para configurar la entrega de claves y la protección de contenido (DRM) al entregar el contenido.
4. **LiveEvents** y **LiveOutputs** para configurar la ingesta y el archivo de contenido de streaming en vivo.
5. **Recursos** para almacenar y publicado contenido multimedia en Azure Storage. 
6. **StreamingEndpoints** para configurar y escalar el empaquetado dinámico, cifrado y streaming tanto de contenido multimedia en vivo como a petición.

### <a name="known-issues"></a>Problemas conocidos

* Al enviar un trabajo, puede especificar que se ingiera el vídeo de origen mediante direcciones URL HTTPS, URL SAS o rutas de acceso a archivos ubicados en Azure Blob Storage. Actualmente, AMS v3 no admite la codificación de transferencia fragmentada a través de direcciones URL HTTPS.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Información general](media-services-overview.md)
