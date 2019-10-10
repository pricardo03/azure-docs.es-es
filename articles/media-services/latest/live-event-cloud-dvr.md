---
title: Uso de cambio de tiempo y directo a vídeo bajo demanda (VOD) de Azure Media Services | Microsoft Docs
description: En este artículo, se explica qué es un objeto LiveOutput y cómo utilizar una DVR en la nube.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 08/27/2019
ms.author: juliako
ms.openlocfilehash: ffcd279830cb49b64ddbb58a888ad7d653918b1b
ms.sourcegitcommit: e1b6a40a9c9341b33df384aa607ae359e4ab0f53
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71338837"
---
# <a name="using-time-shifting-and-live-to-vod-video-on-demand"></a>Uso de cambio de tiempo y directo a vídeo bajo demanda (VOD)

En Azure Media Services, un objeto [Live Output](https://docs.microsoft.com/rest/api/media/liveoutputs) es como una grabadora de vídeo digital que capta y graba el streaming en vivo en un recurso de su cuenta de Media Services. El contenido grabado se conserva en el contenedor definido por el recurso [Asset](https://docs.microsoft.com/rest/api/media/assets) (el contenedor está en la cuenta de Azure Storage asociada a su cuenta). La salida en directo también le permite controlar algunas propiedades del streaming en vivo saliente, como la cantidad de la transmisión que se conserva en la grabación del archivo (por ejemplo, la capacidad de la DVR en la nube) y si los espectadores pueden empezar a ver el streaming en vivo, o no. El archivo en disco es una "ventana" circular de archivo que solo incluye la cantidad de contenido que se especifica en la propiedad **archiveWindowLength** de Live Output. El contenido que está fuera de esta ventana se descarta automáticamente del contenedor de almacenamiento y no se puede recuperar. El valor de archiveWindowLength representa una duración del intervalo de tiempo de ISO 8601 (por ejemplo, PTHH:MM:SS), que especifica la capacidad de la DVR y se puede establecer desde un mínimo de 3 minutos hasta un máximo de 25 horas.

La relación entre un evento en directo y sus salidas en directo es similar a la retransmisión de televisión tradicional, en la que un canal (evento en directo) representa una transmisión constante de vídeo y una grabación (salida en directo) tiene un ámbito que está limitado a un segmento de tiempo específico (por ejemplo, las noticias de la tarde, que se emiten de 18:30 a 19:00). Una vez que la transmisión fluye en el evento en directo, puede comenzar el evento de streaming mediante la creación de un recurso, una salida en vivo y un localizador de streaming. La salida en directo archivará la transmisión y la pondrá a disposición de los usuarios a través del [punto de conexión de streaming](https://docs.microsoft.com/rest/api/media/streamingendpoints). Puede crear varias salidas en directo (hasta un máximo de tres) en un evento en directo con diferentes configuraciones y longitudes de archivo. Para obtener información sobre el flujo de trabajo del streaming en vivo, consulte la sección de [pasos generales](live-streaming-overview.md#general-steps).

## <a name="using-a-dvr-during-an-event"></a>Uso de una DVR durante un evento 

En esta sección se explica cómo usar una DVR durante un evento para controlar qué partes de la transmisión están disponibles para el "rebobinado".

El valor archiveWindowLength determina cuánto tiempo hacia atrás puede un espectador buscar desde la posición actual en vivo. El valor de archiveWindowLength también determina durante cuánto tiempo pueden crecer los manifiestos de cliente.

Suponga que hace streaming de un partido de fútbol y que tiene un valor de ArchiveWindowLength de solo 30 minutos. Un usuario que empieza a ver el evento a los 45 minutos de iniciado el juego puede buscar hacia atrás hasta la marca de 15 minutos como máximo. Las salidas en directo del partido continuarán hasta que se detenga el evento en directo, pero el contenido que quede fuera de archiveWindowLength se descarta ininterrumpidamente desde el almacenamiento y no puede recuperarse. En este ejemplo, el vídeo entre el inicio del evento y la marca de 15 minutos se habría purgado de la DVR y del contenedor de Blob Storage del recurso. El archivo no se puede recuperar y se quita del contenedor en Azure Blob Storage.

Un evento en directo admite que se ejecuten simultáneamente un máximo de tres salidas en directo (puede crear un máximo de tres grabaciones o archivos a partir de una transmisión al mismo tiempo). Esto le permite publicar y archivar distintas partes de un evento, según sea necesario. Suponga que tiene que difundir una fuente lineal en vivo e ininterrumpida y crea "grabaciones" de los distintos programas durante todo el día para ofrecerlas a los clientes como contenido a petición que pueden ver para ponerse al día. En este escenario, primero debe crear un objeto LiveOutput principal con un periodo de archivado de una hora o menos (esta es la transmisión en directo principal que los usuarios sintonizarán). Puede crear un localizador de streaming para esta salida en directo y publicarlo en la aplicación o en el sitio web como la fuente "en directo". Mientras se ejecuta el evento en directo, puede crear mediante programación una segunda salida en directo al comienzo de un programa (o cinco minutos antes para ofrecer algunos manipuladores que permitan recortar la transmisión más adelante). Esta segunda salida en directo puede eliminarse cinco minutos después de que finalice el programa. Con este segundo recurso, puede crear un localizador de streaming para publicar este programa como un recurso a petición en el catálogo de la aplicación. Este proceso se puede repetir varias veces en otros límites o aspectos destacados del programa que desee compartir como vídeos a petición, y todo ello mientras la fuente "en directo" de la primera salida en directo continúa con la difusión de la fuente lineal. 

## <a name="creating-an-archive-for-on-demand-playback"></a>Creación de un archivo para la reproducción a petición

El recurso en el que se está archivando la salida en directo, se convierte automáticamente en un recurso a petición cuando se elimina esa salida en directo. Debe eliminar todas las salidas en directo antes de que un evento en directo pueda detenerse. Puede usar una marca opcional [removeOutputsOnStop](https://docs.microsoft.com/rest/api/media/liveevents/stop#request-body) para quitar automáticamente las salidas en directo cuando se detenga el proceso. 

Incluso después de detener y eliminar el evento, los usuarios podrán transmitir en secuencias el contenido archivado como un vídeo a petición, siempre que no se elimine el recurso. Los recursos que usan los eventos no se deberían eliminar; primero se deben eliminar los eventos.

Si ha publicado el recurso de la salida en directo mediante un localizador de streaming, el evento en directo (hasta la longitud de la ventana de DVR) seguirá estando visible hasta que el localizador de streaming expire o se elimine, lo que ocurra primero.

Para más información, consulte:

- [Introducción al streaming en vivo](live-streaming-overview.md)
- [Tutorial de Live Streaming](stream-live-tutorial-with-api.md)

> [!NOTE]
> Cuando se elimina la salida en directo, no se eliminan el recurso subyacente ni el contenido de dicho recurso. 

## <a name="next-steps"></a>Pasos siguientes

* [Creación de subclips de los vídeos](subclip-video-rest-howto.md).
* [Definición de filtros para los recursos](filters-dynamic-manifest-rest-howto.md).
