---
title: Uso de Standard Encoder en Azure Media Services para codificar vídeos con una escala de velocidad de bits generada automáticamente | Microsoft Docs
description: En este tema se muestra cómo usar Standard Encoder en Media Services para codificar un vídeo de entrada con una escala de velocidad de bits generada automáticamente según la velocidad de bits y la resolución de entrada. La resolución de entrada y la velocidad de bits nunca se superarán. Por ejemplo, si la entrada es 720p a 3 Mbps, la salida permanecerá en 720p en el mejor de los casos y se iniciará a velocidades inferiores a 3 Mbps.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: ec1b4b88e5b9639c3ee9debbd8ac7d48544344dc
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49378965"
---
#  <a name="encode-with-an-auto-generated-bitrate-ladder"></a>Codificación con una escala de velocidad de bits generada automáticamente

## <a name="overview"></a>Información general

En este artículo se explica cómo usar Standard Encoder en Media Services para codificar un vídeo de entrada en una escala de velocidad de bits generada automáticamente (pares de velocidad de bits-resolución) según la velocidad de bits y la resolución de entrada. Esta configuración de codificador integrado, o valor preestablecido, nunca superará la velocidad de bits ni la resolución de entrada. Por ejemplo, si la entrada es 720p a 3 Mbps, la salida permanecerá en 720p en el mejor de los casos y se iniciará a velocidades inferiores a 3 Mbps.

### <a name="encoding-for-streaming"></a>Codificación para streaming

Si se usa el valor predefinido **AdaptiveStreaming** en **Transform**, recibirá una salida adecuada para su entrega con los protocolos de streaming como HLS y DASH. Cuando se usa este valor predefinido, el servicio determina de manera inteligente cuántas capas de vídeo debe generar y la velocidad de bits y resolución. El contenido de salida incluye archivos MP4 en los que el audio codificado en AAC y el vídeo codificado en H.264 no están intercalados.

Para ver un ejemplo de cómo se usa este valor preestablecido, consulte [Streaming de un archivo](stream-files-dotnet-quickstart.md).

## <a name="output"></a>Salida

En esta sección se muestran tres ejemplos de las capas de vídeo de salida generadas por el codificador de Media Services como resultado de la codificación con el valor preestablecido **AdaptiveStreaming**. En todos los casos, la salida contiene un archivo MP4 de solo audio con el audio estéreo codificado a 128 kbps.

### <a name="example-1"></a>Ejemplo 1
Un origen con un alto de "1080" y una tasa de fotogramas de "29.970" genera 6 niveles de vídeo:

|Nivel|Alto|Ancho|Velocidad de bits (kbps)|
|---|---|---|---|
|1|1080|1920|6780|
|2|720|1280|3520|
|3|540|960|2210|
|4|360|640|1150|
|5|270|480|720|
|6|180|320|380|

### <a name="example-2"></a>Ejemplo 2
Un origen con un alto de "720" y una tasa de fotogramas de "23.970" genera 5 niveles de vídeo:

|Nivel|Alto|Ancho|Velocidad de bits (kbps)|
|---|---|---|---|
|1|720|1280|2940|
|2|540|960|1850|
|3|360|640|960|
|4|270|480|600|
|5|180|320|320|

### <a name="example-3"></a>Ejemplo 3
Un origen con un alto de "360" y una tasa de fotogramas de "29.970" genera 3 niveles de vídeo:

|Nivel|Alto|Ancho|Velocidad de bits (kbps)|
|---|---|---|---|
|1|360|640|700|
|2|270|480|440|
|3|180|320|230|

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Streaming de un archivo](stream-files-dotnet-quickstart.md)
