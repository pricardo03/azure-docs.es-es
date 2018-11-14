---
title: Creación de flujos de trabajo con el conector de IoT Central en Microsoft Flow | Microsoft Docs
description: Use el conector de IoT Central en Microsoft Flow para desencadenar flujos de trabajo y crear, actualizar y eliminar dispositivos en los flujos de trabajo.
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: iot-central
manager: peterpr
ms.openlocfilehash: c06939ccb920954c08ef02f38e6d12471ac267db
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "50959297"
---
# <a name="build-workflows-with-the-iot-central-connector-in-microsoft-flow"></a>Creación de flujos de trabajo con el conector de IoT Central en Microsoft Flow

*Este tema se aplica a los compiladores y administradores.*

Use Microsoft Flow para automatizar flujos de trabajo en las muchas aplicaciones y servicios en los que confían los usuarios profesionales. Con el conector de IoT Central de Microsoft Flow, puede desencadenar flujos de trabajo si se activa una regla en IoT Central. En un flujo de trabajo desencadenado por IoT Central o cualquier otra aplicación, puede usar las acciones en el conector de IoT Central para crear un dispositivo, actualizar las propiedades y la configuración de un dispositivo, o bien eliminar un dispositivo. Consulte [estas plantillas de Microsoft Flow](https://aka.ms/iotcentralflowtemplates) que conectan IoT Central a otros servicios como notificaciones móviles y Microsoft Teams.

## <a name="prerequisites"></a>Requisitos previos

- Una aplicación de pago por uso
- Una cuenta personal, profesional o educativa de Microsoft para iniciar sesión en Flow ([más información sobre los planes de Microsoft Flow](https://aka.ms/microsoftflowplans))

## <a name="trigger-a-workflow-when-a-rule-is-triggered"></a>Desencadenar un flujo de trabajo al desencadenar una regla

En esta sección se muestra cómo desencadenar una notificación para dispositivos móviles en la aplicación móvil de Flow cuando se desencadena una regla en IoT Central.

1. Empiece [creando una regla en IoT Central](howto-create-telemetry-rules.md). Tras guardar las condiciones de la regla, haga clic en **Acción de Microsoft Flow** como nueva acción. Una nueva pestaña o ventana, que le llevará a Microsoft Flow, debe abrirse en su explorador.

    ![Crear una acción de Microsoft Flow](media/howto-add-microsoft-flow/createflowaction.PNG)

1. Inicie sesión en Microsoft Flow. No es necesario que se trate de la misma cuenta que usa en IoT Central. Irá a una página de información general donde aparece un conector de IoT Central conectándose a una acción personalizada.

1. Inicie sesión en el conector de IoT Central y haga clic en **Continuar**. Se le dirige a Microsoft Flow Designer para crear su flujo de trabajo. El flujo de trabajo tiene un desencadenador de IoT Central que ya tiene rellenados su Aplicación y Regla.

1. Elija **+ Nuevo paso** y **Agregar una acción**. En este momento, puede agregar la acción que desee a su flujo de trabajo. Como ejemplo, vamos a enviar una notificación móvil. Busque la **notificación** y elija **Notificaciones: enviarme una notificación móvil**.

1. En la acción, rellene el campo Texto con aquello que desee que se diga en su notificación. Puede incluir *Contenido dinámico* de su regla de IoT Central, difundiendo información importante como el nombre de dispositivo y la marca de tiempo en su notificación.

    > [!NOTE]
    > Haga clic en el texto "Ver más" en la ventana Contenido dinámico para obtener valores de medida y propiedad que desencadenaron la regla.

    ![Acción de edición de Flow con panel dinámico abierto](./media/howto-add-microsoft-flow/flowdynamicpane.PNG)

1. Cuando termine de editar su acción, haga clic en **Guardar**. Se le dirigirá a la página de información general de su flujo de trabajo. Aquí puede ver el historial de ejecución y compartirlo con otros compañeros.

    > [!NOTE]
    > Si desea que otros usuarios de su aplicación IoT Central editen esta regla, debe compartirla con ellos en Microsoft Flow. Agregue sus cuentas de Microsoft Flow como propietarios en su flujo de trabajo.

1. Si vuelve a su aplicación IoT Central, verá que esta regla tiene una acción de Microsoft Flow en el área Acciones.

Siempre puede empezar a crear un flujo de trabajo usando el conector de IoT Central en Microsoft Flow. A continuación, puede elegir a qué aplicación IoT Central y a qué regla conectarse.

## <a name="create-a-device-in-a-workflow"></a>Creación de un dispositivo en un flujo de trabajo

En esta sección se muestra cómo crear un nuevo dispositivo en IoT Central con solo pulsar un botón en un dispositivo móvil mediante la aplicación móvil de Microsoft Flow. Puede usar esta acción en Flow para integrar sistemas ERP como Dynamics con IoT Central creando un nuevo dispositivo cuando un dispositivo se agrega en otra parte.

1. Empiece creando un flujo de trabajo en blanco en Microsoft Flow.

1. Busque **Botón de flujo para móviles** como desencadenador.

1. En este desencadenador, agregue una entrada de texto llamada **Nombre de dispositivo**. Cambie el texto de descripción para que sea **Enter the device name of your new device** (Escriba el nombre del nuevo dispositivo).

1. Agregue una nueva acción. Busque la acción **Azure IoT Central - Create a device** (Azure IoT Central: crear un dispositivo).

1. Seleccione su aplicación y elija una plantilla de dispositivo desde la que crear un dispositivo en las listas desplegables. Verá cómo se expande la acción para mostrar todas las propiedades y la configuración del dispositivo.

1. Seleccione el campo Nombre de dispositivo. En el panel Contenido dinámico, elija **Nombre de dispositivo**. Este valor se pasará desde la entrada escrita por el usuario a través de la aplicación móvil, y será el nombre de su nuevo dispositivo en IoT Central. En este ejemplo, el único campo obligatorio es el nombre de dispositivo, indicado por el asterisco rojo. Otra plantilla de dispositivo puede tener varios campos obligatorios que deban rellenarse para crear un nuevo dispositivo.

    ![Panel dinámico de acciones de creación de dispositivos de Flow](./media/howto-add-microsoft-flow/flowcreatedevice.PNG)
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

1. Elija su aplicación en la lista desplegable. Ahora, necesitará el identificador del dispositivo existente que quiere actualizar. Puede obtener el identificador del dispositivo IoT Central en la dirección URL del explorador.

    ![Id. de dispositivo del Device Explorer de IoT Central](./media/howto-add-microsoft-flow/iotcdeviceid.PNG)

1. Puede actualizar el nombre del dispositivo. Para actualizar cualquiera de las propiedades y la configuración del dispositivo, debe seleccionar la plantilla de dispositivo que quiera actualizar en la lista desplegable **Plantilla de dispositivo**. El icono de la acción se expande para mostrar todas las propiedades y la configuración que puede actualizar.

    ![Flujo de trabajo de actualización de dispositivos de Flow](./media/howto-add-microsoft-flow/flowupdatedevice.PNG)

1. Seleccione cada una de las propiedades y la configuración que desee actualizar. En el panel Contenido dinámico, elija la entrada correspondiente del desencadenador. En este ejemplo, el valor Ubicación se propaga hacia abajo para actualizar la propiedad Ubicación del dispositivo.

1. Por último, guarde su flujo de trabajo.

1. Pruebe su flujo de trabajo en la aplicación móvil de Microsoft Flow. Vaya a la pestaña **Botones** de la aplicación. Debe ver su flujo de trabajo Botón -> Actualización de un dispositivo. Escriba las entradas y vea cómo se actualiza el dispositivo en IoT Central.

## <a name="delete-a-device-in-a-workflow"></a>Eliminación de un dispositivo en un flujo de trabajo

Puede eliminar un dispositivo por su identificador de dispositivo con la acción **Azure IoT Central - Delete a device** (Azure IoT Central: eliminar un dispositivo). Este es un flujo de trabajo de ejemplo que elimina un dispositivo con solo pulsar un botón en la aplicación móvil de Microsoft Flow.

   ![Flujo de trabajo de eliminación de dispositivos de Flow](./media/howto-add-microsoft-flow/flowdeletedevice.PNG)
    
## <a name="troubleshooting"></a>solución de problemas

Si tiene problemas para crear una conexión al conector de Azure IoT Central, estas sugerencias le ayudarán.

1. Las cuentas personales de Microsoft (como los dominios @hotmail.com, @live.com y @outlook.com) no se admiten en este momento. Debe usar una cuenta profesional o educativa de AAD.

2. Para usar el conector de IoT Central de Microsoft Flow, debe haber iniciado sesión al menos una vez en la aplicación IoT Central. En caso contrario, la aplicación no aparecerá en las listas desplegables de aplicaciones.

3. Si recibe un error mientras usa una cuenta de AAD, intente abrir Windows PowerShell y ejecute los siguientes commandlets como administrador.
    ``` PowerShell
    Install-Module AzureAD
    Connect-AzureAD
    New-AzureADServicePrincipal -AppId 9edfcdd9-0bc5-4bd4-b287-c3afc716aac7 -DisplayName "Azure IoT Central"
    ```
    
## <a name="next-steps"></a>Pasos siguientes
Ahora que ya sabe cómo usar Microsoft Flow para crear flujos de trabajo, el siguiente paso sugerido es [administrar dispositivos](howto-manage-devices.md).

