---
title: Información sobre los codificadores recomendados por Azure Media Services | Microsoft Docs
description: Información sobre los codificadores recomendados por Media Services
services: media-services
keywords: encoding;encoders;media
author: dbgeorge
manager: johndeu
ms.author: johndeu
ms.date: 02/09/2019
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 76c5c5d90045e1697f698895e522ffda47d47565
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57447728"
---
# <a name="recommended-on-premises-encoders"></a>Codificadores locales recomendados
Cuando realice streaming en directo con Azure Media Services, puede especificar cómo desea que el canal reciba la transmisión de entrada. Si decide usar un codificador local en un canal de codificación en directo, el codificador debe insertar una transmisión de alta calidad sola velocidad de bits como salida. Si decide usar un codificador local en con un paso a través del canal, el codificador debe insertar una secuencia de varias velocidades de bits como salida con todas las calidades de salida deseado. Para obtener más información, consulte [streaming en vivo con en local de los codificadores](media-services-live-streaming-with-onprem-encoders.md).

Azure Media Services recomienda usar uno de los codificadores en directo que tienen RTMP como salida:
- Adobe Flash Media Live Encoder 3.2
- Haivision Makito X HEVC
- Haivision KB
- Telestream Wirecast 8.1+
- Telestream Wirecast S
- Teradek Slice 756
- TriCaster 8000
- Tricaster Mini HD-4
- OBS Studio
- VMIX
- xStream
- Switcher Studio (iOS)

Azure Media Services recomienda usar uno de los codificadores en directo siguientes que tienen MP4 fragmentado con múltiples velocidades de bits (Smooth Streaming) como salida:
- Media Excel Hero Live y Hero 4K (UHD/HEVC)
- Ateme TITAN Live
- Cisco Digital Media Encoder 2200
- Elemental Live
- Envivio 4Caster C4 Gen III
- Imagine Communications Selenio MCP3

> [!NOTE]
> Un codificador en directo puede enviar una transmisión con una sola velocidad de bits a un canal de paso a través, pero esta configuración no se recomienda porque no permite el streaming con velocidad de bits adaptable al cliente.

## <a name="how-to-become-an-on-premises-encoder-partner"></a>Cómo convertirse en un socio de codificador local en
Como un Azure Media Services en el socio de codificador local, Media Services promueve su producto al recomendar su codificador a clientes empresariales. Para convertirse en un socio de codificador en entorno local, debe comprobar la compatibilidad del codificador en entorno local con Media Services. Para hacerlo, complete las comprobaciones siguientes:

Comprobación del canal de paso a través
1. Cree o visite la cuenta de Azure Media Services
2. Cree e inicie un canal de **paso a través**
3. Configure el codificador para insertar una transmisión en directo con múltiples velocidades de bits
4. Cree un evento en directo publicado
5. Ejecute el codificador en directo durante aproximadamente diez minutos
6. Detenga el evento en directo
7. Cree, inicie un punto de conexión de streaming, use un reproductor como [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html) para ver el recurso archivado para asegurarse de que la reproducción no tiene problemas visibles para todos los niveles de calidad (o también para ver y validar a través de la dirección URL de vista previa durante la sesión en vivo antes del paso 6)
8. Anote el id. del activo, la dirección URL del streaming publicado del archivo en directo y la configuración y la versión que se usó desde el codificador en directo
9. Restablezca el estado del canal después de crear cada ejemplo
10. Repita del paso 3 al paso 9 para todas las configuraciones compatibles con el codificador (con y sin señalización de anuncios, leyendas o velocidades de codificación distintas)

Comprobación del canal de codificación en directo
1. Cree o visite la cuenta de Azure Media Services
2. Cree e inicie un canal de **codificación en directo**
3. Configure el codificador para insertar una transmisión en directo con una velocidad de bits única
4. Cree un evento en directo publicado
5. Ejecute el codificador en directo durante aproximadamente diez minutos
6. Detenga el evento en directo
7. Cree, inicie un punto de conexión de streaming, use un reproductor como [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html) para ver el recurso archivado para asegurarse de que la reproducción no tiene problemas visibles para todos los niveles de calidad (o también para ver y validar a través de la dirección URL de vista previa durante la sesión en vivo antes del paso 6)
8. Anote el id. del activo, la dirección URL del streaming publicado del archivo en directo y la configuración y la versión que se usó desde el codificador en directo
9. Restablezca el estado del canal después de crear cada ejemplo
10. Repita del paso 3 al paso 9 para todas las configuraciones compatibles con el codificador (con y sin señalización de anuncios, leyendas o velocidades de codificación variadas)

Comprobación de duración
1. Cree o visite la cuenta de Azure Media Services
2. Cree e inicie un canal de **paso a través**
3. Configure el codificador para insertar una transmisión en directo con múltiples velocidades de bits
4. Cree un evento en directo publicado
5. Ejecute el codificador en directo durante una semana o más
6. Use un reproductor, como [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html), para ver el streaming en vivo cada cierto tiempo (o recurso archivado) para asegurarse de que la reproducción no tenga problemas visibles
7. Detenga el evento en directo
8. Anote el id. del activo, la dirección URL del streaming publicado del archivo en directo y la configuración y la versión que se usó desde el codificador en directo

Por último, envíe la configuración que anotó y los parámetros del archivo activo a Media Services por correo electrónico a amsstreaming@microsoft.com. Una vez que reciba esta información, Media Services realiza pruebas de comprobación en los ejemplos del codificador en directo. Puede ponerse en contacto con Media Services si tiene alguna pregunta respecto de este proceso.
