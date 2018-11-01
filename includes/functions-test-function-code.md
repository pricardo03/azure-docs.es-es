---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: f7e023bcfeaa07a4ee9a80ccf4ec17120605c1ba
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50134247"
---
## <a name="test"></a>Prueba de la función

Use cURL para probar la función implementada en un equipo Mac o Linux o use Bash en Windows. Ejecute el siguiente comando cURL, reemplazando el marcador de posición `<app_name>` por el nombre de su Function App. Anexe la cadena de consulta `&name=<yourname>` a la dirección URL.

```bash
curl https://<app_name>.azurewebsites.net/api/HttpTrigger?name=<yourname>
```  

![La respuesta de función que se muestra en un explorador.](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png)  

Si no tiene cURL disponible en su línea de comandos, escriba la misma dirección URL en la dirección de su explorador web. De nuevo, reemplace el marcador de posición `<app_name>` por el nombre de su Function App, anexe la cadena de consulta `&name=<yourname>` a la dirección URL y ejecute la solicitud.

    https://<app_name>.azurewebsites.net/api/HttpTrigger?name=<yourname>
   
![La respuesta de función que se muestra en un explorador.](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  
