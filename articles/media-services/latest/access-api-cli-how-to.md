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
ms.openlocfilehash: 43f9443e4b5cd700500bd9803f2737ed9e0aa633
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55223175"
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

## <a name="next-steps"></a>Pasos siguientes

[Streaming de un archivo](stream-files-dotnet-quickstart.md)

## <a name="see-also"></a>Otras referencias

[CLI de Azure](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
