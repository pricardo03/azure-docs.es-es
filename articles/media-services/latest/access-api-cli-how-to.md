---
title: 'Acceso a la API de Azure Media Services: CLI de Azure | Microsoft Docs'
description: Siga los pasos de este artículo de procedimientos para acceder a Azure Media Services API.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: mvc
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: 06ca26ee9a8af7f49aba6a48a1831d30d5886851
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2020
ms.locfileid: "76896326"
---
# <a name="access-azure-media-services-api-with-the-azure-cli"></a>Acceso a la API de Azure Media Services con la CLI de Azure
 
Para usar la autenticación de entidades de servicio de Azure AD para conectarse a la API de Azure Media Services, la aplicación debe solicitar un token de Azure AD que tenga los parámetros siguientes:

* Punto de conexión de inquilino de Azure AD
* URI del recurso de Media Services
* URI del recurso de Media Services de REST
* Valores de aplicación de Azure AD: el identificador de cliente y el secreto de cliente

> [!IMPORTANT]
> Para obtener una explicación detallada, consulte [Acceso a las API de Media Services v3](media-services-apis-overview.md#accessing-the-azure-media-services-api).

En este artículo se muestra cómo usar la CLI de Azure para crear una entidad de servicio y una aplicación de Azure AD y obtener los valores necesarios para acceder a los recursos de Azure Media Services.

## <a name="prerequisites"></a>Prerequisites 

[Cree una cuenta de Media Services](create-account-cli-how-to.md).

Asegúrese de recordar los valores que usó para el nombre de la cuenta de Media Services y el nombre del grupo de recursos.
 
[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="see-also"></a>Consulte también

- [Escalado de unidades reservadas de multimedia: CLI](media-reserved-units-cli-how-to.md)
- [Creación de una cuenta de Media Services: CLI](create-account-cli-how-to.md) 
- [Restablecimiento de las credenciales de la cuenta: CLI](cli-reset-account-credentials.md)
- [Creación de recursos: CLI](cli-create-asset.md)
- [Carga de un archivo: CLI](cli-upload-file-asset.md)
- [Creación de transformaciones: CLI](cli-create-transform.md)
- [Codificación con una transformación personalizada: CLI](custom-preset-cli-howto.md)
- [Creación de trabajos: CLI](cli-create-jobs.md)
- [Creación de EventGrid: CLI](job-state-events-cli-how-to.md)
- [Publicación de un recurso: CLI](cli-publish-asset.md)
- [Filtrado: CLI](filters-dynamic-manifest-cli-howto.md)
- [CLI de Azure](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)

## <a name="next-steps"></a>Pasos siguientes

El punto de conexión de streaming desde el que va a transmitir el contenido debe estar en estado Running (En ejecución). El siguiente comando de la CLI de Azure inicia el punto de conexión de streaming predeterminado:

`az ams streaming-endpoint start -n default -a <amsaccount> -g <amsResourceGroup>`

