---
title: Creación de flujos de trabajo con el conector de Azure IoT Central en Microsoft Flow | Microsoft Docs
description: Use el conector de IoT Central en Microsoft Flow para desencadenar flujos de trabajo y crear, actualizar y eliminar dispositivos, así como ejecutar comandos en flujos de trabajo.
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 04/25/2019
ms.topic: conceptual
ms.service: iot-central
manager: hegate
ms.openlocfilehash: e8bc8b8d4e3585ea4c0505f2e36abc6d1da7f8eb
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797720"
---
# <a name="build-workflows-with-the-iot-central-connector-in-microsoft-flow"></a>Creación de flujos de trabajo con el conector de IoT Central en Microsoft Flow

*Este tema se aplica a los compiladores y administradores.*

Use Microsoft Flow para automatizar flujos de trabajo en las muchas aplicaciones y servicios en los que confían los usuarios profesionales. Con el conector de IoT Central de Microsoft Flow, puede desencadenar flujos de trabajo si se activa una regla en IoT Central. En un flujo de trabajo desencadenado por IoT Central o cualquier otra aplicación, puede usar las acciones en el conector de IoT Central para hacer lo siguiente:
- Crear un dispositivo
- Obtener información sobre el dispositivo
- Actualizar las propiedades y la configuración de un dispositivo
- Ejecutar un comando en un dispositivo
- Eliminar un dispositivo

Consulte [estas plantillas de Microsoft Flow](https://aka.ms/iotcentralflowtemplates) que conectan IoT Central a otros servicios como notificaciones móviles y Microsoft Teams.

## <a name="prerequisites"></a>Requisitos previos

- Una aplicación de pago por uso
- Una cuenta personal, profesional o educativa de Microsoft para usar Microsoft Flow ([más información sobre los planes de Microsoft Flow](https://aka.ms/microsoftflowplans))
- Una cuenta profesional o educativa para usar el conector de Azure IoT Central

## <a name="trigger-a-workflow"></a>Desencadenamiento de un flujo de trabajo

En esta sección se muestra cómo desencadenar una notificación móvil en la aplicación móvil de Flow cuando se activa una regla en IoT Central. Puede crear este flujo de trabajo completo dentro de la aplicación IoT Central mediante el diseñador Microsoft Flow integrado.

1. Empiece [creando una regla en IoT Central](howto-create-telemetry-rules.md). Tras guardar las condiciones de la regla, haga clic en **Acción de Microsoft Flow** como nueva acción. Se abre un cuadro de diálogo para que pueda configurar el flujo de trabajo. Para iniciar sesión en Microsoft Flow se usará la cuenta de usuario de IoT Central en la que haya iniciado sesión.

    ![Crear una acción de Microsoft Flow](media/howto-add-microsoft-flow/createflowaction.png)

1. Verá una lista de flujos de trabajo que tienen acceso a esta regla IoT Central y están vinculados con ella. Haga clic en **Explorar plantillas** o **Nuevo > Crear desde plantilla** para elegir entre cualquiera de las plantillas disponibles. 

    ![Plantillas de Microsoft Flow disponibles](media/howto-add-microsoft-flow/flowtemplates1.png)

1. Se le pedirá que inicie sesión en los conectores en la plantilla elegida. 

    > [!NOTE]
    > Para usar el conector de Azure IoT Central, debe iniciar sesión con una cuenta de Azure Active Directory (cuenta profesional o educativa). El conector de Azure IoT Central no admite las cuentas personales (por ejemplo, abc@outlook.com o abc@live.com).

    Una vez que haya iniciado sesión en los conectores, se abrirá el diseñador para que pueda compilar el flujo de trabajo. El flujo de trabajo tiene un desencadenador de IoT Central que ya tiene rellenados su Aplicación y Regla.

1. Para personalizar el flujo de trabajo, personalice la información que se pasa a la acción y agregue nuevas acciones. En este ejemplo, la acción es **Notifications - Send me a mobile notification** (Notificaciones: enviarme una notificación móvil). Puede incluir *Contenido dinámico* de su regla de IoT Central, difundiendo información importante como el nombre de dispositivo y la marca de tiempo en su notificación.

    > [!NOTE]
    > Seleccione el texto **Ver más** en la ventana Contenido dinámico para obtener los valores de medida y propiedad que desencadenaron la regla.

    ![Acción de edición de Flow con panel dinámico abierto](./media/howto-add-microsoft-flow/flowdynamicpane1.png)

1. Cuando termine de editar su acción, seleccione **Guardar**. Se le dirigirá a la página de información general de su flujo de trabajo. Aquí puede ver el historial de ejecución y compartirlo con otros compañeros.

    > [!NOTE]
    > Si desea que otros usuarios de su aplicación IoT Central editen esta regla, debe compartirla con ellos en Microsoft Flow. Agregue sus cuentas de Microsoft Flow como propietarios en su flujo de trabajo.

1. Si vuelve a su aplicación IoT Central, verá que esta regla tiene una acción de Microsoft Flow en el área Acciones.

También puede crear flujos de trabajo mediante el conector de IoT Central directamente desde Microsoft Flow. A continuación, puede elegir qué aplicación IoT Central a la que conectarse.

## <a name="create-a-device-in-a-workflow"></a>Creación de un dispositivo en un flujo de trabajo

En esta sección se muestra cómo crear un nuevo dispositivo en IoT Central con solo pulsar un botón en un dispositivo móvil mediante la aplicación móvil de Microsoft Flow. Puede usar esta acción en Flow para integrar sistemas ERP como Dynamics con IoT Central creando un nuevo dispositivo cuando un dispositivo se agrega en otra parte.

1. Empiece creando un flujo de trabajo en blanco en Microsoft Flow.

1. Busque **Botón de flujo para móviles** como desencadenador.

1. En este desencadenador, agregue una entrada de texto llamada **Nombre de dispositivo**. Cambie el texto de descripción para que sea **Enter the device name of your new device** (Escriba el nombre del nuevo dispositivo).

1. Agregue una nueva acción. Busque la acción **Azure IoT Central - Create a device** (Azure IoT Central: crear un dispositivo).

1. Seleccione su aplicación y elija una plantilla de dispositivo desde la que crear un dispositivo en las listas desplegables. Verá cómo se expande la acción para mostrar todas las propiedades y la configuración del dispositivo.

1. Seleccione el campo Nombre de dispositivo. En el panel Contenido dinámico, elija **Nombre de dispositivo**. Este valor se pasa desde la entrada escrita por el usuario a través de la aplicación móvil, y es el nombre de su nuevo dispositivo en IoT Central. En este ejemplo, el único campo obligatorio es el nombre de dispositivo, indicado por el asterisco rojo. Otra plantilla de dispositivo puede tener varios campos obligatorios que deban rellenarse para crear un nuevo dispositivo.

    ![Panel dinámico de acciones de creación de dispositivos de Flow](./media/howto-add-microsoft-flow/flowcreatedevice1.png)

1. (Opcional) Rellene otros campos como considere oportuno para su creación de nuevos dispositivos.

1. Por último, guarde su flujo de trabajo.

1. Pruebe su flujo de trabajo en la aplicación móvil de Microsoft Flow. Vaya a la pestaña **Botones** de la aplicación. Debe ver su flujo de trabajo Botón -> Creación de un dispositivo. Escriba el nombre de su nuevo dispositivo y vea cómo aparece en IoT Central.

    ![Captura de pantalla móvil de creación de dispositivos de Flow](./media/howto-add-microsoft-flow/flowmobilescreenshot.png)

## <a name="update-a-device-in-a-workflow"></a>Actualización de un dispositivo en un flujo de trabajo

En esta sección se muestra cómo actualizar las propiedades y la configuración de un dispositivo en IoT Central con solo pulsar un botón en un dispositivo móvil mediante la aplicación móvil de Microsoft Flow.

1. Empiece creando un flujo de trabajo en blanco en Microsoft Flow.

1. Busque **Botón de flujo para móviles** como desencadenador.

1. En este desencadenador, agregue una entrada como una entrada de texto **Ubicación** que corresponda a una configuración o propiedad que desee cambiar. Cambie el texto de descripción.

1. Agregue una nueva acción. Busque la acción **Azure IoT Central - Update a device** (Azure IoT Central: actualizar un dispositivo).

1. Elija su aplicación en la lista desplegable. Ahora, necesitará el identificador del dispositivo existente que quiere actualizar. 

    > [!NOTE] 
    > **Debe usar el identificador que se encuentra en la dirección URL** de la página de detalles del dispositivo que desea actualizar. El identificador de dispositivo que se encuentra en la lista de dispositivos del explorador de dispositivos no es el id. adecuado para usar en Microsoft Flow.

    ![Id. de IoT Central desde la dirección URL](./media/howto-add-microsoft-flow/iotcdeviceidurl.png)

1. Puede actualizar el nombre del dispositivo. Para actualizar cualquiera de las propiedades y la configuración del dispositivo, debe seleccionar la plantilla de dispositivo que quiera actualizar en la lista desplegable **Plantilla de dispositivo**. El icono de la acción se expande para mostrar todas las propiedades y la configuración que puede actualizar.

    ![Flujo de trabajo de actualización de dispositivos de Flow](./media/howto-add-microsoft-flow/flowupdatedevice1.png)

1. Seleccione cada una de las propiedades y la configuración que desee actualizar. En el panel Contenido dinámico, elija la entrada correspondiente del desencadenador. En este ejemplo, el valor Ubicación se propaga hacia abajo para actualizar la propiedad Ubicación del dispositivo.

1. Por último, guarde su flujo de trabajo.

1. Pruebe su flujo de trabajo en la aplicación móvil de Microsoft Flow. Vaya a la pestaña **Botones** de la aplicación. Debe ver su flujo de trabajo Botón -> Actualización de un dispositivo. Escriba las entradas y vea cómo se actualiza el dispositivo en IoT Central.

## <a name="get-device-information-in-a-workflow"></a>Obtener información del dispositivo en un flujo de trabajo

Puede obtener información del dispositivo mediante su identificador mediante la acción **Azure IoT Central - Get a device** (Azure IoT Central: obtener un dispositivo). 
> [!NOTE] 
> **Debe usar el identificador que se encuentra en la dirección URL** de la página de detalles del dispositivo que desea actualizar. El identificador de dispositivo que se encuentra en la lista de dispositivos del explorador de dispositivos no es el id. adecuado para usar en Microsoft Flow.

Puede obtener información, tal como el nombre del dispositivo, nombre de la plantilla de dispositivo, los valores de propiedad y los valores de configuración, para pasar a las acciones posteriores en el flujo de trabajo. Aquí proporcionamos un flujo de trabajo de ejemplo que pasa el valor de propiedad Nombre de cliente de un dispositivo a Microsoft Teams.

   ![Flujo de trabajo para obtener el dispositivo](./media/howto-add-microsoft-flow/flowgetdevice1.png)


## <a name="run-a-command-on-a-device-in-a-workflow"></a>Ejecución de un comando en un dispositivo en un flujo de trabajo
Puede ejecutar un comando en un dispositivo especificado por su identificador mediante la acción **Azure IoT Central - Run a command** (Azure IoT Central: ejecutar un comando). 

> [!NOTE] 
> **Debe usar el identificador que se encuentra en la dirección URL** de la página de detalles del dispositivo que desea actualizar. El identificador de dispositivo que se encuentra en la lista de dispositivos del explorador de dispositivos no es el id. adecuado para usar en Microsoft Flow.
    
Puede elegir el comando para ejecutar y pasar los parámetros del comando a través de esta acción. Aquí proporcionamos un flujo de trabajo de ejemplo que ejecuta un comando de reinicio del dispositivo desde un botón en la aplicación móvil Microsoft Flow.

   ![Flujo de trabajo para obtener el dispositivo](./media/howto-add-microsoft-flow/flowrunacommand1.png)

## <a name="delete-a-device-in-a-workflow"></a>Eliminación de un dispositivo en un flujo de trabajo

Puede eliminar un dispositivo por su identificador mediante la acción **Azure IoT Central - Delete a device** (Azure IoT Central: eliminar un dispositivo). 
> [!NOTE] 
> **Debe usar el identificador que se encuentra en la dirección URL** de la página de detalles del dispositivo que desea actualizar. El identificador de dispositivo que se encuentra en la lista de dispositivos del explorador de dispositivos no es el id. adecuado para usar en Microsoft Flow.

Este es un flujo de trabajo de ejemplo que elimina un dispositivo con solo pulsar un botón en la aplicación móvil de Microsoft Flow.

   ![Flujo de trabajo de eliminación de dispositivos de Flow](./media/howto-add-microsoft-flow/flowdeletedevice.png)

## <a name="troubleshooting"></a>solución de problemas

Si tiene problemas para crear una conexión al conector de Azure IoT Central, estas sugerencias le ayudarán.

1. Las cuentas personales de Microsoft (como los dominios @hotmail.com, @live.com y @outlook.com) no se admiten en este momento. Debe usar una cuenta profesional o educativa de Azure Active Directory (AD).

2. Para usar el conector de IoT Central de Microsoft Flow, debe haber iniciado sesión al menos una vez en la aplicación IoT Central. En caso contrario, la aplicación no aparecerá en las listas desplegables de aplicaciones.

3. Si recibe un error mientras usa una cuenta de Azure AD, intente abrir Windows PowerShell y ejecutar los siguientes commandlets como administrador.

    ``` PowerShell
    Install-Module AzureAD
    Connect-AzureAD
    New-AzureADServicePrincipal -AppId 9edfcdd9-0bc5-4bd4-b287-c3afc716aac7 -DisplayName "Azure IoT Central"
    ```

## <a name="next-steps"></a>Pasos siguientes

Ahora que ya sabe cómo usar Microsoft Flow para crear flujos de trabajo, el siguiente paso sugerido es [administrar los dispositivos](howto-manage-devices.md).

