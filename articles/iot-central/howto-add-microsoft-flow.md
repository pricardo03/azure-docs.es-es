---
title: Creación de flujos de trabajo con el conector de Azure IoT Central en Microsoft Flow | Microsoft Docs
description: Usar el conector de IoT Central en Microsoft Flow para desencadenar flujos de trabajo y crear, obtener, actualizar, eliminar los dispositivos y ejecutar comandos en los flujos de trabajo.
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 03/26/2019
ms.topic: conceptual
ms.service: iot-central
manager: hegate
ms.openlocfilehash: 2c4ee6a2feb737bcafc64b1c8503c03757a53364
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60887735"
---
# <a name="build-workflows-with-the-iot-central-connector-in-microsoft-flow"></a>Creación de flujos de trabajo con el conector de IoT Central en Microsoft Flow

*Este tema se aplica a los compiladores y administradores.*

Use Microsoft Flow para automatizar flujos de trabajo en las muchas aplicaciones y servicios en los que confían los usuarios profesionales. Con el conector de IoT Central de Microsoft Flow, puede desencadenar flujos de trabajo si se activa una regla en IoT Central. En un flujo de trabajo desencadenado por IoT Central o cualquier otra aplicación, puede usar las acciones en el conector de IoT Central:
- Crear un dispositivo
- Obtener información del dispositivo
- Actualice las propiedades y la configuración de un dispositivo
- Ejecutar un comando en un dispositivo
- Eliminar un dispositivo

Consulte [estas plantillas de Microsoft Flow](https://aka.ms/iotcentralflowtemplates) que conectan IoT Central a otros servicios como notificaciones móviles y Microsoft Teams.

## <a name="prerequisites"></a>Requisitos previos

- Una aplicación de pago por uso
- Una cuenta personal, profesional o educativa de Microsoft para iniciar sesión en Flow ([más información sobre los planes de Microsoft Flow](https://aka.ms/microsoftflowplans))

## <a name="trigger-a-workflow"></a>Desencadenar un flujo de trabajo

En esta sección se muestra cómo desencadenar una notificación móvil en la aplicación móvil de Flow cuando se activa una regla en IoT Central. Puede crear este flujo de trabajo completo dentro de la aplicación IoT Central mediante el diseñador incrustado de Microsoft Flow.

1. Empiece [creando una regla en IoT Central](howto-create-telemetry-rules.md). Después de guardar las condiciones de regla, seleccione el **Microsoft Flow acción** como una acción nueva. Se abrirá una ventana de cuadro de diálogo para que configurar el flujo de trabajo. Que haya iniciado sesión en la cuenta de usuario de IoT Central se usará para iniciar sesión en Microsoft Flow.

    ![Crear una acción de Microsoft Flow](media/howto-add-microsoft-flow/createflowaction.png)

1. Verá una lista de flujos de trabajo tha tienen acceso a y se adjuntan a esta regla IoT Central. Haga clic en **Explore plantillas** o **nuevo > crear desde plantilla** y puede elegir entre cualquiera de las plantillas disponibles. 

    ![Plantillas disponibles de Microsoft Flow](media/howto-add-microsoft-flow/flowtemplates.png)

1. Se le pedirá que inicie sesión en los conectores en la plantilla elegida. Una vez que los conectores se ha iniciado sesión, se colocará en el diseñador para crear el flujo de trabajo. El flujo de trabajo tiene un desencadenador de IoT Central que ya tiene rellenados su Aplicación y Regla.

1. Puede personalizar el flujo de trabajo mediante la personalización de la información que se pasan a la acción y agregar nuevas acciones. En este ejemplo, la acción es **notificaciones - Enviarme una notificación móvil**. Puede incluir *Contenido dinámico* de su regla de IoT Central, difundiendo información importante como el nombre de dispositivo y la marca de tiempo en su notificación.

    > [!NOTE]
    > Seleccione el **más** texto en la ventana de contenido dinámico para obtener los valores de propiedad y medición que desencadenó la regla.

    ![Acción de edición de Flow con panel dinámico abierto](./media/howto-add-microsoft-flow/flowdynamicpane.png)

1. Cuando haya terminado la acción de edición, seleccione **guardar**. Se le dirigirá a la página de información general de su flujo de trabajo. Aquí puede ver el historial de ejecución y compartirlo con otros compañeros.

    > [!NOTE]
    > Si desea que otros usuarios de su aplicación IoT Central editen esta regla, debe compartirla con ellos en Microsoft Flow. Agregue sus cuentas de Microsoft Flow como propietarios en su flujo de trabajo.

1. Si vuelve a la aplicación IoT Central, verá que esta regla tiene una acción de Microsoft Flow en el área de acciones.

También puede crear flujos de trabajo mediante el conector de IoT Central directamente desde Microsoft Flow. A continuación, puede elegir qué aplicación IoT Central para conectarse a.

## <a name="create-a-device-in-a-workflow"></a>Creación de un dispositivo en un flujo de trabajo

En esta sección se muestra cómo crear un nuevo dispositivo en IoT Central con solo pulsar un botón en un dispositivo móvil mediante la aplicación móvil de Microsoft Flow. Puede usar esta acción en Flow para integrar sistemas ERP como Dynamics con IoT Central creando un nuevo dispositivo cuando un dispositivo se agrega en otra parte.

1. Empiece creando un flujo de trabajo en blanco en Microsoft Flow.

1. Busque **Botón de flujo para móviles** como desencadenador.

1. En este desencadenador, agregue una entrada de texto llamada **Nombre de dispositivo**. Cambie el texto de descripción para que sea **Enter the device name of your new device** (Escriba el nombre del nuevo dispositivo).

1. Agregue una nueva acción. Busque la acción **Azure IoT Central - Create a device** (Azure IoT Central: crear un dispositivo).

1. Seleccione su aplicación y elija una plantilla de dispositivo desde la que crear un dispositivo en las listas desplegables. Verá cómo se expande la acción para mostrar todas las propiedades y la configuración del dispositivo.

1. Seleccione el campo Nombre de dispositivo. En el panel Contenido dinámico, elija **Nombre de dispositivo**. Este valor se pasa de la entrada del usuario se escribe a través de la aplicación móvil y es el nombre del nuevo dispositivo en IoT Central. En este ejemplo, el único campo obligatorio es el nombre de dispositivo, indicado por el asterisco rojo. Otra plantilla de dispositivo puede tener varios campos obligatorios que deban rellenarse para crear un nuevo dispositivo.

    ![Panel dinámico de acciones de creación de dispositivos de Flow](./media/howto-add-microsoft-flow/flowcreatedevice.png)

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

1. Elija su aplicación en la lista desplegable. Ahora, necesitará el identificador del dispositivo existente que quiere actualizar. El identificador del dispositivo de IoT Central se puede obtener en **Device Explorer**.

    ![Id. de dispositivo del Device Explorer de IoT Central](./media/howto-add-microsoft-flow/iotcdeviceid.png)

1. Puede actualizar el nombre del dispositivo. Para actualizar cualquiera de las propiedades y la configuración del dispositivo, debe seleccionar la plantilla de dispositivo que quiera actualizar en la lista desplegable **Plantilla de dispositivo**. El icono de la acción se expande para mostrar todas las propiedades y la configuración que puede actualizar.

    ![Flujo de trabajo de actualización de dispositivos de Flow](./media/howto-add-microsoft-flow/flowupdatedevice.png)

1. Seleccione cada una de las propiedades y la configuración que desee actualizar. En el panel Contenido dinámico, elija la entrada correspondiente del desencadenador. En este ejemplo, el valor Ubicación se propaga hacia abajo para actualizar la propiedad Ubicación del dispositivo.

1. Por último, guarde su flujo de trabajo.

1. Pruebe su flujo de trabajo en la aplicación móvil de Microsoft Flow. Vaya a la pestaña **Botones** de la aplicación. Debe ver su flujo de trabajo Botón -> Actualización de un dispositivo. Escriba las entradas y vea cómo se actualiza el dispositivo en IoT Central.

## <a name="get-device-information-in-a-workflow"></a>Obtener información del dispositivo en un flujo de trabajo

Puede obtener información del dispositivo mediante su identificador de dispositivo utilizando el **Azure IoT Central - obtener un dispositivo** acción. Puede obtener información como el nombre del dispositivo, nombre de la plantilla de dispositivo, los valores de propiedad y valores de configuración para pasar a las acciones posteriores en el flujo de trabajo. Este es un flujo de trabajo de ejemplo que se pasa el valor de propiedad de nombre de cliente desde un dispositivo a Microsoft Teams.

   ![El flujo de trabajo de flujo de get dispositivos](./media/howto-add-microsoft-flow/flowgetdevice.png)


## <a name="run-a-command-on-a-device-in-a-workflow"></a>Ejecutar un comando en un dispositivo en un flujo de trabajo
Puede ejecutar un comando en un dispositivo especificado por su Id. de dispositivo mediante el **Azure IoT Central - ejecutar un comando** acción. Puede elegir el comando para ejecutar y pasar los parámetros del comando a través de esta acción. Este es un flujo de trabajo de ejemplo que se ejecuta un comando de reinicio del dispositivo desde un botón en la aplicación móvil Microsoft Flow.

   ![El flujo de trabajo de flujo de get dispositivos](./media/howto-add-microsoft-flow/flowrunacommand.png)

## <a name="delete-a-device-in-a-workflow"></a>Eliminación de un dispositivo en un flujo de trabajo

Puede eliminar un dispositivo por su identificador de dispositivo con la acción **Azure IoT Central - Delete a device** (Azure IoT Central: eliminar un dispositivo). Este es un flujo de trabajo de ejemplo que elimina un dispositivo con solo pulsar un botón en la aplicación móvil de Microsoft Flow.

   ![Flujo de trabajo de eliminación de dispositivos de Flow](./media/howto-add-microsoft-flow/flowdeletedevice.png)

## <a name="troubleshooting"></a>solución de problemas

Si tiene problemas para crear una conexión al conector de Azure IoT Central, estas sugerencias le ayudarán.

1. Las cuentas personales de Microsoft (como los dominios @hotmail.com, @live.com y @outlook.com) no se admiten en este momento. Debe usar un trabajo de Azure Active Directory (AD) o educativa.

2. Para usar el conector de IoT Central de Microsoft Flow, debe haber iniciado sesión al menos una vez en la aplicación IoT Central. En caso contrario, la aplicación no aparecerá en las listas desplegables de aplicaciones.

3. Si recibe un error mientras usa una cuenta de Azure AD, pruebe a abrir Windows PowerShell y ejecute los commandlets siguientes como administrador.

    ``` PowerShell
    Install-Module AzureAD
    Connect-AzureAD
    New-AzureADServicePrincipal -AppId 9edfcdd9-0bc5-4bd4-b287-c3afc716aac7 -DisplayName "Azure IoT Central"
    ```

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a usar Microsoft Flow para crear flujos de trabajo, el siguiente paso sugerido es [administrar dispositivos](howto-manage-devices.md).

