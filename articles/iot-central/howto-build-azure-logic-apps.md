---
title: Creación de flujos de trabajo con el conector de IoT Central en Azure Logic Apps | Microsoft Docs
description: Use el conector de IoT Central en Azure Logic Apps para desencadenar flujos de trabajo y crear, actualizar y eliminar dispositivos en los flujos de trabajo.
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 06/09/2019
ms.topic: conceptual
ms.service: iot-central
manager: peterpr
ms.openlocfilehash: 2269ede83ad64172e316a208871ab65053b2d10e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67053079"
---
# <a name="build-workflows-with-the-iot-central-connector-in-azure-logic-apps"></a>Creación de flujos de trabajo con el conector de IoT Central en Azure Logic Apps

*Este tema se aplica a los compiladores y administradores.*

Use Azure Logic Apps para compilar flujos de trabajo escalables automatizados que integran aplicaciones y datos en los servicios en la nube y los sistemas locales. Azure Logic Apps tiene muchos conectores entre los muchos servicios de Azure, servicios de Microsoft y otros productos de software como servicio (SaaS). Con el conector de IoT Central de Azure Logic Apps, puede desencadenar flujos de trabajo si se activa una regla en IoT Central. También puede usar las acciones en el conector de IoT Central para crear un dispositivo, actualizar las propiedades y la configuración de un dispositivo, o bien eliminar un dispositivo.

Puede usar el conector de IoT Central en Microsoft Flow. Azure Logic Apps y Microsoft Flow son servicios de integración orientados al diseñador, pero se dirigen a audiencias ligeramente diferentes. Flow permite a cualquier administrativo de oficina compilar los flujos de trabajo que necesita. Logic Apps permite a los profesionales de TI compilar las integraciones que necesitan para conectar datos. Vea [aquí](https://docs.microsoft.com/azure/azure-functions/functions-compare-logic-apps-ms-flow-webjobs) la comparación entre Flow y Logic Apps. Obtenga información sobre cómo crear flujos de trabajo con el conector de IoT Central en Microsoft Flow [aquí](howto-add-microsoft-flow.md).

## <a name="prerequisites"></a>Requisitos previos

- Una aplicación de pago por uso
- Una cuenta de Azure y una suscripción para crear y administrar Logic Apps

## <a name="trigger-a-workflow-when-a-rule-is-triggered"></a>Desencadenar un flujo de trabajo al desencadenar una regla

En esta sección se muestra cómo publicar un mensaje en Microsoft Teams cuando se desencadena una regla. Puede configurar el flujo de trabajo para usar otros conectores para realizar acciones como enviar un evento al centro de eventos, crear un nuevo elemento de trabajo de Azure DevOps o insertar una fila nueva en SQL Server.

1. Empiece [creando una regla en IoT Central](howto-create-telemetry-rules.md). Tras guardar las condiciones de la regla, seleccione el icono **Azure Logic Apps** como nueva acción. Seleccione **Crear en Azure Portal**. Se le llevará a Azure Portal para crear una nueva aplicación lógica. Es posible que tenga que iniciar sesión en su cuenta de Azure.

1. Escriba la información requerida para crear una nueva aplicación lógica. Puede elegir una suscripción de Azure en la que aprovisionar la nueva aplicación lógica. No tiene sea la misma suscripción en la que se creó la aplicación de IoT Central. Seleccione **Crear**.

    ![Creación de una aplicación lógica en Azure Portal](./media/howto-build-azure-logic-apps/createinazureportal.png)

1. Una vez que la aplicación lógica se haya creado correctamente, se le dirigirá automáticamente al Diseñador de Logic Apps. Seleccione **Aplicación lógica en blanco**. 

    ![Creación de una aplicación lógica en blanco](./media/howto-build-azure-logic-apps/blanklogicapp.png)

1. En el diseñador, busque "iot central" y elija el desencadenador **Si se activa una regla**. Inicie sesión en el conector con la cuenta con la que inicia sesión en su aplicación de IoT Central.

    ![Inicio de sesión en el conector de IoT Central](./media/howto-build-azure-logic-apps/addtrigger.png)

1. Después de iniciar sesión correctamente, verá que aparecen los campos. Seleccione la **Aplicación** y la **Regla** en las listas desplegables.

    ![Selección de regla y aplicación](./media/howto-build-azure-logic-apps/pickappandrule.png)

1. Agregue una nueva acción. Busque los **equipos de publicación de mensajes** y elija **Publicar un mensaje** en el conector de Microsoft Teams. Inicie sesión en el conector con la cuenta que usa en Microsoft Teams.

1. En la acción, elija el **equipo** y el **canal**. Rellene el campo **Mensaje** con lo que quiere que indique cada mensaje. Puede incluir *Contenido dinámico* de su regla de IoT Central, difundiendo información importante como el nombre de dispositivo y la marca de tiempo en su notificación.
    > [!NOTE]
    > Seleccione el texto **Ver más** en la ventana Contenido dinámico para obtener los valores de medida y propiedad que desencadenaron la regla.

    ![Acción de edición de aplicación lógica con panel dinámico abierto](./media/howto-build-azure-logic-apps/buildworkflow.png)

1. Cuando termine de editar su acción, seleccione **Guardar**.

1. Si vuelve a su aplicación IoT Central, verá que esta regla tiene una acción de Azure Logic Apps en el área Acciones.

Siempre puede empezar a crear un flujo de trabajo usando el conector de IoT Central en Logic Apps en Azure Portal. A continuación, puede elegir a qué aplicación IoT Central y a qué regla conectarse, y funcionará del mismo modo. No es necesario iniciar desde la página de reglas de IoT Central cada vez.

## <a name="create-update-and-delete-a-device-in-a-workflow"></a>Creación, actualización y eliminación de un dispositivo en un flujo de trabajo

Al crear un flujo de trabajo en la aplicación lógica, puede agregar una acción mediante el conector de IoT Central. Las acciones disponibles son **Crear un dispositivo**, **Actualizar un dispositivo** y **Eliminar un dispositivo**.

> [!NOTE]
> Para **Actualizar un dispositivo** y **Eliminar un dispositivo**, necesitará el identificador del dispositivo que desea actualizar o eliminar. El identificador del dispositivo de IoT Central se puede obtener en **Device Explorer**

![Id. de dispositivo del Device Explorer de IoT Central](./media/howto-build-azure-logic-apps/iotcdeviceid.png)

## <a name="next-steps"></a>Pasos siguientes

Ahora que ya sabe cómo usar Microsoft Flow para crear flujos de trabajo, el siguiente paso sugerido es [administrar dispositivos](howto-manage-devices.md).
