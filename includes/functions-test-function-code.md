---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 914c006daf49e22ebec870a549bfdbc63f882647
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2019
ms.locfileid: "55148051"
---
## <a name="test"></a>Prueba de la función

Use cURL para probar la función implementada en un equipo Mac o Linux o use Powershell en Windows. Ejecute el siguiente comando cURL, reemplazando el marcador de posición `<app_name>` por el nombre de su Function App. Anexe la cadena de consulta `&name=<yourname>` a la dirección URL.

```powershell
Invoke-WebRequest -Uri "https://<app_name>.azurewebsites.net/api/MyHttpTrigger?name=<yourname>"
```

```bash
curl https://<app_name>.azurewebsites.net/api/MyHttpTrigger?name=<yourname>
```  

![La respuesta de función que se muestra en un explorador.](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png)  

Si no tiene `cURL`o `Invoke-WebRequest` disponibles en su línea de comandos, escriba la misma dirección URL en la dirección de su explorador web. De nuevo, reemplace el marcador de posición `<app_name>` por el nombre de su Function App, anexe la cadena de consulta `&name=<yourname>` a la dirección URL y ejecute la solicitud.

    https://<app_name>.azurewebsites.net/api/MyHttpTrigger?name=<yourname>
   
![La respuesta de función que se muestra en un explorador.](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  
