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
ms.openlocfilehash: 257fe51cae245708816cd9a7bb0c33b6edf5aa05
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58756002"
---
# <a name="access-azure-media-services-api-with-the-azure-cli"></a>Acceso a la API de Azure Media Services con la CLI de Azure
 
Debe usar la autenticación de la entidad de servicio de Azure AD para conectarse a la API de Azure Media Services. La aplicación debe solicitar un token de Azure AD que tenga los parámetros siguientes:

* Punto de conexión de inquilino de Azure AD
* URI del recurso de Media Services
* URI del recurso de Media Services de REST
* Valores de aplicación de Azure AD: el identificador de cliente y el secreto de cliente

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
