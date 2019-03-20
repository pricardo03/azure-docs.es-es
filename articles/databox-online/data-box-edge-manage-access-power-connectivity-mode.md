---
title: Acceso de dispositivo de borde del cuadro de datos de Microsoft Azure, power y modo de conectividad | Microsoft Docs
description: Describe cómo administrar el acceso, power y modo de conectividad para el dispositivo de borde del cuadro de datos de Azure que ayuda a transferir datos a Azure
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/11/2019
ms.author: alkohli
ms.openlocfilehash: e0aa3468bda9f904d62e9e20545ac5f990cef521
ms.sourcegitcommit: b8f9200112cae265155b8877f7e1621c4bcc53fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/14/2019
ms.locfileid: "57905345"
---
# <a name="manage-access-power-and-connectivity-mode-for-your-azure-data-box-edge-preview"></a>Administrar el acceso, power y modo de conectividad para el borde de cuadro de datos de Azure (versión preliminar)

En este artículo se describe cómo administrar el modo de acceso, alimentación y conectividad para el borde del cuadro de datos de Azure. Estas operaciones se realizan mediante la interfaz de usuario web local o en Azure Portal.

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Administración del acceso al dispositivo
> * Administración del modo de conectividad
> * Administración del encendido

> [!IMPORTANT]
> Data Box Edge se encuentra en versión preliminar. Antes de solicitar e implementar esta solución revise los [términos del servicio de Azure para la versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="manage-device-access"></a>Administración del acceso al dispositivo

El acceso al dispositivo de borde del cuadro de datos se controla mediante el uso de una contraseña de administrador de dispositivos. Para cambiar la contraseña del administrador mediante la interfaz de usuario web local. También puede restablecer la contraseña del administrador del dispositivo en Azure Portal.

### <a name="change-device-administrator-password"></a>Cambio de la contraseña del administrador del dispositivo

Siga estos pasos en la interfaz de usuario local para cambiar la contraseña del administrador del dispositivo.

1. En la interfaz de usuario web local, vaya a **Mantenimiento > Cambio de contraseña**.
2. Escriba la contraseña actual y, a continuación, la nueva contraseña. La contraseña proporcionada debe contener entre 8 y 16 caracteres. La contraseña debe contener tres de los siguientes caracteres: caracteres en mayúsculas, minúsculas, números y caracteres especiales. Confirme la nueva contraseña.

    ![Cambiar contraseña](media/data-box-edge-manage-access-power-connectivity-mode/change-password-1.png)

3. Seleccione **Cambiar contraseña**.
 
### <a name="reset-device-administrator-password"></a>Restablecimiento de la contraseña del administrador del dispositivo

El flujo de trabajo de restablecimiento no requiere que el usuario recupere la contraseña antigua y es útil cuando se pierde la contraseña. Este flujo de trabajo se realiza en Azure Portal.

1. En Azure Portal, vaya a **Información general > Restablecer la contraseña del administrador**.

    ![Restablecimiento de contraseña](media/data-box-edge-manage-access-power-connectivity-mode/reset-password-1.png)


2. Escriba la nueva contraseña y confírmela. La contraseña proporcionada debe contener entre 8 y 16 caracteres. La contraseña debe contener tres de los siguientes caracteres: caracteres en mayúsculas, minúsculas, números y caracteres especiales. Seleccione **restablecer**.

    ![Restablecimiento de contraseña](media/data-box-edge-manage-access-power-connectivity-mode/reset-password-2.png)

## <a name="manage-connectivity-mode"></a>Administración del modo de conectividad

Además el modo completamente conectada de forma predeterminada, el dispositivo también puede ejecutar en modo parcialmente conectado o desconectado por completo. A continuación se describe cada uno de estos modos:

- **Totalmente conectado** -éste es el modo normal de forma predeterminada en el que opera el dispositivo. En este modo está habilitada la carga en la nube y la descarga de datos. Puede usar el portal de Azure o la interfaz de usuario web local para administrar el dispositivo.

- **Parcialmente desconectado**: en este modo, el dispositivo no puede cargar datos en los recursos compartidos; sin embargo, se puede administrar mediante Azure Portal.

    Este modo se suele usar cuando se encuentra en una red de satélite de uso medido y el objetivo es minimizar el consumo de ancho de banda de red. Todavía puede producirse un consumo de red mínimo por las operaciones de supervisión del dispositivo.

- **Desconectado**: en este modo, el dispositivo está completamente desconectado de la nube y las cargas y descargas de la nube están deshabilitadas. Solo se puede administrar el dispositivo mediante la interfaz de usuario web local.

    Este modo se suele usar cuando desea desconectar el dispositivo.

Para cambiar el modo del dispositivo, siga estos pasos:

1. En la interfaz de usuario web local del dispositivo, vaya a **Configuración > Configuración de la nube**.
2. En la lista desplegable, seleccione el modo de operar el dispositivo en. Puede seleccionar desde **completamente conectado**, **parcialmente conectado**, y **completamente desconectado**. Para ejecutar el dispositivo en modo parcialmente desconectado, habilite **Administración en Azure Portal**.

    ![Modo de conectividad](media/data-box-edge-manage-access-power-connectivity-mode/connectivity-mode.png)
 
## <a name="manage-power"></a>Administración del encendido

Puede apagar o reiniciar el dispositivo físico mediante la interfaz de usuario web local. Se recomienda que antes de reiniciar, desconecte los recursos compartidos sin conexión en el servidor de datos y, a continuación, en el dispositivo. Esta acción minimizará la posibilidad de daños en los datos.

1. En la interfaz de usuario web local, vaya a **Mantenimiento > Configuración de encendido**.
2. Seleccione **apagado** o **reiniciar** según lo que se desee.

    ![Configuración de encendido](media/data-box-edge-manage-access-power-connectivity-mode/shut-down-restart-1.png)

3. Cuando se le pida confirmación, seleccione **Sí** para continuar.

> [!NOTE]
> Si apaga el dispositivo físico, deberá presionar el botón de encendido en el dispositivo para activarlo.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [administrar recursos compartidos](data-box-edge-manage-shares.md).