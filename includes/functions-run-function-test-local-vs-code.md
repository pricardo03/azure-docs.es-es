---
title: archivo de inclusión
description: archivo de inclusión
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 05/31/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 88a4fe8b9f0b477ed851a03742a9957c08b7cbf0
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67455180"
---
## <a name="run-the-function-locally"></a>Ejecución local de la función

Azure Functions Core Tools le permite ejecutar un proyecto de Azure Functions en el equipo de desarrollo local.

1. Para probar la función, establezca un punto de interrupción en el código de la función y presione F5 para iniciar el proyecto de la aplicación de función. La salida de Core Tools aparece en el panel **Terminal**.

1. En el panel **Terminal**, copie el punto de conexión de la dirección URL de la función desencadenada por HTTP. Esta dirección URL incluye la clave de función, que se pasa al parámetro de consulta `code`.

    ![Salida local de Azure](./media/functions-run-function-test-local-vs-code/functions-vscode-f5.png)

1. Pegue la dirección URL de la solicitud HTTP en la barra de direcciones del explorador. Agregue la cadena de consulta `?name=<yourname>` a esta dirección URL y ejecute la solicitud. La ejecución se pone en pausa cuando se alcanza el punto de interrupción.

1. Si continúa la ejecución, aquí se muestra la respuesta en el explorador a la solicitud GET:

    ![Respuesta de localhost de la función en el explorador](./media/functions-run-function-test-local-vs-code/functions-test-local-browser.png)

1. Para detener la depuración, presione Mayús + F5.