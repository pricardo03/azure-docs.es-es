---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/24/2019
ms.author: glenga
ms.openlocfilehash: 9381f436aaeedb31732f853a6c4765ac43c6a752
ms.sourcegitcommit: 2aefdf92db8950ff02c94d8b0535bf4096021b11
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/03/2019
ms.locfileid: "70170872"
---
## <a name="test"></a>Prueba de la función en Azure

Use cURL para probar la función implementada. Con la dirección URL que copió del paso anterior, anexe la cadena de consulta `&name=<yourname>` a la dirección URL.

![Uso de cURL para llamar a la función en Azure.](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png) 

También puede pegar la dirección URL que copió, incluida la clave de función, en la barra de direcciones del explorador web. Vuelva a anexar la cadena de consulta `&name=<yourname>` a la dirección URL antes de ejecutar la solicitud.

![Uso de un explorador web para llamar a la función.](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  
