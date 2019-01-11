---
title: 'Información sobre los codificadores locales de streaming en vivo recomendados por Media Services: Azure | Microsoft Docs'
description: Información sobre los codificadores locales de streaming en vivo recomendados por Media Services
services: media-services
keywords: encoding;encoders;media
author: johndeu
manager: johndeu
ms.author: johndeu
ms.date: 12/14/2018
ms.topic: article
ms.service: media-services
ms.openlocfilehash: d1110669bd0ca8c0ba0caf34ef41861c500bdd33
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/27/2018
ms.locfileid: "53790035"
---
# <a name="recommended-live-streaming-encoders"></a>Codificadores de streaming en vivo recomendados

En Media Services, un [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) (canal) representa una canalización para procesar contenido de streaming en vivo. Un canal LiveEvent recibe las secuencias de entrada en vivo de una de estas dos formas:

* Un codificador en directo local envía una secuencia RTMP o Smooth Streaming (MP4 fragmentado) de velocidad de bits múltiple al canal LiveEvent que no está habilitado para realizar la codificación en vivo con Media Services. Las secuencias recopiladas pasan a través de LiveEvents sin más procesamiento. Este método se llama **paso a través**. Un codificador en directo puede enviar una transmisión con una sola velocidad de bits a un canal de paso a través, pero esta configuración no se recomienda porque no permite el streaming con velocidad de bits adaptable al cliente.

  > [!NOTE]
  > El método de paso a través es la forma más económica de realizar un streaming en vivo.

* Un codificador en directo local envía una secuencia de velocidad de bits única al canal LiveEvent que está habilitado para realizar la codificación en vivo con Media Services, con uno de los siguientes formatos: RTMP o Smooth Streaming (MP4 fragmentado). Después, el canal LiveEvent codifica en vivo la secuencia entrante de velocidad de bits única en una secuencia de vídeo de velocidad de bits múltiple (adaptable).

Para obtener información detallada sobre la codificación en vivo con Media Services, vea [Streaming en vivo con Azure Media Services v3](live-streaming-overview.md).

## <a name="live-encoders-that-output-rtmp"></a>Codificadores en directo que generan una salida RTMP

Media Services recomienda usar uno de los siguientes codificadores en directo que tienen RTMP como salida. Los esquemas URL admitidos son `rtmp://` o `rtmps://`.

> [!NOTE]
 > Al hacer el streaming mediante RTMP, compruebe la configuración del firewall o del proxy para confirmar que los puertos TCP salientes 1935 y 1936 están abiertos.<br/>
 Al hacer el streaming mediante RTMPS, compruebe la configuración del firewall o del proxy para confirmar que los puertos TCP salientes 2935 y 2936 están abiertos.

- Adobe Flash Media Live Encoder 3.2
- Haivision KB
- Haivision Makito X HEVC
- OBS Studio
- Switcher Studio (iOS)
- Telestream Wirecast 8.1+
- Telestream Wirecast S
- Teradek Slice 756
- TriCaster 8000
- Tricaster Mini HD-4
- VMIX
- xStream

## <a name="live-encoders-that-output-fragmented-mp4"></a>Codificadores en directo que generan una salida de MP4 fragmentado

Media Services recomienda usar uno de los codificadores en directo siguientes que tienen Smooth Streaming de velocidad de bits múltiple (MP4 fragmentado) como salida. Los esquemas URL admitidos son `rtmp://` o `rtmps://`.

- Ateme TITAN Live
- Cisco Digital Media Encoder 2200
- Elemental Live
- Envivio 4Caster C4 Gen III
- Imagine Communications Selenio MCP3
- Media Excel Hero Live y Hero 4K (UHD/HEVC)

## <a name="how-to-become-an-on-premises-encoder-partner"></a>Convertirse en un asociado de codificador local

Como asociado de codificador local de Azure Media Services, Media Services promueve su producto al recomendar su codificador a clientes empresariales. Para convertirse en un asociado de codificador local, debe comprobar la compatibilidad del codificador local con Media Services. Para hacerlo, complete las comprobaciones siguientes:

### <a name="pass-through-liveevent-verification"></a>Verificación del canal LiveEvent con paso a través

1. Cree la cuenta de Azure Media Services o acceda a ella.
2. Cree e inicie un canal LiveEvent de **paso a través**.
3. Configure el codificador para insertar una transmisión en directo con múltiples velocidades de bits
4. Cree un evento en directo publicado.
5. Ejecute el codificador en directo durante aproximadamente diez minutos.
6. Detenga el evento en directo.
7. Cree, inicie un punto de conexión de streaming, use un reproductor como [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html) para ver el recurso archivado para asegurarse de que la reproducción no tiene problemas visibles para todos los niveles de calidad (o también para ver y validar mediante la dirección URL de vista previa durante la sesión en vivo antes del paso 6).
8. Anote el identificador del recurso, la dirección URL del streaming publicado del archivo en directo y la configuración y la versión que se han usado desde el codificador en directo.
9. Restablezca el estado del canal LiveEvent después de crear cada ejemplo.
10. Repita del paso 3 al 9 para todas las configuraciones compatibles con el codificador (con y sin señalización, subtítulos o velocidades de codificación variadas).

### <a name="live-encoding-liveevent-verification"></a>Verificación del canal LiveEvent con codificación en directo

1. Cree la cuenta de Azure Media Services o acceda a ella.
2. Cree e inicie un canal LiveEvent de **codificación en directo**.
3. Configure el codificador para insertar una transmisión en directo con una velocidad de bits única
4. Cree un evento en directo publicado.
5. Ejecute el codificador en directo durante aproximadamente diez minutos.
6. Detenga el evento en directo.
7. Cree, inicie un punto de conexión de streaming, use un reproductor como [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html) para ver el recurso archivado para asegurarse de que la reproducción no tiene problemas visibles para todos los niveles de calidad (o también para ver y validar mediante la dirección URL de vista previa durante la sesión en vivo antes del paso 6).
8. Anote el identificador del recurso, la dirección URL del streaming publicado del archivo en directo y la configuración y la versión que se han usado desde el codificador en directo.
9. Restablezca el estado del canal LiveEvent después de crear cada ejemplo.
10. Repita del paso 3 al 9 para todas las configuraciones compatibles con el codificador (con y sin señalización, subtítulos o velocidades de codificación variadas).

### <a name="longevity-verification"></a>Comprobación de duración

1. Cree la cuenta de Azure Media Services o acceda a ella.
2. Cree e inicie un canal de **paso a través**.
3. Configure el codificador para insertar una transmisión en directo con múltiples velocidades de bits
4. Cree un evento en directo publicado.
5. Ejecute el codificador en directo durante una semana o más.
6. Use un reproductor, como [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html), para ver el streaming en vivo cada cierto tiempo (o el recurso archivado) para asegurarse de que la reproducción no tenga problemas visibles.
7. Detenga el evento en directo.
8. Anote el identificador del recurso, la dirección URL del streaming publicado del archivo en directo y la configuración y la versión que se han usado desde el codificador en directo.

Por último, envíe la configuración que ha anotado y los parámetros archivados por correo electrónico a Azure Media Services (amsstreaming@microsoft.com) a modo de notificación de que se han superado todos los pasos de la comprobación automática. Además, incluya su información de contacto para poder recibir las actualizaciones que haya. Puede ponerse en contacto con el equipo de Azure Media Services si tiene alguna pregunta con respecto a este proceso.

## <a name="next-steps"></a>Pasos siguientes

[Streaming en vivo con Media Services v3](live-streaming-overview.md)
