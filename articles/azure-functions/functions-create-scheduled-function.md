---
title: Crear una función que se ejecuta según una programación en Azure | Microsoft Docs
description: Obtenga información sobre cómo crear una función de Azure que se ejecuta según la programación que usted defina.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.assetid: ba50ee47-58e0-4972-b67b-828f2dc48701
ms.service: azure-functions
ms.devlang: multiple
ms.topic: quickstart
ms.date: 03/28/2018
ms.author: glenga
ms.custom: mvc, cc996988-fb4f-47
ms.openlocfilehash: d82e80530d375a9d0f0a6d3f491d3070942ca02f
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2019
ms.locfileid: "54331918"
---
# <a name="create-a-function-in-azure-that-is-triggered-by-a-timer"></a>Cree una función en Azure que se desencadena mediante un temporizador

Aprenda a usar Azure Functions para crear una función [sin servidor](https://azure.microsoft.com/solutions/serverless/) que se ejecute según la programación que se defina.

![Creación de una aplicación de función en Azure Portal](./media/functions-create-scheduled-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial:

+ Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-an-azure-function-app"></a>Creación de una Function App de Azure

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Function App creada correctamente.](./media/functions-create-first-azure-function/function-app-create-success.png)

Después, cree una función en la nueva Function App.

<a name="create-function"></a>

## <a name="create-a-timer-triggered-function"></a>Creación de una función desencadenada por un temporizador

1. Expanda su instancia de Function App y haga clic en el botón **+**, que se encuentra junto a **Functions**. Si se trata de la primera función de Function App, seleccione **En el portal** y, después, **Continuar**. En caso contrario, vaya al paso tres.

   ![Página de inicio rápido de Functions en Azure Portal](./media/functions-create-scheduled-function/function-app-quickstart-choose-portal.png)

2. Elija **Más plantillas** y, a continuación, **Finish and view templates** (Finalizar y ver plantillas).

    ![Guía de inicio rápido de Functions para elegir más plantillas](./media/functions-create-scheduled-function/add-first-function.png)

3. En el campo de búsqueda, escriba `timer` y configure el nuevo desencadenador según la configuración especificada en la tabla que aparece debajo de la imagen.

    ![Creación de una función desencadenada mediante un temporizador en Azure Portal.](./media/functions-create-scheduled-function/functions-create-timer-trigger-2.png)

    | Configuración | Valor sugerido | Descripción |
    |---|---|---|
    | **Nombre** | Valor predeterminado | Define el nombre de la función desencadenada por el temporizador. |
    | **Programación** | 0 \*/1 \* \* \* \* | [Expresión CRON](functions-bindings-timer.md#cron-expressions) de seis campos que programa la función para que se ejecute cada minuto. |

4. Haga clic en **Create**(Crear). Se crea una función en el lenguaje elegido que se ejecuta cada minuto.

5. Vea la información de seguimiento que se escribe en los registros para comprobar la ejecución.

    ![Visor de registros de las funciones en Azure Portal.](./media/functions-create-scheduled-function/functions-timer-trigger-view-logs2.png)

Ahora puede cambiar la programación de la función para que se ejecute una vez cada hora, en lugar de cada minuto.

## <a name="update-the-timer-schedule"></a>Actualizar la programación del temporizador

1. Expanda la función y haga clic en **Integrar**. Aquí es donde se definen los enlaces de entrada y salida de la función y se establece la programación. 

2. Escriba el nuevo valor de **Programación** `0 0 */1 * * *` y, después, haga clic en **Guardar**.  

![Programación del temporizador de actualización de funciones en Azure Portal.](./media/functions-create-scheduled-function/functions-timer-trigger-change-schedule.png)

Ahora tiene una función que se ejecuta una vez cada hora. 

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

Ha creado una función que se ejecuta según una programación.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Para obtener más información sobre los desencadenadores de temporizador, vea [Programación de la ejecución de código con Azure Functions](functions-bindings-timer.md).
