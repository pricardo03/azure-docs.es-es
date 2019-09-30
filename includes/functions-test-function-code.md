---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/24/2019
ms.author: glenga
ms.openlocfilehash: 44eb25be12b908c8f951cb20948068da3bfc2928
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71203136"
---
## <a name="test"></a>Comprobación de la función en Azure

Use cURL para comprobar la función implementada. Con la dirección URL que copió del paso anterior, anexe la cadena de consulta `&name=<yourname>` a la dirección URL.

![Uso de cURL para llamar a la función en Azure.](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png) 

También puede pegar la dirección URL que copió, incluida la clave de función, en la barra de direcciones del explorador web. Vuelva a anexar la cadena de consulta `&name=<yourname>` a la dirección URL antes de ejecutar la solicitud.

![Uso de un explorador web para llamar a la función.](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  
