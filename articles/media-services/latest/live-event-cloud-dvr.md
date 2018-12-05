---
title: LiveEvent de Azure Media Services y una DVR en la nube| Microsoft Docs
description: En este artículo se explica qué es LiveOutput y cómo usar una DVR en la nube.
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
ms.date: 11/28/2018
ms.author: juliako
ms.openlocfilehash: 5de7496d73ebe1c89ce27ef27df73b197f34e7c7
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "52636990"
---
# <a name="using-a-cloud-dvr"></a>Uso de una DVR en la nube

Un objeto [LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs) le permite controlar las propiedades de la transmisión saliente en vivo, como qué parte de la transmisión se registra (por ejemplo, la capacidad de la DVR en la nube) y si los usuarios pueden empezar a ver la transmisión en vivo. La relación entre un objeto **LiveEvent** y su relación de **LiveOutput** es similar a la difusión de televisión tradicional, en la que un canal (**LiveEvent**) representa un flujo constante de vídeo y una grabación (**LiveOutput**) tiene un ámbito de un segmento de tiempo específico (por ejemplo, noticias vespertinas de 18:30 a 19:00). Puede grabar televisión con una grabadora de vídeo digital (DVR): la característica equivalente en LiveEvents se administra a través de la propiedad ArchiveWindowLength. Se trata de una duración de timespan ISO 8601 (por ejemplo, PTHH:MM:SS), que especifica la capacidad de la DVR y se puede establecer desde un mínimo de 3 minutos hasta un máximo de 25 horas.

## <a name="liveoutput"></a>LiveOutput

El valor **ArchiveWindowLength** determina hasta qué punto puede un usuario buscar hacia atrás en el tiempo desde la posición actual en vivo.  **ArchiveWindowLength** también determina durante cuánto tiempo pueden aumentar los manifiestos de cliente.

Suponga que hace streaming de un partido de fútbol que tiene un valor **ArchiveWindowLength** de solo 30 minutos. Un usuario que empieza a ver el evento a los 45 minutos de iniciado el juego puede buscar hacia atrás hasta la marca de 15 minutos como máximo. Los objetos **LiveOutput** del juego continuarán hasta que se detenga el objeto **LiveEvent**, pero el contenido que quede fuera de **ArchiveWindowLength** se descarta continuamente desde almacenamiento y no se puede recuperar. En este ejemplo, el vídeo entre el inicio del evento y la marca de 15 minutos se habría purgado de la DVR y del contenedor de Blob Storage del [recurso](https://docs.microsoft.com/rest/api/media/assets). El archivo no se puede recuperar y se quita del contenedor en Azure Blob Storage.

Cada objeto **LiveOutput** se asocia a un **recurso**, que utiliza para grabar el vídeo en el contenedor de Azure Blob Storage asociado. Para publicar el LiveOutput, debe crear un objeto **StreamingLocator** para ese **recurso**. Después de crear un [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators), puede crear una dirección URL de streaming que puede proporcionar a los usuarios.

Un objeto **LiveEvent** admite hasta tres **LiveOutput** en ejecución simultánea, por lo que puede crear como máximo tres grabaciones o archivos desde una transmisión en vivo. Esto le permite publicar y archivar distintas partes de un evento, según sea necesario. Suponga que tiene que difundir una fuente lineal en vivo e ininterrumpida y crea "grabaciones" de los distintos programas durante todo el día para ofrecerlas a los clientes como contenido a petición que pueden ver para ponerse al día. En este escenario, cree primero un objeto LiveOutput principal, con una ventana de archivo corta de una hora o menos: esta es la transmisión en vivo principal que los usuarios sintonizarán. Podría crear un **StreamingLocator** para este **LiveOutput** y publicarlo en la aplicación o en el sitio web como la fuente "En vivo". Mientras el **LiveEvent** se encuentra en ejecución, puede crear mediante programación un segundo objeto **LiveOutput** simultáneo al inicio de un programa o 5 minutos antes para ofrecer algunos manipuladores que se recortan más adelante. Este segundo **LiveOutput** puede eliminarse 5 minutos después de que finalice el programa. Con este segundo **recurso**, puede crear otro **StreamingLocator** para publicar este programa como un recurso a petición en el catálogo de la aplicación. Puede repetir este proceso varias veces para otros límites o aspectos destacados del programa que desea compartir como vídeos a petición, todo esto mientras la fuente "En vivo" del primer objeto **LiveOutput** continúa con la difusión de la fuente lineal. 

> [!NOTE]
> Los objetos **LiveOutput** se inician al crearlos y se detienen cuando se eliminan. Cuando se elimina el objeto **LiveOutput**, no se elimina el **recurso** subyacente ni el contenido del recurso.  

## <a name="next-steps"></a>Pasos siguientes

- [Introducción al streaming en vivo](live-streaming-overview.md)
- [Tutorial de Live Streaming](stream-live-tutorial-with-api.md)

