---
title: Conectarse y administrar dispositivos de borde del cuadro de datos de Microsoft Azure a través de la interfaz de Windows PowerShell | Microsoft Docs
description: Describe cómo conectarse y, a continuación, administrar el borde del cuadro de datos a través de la interfaz de Windows PowerShell.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/29/2019
ms.author: alkohli
ms.openlocfilehash: b4d047f4266d11a5f6b77f33054eb93e31f7090b
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58791582"
---
# <a name="manage-an-azure-data-box-edge-device-via-windows-powershell"></a>Administración de un dispositivo de borde del cuadro de datos de Azure a través de Windows PowerShell

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

## <a name="connect-to-the-powershell-interface"></a>Conectarse a la interfaz de PowerShell

[!INCLUDE [Connect to admin runspace](../../includes/data-box-edge-gateway-connect-minishell.md)]

## <a name="create-a-support-package"></a>Crear un paquete de soporte

[!INCLUDE [Create a support package](../../includes/data-box-edge-gateway-create-support-package.md)]

## <a name="upload-certificate"></a>Carga del certificado

[!INCLUDE [Upload certificate](../../includes/data-box-edge-gateway-upload-certificate.md)]

También puede cargar certificados de IoT Edge para habilitar una conexión segura entre el dispositivo IoT Edge y los dispositivos de nivel inferiores que pueden conectarse a él. Hay tres certificados de IoT Edge (*.pem* formato) que necesita instalar:

- Certificado de CA raíz o el propietario de la entidad de certificación
- Certificado de entidad de certificación de dispositivo
- Certificado de clave de dispositivo

El ejemplo siguiente muestra el uso de este cmdlet para instalar certificados de IoT Edge:

```
Set-HcsCertificate -Scope IotEdge -RootCACertificateFilePath "\\hcfs\root-ca-cert.pem" -DeviceCertificateFilePath "\\hcfs\device-ca-cert.pem\" -DeviceKeyFilePath "\\hcfs\device-key-cert.pem" -Credential "username/password"
```

Para obtener más información sobre certificados, vaya a [certificados de Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/iot-edge-certs) o [instalar certificados en una puerta de enlace](https://docs.microsoft.com/azure/iot-edge/how-to-create-transparent-gateway#install-certificates-on-the-gateway).

## <a name="view-device-information"></a>Ver información del dispositivo

 
[!INCLUDE [View device information](../../includes/data-box-edge-gateway-view-device-info.md)]

## <a name="reset-your-device"></a>Restablecer el dispositivo

[!INCLUDE [Reset your device](../../includes/data-box-edge-gateway-deactivate-device.md)]

## <a name="get-compute-logs"></a>Obtener registros de proceso

Si el rol de ejecución está configurado en el dispositivo, también puede obtener los registros de proceso a través de la interfaz de PowerShell.

1. [Conectarse a la interfaz de PowerShell](#connect-to-the-powershell-interface).
2. Use la `Get-AzureDataBoxEdgeComputeRoleLogs` para obtener los registros de proceso para el dispositivo.

    El ejemplo siguiente muestra el uso de este cmdlet:

    ```powershell
    Get-AzureDataBoxEdgeComputeRoleLogs -Path "\\hcsfs\logs\myacct" -Credential "username/password" -RoleInstanceName "IotRole" -FullLogCollection
    ```

    Esta es una descripción de los parámetros utilizados para el cmdlet:
    - `Path`: Proporcione una ruta de acceso de red al recurso compartido donde desea crear el paquete de registro del proceso.
    - `Credential`: Proporcione el nombre de usuario y la contraseña para el recurso compartido de red.
    - `RoleInstanceName`: Proporciona esta cadena `IotRole` para este parámetro.
    - `FullLogCollection`: Este parámetro garantiza que el paquete de registro contendrá todos los registros de proceso. De forma predeterminada, el paquete de registros contiene solo un subconjunto de registros.

## <a name="monitor-and-troubleshoot-compute-modules"></a>Supervisar y solucionar problemas de los módulos de proceso

[!INCLUDE [Monitor and troubleshoot compute modules](../../includes/data-box-edge-monitor-troubleshoot-compute.md)]


## <a name="next-steps"></a>Pasos siguientes

- Implemente rápidamente [Azure Data Box Edge](data-box-edge-deploy-prep.md) en Azure Portal.