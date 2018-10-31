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
ms.date: 10/22/2018
ms.author: juliako
ms.openlocfilehash: e13afe26d06f5b5b2dcf7eddf00f9ee481312b2c
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50024269"
---
# <a name="transforms-and-jobs"></a>Transformaciones y trabajos
 
Azure Media Services v3 presenta un nuevo recurso de flujo de trabajo con plantilla para una receta que le interesará usar para codificar o analizar los vídeos, llamado [Transformaciones](https://docs.microsoft.com/rest/api/media/transforms). Las **transformaciones** se pueden usar para configurar tareas comunes para codificar o analizar vídeos. Cada **Transformación** describe una receta, o un flujo de trabajo simple de tareas para procesar los archivos de vídeo o audio. 

Un **trabajo** es la solicitud real a Azure Media Services para aplicar la **transformación** a un contenido de vídeo o audio determinado. El **trabajo** especifica información como la ubicación del vídeo de entrada y la ubicación de la salida. Puede especificar la ubicación del vídeo de entrada mediante direcciones URL HTTPS, direcciones URL de SAS o [recursos de Media Services](https://docs.microsoft.com/rest/api/media/assets).  

## <a name="typical-workflow"></a>Flujo de trabajo típico

1. Cree una transformación 
2. Enviar trabajos en esa transformación 
3. Enumerar las transformaciones 
4. Elimine una transformación, si no planea usarla en el futuro. 

Imagine que desea extraer el primer fotograma de todos los vídeos como una imagen en miniatura; los pasos que debería seguir son: 

1. Definir la receta o regla para el procesamiento de los vídeos: "usar el primer fotograma del vídeo como la miniatura". 
2. Para cada vídeo, debería indicar lo siguiente al servicio: 
    1. Dónde encontrar ese vídeo,  
    2. Dónde escribir la imagen en miniatura de salida. 

Una **transformación** ayuda a crear la receta una vez (paso 1), y enviar trabajos mediante esa receta (paso 2).

## <a name="transforms"></a>Transformaciones

Puede crear transformaciones en la cuenta de Media Services directamente mediante la API v3, o bien con cualquiera de los SDK publicados. Azure Resource Manager controla la API v3 de Media Services, por lo que también se pueden usar plantillas de Resource Manager para crear e implementar las transformaciones en la cuenta de Media Services. El control de acceso basado en rol se puede usar para bloquear el acceso a las transformaciones.

### <a name="transform-definition"></a>Definición de transformación

En la tabla siguiente se muestran las propiedades de la transformación y se proporcionan sus definiciones.

|NOMBRE|DESCRIPCIÓN|
|---|---|
|Id|Identificador de recurso completo del recurso.|
|Nombre|Nombre del recurso.|
|properties.created |La fecha y hora UTC cuando se creó la transformación, en formato "AAAA-MM-DDThh:mm:ssZ".|
|properties.description |Una descripción detallada opcional de la transformación.|
|properties.lastModified |La fecha y hora UTC cuando se actualizó la transformación por última vez, en formato "AAAA-MM-DDThh:mm:ssZ".|
|Properties.Outputs |Una matriz de uno o más elementos TransformOutputs que debe generar la transformación.|
|Tipo|Tipo de recurso.|

Para conocer la definición completa, consulte [Transformaciones](https://docs.microsoft.com/rest/api/media/transforms).

Como se explicó anteriormente, una transformación ayuda a crear una receta o regla para procesar los vídeos. Una única transformación puede aplicar más de una regla. Por ejemplo, una transformación podría especificar que cada vídeo se codifique como un archivo MP4 con una velocidad de bits determinada, y que se genere una imagen en miniatura a partir del primer fotograma del vídeo. Tendría que agregar una entrada TransformOutput para cada regla que quiera incluir en la transformación.

> [!NOTE]
> Aunque la definición de transformaciones admite una operación de actualización, debería asegurarse de que todos los trabajos en curso se completen antes de realizar una modificación. Por lo general, podría actualizar una transformación existente para cambiar la descripción, o bien modificar las prioridades de las entradas TransformOutput subyacentes. Si quisiera volver a escribir la receta, tendría que crear una transformación nueva.

## <a name="jobs"></a>Trabajos

Una vez creada una transformación, puede enviar trabajos mediante las API de Media Services o cualquiera de los SDK publicados. El progreso y estado de los trabajos se pueden obtener mediante la supervisión de eventos con Event Grid. Para obtener más información, vea [Supervisión de eventos mediante EventGrid](job-state-events-cli-how-to.md ).

### <a name="jobs-definition"></a>Definición de trabajos

En la tabla siguiente se muestran las propiedades del trabajo y se proporcionan sus definiciones.

|NOMBRE|Descripción|
|---|---|
|id|Identificador de recurso completo del recurso.|
|Nombre   |Nombre del recurso.|
|properties.correlationData |Datos de correlación proporcionados por el cliente (inmutables) que se devuelven como parte de las notificaciones de cambio de estado de Job y JobOutput. Para más información, vea [Esquemas para eventos](media-services-event-schemas.md).<br/><br/>La propiedad también se puede utilizar para el uso de Media Services con varios inquilinos. Puede colocar los identificadores de inquilino en los trabajos. |
|properties.created |La fecha y hora UTC cuando se creó el trabajo, en formato "AAAA-MM-DDThh:mm:ssZ".|
|properties.description |Descripción opcional del trabajo proporcionada por el cliente.|
|properties.input|Las entradas para el trabajo.|
|properties.lastModified    |La fecha y hora UTC cuando se actualizó el trabajo por última vez, en formato "AAAA-MM-DDThh:mm:ssZ".|
|Properties.Outputs|Las salidas del trabajo.|
|properties.priority    |Prioridad con la que se debe procesar el trabajo. Los trabajos con prioridad superior se procesan antes que los trabajos con prioridad inferior. Si no se establece, el valor predeterminado es normal.|
|properties.state   |Estado actual del trabajo.|
|Tipo   |Tipo de recurso.|

Para conocer la definición completa, consulte [Trabajos](https://docs.microsoft.com/rest/api/media/jobs).

> [!NOTE]
> Aunque la definición de los trabajos admite una operación de actualización, las únicas propiedades que se pueden modificar una vez enviado el trabajo son la descripción y la prioridad. Además, un cambio en la prioridad es eficaz solo si el trabajo todavía está en un estado en cola. Si el trabajo ha comenzado el procesamiento, o ha terminado, cambiar la prioridad no tiene ningún efecto.

### <a name="pagination"></a>Paginación

En Media Services v3 se admite la paginación de los trabajos.

> [!TIP]
> Siempre debe usar el vínculo siguiente para enumerar la colección y no tener que depender de un tamaño de página determinado.

Si una respuesta de consulta contiene muchos elementos, el servicio devuelve una propiedad "\@odata.nextLink" para obtener la siguiente página de resultados. Esto se puede utilizar para pasar de página en el conjunto de resultados completo. No puede configurar el tamaño de página. 

Si se crean o eliminan trabajos durante la paginación a través de la colección, los cambios se ven reflejados en los resultados que se devuelven (si esos cambios se encuentran en la parte de la colección que no se ha descargado). 

En el ejemplo de C# siguiente se muestra cómo enumerar los trabajos de la cuenta.

```csharp            
List<string> jobsToDelete = new List<string>();
var pageOfJobs = client.Jobs.List(config.ResourceGroup, config.AccountName, "Encode");

bool exit;
do
{
    foreach (Job j in pageOfJobs)
    {
        jobsToDelete.Add(j.Name);
    }

    if (pageOfJobs.NextPageLink != null)
    {
        pageOfJobs = client.Jobs.ListNext(pageOfJobs.NextPageLink);
        exit = false;
    }
    else
    {
        exit = true;
    }
}
while (!exit);

```

Para obtener ejemplos de REST, vea [Jobs - List](https://docs.microsoft.com/rest/api/media/jobs/list) (Trabajos: lista)


## <a name="next-steps"></a>Pasos siguientes

[Streaming de archivos de vídeo](stream-files-dotnet-quickstart.md)
