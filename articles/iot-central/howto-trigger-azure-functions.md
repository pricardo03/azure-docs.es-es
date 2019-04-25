---
title: Desencadenamiento de Azure Functions con webhooks en Azure IoT Central
description: Cree una aplicación de función que se ejecute cada vez que se desencadene una regla en Azure IoT Central.
author: viv-liu
ms.author: viviali
ms.date: 03/26/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 0d92e9bdf8ec207e5ef0e3f891c162182b5a4fff
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60518323"
---
# <a name="trigger-azure-functions-using-webhooks-in-azure-iot-central"></a>Desencadenamiento de Azure Functions con webhooks en Azure IoT Central

*Este tema se aplica a creadores y administradores.*

Use Azure Functions para ejecutar código sin servidor en la salida del webhook a partir de las reglas de IoT Central. No tendrá que aprovisionar una máquina virtual o publicar una aplicación web para usar Azure Functions, pero en su lugar, puede ejecutar este código sin servidor. Use Azure Functions para transformar la carga del webhook antes de enviarla a su destino final, como una base de datos SQL o Event Grid.

## <a name="prerequisites"></a>Requisitos previos

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="how-to-connect-azure-functions"></a>Cómo conectarse a Azure Functions

1. [Crear una nueva aplicación de función en Azure portal](https://ms.portal.azure.com/#create/Microsoft.FunctionApp).

    ![Crear una nueva aplicación de función en Azure portal](media/howto-trigger-azure-functions/createfunction.png)

2. Expanda la aplicación de función y seleccione el **botón +** junto a las funciones. Si esta función es la primera de ellas en la aplicación de función, seleccione **en portal** como entorno de desarrollo y seleccione **continuar**.

    ![Elegir la función personalizada en la aplicación de función](media/howto-trigger-azure-functions/customfunction.png)

3. Elija **Webhook y API** plantilla y seleccione **crear**. En este tema usa la función de Azure basados en .NET.

    ![Seleccione el desencadenador del webhook genérico.](media/howto-trigger-azure-functions/genericwebhooktrigger.png)

4. En la nueva función, seleccione **<> / Get función URL**, a continuación, copie y guarde el valor. Use este valor para configurar el webhook.

    ![Obtener la dirección URL de la función](media/howto-trigger-azure-functions/getfunctionurl.png)

4. En IoT Central, navegue a la regla a la que quiera conectar su aplicación de función.

5. Agregue una acción de webhook. Escriba un **nombre para mostrar** y pegue la dirección URL de función que copió anteriormente en el campo **Dirección URL de devolución de llamada**.

    ![Escriba la dirección URL de la función en el campo Dirección URL de devolución de llamada.](media/howto-trigger-azure-functions/configurewebhook.PNG)

6. Guarde la regla. Ahora cuando se desencadena la regla, el webhook invoca la aplicación de función se ejecute. En la aplicación de función, puede seleccionar **Monitor** para ver el historial de la invocación de la función. Puede usar Application Insights o la vista clásica para ver el historial.

    ![Supervisar el historial de invocación de la función](media/howto-trigger-azure-functions/monitorfunction.PNG)

Para obtener más información, visite el artículo de Azure Functions sobre la [creación de una función desencadenada por un webhook genérico](https://docs.microsoft.com/azure/azure-functions/functions-create-generic-webhook-triggered-function).

## <a name="next-steps"></a>Pasos siguientes
Ahora que ha aprendido a configurar y usar webhooks, el siguiente paso sugerido es explorar la [creación de flujos de trabajo en Microsoft Flow](howto-add-microsoft-flow.md).
