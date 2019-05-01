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
ms.date: 04/15/2019
ms.author: juliako
ms.openlocfilehash: 8374e4c49012a2c49de41001be0fdb30f9151332
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60733511"
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

## <a name="next-steps"></a>Pasos siguientes

[CLI de Azure](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
