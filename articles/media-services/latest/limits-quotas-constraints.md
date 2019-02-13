---
title: Cuotas y limitaciones en Azure Media Services v3 | Microsoft Docs
description: En este tema se describen las cuotas y limitaciones en Azure Media Services v3
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/03/2019
ms.author: juliako
ms.openlocfilehash: 1bada751ffac45b36faad6e978567233901adc7d
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2019
ms.locfileid: "55700322"
---
# <a name="quotas-and-limitations-in-azure-media-services-v3"></a>Cuotas y limitaciones en Azure Media Services v3

En este artículo se describen las cuotas y limitaciones de Azure Media Services v3.

| Recurso | Límite predeterminado | 
| --- | --- | 
| Recursos por cuenta de Azure Media Services | 1 000 000|
| Filtros de manifiesto dinámico|100|
| JobInputs por trabajo | 50 (cantidad fija)|
| JobOutputs por Job/TransformOutputs en una transformación | 20 (cantidad fija) |
| Archivos por JobInput|10 (cantidad fija)|
| Tamaño de archivo| En algunos casos, existe un límite máximo de tamaño de archivo admitido para el procesamiento en Media Services. <sup>(1)</sup> |
| Trabajos por cuenta de Media Services | 500 000 <sup>(2)</sup> (cantidad fija)|
| Listado de transformaciones|Pagine la respuesta, con 1000 transformaciones por página|
| Listado de trabajos|Pagine la respuesta, con 500 trabajos por página|
| Eventos en directo por cuenta de Media Services |5|
| Cuentas de Media Services en una suscripción única | 25 (fijo) |
| Salidas en vivo con el estado En ejecución por LiveEvent |3|
| Cuentas de almacenamiento | 100<sup>(4)</sup> (cantidad fija) |
| Puntos de conexión de streaming con el estado En ejecución por cuenta de Media Services|2|
| Directivas de streaming | 100 <sup>(3)</sup> |
| Transformaciones por cuenta de Media Services | 100 (cantidad fija)|
| Localizadores de streaming únicos asociados con un recurso al mismo tiempo | 100<sup>(5)</sup> (cantidad fija) |

<sup>1</sup> El tamaño máximo admitido para un único blob es actualmente de 5 TB en Azure Blob Storage. Sin embargo, en Azure Media Services se aplican límites adicionales en función de los tamaños de máquina virtual utilizados por el servicio. Si el archivo de origen es mayor de 260 GB, es muy probable que el trabajo presente un error. Si tiene contenido con formato 4K mayor que el límite de 260 GB, póngase en contacto con nosotros en amshelp@microsoft.com para posibles mitigaciones que admitan su escenario.

<sup>2</sup> Este número incluye los trabajos en cola, terminados, activos y cancelados. No incluye los trabajos eliminados. 

Se eliminarán automáticamente los registros de trabajo de más de 90 días de su cuenta, aunque el número total de registros no llegue a la cuota máxima. 

<sup>3</sup> Al utilizar un objeto [StreamingPolicy](https://docs.microsoft.com/rest/api/media/streamingpolicies) personalizado, debe diseñar un conjunto limitado de dichas directivas para su cuenta de Media Service y reutilizarlas para sus localizadores de streaming siempre que se necesiten las mismas opciones y protocolos de cifrado. No debe crear una nueva directiva de streaming para cada localizador de streaming.

<sup>4</sup> Las cuentas de almacenamiento deben proceder de la misma suscripción de Azure.

<sup>5</sup> Los localizadores de streaming no están diseñados para administrar el control de acceso por usuario. Para conceder derechos de acceso diferentes a usuarios individuales, use las soluciones de administración de derechos digitales (DRM).

## <a name="support-ticket"></a>Incidencia de soporte técnico

En el caso de recursos que no son fijos, puede solicitar que se generen las cuotas abriendo una [incidencia de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). Incluya información detallada en la solicitud en los cambios de cuota que se desean, en los escenarios de casos de uso y las regiones que se requieren. <br/>**No** cree cuentas adicionales de Azure Media Services para obtener límites mayores.

## <a name="next-steps"></a>Pasos siguientes

[Información general](media-services-overview.md)
