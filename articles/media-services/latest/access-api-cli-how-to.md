---
title: 'Acceso a la API de Azure Media Services: CLI de Azure | Microsoft Docs'
description: Siga los pasos de esta guía para acceder a la API de Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: mvc
ms.date: 05/01/2019
ms.author: juliako
ms.openlocfilehash: a27f7597ddc934b925d63507a816ac5816b682d6
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65151048"
---
# <a name="access-azure-media-services-api-with-the-azure-cli"></a>Acceso a la API de Azure Media Services con la CLI de Azure
 
Para usar la autenticación de entidad de servicio de Azure AD para conectarse a la API de Azure Media Services, la aplicación debe solicitar un token de Azure AD que tiene los siguientes parámetros:

* Punto de conexión de inquilino de Azure AD
* URI del recurso de Media Services
* URI del recurso de Media Services de REST
* Valores de aplicación de Azure AD: el identificador de cliente y el secreto de cliente

Para obtener una explicación detallada, consulte [las API de acceso a Media Services v3](media-services-apis-overview.md#accessing-the-azure-media-services-api).

En este artículo se muestra cómo usar la CLI de Azure para crear una entidad de servicio y una aplicación de Azure AD y obtener los valores necesarios para acceder a los recursos de Azure Media Services.

## <a name="prerequisites"></a>Requisitos previos 

[Cree una cuenta de Media Services](create-account-cli-how-to.md).

Asegúrese de recordar los valores que usó para el nombre de la cuenta de Media Services y el nombre del grupo de recursos.
 
[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="see-also"></a>Vea también

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
- [CLI de Azure](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)

## <a name="next-steps"></a>Pasos siguientes

El punto de conexión de Streaming desde el que van a transmitir el contenido debe estar en el estado en ejecución. El siguiente comando CLI inicia el extremo de Streaming de forma predeterminada:


`az ams streaming-endpoint start -n default -a <amsaccount> -g <amsResourceGroup>`
