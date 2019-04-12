---
title: 'Obtención de una clave de firma de la directiva existente mediante el SDK de .NET de Media Services v3: Azure | Microsoft Docs'
description: En este tema se muestra cómo obtener una clave de firma de la directiva existente mediante el SDK de .NET de Media Services v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: seodec18
ms.date: 04/09/2019
ms.author: juliako
ms.openlocfilehash: 49cc2b8c151053377f8f1da0792f10a06695b332
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2019
ms.locfileid: "59496325"
---
# <a name="get-a-signing-key-from-the-existing-policy"></a>Obtención de una clave de firma de la directiva existente

Uno de los principales principios de diseño de la versión v3 de la API es hacerla más segura. las API de V3 no devuelven secretos o las credenciales en **obtener** o **lista** operaciones. Las claves se hacen siempre NULL, se vacían o se sanean de la respuesta. El usuario debe llamar a un método de acción independiente para obtener los secretos o las credenciales. El **lector** rol no puede llamar a las operaciones por lo que no puede llamar a operaciones como Asset.ListContainerSas, StreamingLocator.ListContentKeys, ContentKeyPolicies.GetPolicyPropertiesWithSecrets. Tener acciones independientes le permite establecer permisos de seguridad más granulares de RBAC en un rol personalizado si lo desea.

Para obtener más información, consulte [cuentas RBAC y Media Services](rbac-overview.md)

En el ejemplo de este artículo se muestra cómo usar .NET para obtener una clave de firma a partir de la directiva existente. 
 
## <a name="download"></a>Descargar 

Clone un repositorio de GitHub que contenga el ejemplo de .NET completo en la máquina mediante el siguiente comando:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```
 
El ejemplo de ContentKeyPolicy con secretos se encuentra en la carpeta [EncryptWithDRM](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/EncryptWithDRM).

## <a name="get-contentkeypolicy-with-secrets"></a>Obtención de ContentKeyPolicy con secretos 

Para obtener la clave, use **GetPolicyPropertiesWithSecretsAsync**, tal y como se muestra en el ejemplo siguiente.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="next-steps"></a>Pasos siguientes

[Diseño del sistema de protección de contenido con DRM múltiple con control de acceso](design-multi-drm-system-with-access-control.md) 
