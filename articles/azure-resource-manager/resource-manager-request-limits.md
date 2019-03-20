---
title: Límites de solicitudes de Azure Resource Manager
description: En este artículo se describe cómo usar la limitación con las solicitudes de Azure Resource Manager cuando se han alcanzado los límites de suscripción.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.assetid: e1047233-b8e4-4232-8919-3268d93a3824
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2019
ms.author: tomfitz
ms.custom: seodec18
ms.openlocfilehash: 91a776ba13ffaeeb4f8184371ae45a80d829ae46
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57550650"
---
# <a name="throttling-resource-manager-requests"></a>Limitación de solicitudes de Resource Manager

Para cada suscripción e inquilino de Azure, Resource Manager permite hasta 12 000 solicitudes de lectura y 1200 solicitudes de escritura cada hora. Estas cifras están limitadas al id. de identidad que realiza las solicitudes y al id. de suscripción o de inquilino. Si las solicitudes proceden de más de un id. de entidad, el límite en toda la suscripción o inquilino es mayor a 12 000 y 1200 por hora.

Las solicitudes se aplican a su suscripción o inquilino. Las solicitudes de la suscripción son aquellas en las que se pasa el identificador de la suscripción; por ejemplo, las solicitudes para recuperar los grupos de recursos de la suscripción. Las solicitudes del inquilino no incluyen el identificador de la suscripción; por ejemplo, las solicitudes para recuperar ubicaciones válidas de Azure.

Estos límites se aplican a cada instancia de Azure Resource Manager. En cada región de Azure hay varias instancias, y Azure Resource Manager está implementado en todas las regiones de Azure.  Por lo tanto, en la práctica, los límites son realmente mucho mayores que los mencionados anteriormente, ya que las solicitudes del usuario normalmente se suministran desde muchas instancias diferentes.

Si la aplicación o el script alcanza estos límites, debe limitar las solicitudes. Este artículo muestra cómo determinar las solicitudes restantes que quedan antes de alcanzar el límite y cómo responder cuando se ha alcanzado el límite.

Cuando se alcanza este límite, recibirá el código de estado HTTP **429 Demasiadas solicitudes**.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="remaining-requests"></a>Solicitudes restantes
Puede determinar el número de solicitudes restantes examinando los encabezados de respuesta. Las solicitudes de lectura devuelven un valor en el encabezado para el número de solicitudes de lectura restantes. Escribir las solicitudes incluyen un valor para el número de solicitudes de escritura restantes. En la tabla siguiente se describen los encabezados de respuesta que puede examinar para esos valores:

| Encabezado de respuesta | DESCRIPCIÓN |
| --- | --- |
| x-ms-ratelimit-Remaining-Subscription-Reads |Lecturas restantes del ámbito de la suscripción. Este valor lo devuelven las operaciones de lectura. |
| x-ms-ratelimit-Remaining-Subscription-Writes |Escrituras restantes del ámbito de la suscripción. Este valor lo devuelven las operaciones de escritura. |
| x-ms-ratelimit-Remaining-tenant-Reads |Lecturas restantes del ámbito del inquilino |
| x-ms-ratelimit-Remaining-tenant-Writes |Escrituras restantes del ámbito del inquilino |
| x-ms-ratelimit-Remaining-Subscription-Resource-Requests |Solicitudes de tipos de recursos restantes del ámbito de la suscripción<br /><br />Este valor de encabezado solo se devuelve si un servicio ha invalidado el límite predeterminado. Resource Manager agrega este valor en lugar de las lecturas o escrituras de la suscripción. |
| x-ms-ratelimit-Remaining-Subscription-Resource-Entities-Read |Solicitudes de colección de tipos de recursos restantes del ámbito de la suscripción<br /><br />Este valor de encabezado solo se devuelve si un servicio ha invalidado el límite predeterminado. Este valor proporciona el número de solicitudes de colección restantes (recursos de lista). |
| x-ms-ratelimit-Remaining-tenant-Resource-Requests |Solicitudes de tipos de recurso restantes del ámbito del inquilino<br /><br />Este encabezado solo se agrega para las solicitudes en el nivel del inquilino, y solo si un servicio ha invalidado el límite predeterminado. Resource Manager agrega este valor en lugar de las lecturas o escrituras del inquilino. |
| x-ms-ratelimit-Remaining-tenant-Resource-Entities-Read |Solicitudes de colección de tipos de recursos restantes del ámbito del inquilino<br /><br />Este encabezado solo se agrega para las solicitudes en el nivel del inquilino, y solo si un servicio ha invalidado el límite predeterminado. |

## <a name="retrieving-the-header-values"></a>Recuperación de los valores de encabezado
El proceso de recuperación de estos valores de encabezado del código o el script es igual que el de cualquier valor de encabezado. 

Por ejemplo, en **C#**, recupere el valor del encabezado de un objeto **HttpWebResponse** denominado "**response**"con el código siguiente:

```cs
response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)
```

En **PowerShell**, recupere el valor del encabezado de una operación Invoke-WebRequest.

```powershell
$r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
$r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
```

Para obtener un ejemplo completo de PowerShell, consulte [comprobación de los límites de Resource Manager para una suscripción](https://github.com/Microsoft/csa-misc-utils/tree/master/psh-GetArmLimitsViaAPI).

Si quiere ver las solicitudes restantes de depuración, puede proporcionar el parámetro **-Debug** en el cmdlet **PowerShell**.

```powershell
Get-AzResourceGroup -Debug
```

Que devuelve muchos valores, incluido el siguiente valor de respuesta:

```powershell
DEBUG: ============================ HTTP RESPONSE ============================

Status Code:
OK

Headers:
Pragma                        : no-cache
x-ms-ratelimit-remaining-subscription-reads: 11999
```

Para obtener los límites de escritura, utilice una operación de escritura: 

```powershell
New-AzResourceGroup -Name myresourcegroup -Location westus -Debug
```

Esta operación devuelve muchos valores, incluidos los siguientes:

```powershell
DEBUG: ============================ HTTP RESPONSE ============================

Status Code:
Created

Headers:
Pragma                        : no-cache
x-ms-ratelimit-remaining-subscription-writes: 1199
```

En la **CLI de Azure**, recupere el valor del encabezado mediante la opción más detallada.

```azurecli
az group list --verbose --debug
```

Esta operación devuelve muchos valores, incluidos los siguientes:

```azurecli
msrest.http_logger : Response status: 200
msrest.http_logger : Response headers:
msrest.http_logger :     'Cache-Control': 'no-cache'
msrest.http_logger :     'Pragma': 'no-cache'
msrest.http_logger :     'Content-Type': 'application/json; charset=utf-8'
msrest.http_logger :     'Content-Encoding': 'gzip'
msrest.http_logger :     'Expires': '-1'
msrest.http_logger :     'Vary': 'Accept-Encoding'
msrest.http_logger :     'x-ms-ratelimit-remaining-subscription-reads': '11998'
```

Para obtener los límites de escritura, utilice una operación de escritura: 

```azurecli
az group create -n myresourcegroup --location westus --verbose --debug
```

Esta operación devuelve muchos valores, incluidos los siguientes:

```azurecli
msrest.http_logger : Response status: 201
msrest.http_logger : Response headers:
msrest.http_logger :     'Cache-Control': 'no-cache'
msrest.http_logger :     'Pragma': 'no-cache'
msrest.http_logger :     'Content-Length': '163'
msrest.http_logger :     'Content-Type': 'application/json; charset=utf-8'
msrest.http_logger :     'Expires': '-1'
msrest.http_logger :     'x-ms-ratelimit-remaining-subscription-writes': '1199'
```

## <a name="waiting-before-sending-next-request"></a>Espera antes de enviar la solicitud siguiente
Cuando alcance el límite de solicitudes, Azure Resource Manager devuelve el código de estado HTTP **429** y un valor **Retry-After** del encabezado. El valor **Retry-After** valor especifica el número de segundos que debe esperar (o estar en estado de suspensión) la aplicación antes de enviar la solicitud siguiente. Si envía una solicitud antes de que haya transcurrido el tiempo especificado en el valor de reintento, la solicitud no se procesará y se devolverá un nuevo valor de reintento.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener un ejemplo completo de PowerShell, consulte [comprobación de los límites de Resource Manager para una suscripción](https://github.com/Microsoft/csa-misc-utils/tree/master/psh-GetArmLimitsViaAPI).
* Para más información acerca de límites y cuotas, consulte [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](../azure-subscription-service-limits.md).
* Para obtener información sobre el control de solicitudes asincrónicas de REST, vea [Seguimiento de las operaciones asincrónicas de Azure](resource-manager-async-operations.md).
