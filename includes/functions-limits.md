---
author: ggailey777
ms.service: billing
ms.topic: include
ms.date: 05/09/2019
ms.author: glenga
ms.openlocfilehash: 8f30d9fb2fcfe8f55af13d7726aa8458f8733b3f
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66236012"
---
| Recurso | [Plan de consumo](../articles/azure-functions/functions-scale.md#consumption-plan) | [Plan Premium](../articles/azure-functions/functions-scale.md#premium-plan-public-preview) | [Plan de App Service](../articles/azure-functions/functions-scale.md#app-service-plan)<sup>1</sup> |
| --- | --- | --- | --- |
| Escalado horizontal | Basado en eventos | Basado en eventos | [Manual/autoscale](../articles/app-service/web-sites-scale.md) | 
|Default [tiempo de espera](../articles/azure-functions/functions-scale.md#timeout) (min) |5 | 30 |30<sup>2</sup> |
|Max [tiempo de espera](../articles/azure-functions/functions-scale.md#timeout) (min) |10 | sin enlazar | unbounded<sup>3</sup> |
| Número máximo de conexiones salientes (por instancia) | 600 activo (total de 1200) | sin enlazar | sin enlazar |
| Tamaño de solicitud máximo (MB)<sup>4</sup> | 100 | 100 | 100 |
| La longitud máxima de cadena de consulta<sup>4</sup> | 4096 | 4096 | 4096 |
| La longitud máxima de dirección URL de solicitud<sup>4</sup> | 8192 | 8192 | 8192 |
| [ACU](../articles/virtual-machines/windows/acu.md) por instancia | 100 | 210-840 | 100-840 |
| Memoria máxima (GB por instancia) | 1.5 | 3.5-14 | 1.75-14 |
| Aplicaciones de función por cada plan |100 |100 |unbounded<sup>5</sup> |
| [Planes de App Service](../articles/app-service/overview-hosting-plans.md) | 100 por [región](https://azure.microsoft.com/global-infrastructure/regions/) |100 por grupo de recursos |100 por grupo de recursos |
| Storage<sup>6</sup> |1 GB |250 GB |50-1000 GB |
| Dominios personalizados por aplicación</a> |500<sup>7</sup> |500 |500 |
| Compatibilidad con dominio [Compatibilidad con SSL](../articles/app-service/app-service-web-tutorial-custom-ssl.md) |No compatible, el certificado comodín para *. azurewebsites.net disponible de forma predeterminada| sin enlazar SSL SNI y 1 conexiones SSL de IP incluidos |sin enlazar SSL SNI y 1 conexiones SSL de IP incluidos | 

<sup>1</sup>para los límites específicos de las distintas opciones de plan de App Service, consulte el [los límites del plan de App Service](../articles/azure-subscription-service-limits.md#app-service-limits).  
<sup>2</sup>de forma predeterminada, el tiempo de espera para el tiempo de ejecución de Functions 1.x en un plan de App Service es ilimitado.  
<sup>3</sup>requiere el plan de App Service se establezca en [Always On](../articles/azure-functions/functions-scale.md#always-on). Abonar estándar [tarifas](https://azure.microsoft.com/pricing/details/app-service/).  
<sup>4</sup> estos límites son [establecido en el host](https://github.com/Azure/azure-functions-host/blob/dev/src/WebJobs.Script.WebHost/web.config).  
<sup>5</sup> el número real de aplicaciones de función que puede hospedar depende de la actividad de las aplicaciones, el tamaño de las instancias de máquina y el uso de recursos correspondiente.   
<sup>6</sup>el límite de almacenamiento es el tamaño total del contenido en el almacenamiento temporal en todas las aplicaciones en el mismo plan de App Service. Plan de consumo, usa Azure Files para el almacenamiento temporal.  
<sup>7</sup>cuando se hospeda la aplicación de función en un [plan de consumo](../articles/azure-functions/functions-scale.md#consumption-plan), se admite solo la opción de CNAME. Para aplicaciones de función en un [plan Premium](../articles/azure-functions/functions-scale.md#premium-plan-public-preview) o un [plan de App Service](../articles/azure-functions/functions-scale.md#app-service-plan), puede asignar un dominio personalizado mediante un registro CNAME o un registro. 
