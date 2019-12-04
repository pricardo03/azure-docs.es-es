---
title: Incorporación de un dispositivo Azure IoT Edge a Azure IoT Central | Microsoft Docs
description: Como operador, agregue un dispositivo Azure IoT Edge a la aplicación Azure IoT Central.
author: rangv
ms.author: rangv
ms.date: 10/22/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: f16db7ebff087b164228f2b23d6fa7ec302705bb
ms.sourcegitcommit: dd0304e3a17ab36e02cf9148d5fe22deaac18118
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2019
ms.locfileid: "74406342"
---
# <a name="tutorial-add-an-azure-iot-edge-device-to-your-azure-iot-central-application"></a>Tutorial: Incorporación de un dispositivo Azure IoT Edge a la aplicación Azure IoT Central

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

En este tutorial se explica cómo agregar y configurar un dispositivo Azure IoT Edge a la aplicación Azure IoT Central. En este tutorial, se ha elegido una máquina virtual Linux habilitada para IoT Edge de Azure Marketplace.

Este tutorial se compone de dos partes:

* En primer lugar, como operador, aprenderá a realizar el aprovisionamiento en la nube primero de un dispositivo IoT Edge.
* Luego, aprenderá a realizar el aprovisionamiento en el dispositivo primero de un dispositivo IoT Edge.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Agregar un nuevo dispositivo IoT Edge
> * Configurar el dispositivo IoT Edge para ayudar al aprovisionamiento mediante una clave de firma de acceso compartido (SAS)
> * Ver los paneles y el estado de los módulos en IoT Central
> * Enviar comandos a un módulo que se ejecuta en el dispositivo IoT Edge
> * Establecer propiedades en un módulo que se ejecuta en el dispositivo IoT Edge

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, necesitará una aplicación de Azure IoT Central. Siga [este inicio rápido para crear una aplicación de Azure IoT Central](./quick-deploy-iot-central.md).

## <a name="enable-azure-iot-edge-enrollment-group"></a>Habilitación del grupo de inscripción de Azure IoT Edge
Habilite las claves de SAS para el grupo de inscripción de Azure IoT Edge en la página **Administración**.

![Captura de pantalla de la página Administración con la opción Conexión del dispositivo resaltada](./media/tutorial-add-edge-as-leaf-device/groupenrollment.png)

## <a name="provision-a-cloud-first-azure-iot-edge-device"></a>Aprovisionamiento en la nube primero de un dispositivo Azure IoT Edge  
En esta sección, creará un dispositivo IoT Edge mediante la plantilla Environment Sensor Template y aprovisionará un dispositivo. Seleccione **Dispositivos** > **Environment Sensor Template**. 

![Captura de pantalla de la página Dispositivos con Environment Sensor Template resaltada](./media/tutorial-add-edge-as-leaf-device/deviceexplorer.png)

Seleccione **+ Nuevo** y escriba un nombre y un identificador de dispositivo de su elección. Seleccione **Crear**.

![Captura de pantalla del cuadro de diálogo Crear nuevo dispositivo, con las opciones Id. de dispositivo y Crear resaltadas](./media/tutorial-add-edge-as-leaf-device/cfdevicecredentials.png)

El dispositivo entra en el modo **Registrado**.

![Captura de pantalla de la página de Environment Sensor Template con la opción Estado del dispositivo resaltada](./media/tutorial-add-edge-as-leaf-device/cfregistered.png)

## <a name="deploy-an-iot-edge-enabled-linux-vm"></a>Implementación de una máquina virtual Linux habilitada para IoT Edge

> [!NOTE]
> Puede optar por usar cualquier máquina o dispositivo. El sistema operativo puede ser Linux o Windows.

En este tutorial, se va a utilizar una máquina virtual Linux habilitada para Azure IoT, que se puede crear en Azure. En [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft_iot_edge.iot_edge_vm_ubuntu?tab=Overview), seleccione **OBTENERLA AHORA**. 

![Captura de pantalla de Azure Marketplace con la opción OBTENERLA AHORA resaltada](./media/tutorial-add-edge-as-leaf-device/cfmarketplace.png)

Seleccione **Continuar**.

![Captura de pantalla del cuadro de diálogo Crear esta aplicación en Azure, con la opción Continuar resaltada](./media/tutorial-add-edge-as-leaf-device/cfmarketplacecontinue.png)


Esto le conducirá a Azure Portal. Seleccione **Crear**.

![Captura de pantalla de Azure Portal con la opción Crear resaltada](./media/tutorial-add-edge-as-leaf-device/cfubuntu.png)

Seleccione **Suscripción**, cree un grupo de recursos y seleccione **(EE. UU.) Oeste de EE. UU. 2** para la disponibilidad de la máquina virtual. A continuación, escriba la información de usuario y contraseña. Necesitará el usuario y la contraseña en futuros pasos, así que guárdelos. Seleccione **Revisar + crear**.

![Captura de pantalla de la página de detalles de Crear una máquina virtual, con varias opciones resaltadas](./media/tutorial-add-edge-as-leaf-device/cfvm.png)

Después de la validación, seleccione **Crear**.

![Captura de pantalla de la página Crear una máquina virtual con Validación superada y Crear resaltadas](./media/tutorial-add-edge-as-leaf-device/cfvmvalidated.png)

Los recursos tardan en crearse unos minutos. Haga clic en **Go to resource** (Ir al recurso).

![Captura de pantalla de la página de finalización de la implementación con la opción Ir al recurso resaltada](./media/tutorial-add-edge-as-leaf-device/cfvmdeploymentcomplete.png)

### <a name="provision-vm-as-an-iot-edge-device"></a>Aprovisionamiento de una máquina virtual como un dispositivo IoT Edge 

En **Soporte y solución de problemas**, seleccione **Consola serie**.

![Captura de pantalla de las opciones de Soporte y solución de problemas con Consola serie resaltada](./media/tutorial-add-edge-as-leaf-device/cfserialconsole.png)

Aparece una pantalla similar a la siguiente:

![Captura de pantalla de la consola](./media/tutorial-add-edge-as-leaf-device/cfconsole.png)

Presione Entrar, proporcione el nombre de usuario y la contraseña cuando se le solicite y, de nuevo, presione Entrar. 

![Captura de pantalla de la consola](./media/tutorial-add-edge-as-leaf-device/cfconsolelogin.png)

Para ejecutar un comando como administrador (usuario raíz), escriba **sudo su –** .

![Captura de pantalla de la consola](./media/tutorial-add-edge-as-leaf-device/cfsudo.png)

Compruebe la versión del entorno de ejecución de IoT Edge. En el momento de escribir este artículo, la versión de disponibilidad general es la 1.0.8.

![Captura de pantalla de la consola](./media/tutorial-add-edge-as-leaf-device/cfconsoleversion.png)

Instale el editor Vim o use Nano si lo prefiere. 

![Captura de pantalla de la consola](./media/tutorial-add-edge-as-leaf-device/cfconsolevim.png)

![Captura de pantalla de la consola](./media/tutorial-add-edge-as-leaf-device/cfvim.png)

Edite el archivo config.yaml de IoT Edge.

![Captura de pantalla de la consola](./media/tutorial-add-edge-as-leaf-device/cfconsoleconfig.png)

Desplácese hacia abajo y convierta en comentario la parte de la cadena de conexión del archivo yaml. 

**Antes**

![Captura de pantalla de la consola](./media/tutorial-add-edge-as-leaf-device/cfmanualprovisioning.png)

**Después** presione Esc y la letra "a" minúscula para empezar a editar.

![Captura de pantalla de la consola](./media/tutorial-add-edge-as-leaf-device/cfmanualprovisioningcomments.png)

Quite la marca de comentario de la parte de clave simétrica del archivo yaml. 

**Antes**

![Captura de pantalla de la consola](./media/tutorial-add-edge-as-leaf-device/cfconsolesymmcomments.png)

**Después**

![Captura de pantalla de la consola](./media/tutorial-add-edge-as-leaf-device/cfconsolesymmuncomments.png)

Vaya a IoT Central. Obtenga el identificador de ámbito, el identificador de dispositivo y la clave simétrica del dispositivo IoT Edge.
![Captura de pantalla de IoT Central con varias opciones de conexión de dispositivo resaltadas](./media/tutorial-add-edge-as-leaf-device/cfdeviceconnect.png)

Vaya al equipo Linux y reemplace el identificador de ámbito y el identificador de registro por el identificador de dispositivo y la clave simétrica.

Presione Esc y escriba **:wq!** . Presione Entrar para guardar los cambios.

Reinicie IoT Edge para procesar los cambios y presione Entrar.

![Captura de pantalla de la consola](./media/tutorial-add-edge-as-leaf-device/cfrestart.png)

Escriba **iotedge list** (lista de iotedge). Después de unos minutos, verá tres módulos implementados.

![Captura de pantalla de la consola](./media/tutorial-add-edge-as-leaf-device/cfconsolemodulelist.png)


## <a name="iot-central-device-explorer"></a>Explorador de dispositivos de IoT Central 

En IoT Central, el dispositivo cambia al estado aprovisionado.

![Captura de pantalla con las opciones de los dispositivos de IoT Central con el estado de dispositivo resaltado](./media/tutorial-add-edge-as-leaf-device/cfprovisioned.png)

La pestaña **Módulos** muestra el estado del dispositivo y del módulo en IoT Central. 

![Captura de pantalla de la pestaña Módulos de IoT Central](./media/tutorial-add-edge-as-leaf-device/cfiotcmodulestatus.png)


Podrá ver las propiedades de la nube en un formulario (desde la plantilla de dispositivo que creó en los pasos anteriores). Escriba los valores y seleccione **Guardar**. 

![Captura de pantalla del formulario My Linux Edge Device (Mi dispositivo perimetral de Linux)](./media/tutorial-add-edge-as-leaf-device/deviceinfo.png)

Esta es una vista que se presenta en forma de un icono de panel.

![Captura de pantalla de los iconos de panel de My Linux Edge Device (Mi dispositivo perimetral de Linux)](./media/tutorial-add-edge-as-leaf-device/dashboard.png)

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a trabajar y a administrar dispositivos IoT Edge en IoT Central, este es el siguiente paso sugerido:

<!-- Next how-tos in the sequence -->

> [!div class="nextstepaction"]
> [Configuración de una puerta de enlace transparente](../../iot-edge/how-to-create-transparent-gateway.md)
