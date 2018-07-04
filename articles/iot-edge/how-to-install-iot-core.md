---
title: Instalación de Azure IoT Edge en IoT Core | Microsoft Docs
description: Instalación de la instancia de Azure IoT Edge en tiempo de ejecución en un dispositivo Windows IoT Core
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: veyalla
ms.date: 03/05/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: ae5644a62b794dc8d6ace52f21a452fa70027d39
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37029567"
---
# <a name="install-the-iot-edge-runtime-on-windows-iot-core---preview"></a>Instalación de la instancia de IoT Edge en tiempo de ejecución en un dispositivo Windows IoT Core (versión preliminar)

Azure IoT Edge y [Windows IoT Core](https://docs.microsoft.com/windows/iot-core/) trabajan conjuntamente para habilitar la informática perimetral incluso en dispositivos pequeños. La instancia de Azure IoT Edge en tiempo de ejecución puede ejecutarse incluso en dispositivos de una sola placa (SBC) de tamaño reducido que son muy frecuentes en el sector de IoT. 

Este artículo le guía a través del aprovisionamiento de la instancia en tiempo de ejecución en una placa de desarrollo que ejecuta Windows IoT Core. 

**Actualmente Windows IoT Core admite Azure IoT Edge solo en procesadores Intel x64.**

## <a name="install-the-container-runtime"></a>Instalación del entorno de ejecución del contenedor

1. Configure la placa con la imagen de IoT Core, **compilación 17134 (RS4)**. 
1. Encienda el dispositivo y, luego, [inicie sesión de forma remota con PowerShell][lnk-powershell].
1. En la consola de PowerShell, instale el entorno de tiempo de ejecución del contenedor: 

   ```powershell
   Invoke-WebRequest https://master.dockerproject.org/windows/x86_64/docker-0.0.0-dev.zip -o temp.zip
   Expand-Archive .\temp.zip $env:ProgramFiles -f
   Remove-Item .\temp.zip
   $env:Path += ";$env:programfiles\docker"
   SETX /M PATH "$env:Path"
   dockerd --register-service
   start-service docker
   ```

   >[!NOTE]
   >Este entorno de tiempo de ejecución del contenedor es del servidor de compilación del proyecto Moby y es solo para fines de evaluación. No se ha probado, aprobado ni admitido en Docker.

## <a name="finish-installing"></a>Finalización de la instalación

Instale el demonio de seguridad de IoT Edge y configúrelo con las instrucciones de [este artículo][lnk-install-windows-on-windows].

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene un dispositivo que ejecuta la instancia de IoT Edge en tiempo de ejecución, obtenga información sobre cómo [implementar y supervisar los módulos de IoT Edge a escala][lnk-deploy].

<!--Links-->
[lnk-install-windows-on-windows]: how-to-install-iot-edge-windows-with-windows.md#download-the-edge-daemon-package-and-install
[lnk-powershell]: https://docs.microsoft.com/windows/iot-core/connect-your-device/powershell
[lnk-deploy]: how-to-deploy-monitor.md
[lnk-docker-install]: https://docs.docker.com/engine/installation/linux/docker-ce/binaries#install-server-and-client-binaries-on-windows
[lnk-docker-containers]: https://docs.microsoft.com/virtualization/windowscontainers/quick-start/quick-start-windows-10#2-switch-to-windows-containers