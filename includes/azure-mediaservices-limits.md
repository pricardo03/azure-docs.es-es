---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: b275a86f8fd35c43865fd920d1bfc9994a796a9c
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59805145"
---
>[!NOTE]
>Para obtener recursos que no son fijos, abra una incidencia de soporte técnico para solicitar un aumento en las cuotas. No cree cuentas adicionales de Azure Media Services en un intento de obtener límites mayores.

| Recurso | Límite predeterminado | 
| --- | --- | 
| Cuentas de Azure Media Services en una sola suscripción | 25 (fijo) |
| Unidades por cuenta de Media Services reservadas de multimedia |25 (S1)<br/>10 (S2, S3)<sup>1</sup> | 
| Trabajos por cuenta de Media Services | 50 000<sup>2</sup> |
| Tareas encadenadas por trabajo | 30 (fijo) |
| Recursos por cuenta de Media Services | 1 000 000|
| Recursos por tarea. | 50 |
| Recursos por trabajo | 100 |
| Localizadores únicos asociados a un recurso al mismo tiempo | 5<sup>4</sup> |
| Canales activos por cuenta de Media Services |5|
| Programas en estado detenido por canal  |50|
| Programas en estado de ejecución por canal  |3|
| Transmisión por secuencias los extremos que estén detenido o en ejecución por cuenta de Media Services|2|
| Unidades de streaming por extremo de streaming |10 |
| Cuentas de almacenamiento | 1000<sup>5</sup> (fijo) |
| Directivas | 1,000,000<sup>6</sup> |
| Tamaño de archivo| En algunos escenarios, hay un límite en el tamaño de archivo máximo admitido para el procesamiento en Media Services. <sup>7</sup> |

<sup>1</sup>si cambia el tipo, por ejemplo, de S2 a S1, número máximo de unidades reservadas se restablecen los límites.

<sup>2</sup>este número incluye los trabajos en cola, terminados, activos y cancelados. No incluye los trabajos eliminados. Puede eliminar los trabajos antiguos mediante **IJob.Delete** o **eliminar** solicitud HTTP.

A partir del 1 de abril de 2017, se eliminan automáticamente los registros de trabajo en su cuenta más de 90 días, junto con los registros de la tarea asociada. Eliminación automática se produce incluso si el número total de registros está por debajo de la cuota máxima. Para archivar la información de trabajos y tareas, utilice el código descrito en [administrar recursos con el SDK de .NET de Media Services](../articles/media-services/previous/media-services-dotnet-manage-entities.md).

<sup>3</sup>cuando se realiza una solicitud para enumerar las entidades job, se devuelve un máximo de 1000 trabajos por solicitud. Realizar un seguimiento de todos los trabajos enviados, utilice la parte superior u omitir las consultas como se describe en [opciones de consulta OData](/previous-versions/dynamicscrm-2015/developers-guide/gg309461(v=crm.7)).

<sup>4</sup>los localizadores no están diseñados para administrar el control de acceso por usuario. Para conceder derechos de acceso diferentes a usuarios individuales, use las soluciones de derechos digitales (DRM) de administración. Para obtener más información, consulte [proteger su contenido con Azure Media Services](../articles/media-services/previous/media-services-content-protection-overview.md).

<sup>5</sup>las cuentas de almacenamiento deben ser de la misma suscripción de Azure.

<sup>6</sup>hay un límite de 1 000 000 directivas para diferentes directivas de Media Services. Un ejemplo es para la directiva de localizador o ContentKeyAuthorizationPolicy. 

>[!NOTE]
> Si siempre usa los mismos días y los permisos de acceso, use el mismo identificador de directiva. Para obtener información y un ejemplo, vea [administrar recursos con el SDK de .NET de Media Services](../articles/media-services/previous/media-services-dotnet-manage-entities.md#limit-access-policies).

<sup>7</sup>si va a cargar contenido en un recurso de Media Services para procesarlo con uno de los procesadores de multimedia en el servicio, tenga en cuenta el tamaño máximo de archivo que se admite. Los recursos incluyen codificadores como Media Encoder Standard y motores de Media Encoder Premium Workflow o análisis como Face Detector.

El tamaño máximo admitido para un único blob es actualmente de 5 TB en Azure Blob Storage. Se aplican límites adicionales en Media Services en función de los tamaños de máquina virtual que se usan por el servicio. En la tabla siguiente se muestra los límites en el medio reservado unidades de S1, S2 y S3. Si el archivo de origen es mayor que los límites definidos en la tabla, se produce un error en el trabajo de codificación. Si utiliza la codificación orígenes de resolución de 4K de larga duración, es necesario utilizar unidades reservada de multimedia S3 para lograr el rendimiento necesario. Si tiene contenido de 4K mayor que el límite de 260 GB en las unidades reservadas de multimedia S3, póngase en contacto con nosotros en amshelp@microsoft.com para posibles mitigaciones admitir su escenario.

| Tipo de unidad reservada de medios | Tamaño máximo de entrada (GB)| 
| --- | --- | 
|S1 | 325|
|S2 | 640|
|S3 | 260|
