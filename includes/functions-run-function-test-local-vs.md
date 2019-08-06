---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 07/05/2019
ms.author: glenga
ms.openlocfilehash: 0493de7374cffcc40f0434d84facf50b6a708c7b
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2019
ms.locfileid: "68592866"
---
1. Para ejecutar la función, presione **F5**. También es preciso que habilite una excepción de firewall para que las herramientas puedan controlar las solicitudes de HTTP. Los niveles de autorización nunca se aplican cuando se ejecuta localmente.

2. Copie la dirección URL de la función de los resultados del runtime de Azure Functions.

    ![Runtime local de Azure](./media/functions-run-function-test-local-vs/functions-debug-local-vs.png)

3. Pegue la dirección URL de la solicitud HTTP en la barra de direcciones del explorador. Agregue la cadena de consulta `?name=<YOUR_NAME>` a esta dirección URL y ejecute la solicitud. A continuación se muestra la respuesta en el explorador para la solicitud GET local devuelta por la función: 

    ![Respuesta de localhost de la función en el explorador](./media/functions-run-function-test-local-vs/functions-run-browser-local-vs.png)

4. Para detener la depuración, presione **Mayús + F5**.