---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 08/09/2019
ms.author: jroth
ms.openlocfilehash: 8cdfe82888cca471aa5aae6717a16f62000c4ad5
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2019
ms.locfileid: "71839241"
---
| Resource | Gratuito | Compartido | Básica | Estándar | Premium (v2) | Aislado </th> |
| --- | --- | --- | --- | --- | --- | --- |
| [Aplicaciones web, móviles o de API](https://azure.microsoft.com/services/app-service/) por [plan de Azure App Service](../articles/app-service/overview-hosting-plans.md)<sup>1</sup> |10 |100 |Ilimitado<sup>2</sup> |Ilimitado<sup>2</sup> |Ilimitado<sup>2</sup> |Ilimitado<sup>2</sup>|
| [plan de App Service](../articles/app-service/overview-hosting-plans.md) |10 por región |10 por grupo de recursos |100 por grupo de recursos |100 por grupo de recursos |100 por grupo de recursos |100 por grupo de recursos|
| Tipo de instancia de proceso |Compartido |Compartido |Dedicado<sup>3</sup> |Dedicado<sup>3</sup> |Dedicado<sup>3</sup></p> |Dedicado<sup>3</sup>|
| [Escalar horizontalmente](../articles/app-service/manage-scale-up.md) (número máximo de instancias) |1 compartido |1 compartido |3 dedicados<sup>3</sup> |10 dedicados<sup>3</sup> |20 dedicados<sup>3</sup>|100 dedicados<sup>4</sup>|
| Almacenamiento<sup>5</sup> |1 GB<sup>5</sup> |1 GB<sup>5</sup> |10 GB<sup>5</sup> |50 GB<sup>5</sup> |250 GB<sup>5</sup></p> |1 TB<sup>5</sup>|
| Tiempo de CPU (5 minutos)<sup>6</sup> |3 minutos |3 minutos |Sin límite, se paga según las [tarifas](https://azure.microsoft.com/pricing/details/app-service/)</a> estándar. |Sin límite, se paga según las [tarifas](https://azure.microsoft.com/pricing/details/app-service/)</a> estándar. |Sin límite, se paga según las [tarifas](https://azure.microsoft.com/pricing/details/app-service/)</a> estándar. |Sin límite, se paga según las [tarifas](https://azure.microsoft.com/pricing/details/app-service/)</a> estándar.|
| Tiempo de CPU (día)<sup>6</sup> |60 minutos |240 minutos |Sin límite, se paga según las [tarifas](https://azure.microsoft.com/pricing/details/app-service/)</a> estándar. |Sin límite, se paga según las [tarifas](https://azure.microsoft.com/pricing/details/app-service/)</a> estándar. |Sin límite, se paga según las [tarifas](https://azure.microsoft.com/pricing/details/app-service/)</a> estándar. |Sin límite, se paga según las [tarifas](https://azure.microsoft.com/pricing/details/app-service/)</a> estándar. |
| Memoria (1 hora) |1024 MB por plan de App Service |1024 MB por aplicación |N/D |N/D |N/D |N/D |
| Ancho de banda |165 MB |Ilimitado; se aplican [tasas por transferencia de datos](https://azure.microsoft.com/pricing/details/data-transfers/) |Ilimitado; se aplican [tasas por transferencia de datos](https://azure.microsoft.com/pricing/details/data-transfers/) |Ilimitado; se aplican [tasas por transferencia de datos](https://azure.microsoft.com/pricing/details/data-transfers/) |Ilimitado; se aplican [tasas por transferencia de datos](https://azure.microsoft.com/pricing/details/data-transfers/) |Ilimitado; se aplican [tasas por transferencia de datos](https://azure.microsoft.com/pricing/details/data-transfers/) |
| Arquitectura de la aplicación |32 bits |32 bits |32 bits/64 bits |32 bits/64 bits |32 bits/64 bits |32 bits/64 bits |
| Web Sockets por instancia<sup>7</sup> |5 |35 |350 |Ilimitado |Ilimitado |Ilimitado |
| Conexiones de depurador [simultáneas](../articles/app-service/troubleshoot-dotnet-visual-studio.md) por aplicación |1 |1 |1 |5 |5 |5 |
| Instancias de App Service Certificate por suscripción<sup>10</sup>| No compatible | No compatible |10 |10 |10 |10 |
| Dominios personalizados por aplicación</a> |0 (solo subdominio de azurewebsites.net)|500 |500 |500 |500 |500 |
| Compatibilidad con dominio [Compatibilidad con SSL](../articles/app-service/app-service-web-tutorial-custom-ssl.md) |No admitido, certificado comodín para *.azurewebsites.net disponible de forma predeterminada.|No admitido, certificado comodín para *.azurewebsites.net disponible de forma predeterminada.|Conexiones SSL SNI ilimitadas |Se incluyen conexiones SNI SSL ilimitadas y 1 conexión SSL de IP |Se incluyen conexiones SNI SSL ilimitadas y 1 conexión SSL de IP | Se incluyen conexiones SNI SSL ilimitadas y 1 conexión SSL de IP|
| Equilibrador de carga integrado | |X |X |X |X |X<sup>9</sup> |
| [Siempre activado](../articles/app-service/configure-common.md) | | |X |X |X |X |
| [Copias de seguridad programadas](../articles/app-service/manage-backup.md) | | | | Copias de seguridad programadas cada 2 horas, con un máximo de 12 copias de seguridad al día (manuales y programadas). | Copias de seguridad programadas cada hora, con un máximo de 50 copias de seguridad al día (manuales y programadas). | Copias de seguridad programadas cada hora, con un máximo de 50 copias de seguridad al día (manuales y programadas). |
| [Autoscale](../articles/app-service/manage-scale-up.md) | | | |X |X |X |
| [WebJobs](../articles/app-service/webjobs-create.md)<sup>8</sup> |X |X |X |X |X |X |
| [Compatibilidad con Azure Scheduler](https://azure.microsoft.com/services/scheduler/) | |X |X |X |X |X |
| [Supervisión de extremos](../articles/app-service/web-sites-monitor.md) | | |X |X |X |X |
| [Espacios de ensayo](../articles/app-service/deploy-staging-slots.md) | | | |5 |20 |20 |
| Contrato de nivel de servicio | |  |99,95 %|99,95 %|99,95 %|99,95 %|  

<sup>1</sup>Las aplicaciones y las cuotas de almacenamiento son por plan de App Service, a menos que se indique lo contrario.  
<sup>2</sup>El número real de aplicaciones que puedes hospedar en estas máquinas depende de la actividad de las aplicaciones, el tamaño de las instancias de máquina y el correspondiente uso de los recursos.  
<sup>3</sup>Las instancias dedicadas pueden tener diferentes tamaños. Para más información, consulte [Precios de App Service](https://azure.microsoft.com/pricing/details/app-service/).  
<sup>4</sup>Se permiten más a petición.  
<sup>5</sup>El límite de almacenamiento es el tamaño total del contenido en todas las aplicaciones en el mismo plan de App Service. El tamaño total del contenido de todas las aplicaciones en todos los planes de App Service en un solo grupo de recursos y región no puede superar los 500 GB.  
<sup>6</sup>Estos recursos están limitados por los recursos físicos en las instancias dedicadas (el tamaño de la instancia y el número de instancias).  
<sup>7</sup>Si se escala una aplicación en el nivel Básico a dos instancias, dispones de 350 conexiones simultáneas para cada una de las dos. En el nivel Estándar u otro superior, no hay ningún límite teórico para los sockets web, pero otros factores pueden limitar el número de estos. Por ejemplo, el número máximo de solicitudes simultáneas permitidas (que define `maxConcurrentRequestsPerCpu`) es: 7500 por máquina virtual pequeña, 15 000 por máquina virtual mediana (7500 x 2 núcleos) y 75 000 por máquina virtual de gran tamaño (18 750 x 4 núcleos).  
<sup>8</sup>Ejecute scripts o archivos ejecutables personalizados bajo demanda, según una programación o de manera continua como tarea en segundo plano dentro de su instancia de App Service. Siempre disponible se requiere para la ejecución continua de Trabajos web. Se requiere Azure Scheduler de nivel Gratis o Estándar para Trabajos web programados. No hay ningún límite predefinido en el número de trabajos web que se pueden ejecutar en una instancia de App Service. Hay límites prácticos que dependen de lo que el código de aplicación intente hacer.  
<sup>9</sup>Las SKU de App Service aislado pueden tener equilibrio de carga interno (ILB) con Azure Load Balancer, lo que significa que no hay conectividad pública desde Internet. Como resultado, algunas características de un App Service aislado con ILB deben usarse desde máquinas que tienen acceso directo al punto de conexión de red del ILB.  
<sup>10</sup> El límite de cuota de App Service Certificate por suscripción se puede aumentar mediante una solicitud de soporte técnico hasta un límite máximo de 200.  