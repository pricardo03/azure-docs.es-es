---
title: Desencadenamiento de Azure Functions con webhooks en Azure IoT Central
description: Cree una aplicación de función que se ejecute cada vez que se desencadene una regla en Azure IoT Central.
author: viv-liu
ms.author: viviali
ms.date: 09/17/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 694c259472bf7e18f0ae3d424acf5b5cfb7ea7ab
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "46294261"
---
# <a name="trigger-azure-functions-using-webhooks-in-azure-iot-central"></a>Desencadenamiento de Azure Functions con webhooks en Azure IoT Central

*Este tema se aplica a creadores y administradores.*

Use Azure Functions para ejecutar código sin servidor en la salida del webhook a partir de las reglas de IoT Central. No tendrá que aprovisionar ninguna máquina virtual ni publicar ninguna aplicación web para usar Azure Functions, sino que, en su lugar, podrá ejecutar este código sin servidor. Use Azure Functions para transformar la carga del webhook antes de enviarla a su destino final, como una base de datos SQL o Event Grid. 

## <a name="prerequisites"></a>Requisitos previos
+ Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="how-to-connect-azure-functions"></a>Cómo conectarse a Azure Functions

1. [Cree una aplicación de función en Azure Portal](https://ms.portal.azure.com/#create/Microsoft.FunctionApp).

    ![Crear una aplicación de función en Azure Portal](media/howto-trigger-azure-functions/createfunction.png)

2. Expanda su aplicación de función y haga clic en el **botón +**, que se encuentra junto a Functions. Si esta función es la primera de su instancia de Function App, seleccione **Función personalizada**. Se muestra el conjunto completo de plantillas de funciones.
    
    ![Elegir la función personalizada en la aplicación de función](media/howto-trigger-azure-functions/customfunction.png)

3. En el campo de búsqueda, escriba **"genérico"** y seleccione el idioma que quiera para la plantilla del desencadenador del webhook genérico. Este tema utiliza una función de C#. 

    ![Seleccione el desencadenador del webhook genérico.](media/howto-trigger-azure-functions/genericwebhooktrigger.png)

4. En la función nueva, haga clic en **</> Obtener la dirección URL de la función** y copie y guarde los valores. Usará este valor para configurar el webhook. 

    ![Obtener la dirección URL de la función](media/howto-trigger-azure-functions/getfunctionurl.png)
4. En IoT Central, navegue a la regla a la que quiera conectar su aplicación de función.

5. Agregue una acción de webhook. Escriba un **nombre para mostrar** y pegue la dirección URL de función que copió anteriormente en el campo **Dirección URL de devolución de llamada**.

    ![Escriba la dirección URL de la función en el campo Dirección URL de devolución de llamada.](media/howto-trigger-azure-functions/configurewebhook.PNG)

6. Guarde la regla. Ahora cuando se desencadene la regla, el webhook invocará la ejecución de la aplicación de función. En la aplicación de función, puede hacer clic en **Monitor** para ver el historial de la invocación de la función. Puede usar Application Insights o la vista clásica para ver el historial.

    ![Supervisar el historial de invocación de la función](media/howto-trigger-azure-functions/monitorfunction.PNG)

Para obtener más información, visite el artículo de Azure Functions sobre la [creación de una función desencadenada por un webhook genérico](https://docs.microsoft.com/azure/azure-functions/functions-create-generic-webhook-triggered-function). 

## <a name="next-steps"></a>Pasos siguientes
Ahora que ha aprendido a configurar y usar webhooks, el siguiente paso sugerido es explorar la [creación de flujos de trabajo en Microsoft Flow](howto-add-microsoft-flow.md).
