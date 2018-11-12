---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: ae88a6132c79d9d9f930f6445e53b0f0452c9f2a
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51262614"
---
## <a name="test"></a>Prueba de la función

Use cURL para probar la función implementada en un equipo Mac o Linux o use Bash en Windows. Ejecute el siguiente comando cURL, reemplazando el marcador de posición `<app_name>` por el nombre de su Function App. Anexe la cadena de consulta `&name=<yourname>` a la dirección URL.

```bash
curl https://<app_name>.azurewebsites.net/api/MyHttpTrigger?name=<yourname>
```  

![La respuesta de función que se muestra en un explorador.](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png)  

Si no tiene cURL disponible en su línea de comandos, escriba la misma dirección URL en la dirección de su explorador web. De nuevo, reemplace el marcador de posición `<app_name>` por el nombre de su Function App, anexe la cadena de consulta `&name=<yourname>` a la dirección URL y ejecute la solicitud.

    https://<app_name>.azurewebsites.net/api/MyHttpTrigger?name=<yourname>
   
![La respuesta de función que se muestra en un explorador.](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  
