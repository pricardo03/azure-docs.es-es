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
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: 5d7c338dc9d5028552d9b33802982ffda97a56f0
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49389860"
---
# <a name="access-azure-media-services-api-with-the-azure-cli"></a>Acceso a la API de Azure Media Services con la CLI de Azure
 
Debe usar la autenticación de la entidad de servicio de Azure AD para conectarse a la API de Azure Media Services. La aplicación debe solicitar un token de Azure AD que tenga los parámetros siguientes:

* Punto de conexión de inquilino de Azure AD
* URI del recurso de Media Services
* URI del recurso de Media Services de REST
* Valores de aplicación de Azure AD: el identificador de cliente y el secreto de cliente

En este artículo se muestra cómo usar la CLI de Azure para crear una entidad de servicio y una aplicación de Azure AD y obtener los valores necesarios para acceder a los recursos de Azure Media Services.

## <a name="prerequisites"></a>Requisitos previos 

Cree una cuenta de Azure Media Services tal como se describe en [este inicio rápido](create-account-cli-quickstart.md).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en [Azure Portal](http://portal.azure.com) e inicie **CloudShell** para ejecutar comandos de la CLI, tal como se muestra en los siguientes pasos.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Si decide instalar y usar la CLI localmente, para este artículo es preciso la CLI de Azure versión 2.0 o posterior. Ejecute `az --version` para encontrar la versión que tiene. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli). 

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Streaming de un archivo](stream-files-dotnet-quickstart.md)

## <a name="see-also"></a>Otras referencias

[CLI de Azure](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
