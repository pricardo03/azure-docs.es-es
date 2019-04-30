---
title: 'Actualización de la versión de IoT Edge en los dispositivos: Azure IoT Edge | Microsoft Docs'
description: Cómo actualizar los dispositivos IoT Edge para ejecutar las versiones más recientes del demonio de seguridad y el entorno de ejecución de IoT Edge
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/17/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: a3dd7f78362b5f5c99dc4a74fe0a32c4d26be5b7
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62125940"
---
# <a name="update-the-iot-edge-security-daemon-and-runtime"></a>Actualice el archivo de configuración del demonio de seguridad y el entorno de ejecución de IoT Edge.

Como el servicio de IoT Edge versiones nuevas versiones, desea actualizar dispositivos de IoT Edge para las últimas características y mejoras de seguridad. En este artículo se proporciona información sobre cómo actualizar los dispositivos IoT Edge cuando hay una versión nueva disponible. 

Es necesario actualizar dos componentes de un dispositivo IoT Edge si quiere pasar a una versión más reciente. El primero es el demonio de seguridad, que se ejecuta en el dispositivo e inicia los módulos en tiempo de ejecución cuando se inicia el dispositivo. Actualmente, el demonio de seguridad solo puede actualizarse desde el propio dispositivo. El segundo componente es el entorno de ejecución, formado por los módulos Centro de IoT Edge y Agente de IoT Edge. En función del modo de estructurar la implementación, el entorno de ejecución puede actualizarse desde el dispositivo o de forma remota. 

Para obtener la versión más reciente de Azure IoT Edge, consulte [Versiones de Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases).

>[!IMPORTANT]
>Si está ejecutando Azure IoT Edge en un dispositivo de Windows, no actualice a la versión 1.0.5 si una de las siguientes afirmaciones es aplicable a su dispositivo: 
>* No ha actualizado el dispositivo a la compilación 17763 de Windows. La versión 1.0.5 de IoT Edge no es compatible con compilaciones de Windows anteriores a 17763.
>* Ejecuta los módulos de Java o Node.js en el dispositivo de Windows. Omita la versión 1.0.5 incluso si ha actualizado el dispositivo de Windows a la compilación más reciente. 
>
>Para obtener más información acerca de la versión 1.0.5 de IoT Edge, consulte las [notas de la versión 1.0.5](https://github.com/Azure/azure-iotedge/releases/tag/1.0.5). Para obtener más información acerca de cómo evitar que las herramientas de desarrollo de la actualización a la versión más reciente, consulte [el blog de desarrolladores de IoT](https://devblogs.microsoft.com/iotdev/).


## <a name="update-the-security-daemon"></a>Actualización del demonio de seguridad

El demonio de seguridad de IoT Edge es un componente nativo que debe actualizarse mediante el administrador de paquetes en el dispositivo IoT Edge. 

Compruebe la versión del demonio de seguridad que se ejecuta en el dispositivo mediante el comando `iotedge version`. 

### <a name="linux-devices"></a>Dispositivos Linux

En los dispositivos Linux, use apt-get o el administrador de paquetes adecuado para actualizar el demonio de seguridad. 

```bash
apt-get update
apt-get install libiothsm iotedge
```

### <a name="windows-devices"></a>Dispositivos Windows

En los dispositivos Windows, use el script de PowerShell para desinstalar y, a continuación, volver a instalar el demonio de seguridad. El script de instalación extrae automáticamente la versión más reciente del demonio de seguridad. 

Desinstale el demonio de seguridad en una sesión de PowerShell de administrador. 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Uninstall-SecurityDaemon
```

Ejecuta el `Uninstall-SecurityDaemon` comando sin parámetros solo quita el demonio de seguridad de su dispositivo, junto con las dos imágenes de contenedor en tiempo de ejecución. El archivo config.yaml se mantiene en el dispositivo, así como los datos del motor de contenedor Moby. Mantener la información de configuración significa que no tendrá que proporcionar la cadena de conexión o la información del servicio Device Provisioning para el dispositivo nuevo durante el proceso de instalación. 

Vuelva a instalar el demonio de seguridad según si el dispositivo IoT Edge usa contenedores de Windows o contenedores de Linux. Reemplazar la frase **\<Windows o Linux\>** con los sistemas operativos de contenedor adecuado. Use la marca **-ExistingConfig** para apuntar al archivo config.yaml existente en el dispositivo. 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -ExistingConfig -ContainerOS <Windows or Linux>
```

Si desea instalar una versión específica del demonio de seguridad, descargue el archivo adecuado iotedged-windows.zip de las [versiones de IoT Edge](https://github.com/Azure/azure-iotedge/releases). A continuación, use el parámetro `-OfflineInstallationPath` para que apunte a la ubicación del archivo. Para más información, consulte la [Instalación sin conexión](how-to-install-iot-edge-windows.md#offline-installation).

## <a name="update-the-runtime-containers"></a>Actualización de los contenedores del entorno de ejecución

El modo de actualizar los contenedores del Centro de IoT Edge y del Agente de IoT Edge depende de si usa etiquetas graduales (por ejemplo, 1.0) o etiquetas específicas (por ejemplo, 1.0.2) en la implementación. 

Compruebe la versión de los módulos Agente de IoT Edge y Centro de IoT Edge instalada actualmente en el dispositivo mediante los comandos `iotedge logs edgeAgent` o `iotedge logs edgeHub`. 

  ![Búsqueda de la versión del contenedor en los registros](./media/how-to-update-iot-edge/container-version.png)

### <a name="understand-iot-edge-tags"></a>Información sobre las etiquetas de IoT Edge

Las imágenes del Agente de IoT Edge y del centro de IoT Edge se etiquetan con la versión de IoT Edge a la que están asociadas. Hay dos maneras diferentes de usar etiquetas con las imágenes del entorno de ejecución: 

* **Etiquetas graduales**: use solo los dos primeros valores del número de versión para obtener la imagen más reciente que coincida con esos dígitos. Por ejemplo, 1.0 se actualiza cada vez que hay una nueva versión para que apunte a la versión 1.0.x más reciente. Si el entorno de ejecución del contenedor en el dispositivo IoT Edge extrae la imagen de nuevo, se actualizan los módulos del entorno de ejecución a la versión más reciente. Este enfoque se recomienda para fines de desarrollo. Las implementaciones de Azure Portal tienen como valor predeterminado las etiquetas graduales. 
* **Etiquetas específicas**: use los tres valores del número de versión para establecer explícitamente la versión de la imagen. Por ejemplo, la versión 1.0.2 no cambiará después de su lanzamiento inicial. Puede declarar un nuevo número de versión del manifiesto de implementación cuando esté listo para actualizar. Este enfoque se recomienda para fines de producción.

### <a name="update-a-rolling-tag-image"></a>Actualización de una imagen de etiqueta gradual

Si usa etiquetas graduales en la implementación (por ejemplo, mcr.microsoft.com/azureiotedge-hub:**1.0**), deberá forzar el entorno de ejecución del contenedor en el dispositivo para extraer la versión más reciente de la imagen. 

Elimine la versión local de la imagen del dispositivo IoT Edge. En equipos con Windows, al desinstalar el demonio de seguridad también se quitan las imágenes en tiempo de ejecución, por lo que no es necesario volver a realizar este paso. 

```bash
docker rmi mcr.microsoft.com/azureiotedge-hub:1.0
docker rmi mcr.microsoft.com/azureiotedge-agent:1.0
```

Es posible que tenga que usar la marca `-f` de fuerza para eliminar las imágenes. 

El servicio de IoT Edge extraerá las versiones más recientes de las imágenes del entorno de ejecución y volverá a iniciarlas automáticamente en el dispositivo. 

### <a name="update-a-specific-tag-image"></a>Actualización de una etiqueta específica

Si usa etiquetas específicas en la implementación (por ejemplo, mcr.microsoft.com/azureiotedge-hub:**1.0.2**), solo tiene que actualizar la etiqueta en el manifiesto de implementación y aplicar los cambios en el dispositivo. 

En Azure Portal, las imágenes de implementación del entorno de ejecución se declaran en la sección **Configurar las opciones avanzadas del entorno en tiempo de ejecución de Edge**. 

![Configurar opciones de tiempo de ejecución de edge avanzadas](./media/how-to-update-iot-edge/configure-runtime.png)

En un manifiesto de implementación de JSON, actualice las imágenes del módulo en la sección **systemModules**. 

```json
"systemModules": {
  "edgeAgent": {
    "type": "docker",
    "settings": {
      "image": "mcr.microsoft.com/azureiotedge-agent:1.0.2",
      "createOptions": ""
    }
  },
  "edgeHub": {
    "type": "docker",
    "status": "running",
    "restartPolicy": "always",
    "settings": {
      "image": "mcr.microsoft.com/azureiotedge-hub:1.0.2",
      "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"5671/tcp\":[{\"HostPort\":\"5671\"}], \"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}]}}}"
    }
  }
},
```

## <a name="next-steps"></a>Pasos siguientes

Consulte las últimas [versiones de Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases).

Permanezca actualizado con los anuncios y actualizaciones recientes del [blog Internet of Things](https://azure.microsoft.com/blog/topics/internet-of-things/) (Internet de las cosas) 