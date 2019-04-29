---
title: Ejecución de Azure IoT Edge en máquinas virtuales Ubuntu | Microsoft Docs
description: Instrucciones de configuración de Azure IoT Edge en máquinas virtuales de Azure Marketplace Ubuntu 16.04
author: gregman-msft
manager: arjmands
ms.reviewer: kgremban
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 03/29/2019
ms.author: gregman
ms.openlocfilehash: 7062bd2dbd8c375b8dd3fad348e5cc26de8f36d2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60595127"
---
# <a name="run-azure-iot-edge-on-ubuntu-virtual-machines"></a>Ejecución de Azure IoT Edge en máquinas virtuales Ubuntu

El entorno de ejecución de Azure IoT Edge es lo que convierte a un dispositivo en un dispositivo IoT Edge. El entorno de ejecución se puede implementar en dispositivos tan pequeños como un Raspberry Pi o tan grandes como un servidor industrial. Una vez que un dispositivo está configurado con el entorno de ejecución de IoT Edge, puede empezar a implementar lógica de negocios en él desde la nube.

Para más información acerca de cómo funciona el entorno de ejecución de IoT Edge y los componentes que se incluyen, consulte [Información del entorno de ejecución de Azure IoT Edge y su arquitectura](iot-edge-runtime.md).

En este artículo se enumeran los pasos para ejecutar el entorno de ejecución de Azure IoT Edge en una máquina virtual de Ubuntu 16.04 con la [oferta de Azure Marketplace de Azure IoT Edge en Ubuntu](https://aka.ms/azure-iot-edge-ubuntuvm) preconfigurada. 

En el primer arranque, la máquina virtual de Azure IoT Edge en Ubuntu preinstala la versión más reciente del entorno de ejecución de Azure IoT Edge. También incluye un script para establecer la cadena de conexión y, a continuación, reiniciar el entorno de ejecución, que puede activarse de forma remota a través del portal de la máquina virtual de Azure o la línea de comandos de Azure, lo que le permite configurar y conectar fácilmente el dispositivo de IoT Edge sin iniciar un SSH ni una sesión de escritorio remoto. Este script esperará para establecer la cadena de conexión hasta que el cliente de IoT Edge se instale completamente de manera que no tenga que compilarlo en su automatización.

## <a name="deploy-from-the-azure-marketplace"></a>Implementación desde Azure Marketplace
1.  Navegue hasta la oferta de Marketplace [Azure IoT Edge on Ubuntu](https://aka.ms/azure-iot-edge-ubuntuvm) (Azure IoT Edge en Ubuntu) o busque "Azure IoT Edge en Ubuntu" en [Azure Marketplace](https://azuremarketplace.microsoft.com/).
2.  Seleccione **Obtenerla ahora** y, luego, **Continuar** en el cuadro de diálogo siguiente.
3.  Una vez en Azure Portal, seleccione **Crear** y siga el asistente para implementar la máquina virtual. 
    *   Si es la primera vez que prueba una máquina virtual, es más fácil usar una contraseña y habilitar el SSH en el menú del puerto de entrada público. 
    *   Si tiene una carga de trabajo con uso intensivo de recursos, debe actualizar el tamaño de máquina virtual mediante la adición de más CPU o memoria.
4.  Una vez implementada la máquina virtual, configúrela para conectarse a IoT Hub:
    1.  Copie la cadena de conexión del dispositivo desde el dispositivo de IoT Edge creado en IoT Hub (puede seguir la guía [Registro de un nuevo dispositivo Azure IoT Edge desde Azure Portal](how-to-register-device-portal.md) si no está familiarizado con este proceso)
    1.  Seleccione el recurso de máquina virtual recién creado en Azure Portal y abra la opción **Ejecución de comando**.
    1.  Seleccione la opción **RunShellScript**.
    1.  Ejecute el siguiente script a través de la ventana de comandos con la cadena de conexión del dispositivo: `/etc/iotedge/configedge.sh “{device_connection_string}”`.
    1.  Seleccione **Ejecutar**.
    1.  Espere unos minutos y, entonces, la pantalla debe proporcionar un mensaje de confirmación que indica la cadena de conexión se estableció correctamente.


## <a name="deploy-from-the-azure-portal"></a>Implementar desde el portal de Azure
En Azure Portal, busque "Azure IoT Edge" y seleccione **Ubuntu Server 16.04 LTS + Azure IoT Edge runtime** (Ubuntu Server 16.04 LTS + entorno de ejecución Azure IoT Edge) para iniciar el flujo de trabajo de creación de la máquina virtual. Desde ahí, siga los pasos 3 y 4 de las instrucciones de "Implementación desde Azure Marketplace" anteriores.

## <a name="deploy-from-azure-cli"></a>Implementación desde la CLI de Azure
1. Si se trata de la primera vez que implementa una máquina virtual desde la CLI, necesita habilitar la implementación mediante programación para la suscripción de Azure:
   1. Abra la oferta de Marketplace [Azure IoT Edge on Ubuntu](https://aka.ms/azure-iot-edge-ubuntuvm) (Azure IoT Edge en Ubuntu).
   1. Seleccione **Obtenerla ahora** y, luego, **Continuar** en el cuadro de diálogo siguiente.
   1. Seleccione **¿Quiere realizar una implementación mediante programación? Empiece ahora** en la parte inferior del cuadro de diálogo en el portal.
   1. Haga clic en el botón **Habilitar** situado en la página **Configurar la implementación mediante programación** y, a continuación, haga clic en **Guardar**.
1. Si está usando la CLI de Azure en el escritorio, empiece por iniciar sesión:

   ```azurecli-interactive
   az login
   ```
    
1. Si tiene varias suscripciones, seleccione la que desea utilizar:
   1. Muestre las suscripciones:
    
      ```azurecli-interactive
      az account list --output table
      ```
    
   1. Copie el campo SubscriptionID para la suscripción que desea utilizar.
   1. Ejecute este comando con el identificador que acaba de copiar:
    
      ```azurecli-interactive 
      az account set -s {SubscriptionId}
      ```
    
1. Cree un grupo de recursos nuevo (o especifique uno que ya exista en los pasos siguientes):

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus2
   ```
    
1. Creación de una máquina virtual:

   ```azurecli-interactive
   az vm create --resource-group IoTEdgeResources --name EdgeVM –-image microsoft_iot_edge:iot_edge_vm_ubuntu:ubuntu_1604_edgeruntimeonly:latest --admin-username azureuser --generate-ssh-keys --size Standard_DS1_v2
   ```

1. Establezca la cadena de conexión del dispositivo (puede seguir la guía [Registro de un nuevo dispositivo Azure IoT Edge con la CLI de Azure](how-to-register-device-cli.md) si no está familiarizado con este proceso):

   ```azurecli-interactive
   az vm run-command invoke -g IoTEdgeResources -n EdgeVM --command-id RunShellScript --script "/etc/iotedge/configedge.sh '{device_connection_string}'"
   ```

Si desea conectarse mediante SSH a la máquina virtual después de la instalación, use el elemento publicIpAddress con el comando: `ssh azureuser@{publicIpAddress}`


## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene un dispositivo IoT Edge aprovisionado con el entorno de ejecución instalado, puede [implementar módulos de IoT Edge](how-to-deploy-modules-portal.md).

Si tiene problemas con el tiempo de ejecución de IoT Edge que se instale correctamente, consulte el [solución de problemas](troubleshoot.md) página.

Para actualizar una instalación existente a la versión más reciente de IoT Edge, vea [Actualice el archivo de configuración del demonio de seguridad y el entorno de ejecución de IoT Edge](how-to-update-iot-edge.md).
