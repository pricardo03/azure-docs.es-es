---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/24/2019
ms.author: glenga
ms.openlocfilehash: 9c972508c98e87771154bd26cc166c6bea4201ee
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2020
ms.locfileid: "76279563"
---
## <a name="test"></a>Comprobación de la función en Azure

Puede usar un explorador web para comprobar la función implementada.  Copie la dirección URL, incluida la clave de función, en la barra de direcciones del explorador web. Anexe la cadena de consulta `&name=<yourname>` a la dirección URL antes de ejecutar la solicitud.

![Uso de un explorador web para llamar a la función.](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  

También puede usar cURL para comprobar la función implementada. Con la dirección URL que copió del paso anterior, anexe la cadena de consulta `&name=<yourname>` a la dirección URL.

![Uso de cURL para llamar a la función en Azure.](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png) 

