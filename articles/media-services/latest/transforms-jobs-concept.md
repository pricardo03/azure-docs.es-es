---
title: Transformaciones y trabajos en Azure Media Services | Microsoft Docs
description: Al usar Media Services, debe crear una transformación para describir las reglas o especificaciones para procesar los vídeos. En este artículo se proporciona información general sobre qué es un transformación y cómo se usa.
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
ms.openlocfilehash: e84f74fe4678a65a33c9cc728f290e7c905b2261
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "55743742"
---
# <a name="transforms-and-jobs"></a>Transformaciones y trabajos
 
Use [transformaciones](https://docs.microsoft.com/rest/api/media/transforms) para configurar tareas comunes para codificar o analizar vídeos. Cada **Transformación** describe una receta, o un flujo de trabajo simple de tareas para procesar los archivos de vídeo o audio. Una única transformación puede aplicar más de una regla. Por ejemplo, una transformación podría especificar que cada vídeo se codifique como un archivo MP4 con una velocidad de bits determinada, y que se genere una imagen en miniatura a partir del primer fotograma del vídeo. Tendría que agregar una entrada TransformOutput para cada regla que quiera incluir en la transformación. Puede crear transformaciones en la cuenta de Media Services mediante la API v3 de Media Services, o bien con cualquiera de los SDK publicados. Azure Resource Manager controla la API v3 de Media Services, por lo que también se pueden usar plantillas de Resource Manager para crear e implementar las transformaciones en la cuenta de Media Services. El control de acceso basado en rol se puede usar para bloquear el acceso a las transformaciones.

La operación de actualización en la entidad [Transformación](https://docs.microsoft.com/rest/api/media/transforms) está diseñada para realizar cambios en la descripción o las prioridades del elemento TransformOutputs subyacente. Se recomienda realizar tales actualizaciones cuando se hayan completado todos los trabajos en curso. Si piensa volver a escribir la receta, deberá crear una nueva transformación.

Un [trabajo](https://docs.microsoft.com/rest/api/media/jobs) es la solicitud real a Azure Media Services para aplicar la **transformación** a un contenido de vídeo o audio determinado. Una vez creada la transformación, puede enviar trabajos mediante las API de Media Services o cualquiera de los SDK publicados. El **trabajo** especifica información como la ubicación del vídeo de entrada y la ubicación de la salida. Puede especificar la ubicación del vídeo de entrada mediante: direcciones URL HTTPS, direcciones URL SAS o [recursos](https://docs.microsoft.com/rest/api/media/assets). El progreso y estado de los trabajos se pueden obtener mediante la supervisión de eventos con Event Grid. Para obtener más información, vea [Supervisión de eventos mediante EventGrid](job-state-events-cli-how-to.md).

La operación de actualización en la entidad [Trabajo](https://docs.microsoft.com/rest/api/media/jobs) puede usarse para modificar las propiedades *description* y *priority* después de enviar el trabajo. Un cambio en la propiedad *priority* es eficaz solo si el trabajo todavía está en un estado en cola. Si el trabajo ha comenzado a procesarse o ha finalizado, cambiar la prioridad no tiene ningún efecto.

> [!NOTE]
> Las propiedades de **Transformación** y **Trabajo** que son del tipo Datetime siempre están en formato UTC.

## <a name="typical-workflow"></a>Flujo de trabajo típico

1. Cree una transformación 
2. Enviar trabajos en esa transformación 
3. Enumerar las transformaciones 
4. Elimine una transformación, si no planea usarla en el futuro. 

### <a name="example"></a>Ejemplo

Imagine que desea extraer el primer fotograma de todos los vídeos como una imagen en miniatura; los pasos que debería seguir son: 

1. Definir la receta o regla para el procesamiento de los vídeos: "usar el primer fotograma del vídeo como la miniatura". 
2. Para cada vídeo, debería indicar lo siguiente al servicio: 
    1. Dónde encontrar ese vídeo,  
    2. Dónde escribir la imagen en miniatura de salida. 

Una **transformación** ayuda a crear la receta una vez (paso 1), y enviar trabajos mediante esa receta (paso 2).

## <a name="paging"></a>Paginación

Consulte [Filtrado, ordenación y paginación de entidades de Media Services](entities-overview.md).

## <a name="next-steps"></a>Pasos siguientes

[Carga, codificación y transmisión de archivos de vídeo](stream-files-tutorial-with-api.md)
