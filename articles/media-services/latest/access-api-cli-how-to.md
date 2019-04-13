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
ms.date: 01/28/2019
ms.author: juliako
ms.openlocfilehash: 1b872c5c2ff0f581300a843650d7434c7c526c84
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2019
ms.locfileid: "59545626"
---
# <a name="access-azure-media-services-api-with-the-azure-cli"></a>Acceso a la API de Azure Media Services con la CLI de Azure
 
Debe usar la autenticación de la entidad de servicio de Azure AD para conectarse a la API de Azure Media Services. La aplicación debe solicitar un token de Azure AD que tenga los parámetros siguientes:

* Punto de conexión de inquilino de Azure AD
* URI del recurso de Media Services
* URI del recurso de Media Services de REST
* Valores de aplicación de Azure AD: el identificador de cliente y el secreto de cliente

Para obtener más información, consulte [desarrollar con las API de Media Services v3](media-services-apis-overview.md).

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
