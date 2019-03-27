---
title: Acceso, encendido y modo de conectividad del dispositivo Microsoft Azure Data Box Gateway | Microsoft Docs
description: Describe cómo administrar el acceso, encendido y modo de conectividad del dispositivo Azure Data Box Gateway que ayuda a transferir datos a Azure
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 03/25/2019
ms.author: alkohli
ms.openlocfilehash: 72d3455f37d0ccef0dd5b7d8882f70670de07572
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58497327"
---
# <a name="manage-access-power-and-connectivity-mode-for-your-azure-data-box-gateway"></a>Administrar el acceso, power y modo de conectividad para la puerta de enlace del cuadro de datos de Azure

En este artículo se describe cómo administrar el acceso, encendido y modo de conectividad de Azure Data Box Gateway. Estas operaciones se realizan mediante la interfaz de usuario web local o en Azure Portal.

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Administración del acceso al dispositivo
> * Administración del modo de conectividad
> * Administración del encendido

## <a name="manage-device-access"></a>Administración del acceso al dispositivo

El acceso al dispositivo de puerta de enlace de datos cuadro se controla mediante el uso de una contraseña del dispositivo. Puede cambiar la contraseña a través de la interfaz de usuario web local. También puede restablecer la contraseña del dispositivo en el portal de Azure.

### <a name="change-device-password"></a>Cambiar la contraseña del dispositivo

Siga estos pasos en la interfaz de usuario local para cambiar la contraseña del dispositivo.

1. En la interfaz de usuario web local, vaya a **Mantenimiento > Cambio de contraseña**.
2. Escriba la contraseña actual y, a continuación, la nueva contraseña. La contraseña proporcionada debe contener entre 8 y 16 caracteres. La contraseña debe contener tres de los siguientes caracteres: caracteres en mayúsculas, minúsculas, números y caracteres especiales. Confirme la nueva contraseña.

    ![Cambiar contraseña](media/data-box-gateway-manage-access-power-connectivity-mode/change-password-1.png)

3. Haga clic en **Cambiar contraseña**.
 
### <a name="reset-device-password"></a>Restablecer la contraseña del dispositivo

El flujo de trabajo de restablecimiento no requiere que el usuario recupere la contraseña antigua y es útil cuando se pierde la contraseña. Este flujo de trabajo se realiza en Azure Portal.

1. En Azure Portal, vaya a **Información general > Restablecer la contraseña del administrador**.

    ![Restablecimiento de contraseña](media/data-box-gateway-manage-access-power-connectivity-mode/reset-password-1.png)

 
2. Escriba la nueva contraseña y confírmela. La contraseña proporcionada debe contener entre 8 y 16 caracteres. La contraseña debe contener tres de los siguientes caracteres: caracteres en mayúsculas, minúsculas, números y caracteres especiales. Haga clic en **Restablecer**.

    ![Restablecimiento de contraseña](media/data-box-gateway-manage-access-power-connectivity-mode/reset-password-2.png)

## <a name="manage-connectivity-mode"></a>Administración del modo de conectividad

Aparte del modo normal predeterminado, el dispositivo también se puede ejecutar en modo parcialmente desconectado o en modo desconectado. A continuación se describe cada uno de estos modos:

- **Parcialmente desconectado**: en este modo, el dispositivo no puede cargar datos en los recursos compartidos; sin embargo, se puede administrar mediante Azure Portal.

    Este modo se suele usar cuando se encuentra en una red de satélite de uso medido y el objetivo es minimizar el consumo de ancho de banda de red. Todavía puede producirse un consumo de red mínimo por las operaciones de supervisión del dispositivo.

- **Desconectado**: en este modo, el dispositivo está completamente desconectado de la nube y las cargas y descargas de la nube están deshabilitadas. Solo se puede administrar el dispositivo mediante la interfaz de usuario web local.

    Este modo se suele usar cuando desea desconectar el dispositivo.

Para cambiar el modo del dispositivo, siga estos pasos:

1. En la interfaz de usuario web local del dispositivo, vaya a **Configuración > Configuración de la nube**.
2. Deshabilite la opción **Carga y descarga de la nube**.
3. Para ejecutar el dispositivo en modo parcialmente desconectado, habilite **Administración en Azure Portal**.

    ![Modo de conectividad](media/data-box-gateway-manage-access-power-connectivity-mode/connectivity-mode-1.png)
 
4. Para ejecutar el dispositivo en modo desconectado, deshabilite **Administración en Azure Portal**. Ahora solo se puede administrar el dispositivo mediante la interfaz de usuario web local.

    ![Modo de conectividad](media/data-box-gateway-manage-access-power-connectivity-mode/connectivity-mode-2.png)

## <a name="manage-power"></a>Administración del encendido

Puede apagar o reiniciar el dispositivo físico y virtual mediante la interfaz de usuario web local. Se recomienda que antes de reiniciar, desconecte los recursos compartidos del host y, luego, el dispositivo. Esta acción minimizará la posibilidad de daños en los datos.

1. En la interfaz de usuario web local, vaya a **Mantenimiento > Configuración de encendido**.
2. Haga clic en **Apagar** o **Reiniciar** según lo que se desee.

    ![Configuración de encendido](media/data-box-gateway-manage-access-power-connectivity-mode/shut-down-restart-1.png)

3. Cuando se le pida confirmación, haga clic en **Sí** para continuar.

> [!NOTE]
> Si apaga el dispositivo virtual, deberá iniciar el dispositivo mediante la administración del hipervisor.
