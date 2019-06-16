---
title: Eventos en directo de Azure Media Services y DVR en la nube| Microsoft Docs
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
ms.date: 01/14/2019
ms.author: juliako
ms.openlocfilehash: 4dd14587ec7e1473953981c1ef8c32c59eb9a1d6
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60322342"
---
# <a name="using-a-cloud-dvr"></a>Uso de una DVR en la nube

El objeto [LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs) permite controlar las propiedades de la transmisión de salida en directo, como el volumen de la transmisión que se va a grabar (por ejemplo, la capacidad de la DVR en la nube) y si los espectadores pueden empezar o no a ver la transmisión en directo. La relación entre un objeto **LiveEvent** y su objeto **LiveOutput** es similar a la difusión por televisión tradicional, en la que un canal (objeto **LiveEvent**) representa un flujo constante de vídeo y una grabación (objeto **LiveOutput**) tiene un ámbito que está limitado a un segmento de tiempo específico (por ejemplo, las noticias de la tarde, que se emiten de 18:30 a 19:00). Puede grabar un programa de televisión utilizando una grabadora de vídeo digital (DVR). La característica equivalente de los eventos en directo se administra con la propiedad ArchiveWindowLength. Se trata de una duración de timespan ISO 8601 (por ejemplo, PTHH:MM:SS), que especifica la capacidad de la DVR y se puede establecer desde un mínimo de 3 minutos hasta un máximo de 25 horas.

## <a name="live-output"></a>Objeto LiveOutput

El valor **ArchiveWindowLength** determina hasta qué punto puede un usuario buscar hacia atrás en el tiempo desde la posición actual en vivo.  **ArchiveWindowLength** también determina durante cuánto tiempo pueden aumentar los manifiestos de cliente.

Suponga que hace streaming de un partido de fútbol que tiene un valor **ArchiveWindowLength** de solo 30 minutos. Un usuario que empieza a ver el evento a los 45 minutos de iniciado el juego puede buscar hacia atrás hasta la marca de 15 minutos como máximo. Los objetos **LiveOutput** del juego continuarán hasta que el objeto **LiveEvent** se detenga, pero el contenido que quede fuera de **ArchiveWindowLength** se descartará ininterrumpidamente desde el almacenamiento y no podrá recuperarse. En este ejemplo, el vídeo entre el inicio del evento y la marca de 15 minutos se habría purgado de la DVR y del contenedor de Blob Storage del [recurso](https://docs.microsoft.com/rest/api/media/assets). El archivo no se puede recuperar y se quita del contenedor en Azure Blob Storage.

Cada objeto **LiveOutput** está asociada a un **recurso** que dicha salida utiliza para grabar el vídeo en el contenedor de Azure Blob Storage asociado. Si desea publicar el objeto LiveOutput, debe crear un objeto **StreamingLocator** para ese **recurso**. Después de crear un [localizador de streaming](https://docs.microsoft.com/rest/api/media/streaminglocators), puede crear una dirección URL de streaming y proporcionársela a los usuarios.

Un objeto **LiveEvent** admite que como mucho se ejecuten tres objetos **LiveOutput** simultáneamente, por lo que puede crear un máximo de tres grabaciones o archivos a partir de una transmisión en directo. Esto le permite publicar y archivar distintas partes de un evento, según sea necesario. Suponga que tiene que difundir una fuente lineal en vivo e ininterrumpida y crea "grabaciones" de los distintos programas durante todo el día para ofrecerlas a los clientes como contenido a petición que pueden ver para ponerse al día. En este escenario, primero debe crear un objeto LiveOutput principal con un periodo de archivado de una hora o menos (esta es la transmisión en directo principal que los usuarios sintonizarán). Puede crear un objeto **StreamingLocator** para este objeto **LiveOutput** y publicarlo en la aplicación o en el sitio web como la fuente "en directo". Mientras el objeto **LiveEvent** está en ejecución, puede crear mediante programación otro objeto **LiveOutput** simultáneo al comienzo de un programa (o cinco minutos antes para ofrecer algunos manipuladores que permitan recortar la transmisión más adelante). Este segundo objeto **LiveOutput** puede eliminarse cinco minutos después de que finalice el programa. Con este segundo **recurso**, puede crear otro **localizador de streaming** para publicar este programa como un recurso a petición en el catálogo de la aplicación. Puede repetir este proceso varias veces con otros límites o aspectos destacados del programa que desee compartir como vídeos a petición, todo esto mientras la fuente "en directo" del primer objeto **LiveOutput** continúa con la difusión de la fuente lineal. 

> [!NOTE]
> Los objetos **LiveOutput** comienzan cuando se crean y se detienen cuando se eliminan. Cuando se elimina el objeto **LiveOutput**, no se elimina el **recurso** subyacente ni su contenido. 
>
> Si ha publicado el recurso del objeto **LiveOutput** utilizando un objeto **StreamingLocator**, el objeto **LiveEvent** (hasta la longitud de la ventana de DVR) seguirá estando visible hasta la expiración o la eliminación del objeto **StreamingLocator**, lo que ocurra primero.

## <a name="next-steps"></a>Pasos siguientes

- [Introducción al streaming en vivo](live-streaming-overview.md)
- [Tutorial de Live Streaming](stream-live-tutorial-with-api.md)

