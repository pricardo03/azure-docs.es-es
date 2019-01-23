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
ms.date: 12/08/2018
ms.author: juliako
ms.openlocfilehash: 882f4650c0a3d558ee06c96658b779f9f0c76f76
ms.sourcegitcommit: 3ba9bb78e35c3c3c3c8991b64282f5001fd0a67b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2019
ms.locfileid: "54322488"
---
# <a name="get-a-signing-key-from-the-existing-policy"></a>Obtención de una clave de firma de la directiva existente

Uno de los principales principios de diseño de la versión v3 de la API es hacerla más segura. Las API de la versión v3 no devuelven secretos o credenciales en una operación **Get** o **List**. Las claves se hacen siempre NULL, se vacían o se sanean de la respuesta. Debe llamar a un método de acción independiente para obtener los secretos o las credenciales. Acciones independientes le permiten establecer distintos permisos de seguridad RBAC en caso de que algunas API recuperen o muestren secretos y otras no. Para más información acerca de cómo administrar el acceso mediante RBAC, consulte [Uso de RBAC para administrar el acceso](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest).

Estos son algunos ejemplos 

* no devolver los valores de ContentKey en la operación Get del objeto StreamingLocator, 
* no devolver las claves de restricción en la operación Get de ContentKeyPolicy, 
* no devolver la parte de la cadena de consulta de la dirección URL (para quitar la firma) de direcciones URL de entrada HTTP de los trabajos.

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
