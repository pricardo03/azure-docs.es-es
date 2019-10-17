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
ms.date: 10/11/2019
ms.author: juliako
ms.openlocfilehash: 819548d784e5cba9fcec6b2110137d91bf28e03d
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2019
ms.locfileid: "72296914"
---
# <a name="quotas-and-limitations-in-azure-media-services-v3"></a>Cuotas y limitaciones en Azure Media Services v3

En este artículo se describen las cuotas y limitaciones de Azure Media Services v3.

| Resource | Límite predeterminado | 
| --- | --- | 
| Recursos por cuenta de Azure Media Services | 1 000 000|
| Filtros de manifiesto dinámico|100|
| JobInputs por trabajo | 50 (cantidad fija)|
| JobOutputs por trabajo | 20 (cantidad fija) |
| TransformOutputs en una transformación | 20 (cantidad fija) |
| Archivos por JobInput|10 (cantidad fija)|
| Tamaño de archivo| En algunos casos, existe un límite máximo de tamaño de archivo admitido para el procesamiento en Media Services. <sup>(1)</sup> |
| Trabajos por cuenta de Media Services | 500 000 <sup>(2)</sup> (cantidad fija)|
| Eventos en directo por cuenta de Media Services |5|
| Cuentas de Media Services en una suscripción única | 25 (fijo) |
| Salidas en directo por evento en directo |3 <sup>(3)</sup> |
| Duración máxima de la salida en directo | 25 horas |
| Cuentas de almacenamiento | 100<sup>(4)</sup> (cantidad fija) |
| Puntos de conexión de streaming (detenidos o en ejecución) por cuenta de Media Services|2 (fijo)|
| Directivas de streaming | 100 <sup>(5)</sup> |
| Transformaciones por cuenta de Media Services | 100 (cantidad fija)|
| Localizadores de streaming únicos asociados con un recurso al mismo tiempo | 100<sup>(6)</sup> (fijo) |
| Opciones por directiva de clave de contenido |30 | 
| Licencias por mes para cada uno de los tipos DRM en el servicio de entrega de claves de Media Services por cuenta|1 000 000|

<sup>1</sup> El tamaño máximo admitido para un único blob es actualmente de 5 TB en Azure Blob Storage. En Media Services, se aplican límites adicionales en función de los tamaños de VM utilizados por el servicio. El límite de tamaño se aplica a los archivos que se cargan y también a los que se generan como resultado del procesamiento (codificación o análisis) de Media Services. Si el archivo de origen es mayor de 260 GB, es muy probable que el trabajo presente un error. 

En la tabla siguiente se muestran los límites en cada una de las unidades reservadas de multimedia (S1, S2 y S3). Si el archivo de origen es mayor que los límites definidos en la tabla, se producirá un error en el trabajo de codificación. Si codifica orígenes de resolución en 4K de larga duración, debe usar unidades reservadas de multimedia S3 para lograr el rendimiento necesario. Si tiene contenido en formato 4K mayor que el límite de 260 GB en las unidades reservadas de multimedia S3, póngase en contacto con nosotros en amshelp@microsoft.com para conocer las posibles mitigaciones que permitan admitir su escenario.

|Tipo de unidad reservada de medios   |Tamaño máximo de entrada (GB)|
|---|---|
|S1 |   26|
|S2 | 60|
|S3 |260|

<sup>2</sup> Este número incluye los trabajos en cola, terminados, activos y cancelados. No incluye los trabajos eliminados. 

Se eliminarán automáticamente los registros de trabajo de más de 90 días de su cuenta, aunque el número total de registros no llegue a la cuota máxima. 

<sup>3</sup> Los objetos LiveOutput comienzan cuando se crean y se detienen cuando se eliminan.

<sup>4</sup> Las cuentas de almacenamiento deben proceder de la misma suscripción de Azure.

<sup>5</sup> Al usar una [directiva de streaming](https://docs.microsoft.com/rest/api/media/streamingpolicies) personalizada, debe diseñar un conjunto limitado de dichas directivas para su cuenta de Media Service y reutilizarlas para sus localizadores de streaming siempre que se necesiten las mismas opciones y protocolos de cifrado. No debe crear una nueva directiva de streaming para cada localizador de streaming.

<sup>6</sup> Los localizadores de streaming no están diseñados para administrar el control de acceso por usuario. Para conceder derechos de acceso diferentes a usuarios individuales, use las soluciones de administración de derechos digitales (DRM).

## <a name="support-ticket"></a>Incidencia de soporte técnico

En el caso de recursos que no son fijos, puede solicitar que se generen las cuotas abriendo una [incidencia de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). Incluya información detallada en la solicitud en los cambios de cuota que se desean, en los escenarios de casos de uso y las regiones que se requieren. <br/>**No** cree cuentas adicionales de Azure Media Services para obtener límites mayores.

## <a name="next-steps"></a>Pasos siguientes

[Información general](media-services-overview.md)
