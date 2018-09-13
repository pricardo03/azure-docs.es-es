---
title: Dispositivos simulados personalizados para la implementación de IoT - Azure | Microsoft Docs
description: En esta guía práctica se muestra cómo implementar dispositivos simulados personalizados en el acelerador de soluciones de simulación de dispositivos.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 08/14/2018
ms.topic: conceptual
ms.openlocfilehash: e51d0330c3ed804bff8cdb06265bb8c39141733f
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2018
ms.locfileid: "43346647"
---
# <a name="deploy-a-new-simulated-device"></a>Implementar un nuevo dispositivo simulado

Los aceleradores de soluciones Supervisión remota y Simulación de dispositivos le permiten definir sus propios dispositivos simulados. En este artículo se muestra cómo implementar un tipo de dispositivo refrigerador personalizado, y un nuevo tipo de dispositivo de bombilla para el acelerador de soluciones de simulación de dispositivos.

En los pasos de este artículo se supone que ya ha completado la guía paso a paso [Create and test a new simulated device](iot-accelerators-remote-monitoring-create-simulated-device.md) (Crear y probar un nuevo dispositivo simulado) y que tiene los archivos que definen los nuevos tipos de dispositivos referentes al refrigerador personalizado y de bombilla.

Los pasos de esta guía práctica le muestran cómo:

1. Usar SSH para obtener acceso al sistema de archivos de la máquina virtual que hospeda el acelerador de soluciones de simulación de dispositivos.

1. Configurar Docker para cargar los modelos de dispositivo desde una ubicación fuera del contenedor Docker.

1. Ejecutar una simulación mediante archivos de modelo de dispositivo personalizados.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Recuerde que para completar los pasos de esta guía paso a paso, necesita una suscripción activa a Azure.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para completar esta guía paso a paso, necesita:

- Una instancia implementada del [acelerador de soluciones de simulación de dispositivos](https://www.azureiotsolutions.com/Accelerators#solutions/types/DS).
- Un shell local de **Bash** para ejecutar los comandos `ssh` y `scp`. En Windows, una manera fácil de instalar **Bash** consiste en instalar [git](https://git-scm.com/download/win).
- Los archivos de modelo de dispositivo personalizado, como los descritos en [Create and test a new simulated device](iot-accelerators-remote-monitoring-create-simulated-device.md) (Crear y probar un nuevo dispositivo simulado).

[!INCLUDE [iot-solution-accelerators-access-vm](../../includes/iot-solution-accelerators-access-vm.md)]

## <a name="configure-docker"></a>Configurar Docker

En esta sección, debe configurar Docker para que cargue los archivos de modelo de dispositivo desde la carpeta **/tmp/devicemodels** en la máquina virtual, en lugar de hacerlo desde el contenedor Docker. Ejecute los comandos en esta sección en un shell de **Bash** en su máquina local:

1. Use SSH para conectarse a la máquina virtual en Azure desde su máquina local. El siguiente comando asume que la dirección IP pública de la máquina virtual **vm-vikxv** es **104.41.128.108**; reemplace este valor con la dirección IP pública de su máquina virtual de la sección anterior:

   ```sh
    ssh azureuser@104.41.128.108
    ```

    Siga las instrucciones para iniciar sesión en la máquina virtual con la contraseña que configuró en la sección anterior.

1. Configure el servicio de simulación del dispositivo para cargar los modelos del dispositivo desde fuera del contenedor. En primer lugar, abra el archivo de configuración de Docker:

    ```sh
    sudo nano /app/docker-compose.yml
    ```

    Busque la configuración del contenedor **devicesimulation** y edite la configuración **Volúmenes** tal como se muestra en el fragmento de código siguiente:

    ```yml
    # To mount custom device models, upload the files to the VM, edit and uncomment the following line:
          - /tmp/devicemodels:/app/webservice/data/devicemodels:ro
    # To mount a custom service configuration, create a custom file, edit and uncomment the following line:
    #     - /app/custom-device-simulation-appsettings.ini:/app/webservice/appsettings.ini:ro
    ```

    Guarde los cambios.

1. Cree las carpetas para almacenar los archivos JSON y los archivos de script:

    ```sh
    sudo mkdir -p /tmp/devicemodels/scripts
    ```

    Mantenga la ventana **Bash** y su sesión SSH abierta.

1. Copie los archivos de modelo de dispositivo personalizado en la máquina virtual. Ejecute este comando en otro shell de **Bash** en la máquina donde creó sus modelos de dispositivo personalizados. En primer lugar, vaya a la carpeta local que contiene los archivos JSON de modelo de dispositivo. Con los siguientes comandos se asume que la dirección IP pública de la máquina virtual es **104.41.128.108**: reemplace este valor con la dirección IP pública de su máquina virtual. Escriba la contraseña de la máquina virtual cuando se le solicite:

    ```sh
    scp *json azureuser@104.41.128.108:/tmp/devicemodels
    cd scripts
    scp *js azureuser@104.41.128.108:/tmp/devicemodels/scripts
    ```

1. Reinicie el contenedor de Docker de la simulación de dispositivo para usar los nuevos modelos de dispositivo. Ejecute los siguientes comandos en el shell de **Bash** con la sesión SSH abierta en la máquina virtual:

    ```sh
    sudo /app/start.sh
    ```

    Si quiere ver el estado de los contenedores de Docker en ejecución y sus id. de contenedor, use el siguiente comando:

    ```sh
    docker ps
    ```

    Si quiere ver el registro desde el contenedor de simulación del dispositivo, ejecute el siguiente comando. Reemplace el id. del contenedor con el id. del contenedor de simulación del dispositivo:

    ```sh
    docker logs -f 5d3f3e78822e
    ```

## <a name="run-simulation"></a>Ejecutar simulación

Ahora puede ejecutar una simulación usando sus modelos de dispositivos personalizados:

1. Inicie la interfaz de usuario web de Simulación de dispositivo desde los [aceleradores de soluciones de Microsoft Azure IoT](https://www.azureiotsolutions.com/Accelerators#dashboard).

1. Use la interfaz de usuario web para configurar y ejecutar una simulación mediante uno de los modelos de dispositivo personalizado.

1. Al ejecutar una simulación, haga clic en **View IoT Hub metrics in the Azure portal** (Ver las métricas de IoT Hub en Azure Portal) para supervisar la simulación. Como alternativa, puede usar el siguiente comando de la CLI de Azure para supervisar el dispositivo en el tráfico en la nube. Reemplace el nombre de la instancia de IoT Hub con el nombre del centro:

    ```azurecli-interactive
    az iot hub monitor-events -n contoso-simulation9dc75
    ```

## <a name="clean-up-resources"></a>Limpieza de recursos

Si tiene previsto explorar más a fondo, deje implementado el acelerador de soluciones de simulación de dispositivo.

Si ya no necesita el acelerador de soluciones, elimínelo de la página [Soluciones aprovisionadas](https://www.azureiotsolutions.com/Accelerators#dashboard). Para ello, selecciónelo y, a continuación, haga clic en **Eliminar solución**.

## <a name="next-steps"></a>Pasos siguientes

En esta guía se muestra cómo implementar modelos de dispositivos personalizados en el acelerador de soluciones de simulación de dispositivos. El siguiente paso sugerido es obtener más información sobre el [esquema de modelos de dispositivos](iot-accelerators-device-simulation-device-schema.md).
