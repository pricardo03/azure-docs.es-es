---
title: Instalación de Azure IoT Edge en IoT Core | Microsoft Docs
description: Instalación de la instancia de Azure IoT Edge en tiempo de ejecución en un dispositivo Windows IoT Core
author: kgremban
manager: philmea
ms.author: kgremban
ms.reviewer: veyalla
ms.date: 03/05/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 17afc095fa0500ca98559a9523282d75949faa60
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2018
ms.locfileid: "51564833"
---
# <a name="install-the-iot-edge-runtime-on-windows-iot-core---preview"></a>Instalación de la instancia de IoT Edge en tiempo de ejecución en un dispositivo Windows IoT Core (versión preliminar)

Azure IoT Edge y [Windows IoT Core](https://docs.microsoft.com/windows/iot-core/) trabajan conjuntamente para habilitar la informática perimetral incluso en dispositivos pequeños. La instancia de Azure IoT Edge en tiempo de ejecución puede ejecutarse incluso en dispositivos de una sola placa (SBC) de tamaño reducido que son muy frecuentes en el sector de IoT. 

Este artículo le guía a través del aprovisionamiento de la instancia en tiempo de ejecución en una placa de desarrollo que ejecuta Windows IoT Core. 

**Actualmente Windows IoT Core admite Azure IoT Edge solo en procesadores Intel x64.**

## <a name="install-the-container-runtime"></a>Instalación del entorno de ejecución del contenedor

1. Configure la placa con la imagen de IoT Core, **compilación 17134 (RS4)**. 
1. Active el dispositivo y, luego, [inicie sesión de forma remota con PowerShell](https://docs.microsoft.com/windows/iot-core/connect-your-device/powershell).
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

Instale el demonio de seguridad de IoT Edge y configúrelo con las instrucciones de [este artículo](how-to-install-iot-edge-windows-with-windows.md).

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene un dispositivo que ejecuta el entorno de ejecución de Azure IoT Edge, obtenga información sobre cómo [implementar y supervisar los módulos de IoT Edge a escala](how-to-deploy-monitor.md).