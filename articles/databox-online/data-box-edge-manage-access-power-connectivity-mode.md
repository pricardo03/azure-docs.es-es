---
title: Acceso de dispositivo de borde del cuadro de datos Azure, power y modo de conectividad | Microsoft Docs
description: Describe cómo administrar el acceso, power y modo de conectividad para el dispositivo de borde del cuadro de datos de Azure que ayuda a transferir datos a Azure
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 06/03/2019
ms.author: alkohli
ms.openlocfilehash: 8937f4c47f0fa84d4ec371e951cff8a2fdaa8481
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66476906"
---
# <a name="manage-access-power-and-connectivity-mode-for-your-azure-data-box-edge"></a>Administrar el acceso, power y modo de conectividad para el borde del cuadro de datos de Azure

En este artículo se describe cómo administrar el modo de acceso, alimentación y conectividad para el borde del cuadro de datos de Azure. Estas operaciones se realizan mediante la interfaz de usuario web local o en Azure Portal.

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Administración del acceso al dispositivo
> * Administración del modo de conectividad
> * Administración del encendido


## <a name="manage-device-access"></a>Administración del acceso al dispositivo

El acceso al dispositivo de borde del cuadro de datos se controla mediante el uso de una contraseña del dispositivo. Puede cambiar la contraseña a través de la interfaz de usuario web local. También puede restablecer la contraseña del dispositivo en el portal de Azure.

### <a name="change-device-password"></a>Cambiar la contraseña del dispositivo

Siga estos pasos en la interfaz de usuario local para cambiar la contraseña del dispositivo.

1. En la interfaz de usuario web local, vaya a **Mantenimiento > Cambio de contraseña**.
2. Escriba la contraseña actual y, a continuación, la nueva contraseña. La contraseña proporcionada debe contener entre 8 y 16 caracteres. La contraseña debe contener tres de los siguientes caracteres: caracteres en mayúsculas, minúsculas, números y caracteres especiales. Confirme la nueva contraseña.

    ![Cambiar contraseña](media/data-box-edge-manage-access-power-connectivity-mode/change-password-1.png)

3. Seleccione **Cambiar contraseña**.
 
### <a name="reset-device-password"></a>Restablecer la contraseña del dispositivo

El flujo de trabajo de restablecimiento no requiere que el usuario recupere la contraseña antigua y es útil cuando se pierde la contraseña. Este flujo de trabajo se realiza en Azure Portal.

1. En Azure Portal, vaya a **Información general > Restablecer la contraseña del administrador**.

    ![Restablecimiento de contraseña](media/data-box-edge-manage-access-power-connectivity-mode/reset-password-1.png)


2. Escriba la nueva contraseña y confírmela. La contraseña proporcionada debe contener entre 8 y 16 caracteres. La contraseña debe contener tres de los siguientes caracteres: caracteres en mayúsculas, minúsculas, números y caracteres especiales. Seleccione **restablecer**.

    ![Restablecimiento de contraseña](media/data-box-edge-manage-access-power-connectivity-mode/reset-password-2.png)

## <a name="manage-resource-access"></a>Administrar el acceso a los recursos

Para crear la puerta de enlace de datos de cuadro de Edge/Data cuadro, IoT Hub y el recurso de almacenamiento de Azure, necesitará permisos como colaborador o superior en un nivel de grupo de recursos. También necesita los proveedores de recursos correspondiente que se registrarán. Para las operaciones que implican las credenciales y la clave de activación, también se necesitan permisos para Azure Active Directory Graph API. Estos métodos se describen en las secciones siguientes.

### <a name="manage-microsoft-azure-active-directory-graph-api-permissions"></a>Administrar permisos de Microsoft Azure Active Directory Graph API

Al generar la clave de activación para el dispositivo de borde del cuadro de datos o realizar cualquier operación que requiera credenciales, necesitará permisos para la API Graph de Azure Active Directory. Las operaciones que se necesitan credenciales podrían ser:

-  Crear un recurso compartido con una cuenta de almacenamiento asociada.
-  Creación de un usuario que puede tener acceso a los recursos compartidos en el dispositivo.

Debe tener un `User` tener acceso a los inquilinos de Active Directory como sea necesario poder `Read all directory objects`. No puede ser un usuario invitado ya que no tienen permisos para `Read all directory objects`. Si es un invitado, entonces las operaciones como la generación de una clave de activación, creación de un recurso compartido en el dispositivo de borde del cuadro de datos, creación de un usuario se todos producirá un error.

Para obtener más información sobre cómo proporcionar acceso a los usuarios a Azure Active Directory Graph API, consulte [predeterminado de acceso para los administradores, usuarios y usuarios invitados](https://docs.microsoft.com/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes#default-access-for-administrators-users-and-guest-users-).

### <a name="register-resource-providers"></a>Registro de proveedores de recursos

Para aprovisionar un recurso de Azure (en el modelo de Azure Resource Manager), necesita un proveedor de recursos que admite la creación de ese recurso. Por ejemplo, para aprovisionar una máquina virtual, debe tener un proveedor de recursos "Microsoft.Compute" disponible en la suscripción.
 
Los proveedores de recursos se registran en el nivel de la suscripción. De forma predeterminada, cualquier nueva suscripción de Azure es registrado previamente con una lista de proveedores de recursos usados. El proveedor de recursos para 'Microsoft.DataBoxEdge' no se incluye en esta lista.

No es necesario conceder permisos de acceso para el nivel de suscripción para los usuarios puedan crear recursos como 'Microsoft.DataBoxEdge' dentro de sus grupos de recursos que tienen derechos de propietario, siempre y cuando los proveedores de recursos para estos recursos ya está registrado.

Antes de intentar crear cualquier recurso, asegúrese de que está registrado el proveedor de recursos en la suscripción. Si no está registrado el proveedor de recursos, deberá asegurarse de que el usuario que crea el nuevo recurso tiene derechos suficientes para registrar el proveedor de recursos necesarios en el nivel de suscripción. Si aún no lo ha hecho así, verá el siguiente error:

*La suscripción <Subscription name> no tiene permisos para registrar los proveedores de recursos: Microsoft.DataBoxEdge.*


Para obtener una lista de proveedores de recursos registrado en la suscripción actual, ejecute el siguiente comando:

```PowerShell
Get-AzResourceProvider -ListAvailable |where {$_.Registrationstate -eq "Registered"}
```

Para el dispositivo de borde del cuadro de datos, `Microsoft.DataBoxEdge` debe registrarse. Para registrar `Microsoft.DataBoxEdge`, Administrador de suscripciones debe ejecutar el comando siguiente:

```PowerShell
Register-AzResourceProvider -ProviderNamespace Microsoft.DataBoxEdge
```

Para obtener más información sobre cómo registrar un proveedor de recursos, consulte [resolución de errores de registro del proveedor de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-register-provider-errors).

## <a name="manage-connectivity-mode"></a>Administración del modo de conectividad

Además el modo completamente conectada de forma predeterminada, el dispositivo también puede ejecutar en modo parcialmente conectado o desconectado por completo. A continuación se describe cada uno de estos modos:

- **Totalmente conectado** -éste es el modo normal de forma predeterminada en el que opera el dispositivo. En este modo está habilitada la carga en la nube y la descarga de datos. Puede usar el portal de Azure o la interfaz de usuario web local para administrar el dispositivo.

- **Se desconectó parcialmente** : en este modo, el dispositivo no se puede cargar o descargar cualquier recurso compartido de datos pero se pueden administrar a través del portal de Azure.

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

- Aprenda a [administrar los recursos compartidos](data-box-edge-manage-shares.md).