---
title: Ejecutar Azure IoT Edge en máquinas virtuales de Windows Server | Microsoft Docs
description: Instrucciones de configuración de Azure IoT Edge en Windows Server Marketplace Virtual Machines
author: gregman-msft
manager: arjmands
ms.reviewer: kgremban
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 03/29/2019
ms.author: gregman
ms.openlocfilehash: be7479d3f042d6e64428a07e0509907b78595200
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65159786"
---
# <a name="run-azure-iot-edge-on-windows-server-virtual-machines"></a>Ejecutar Azure IoT Edge en máquinas virtuales de Windows Server
El entorno de ejecución de Azure IoT Edge es lo que convierte a un dispositivo en un dispositivo IoT Edge. El entorno de ejecución se puede implementar en dispositivos tan pequeños como un Raspberry Pi o tan grandes como un servidor industrial. Una vez que un dispositivo está configurado con el entorno de ejecución de IoT Edge, puede empezar a implementar lógica de negocios en él desde la nube.

Para más información acerca de cómo funciona el entorno de ejecución de IoT Edge y los componentes que se incluyen, consulte [Información del entorno de ejecución de Azure IoT Edge y su arquitectura](iot-edge-runtime.md).

En este artículo se enumera los pasos para ejecutar el tiempo de ejecución de Azure IoT Edge en una máquina virtual de Windows Server 2019 mediante el [Windows Server](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WindowsServer?tab=Overview) oferta de Azure Marketplace. Siga las instrucciones de [instalar el runtime de Azure IoT Edge](how-to-install-iot-edge-windows.md) en Windows para su uso con otras versiones.

> [!NOTE]
> El runtime de IoT Edge en Windows Server está en [versión preliminar pública](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="deploy-from-the-azure-marketplace"></a>Implementación desde Azure Marketplace
1.  Navegue hasta la [Windows Server](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WindowsServer?tab=Overview) oferta de Azure Marketplace o buscando "Windows Server" en [Azure Marketplace](https://azuremarketplace.microsoft.com/)
2.  Seleccione **obtener ahora** 
3.  En **plan de Software**, busque "2019 Datacenter Server Core con contenedores de Windows Server" y, a continuación, seleccione **continuar** en el cuadro de diálogo siguiente.
    * También puede usar estas instrucciones para otras versiones de Windows Server con contenedores
4.  Una vez en Azure Portal, seleccione **Crear** y siga el asistente para implementar la máquina virtual. 
    *   Si es la primera vez que se pruebe una máquina virtual, es más fácil utilizar una contraseña y para habilitar RDP y SSH en el menú de puerto de entrada público. 
    *   Si tiene una carga de trabajo con uso intensivo de recursos, debe actualizar el tamaño de máquina virtual mediante la adición de más CPU o memoria.
5.  Una vez implementada la máquina virtual, configúrela para conectarse a IoT Hub:
    1.  Copie la cadena de conexión del dispositivo desde el dispositivo de IoT Edge creado en IoT Hub (puede seguir la guía [Registro de un nuevo dispositivo Azure IoT Edge desde Azure Portal](how-to-register-device-portal.md) si no está familiarizado con este proceso)
    1.  Seleccione el recurso de máquina virtual recién creado en Azure Portal y abra la opción **Ejecución de comando**.
    1.  Seleccione el **RunPowerShellScript** opción
    1.  Copie esta secuencia de comandos en la ventana de comandos con la cadena de conexión del dispositivo: 
        ```powershell
        . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
        Install-IoTEdge -Manual -DeviceConnectionString '<connection-string>'
        ```
    1.  Ejecute el script para instalar el runtime de Edge y establecer la cadena de conexión seleccionando **ejecutar**
    1.  Después de un minuto o dos, debería ver un mensaje que se haya instalado el runtime de Edge y se ha aprovisionado correctamente.


## <a name="deploy-from-the-azure-portal"></a>Implementar desde el portal de Azure
1. Desde el portal de Azure, busque "Windows Server" y seleccione **2019 de Windows Server Datacenter** para iniciar el flujo de trabajo de creación de máquina virtual. 
2. Desde **seleccionar un plan de software** elija "2019 Datacenter Server Core con contenedores de Windows Server" y luego seleccione **Create**
3. Complete las instrucciones del paso 5 de la "implementación de Azure Marketplace" anteriores.

## <a name="deploy-from-azure-cli"></a>Implementación desde la CLI de Azure
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
   1. Ejecute este comando con el identificador que copió:
    
      ```azurecli-interactive 
      az account set -s {SubscriptionId}
      ```
    
1. Cree un grupo de recursos nuevo (o especifique uno que ya exista en los pasos siguientes):

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus2
   ```
    
1. Creación de una máquina virtual:

   ```azurecli-interactive
   az vm create -g IoTEdgeResources -n EdgeVM --image MicrosoftWindowsServer:WindowsServer:2019-Datacenter-Core-with-Containers:latest  --admin-username azureuser --generate-ssh-keys --size Standard_DS1_v2
   ```
   * Este comando le solicitará una contraseña, pero puede agregar la opción `--admin-password` establecerlo más fácilmente en una secuencia de comandos
   * La imagen de Windows Server Core tiene comandos línea compatibilidad solo con Escritorio remoto, por lo tanto, si desea que la experiencia de escritorio completa, especifique `MicrosoftWindowsServer:WindowsServer:2019-Datacenter-with-Containers:latest` como imagen

1. Establezca la cadena de conexión del dispositivo (puede seguir la guía [Registro de un nuevo dispositivo Azure IoT Edge con la CLI de Azure](how-to-register-device-cli.md) si no está familiarizado con este proceso):

   ```azurecli-interactive
   az vm run-command invoke -g IoTEdgeResources -n EdgeVM --command-id RunPowerShellScript --script ". {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `Install-IoTEdge -Manual -DeviceConnectionString '<connection-string>'"
   ```

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene un dispositivo IoT Edge aprovisionado con el entorno de ejecución instalado, puede [implementar módulos de IoT Edge](how-to-deploy-modules-portal.md).

Si tiene problemas con el runtime de Edge que se instale correctamente, consulte el [solución de problemas](troubleshoot.md) página.

Para actualizar una instalación existente a la versión más reciente de IoT Edge, vea [Actualice el archivo de configuración del demonio de seguridad y el entorno de ejecución de IoT Edge](how-to-update-iot-edge.md).

Más información sobre el uso de las máquinas virtuales de Windows en el [documentación sobre máquinas virtuales de Windows](https://docs.microsoft.com/azure/virtual-machines/windows/).

Si desea conectarse mediante SSH a la máquina virtual después de la instalación, siga el [instalación de OpenSSH para Windows Server](https://docs.microsoft.com/windows-server/administration/openssh/openssh_install_firstuse#installing-openssh-with-powershell) guía mediante powershell remoto o de escritorio remoto.
