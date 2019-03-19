---
title: Conectarse y administrar dispositivos de borde del cuadro de datos de Microsoft Azure a través de la interfaz de Windows PowerShell | Microsoft Docs
description: Describe cómo conectarse y, a continuación, administrar el borde del cuadro de datos a través de la interfaz de Windows PowerShell.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: b3effdbace2be582bfe85d0402088f8aa0d96fe7
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57556457"
---
# <a name="manage-an-azure-data-box-edge-device-via-windows-powershell-preview"></a>Administración de un dispositivo de borde del cuadro de datos de Azure a través de Windows PowerShell (versión preliminar)

Solución de borde del cuadro de datos de Azure le permite procesar datos y enviarlo a través de la red en Azure. En este artículo se describe algunas de las tareas de configuración y administración para el dispositivo de borde del cuadro de datos. Puede usar el portal de Azure, IU web local o la interfaz de Windows PowerShell para administrar el dispositivo.

En este artículo se centra en las tareas que realizar mediante la interfaz de PowerShell.

En este artículo incluye los siguientes procedimientos:

- Conectarse a la interfaz de PowerShell
- Iniciar una sesión de soporte técnico
- Crear un paquete de soporte
- Carga del certificado
- Restablecer el dispositivo
- Ver información del dispositivo
- Obtener registros de proceso
- Supervisar y solucionar problemas de los módulos de proceso

> [!IMPORTANT]
> Borde del cuadro de datos de Azure está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="connect-to-the-powershell-interface"></a>Conectarse a la interfaz de PowerShell

[!INCLUDE [Connect to admin runspace](../../includes/data-box-edge-gateway-connect-minishell.md)]

## <a name="start-a-support-session"></a>Iniciar una sesión de soporte técnico

[!INCLUDE [Connect to support runspace](../../includes/data-box-edge-gateway-connect-support.md)]

## <a name="create-a-support-package"></a>Crear un paquete de soporte

[!INCLUDE [Create a support package](../../includes/data-box-edge-gateway-create-support-package.md)]

## <a name="upload-certificate"></a>Carga del certificado

[!INCLUDE [Upload certificate](../../includes/data-box-edge-gateway-upload-certificate.md)]

## <a name="view-device-information"></a>Ver información del dispositivo
 
[!INCLUDE [View device information](../../includes/data-box-edge-gateway-view-device-info.md)]

## <a name="reset-your-device"></a>Restablecer el dispositivo

[!INCLUDE [Reset your device](../../includes/data-box-edge-gateway-deactivate-device.md)]

## <a name="get-compute-logs"></a>Obtener registros de proceso

Si el rol de ejecución está configurado en el dispositivo, también puede obtener los registros de proceso a través de la interfaz de PowerShell.

1. [Conectarse a la interfaz de PowerShell](#connect-to-the-powershell-interface).
2. Use la `Get-AzureDataBoxEdgeComputeRoleLogs` para obtener los registros de proceso para el dispositivo.

    El ejemplo siguiente muestra el uso de este cmdlet:

    ```
    Get-AzureDataBoxEdgeComputeRoleLogs -Path "\\hcsfs\logs\myacct" -Credential "username/password" -RoleInstanceName "IotRole" -FullLogCollection
    ```
    Esta es una descripción de los parámetros utilizados para el cmdlet: 
    - `Path`: Proporcione una ruta de acceso de red al recurso compartido donde desea crear el paquete de registro del proceso.
    - `Credential`: Proporcione el nombre de usuario y la contraseña para el recurso compartido de red.
    - `RoleInstanceName`: Proporciona esta cadena `IotRole` para este parámetro.
    - `FullLogCollection`: Este parámetro garantiza que el paquete de registro contendrá todos los registros de proceso. De forma predeterminada, el paquete de registros contiene solo un subconjunto de registros.


## <a name="next-steps"></a>Pasos siguientes

- Implemente rápidamente [Azure Data Box Edge](data-box-edge-deploy-prep.md) en Azure Portal.
