---
title: Adición de un dispositivo Azure IoT Edge a Azure IoT Central | Microsoft Docs
description: Como operador, agregará un dispositivo Azure IoT Edge a Azure IoT Central.
author: rangv
ms.author: rangv
ms.date: 10/22/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: ae80a624ed1f85a1f59fea79b152a4bc31067ad1
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2019
ms.locfileid: "73892641"
---
# <a name="tutorial-add-an-azure-iot-edge-device-to-your-azure-iot-central-application-preview-features"></a>Tutorial: Adición de un dispositivo Azure IoT Edge a la aplicación de Azure IoT Central (características de la versión preliminar)

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

En este tutorial se explica cómo agregar y configurar un *dispositivo Azure IoT Edge* a la aplicación de Microsoft Azure IoT Central. En este tutorial, se ha elegido una máquina virtual Linux habilitada para Azure IoT Edge de Azure Marketplace.

Este tutorial se compone de dos partes:

* En primer lugar, como operador, aprenderá a realizar el aprovisionamiento "primero en la nube" de un dispositivo Azure IoT Edge.
* Luego, aprenderá a realizar el aprovisionamiento "primero en el dispositivo" de un dispositivo Azure IoT Edge.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Agregar un nuevo dispositivo Azure IoT Edge
> * Configurar el dispositivo Azure IoT Edge para facilitar el aprovisionamiento mediante la clave SAS
> * Ver los paneles y el estado de los módulos en IoT Central
> * Enviar comandos a un módulo que se ejecuta en el dispositivo Azure IoT Edge
> * Establecer propiedades en un módulo que se ejecuta en el dispositivo Azure IoT Edge

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, necesitará una aplicación de Azure IoT Central. Siga este inicio rápido para [crear una aplicación de Azure IoT Central](./quick-deploy-iot-central.md).

## <a name="enable-azure-iot-edge-enrollment-group"></a>Habilitación del grupo de inscripción de Azure IoT Edge
Habilite las claves de SAS para el grupo de inscripción de Azure IoT Edge en la página Administration (Administración).

![Plantilla de dispositivo: Azure IoT Edge](./media/tutorial-add-edge-as-leaf-device/groupenrollment.png)

## <a name="cloud-first-azure-iot-edge-device-provisioning"></a>Aprovisionamiento "primero en la nube" de un dispositivo Azure IoT Edge   
En esta sección, creará un dispositivo Azure IoT Edge con la **plantilla de sensor de entorno** y aprovisionará un dispositivo. Haga clic en Devices (Dispositivos) en el panel de navegación izquierdo y seleccione la plantilla Environment Sensor. 

![Plantilla de dispositivo: Azure IoT Edge](./media/tutorial-add-edge-as-leaf-device/deviceexplorer.png)

Haga clic en **+ New** (+ Nuevo) y escriba un nombre y un identificador de dispositivo adecuados en su caso. 

![Plantilla de dispositivo: Azure IoT Edge](./media/tutorial-add-edge-as-leaf-device/cfdevicecredentials.png)

El dispositivo entra en el modo **Registered** (Registrado).

![Plantilla de dispositivo: Azure IoT Edge](./media/tutorial-add-edge-as-leaf-device/cfregistered.png)

## <a name="deploy-an-azure-iot-edge-enabled-linux-vm"></a>Implementación de una máquina virtual Linux habilitada para Azure IoT Edge

>Nota: Puede optar por usar cualquier máquina o dispositivo. Sistema operativo: Linux o Windows.

En este tutorial, se ha elegido una máquina virtual Linux habilitada para Azure IoT, que se puede crear en Azure. Vaya a [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft_iot_edge.iot_edge_vm_ubuntu?tab=Overview) y haga clic en el botón **Obtenerla ahora**. 

![Azure Marketplace](./media/tutorial-add-edge-as-leaf-device/cfmarketplace.png)

Haga clic en **Continue** (Continuar).

![Azure Marketplace](./media/tutorial-add-edge-as-leaf-device/cfmarketplacecontinue.png)


Irá a Azure Portal. Haga clic en el botón **Crear**.

![Azure Marketplace](./media/tutorial-add-edge-as-leaf-device/cfubuntu.png)

Seleccione la suscripción, cree preferiblemente un grupo de recursos, seleccione oeste de EE. UU.2 como ubicación de disponibilidad de la máquina virtual y escriba el usuario y la contraseña. No olvide que el usuario y la contraseña serán necesarios en futuros pasos. Haga clic en **Revisar y crear**.

![Máquina virtual de Ubuntu](./media/tutorial-add-edge-as-leaf-device/cfvm.png)

Una vez validados, haga clic en **Crear**.

![Máquina virtual de Ubuntu](./media/tutorial-add-edge-as-leaf-device/cfvmvalidated.png)

Los recursos tardan en crearse unos minutos. Haga clic en **Ir al recurso**.

![Máquina virtual de Ubuntu](./media/tutorial-add-edge-as-leaf-device/cfvmdeploymentcomplete.png)

### <a name="provision-vm-as-azure-iot-edge-device"></a>Aprovisionamiento de una máquina virtual como dispositivo Azure IoT Edge 

En Soporte técnico y solución de problemas en el panel de navegación izquierdo, haga clic en Consola serie.

![Máquina virtual de Ubuntu](./media/tutorial-add-edge-as-leaf-device/cfserialconsole.png)

Verá una pantalla similar a la siguiente:

![Máquina virtual de Ubuntu](./media/tutorial-add-edge-as-leaf-device/cfconsole.png)

Presione Entrar, proporcione el nombre de usuario y la contraseña cuando se le solicite y, de nuevo, presione Entrar. 

![Máquina virtual de Ubuntu](./media/tutorial-add-edge-as-leaf-device/cfconsolelogin.png)

Para ejecutar un comando como administrador o raíz, ejecute el comando **sudo su –** .

![Máquina virtual de Ubuntu](./media/tutorial-add-edge-as-leaf-device/cfsudo.png)

Compruebe la versión en tiempo de ejecución de Azure IoT Edge. La versión actual con disponibilidad general es 1.0.8 o posterior.

![Máquina virtual de Ubuntu](./media/tutorial-add-edge-as-leaf-device/cfconsoleversion.png)

Instale el editor Vim o, si lo prefiere, use Nano. 

![Máquina virtual de Ubuntu](./media/tutorial-add-edge-as-leaf-device/cfconsolevim.png)

![Máquina virtual de Ubuntu](./media/tutorial-add-edge-as-leaf-device/cfvim.png)

Edite el archivo config.yaml de Azure IoT Edge.

![Máquina virtual de Ubuntu](./media/tutorial-add-edge-as-leaf-device/cfconsoleconfig.png)

Desplácese hacia abajo y comente la parte de la cadena de conexión del archivo YAML. 

**Antes**

![Máquina virtual de Ubuntu](./media/tutorial-add-edge-as-leaf-device/cfmanualprovisioning.png)

**Después** (presione ESC y la letra "a" minúscula para empezar a editar)

![Máquina virtual de Ubuntu](./media/tutorial-add-edge-as-leaf-device/cfmanualprovisioningcomments.png)

Quite la marca de comentario de la parte de la clave simétrica del archivo YAML. 

**Antes**

![Máquina virtual de Ubuntu](./media/tutorial-add-edge-as-leaf-device/cfconsolesymmcomments.png)

**Después**

![Máquina virtual de Ubuntu](./media/tutorial-add-edge-as-leaf-device/cfconsolesymmuncomments.png)

Vaya a IoT Central y obtenga el identificador de ámbito, el identificador de dispositivo y la clave simétrica del dispositivo Azure IoT Edge. ![Conexión del dispositivo](./media/tutorial-add-edge-as-leaf-device/cfdeviceconnect.png)

Vaya al cuadro de Linux y reemplace el identificador de ámbito y el identificador de registro por el identificador de dispositivo y la clave simétrica.

Presione **Esc** y escriba **:wq!** . Luego, presione **Entrar** para guardar los cambios.

Reinicie Azure IoT Edge para procesar los cambios y presione **Entrar**.

![Conexión del dispositivo](./media/tutorial-add-edge-as-leaf-device/cfrestart.png)

Escriba **iotedge list**; a los pocos minutos verá tres módulos implementados:

![Conexión del dispositivo](./media/tutorial-add-edge-as-leaf-device/cfconsolemodulelist.png)


## <a name="iot-central-device-explorer"></a>Explorador de dispositivos de IoT Central 

En IoT Central, el dispositivo se moverá al estado aprovisionado.

![Conexión del dispositivo](./media/tutorial-add-edge-as-leaf-device/cfprovisioned.png)

La pestaña Modules (Módulos) mostrará el estado del dispositivo y del módulo en IoT Central. 

![Conexión del dispositivo](./media/tutorial-add-edge-as-leaf-device/cfiotcmodulestatus.png)


Las propiedades de la nube se mostrarán en un formulario (a partir de la plantilla de dispositivo que creó en los pasos anteriores). Escriba los valores y haga clic en **Save** (Guardar). 

![Conexión del dispositivo](./media/tutorial-add-edge-as-leaf-device/deviceinfo.png)

Icono del panel

![Conexión del dispositivo](./media/tutorial-add-edge-as-leaf-device/dashboard.png)

En este tutorial aprendió lo siguiente:

* Agregar un nuevo dispositivo Azure IoT Edge
* Configurar el dispositivo Azure IoT Edge para facilitar el aprovisionamiento mediante la clave SAS
* Ver los paneles y el estado de los módulos en IoT Central
* Enviar comandos a un módulo que se ejecuta en el dispositivo Azure IoT Edge
* Establecer propiedades en un módulo que se ejecuta en el dispositivo Azure IoT Edge

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a trabajar con dispositivos de Azure IoT Edge en IoT Central, este es el siguiente paso recomendado:

<!-- Next how-tos in the sequence -->

Configuración de una puerta de enlace transparente, que viene a continuación de este tutorial.

> [!div class="nextstepaction"]
> [Configuración de una puerta de enlace transparente](../../iot-edge/how-to-create-transparent-gateway.md)
