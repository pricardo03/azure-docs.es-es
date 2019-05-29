---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/24/2019
ms.author: glenga
ms.openlocfilehash: a3f75b7273164abc5318f16e9ab8d9883ff0c0aa
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66132136"
---
## <a name="test"></a>Prueba de la función en Azure

Use cURL para probar la función implementada. Con la dirección URL que copió del paso anterior, anexe la cadena de consulta `&name=<yourname>` a la dirección URL como en el ejemplo siguiente:

```bash
curl https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....&name=<yourname>
```

![Uso de cURL para llamar a la función en Azure.](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png) 

También puede pegar la dirección URL que copió en la barra de direcciones del explorador web. Vuelva a anexar la cadena de consulta `&name=<yourname>` a la dirección URL antes de ejecutar la solicitud.

![Uso de un explorador web para llamar a la función.](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  
