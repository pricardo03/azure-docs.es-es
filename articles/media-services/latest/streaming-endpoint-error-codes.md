---
title: Errores de empaquetado y el origen de Azure de Media Services | Microsoft Docs
description: En este tema se describe los errores que puede recibir desde el servicio de extremo de Streaming de servicios de Azure Media (Origin).
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2019
ms.author: juliako
ms.openlocfilehash: ebcda6026f79bc88df91471d8be88316ba57bfc6
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65411371"
---
# <a name="streaming-endpoint-origin-errors"></a>Errores de transmisión por secuencias (origen) del punto de conexión 

En este tema se describe los errores que puede recibir desde el Azure Media Services [servicio Streamingendpoint](streaming-endpoint-concept.md).

## <a name="400-bad-request"></a>400 - Solicitud incorrecta

La solicitud contiene información no válida y se rechaza con estos códigos de error y debido a uno de los siguientes motivos:

|Código de error|Valor hexadecimal |Descripción del error|
|---|---|---|
|MPE_BAD_URL_SYNTAX |0x80890201|Un error de sintaxis o el formato de dirección URL. Algunos ejemplos son las solicitudes de un tipo no válido, un fragmento no válido o un seguimiento no válido. |
|MPE_ENC_ENCRYPTION_NOT_SPECIFIED_IN_URL |0x8088024C|La solicitud no tiene ninguna etiqueta de cifrado en la dirección URL. Las solicitudes CMAF requieren una etiqueta de cifrado en la dirección URL. Otros protocolos que se configuran con más de un tipo de cifrado requieren también la etiqueta de cifrado para la anulación de ambigüedades. |
|MPE_STORAGE_BAD_URL_SYNTAX |0x808900E9|Error en la solicitud en el almacenamiento para satisfacer la solicitud con un error de solicitud incorrecta. |

## <a name="403-forbidden"></a>403 Prohibido

No se aceptó la solicitud debido a una de las siguientes razones:

|Código de error|Valor hexadecimal |Descripción del error|
|---|---|---|
|MPE_STORAGE_AUTHENTICATION_FAILED |0x808900EA|Error al solicitar almacenamiento para satisfacer la solicitud con un error de autenticación. Esto puede ocurrir si se giran las claves de almacenamiento y el servicio no pudo sincronizar las claves de almacenamiento. <br/><br/>Póngase en contacto con Azure admitir yendo a [ayuda y soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) en Azure portal.|
|MPE_STORAGE_INSUFFICIENT_ACCOUNT_PERMISSIONS |0x808900EB |Error de operación de almacenamiento, no se pudo acceso debido a permisos insuficientes de cuenta. |
|MPE_STORAGE_ACCOUNT_IS_DISABLED |0x808900EC |Error al solicitar almacenamiento para satisfacer la solicitud porque la cuenta de almacenamiento está deshabilitado. |
|MPE_STORAGE_AUTHENTICATION_FAILURE |0x808900F3 |Error de operación de almacenamiento, acceso erróneas debido a errores genéricos. |
|MPE_OUTPUT_FORMAT_BLOCKED |0x80890207 |El formato de salida está bloqueado debido a la configuración en el objeto StreamingPolicy. |
|MPE_ENC_ENCRYPTION_REQUIRED |0x8088021E |Se necesita cifrado para el contenido, directiva de entrega es necesaria para el formato de salida. |
|MPE_ENC_ENCRYPTION_NOT_SET_IN_DELIVERY_POLICY |0x8088024D |El cifrado no se establece en la configuración de directiva de entrega. |

## <a name="404-not-found"></a>405 No encontrado

La operación está intentando actuar en un recurso que ya no existe. Por ejemplo, el recurso que ya se eliminara.

|Código de error|Valor hexadecimal |Descripción del error|
|---|---|---|
|MPE_EGRESS_TRACK_NOT_FOUND |0x80890209 |No se encuentra el seguimiento solicitado. |
|MPE_RESOURCE_NOT_FOUND |0x808901F9 |No se encuentra el recurso solicitado. |
|MPE_UNAUTHORIZED |0x80890244 |El acceso no está autorizado. |
|MPE_EGRESS_TIMESTAMP_NOT_FOUND |0x8089020A |No se encuentra la marca de tiempo solicitado. |
|MPE_EGRESS_FILTER_NOT_FOUND |0x8089020C |No se encuentra el filtro de manifiesto dinámico solicitado. |
|MPE_FRAGMENT_BY_INDEX_NOT_FOUND |0x80890252 |El índice de fragmentos solicitada está fuera del intervalo válido. |
|MPE_LIVE_MEDIA_ENTRIES_NOT_FOUND |0x80890254 |Medios no se puede encontrar las entradas de obtener el búfer moov en directo. |
|MPE_FRAGMENT_TIMESTAMP_NOT_FOUND |0x80890255 |No se encuentra el fragmento en el momento solicitado de una determinada pista.<br/><br/>Podría ser que el fragmento no está en el almacenamiento. Pruebe una capa de la presentación que podría tener un fragmento diferente. |
|MPE_MANIFEST_MEDIA_ENTRY_NOT_FOUND |0x80890256 |No se encuentra la entrada de multimedia para la velocidad de bits solicitado en el manifiesto. <br/><br/>Podría ser que el Reproductor le pida una pista de vídeo de un determinado velocidad de bits que no se encontró en el manifiesto.|
|MPE_METADATA_NOT_FOUND |0x80890257 |No se puede encontrar ciertos metadatos en el manifiesto o no se puede encontrar la reorganización de almacenamiento. |
|MPE_STORAGE_RESOURCE_NOT_FOUND |0x808900ED |Error de operación de almacenamiento, no se encuentra el recurso. |

## <a name="409-conflict"></a>409 Conflicto

El identificador proporcionado para un recurso en un `PUT` o `POST` operación ha sido tomada por un recurso existente. Para resolver este problema, utilice otro identificador para el recurso.

|Código de error|Valor hexadecimal |Descripción del error|
|---|---|---|
|MPE_STORAGE_CONFLICT  |0x808900EE  |Error de operación de almacenamiento, error de conflicto.  |

## <a name="410"></a>410

|Código de error|Valor hexadecimal |Descripción del error|
|---|---|---|
|MPE_FILTER_FORCE_END_LEFT_EDGE_CROSSED_DVR_WINDOW|0x80890263|Para streaming en vivo, cuando el filtro que tiene es forceEndTimestamp establecido en true, el inicio o el final de marca de tiempo fuera de la ventana DVR actual.|

## <a name="412-precondition-failure"></a>Error 412 Error de condición previa

La operación especificó un valor eTag que es diferente de la versión disponible en el servidor, es decir, un error de simultaneidad optimista. Vuelva a intentar la solicitud después de leer la versión más reciente del recurso y actualizar la eTag en la solicitud.

|Código de error|Valor hexadecimal |Descripción del error|
|---|---|---|
|MPE_FRAGMENT_NOT_READY |0x80890200 |El fragmento solicitado no está listo.|
|MPE_STORAGE_PRECONDITION_FAILED| 0x808900EF|Error de operación de almacenamiento, un error de condición previa.|

## <a name="415-unsupported-media-type"></a>Tipo de medio no compatible 415

El formato de carga enviado por el cliente está en un formato no admitido.

|Código de error|Valor hexadecimal |Descripción del error|
|---|---|---|
|MPE_ENC_ALREADY_ENCRYPTED| 0x8088021F| No se debe aplicar el cifrado de contenido ya cifrado.|
|MPE_ENC_INVALID_INPUT_ENCRYPTION_FORMAT|0x8088021D |El cifrado no es válido para el formato de entrada.|
|MPE_INVALID_ASSET_DELIVERY_POLICY_TYPE|0x8088021C| Tipo de directiva de entrega no es válido.|
|MPE_ENC_MULTIPLE_SAME_DELIVERY_TYPE|0x8088024E |La configuración original puede compartirse entre varios formatos de salida.|
|MPE_FORMAT_NOT_SUPPORTED|0x80890205|No se admite el formato multimedia o el tipo. Por ejemplo, Media Services no admite el número de nivel de calidad que sea más de 64. En la etiqueta de vídeo FLV, Media Services no admite un fotograma de vídeo con SPS varios y varios de PPS.|
|MPE_INPUT_FORMAT_NOT_SUPPORTED|0x80890218| No se admite el formato de entrada de recurso solicitado. Media Services admite Smooth (directo), MP4 (VoD) y progresiva descargar formatos.|
|MPE_OUTPUT_FORMAT_NOT_SUPPORTED|0x8089020D|No se admite el formato de salida solicitado. Media Services admite Smooth, DASH (CSF, CMAF), progresiva y HLS (v3, v4, CMAF), descargue los formatos.|
|MPE_ENCRYPTION_NOT_SUPPORTED|0x80890208|Se detectó el tipo de cifrado no admitido.|
|MPE_MEDIA_TYPE_NOT_SUPPORTED|0x8089020E|El tipo de medio solicitado no es compatible con el formato de salida. Los tipos admitidos son vídeo, audio o subtítulo "SUBT".|
|MPE_MEDIA_ENCODING_NOT_SUPPORTED|0x8089020F|Los medios de origen activo se codifican con un formato multimedia que no es compatible con el formato de salida.|
|MPE_VIDEO_ENCODING_NOT_SUPPORTED|0x80890210|El recurso de origen se codificó con un formato de vídeo que no es compatible con el formato de salida. Se admiten H.264 AVC, H.265 (HEVC, hev1 o hvc1).|
|MPE_AUDIO_ENCODING_NOT_SUPPORTED|0x80890211|El recurso de origen se codificó con un formato de audio que no es compatible con el formato de salida. Formatos de audio admitidos son AAC, E-AC3 (DD +), Dolby DTS.|
|MPE_SOURCE_PROTECTION_CONVERSION_NOT_SUPPORTED|0x80890212|El recurso protegido de origen no se puede convertir al formato de salida.|
|MPE_OUTPUT_PROTECTION_FORMAT_NOT_SUPPORTED|0x80890213|El formato de protección no es compatible con el formato de salida.|
|MPE_INPUT_PROTECTION_FORMAT_NOT_SUPPORTED|0x80890219|El formato de protección no es compatible con el formato de entrada.|
|MPE_INVALID_VIDEO_NAL_UNIT|0x80890231|Unidad no válida vídeo NAL, por ejemplo, solo la primera NAL en el ejemplo puede ser un AUD.|
|MPE_INVALID_NALU_SIZE|0x80890260|Tamaño de unidad NAL no válido.|
|MPE_INVALID_NALU_LENGTH_FIELD|0x80890261|Valor de longitud de unidad NAL no válido.|
|MPE_FILTER_INVALID|0x80890236|Filtros de manifiesto dinámico no válidos.|
|MPE_FILTER_VERSION_INVALID|0x80890237|Versiones de filtro no válido o incompatible.|
|MPE_FILTER_TYPE_INVALID|0x80890238|Tipo de filtro no válido.|
|MPE_FILTER_RANGE_ATTRIBUTE_INVALID|0x80890239|Intervalo no válido especificado por el filtro.|
|MPE_FILTER_TRACK_ATTRIBUTE_INVALID|0x8089023A|Atributo de seguimiento no válido especificado por el filtro.|
|MPE_FILTER_PRESENTATION_WINDOW_INVALID|0x8089023B|Duración de la ventana de presentación no se especifica mediante el filtro.|
|MPE_FILTER_LIVE_BACKOFF_INVALID|0x8089023C|Válido en vivo de interrupción especificado por el filtro.|
|MPE_FILTER_MULTIPLE_SAME_TYPE_FILTERS|0x8089023D|Se admite solo un elemento absTimeInHNS en filtros heredados.|
|MPE_FILTER_REMOVED_ALL_STREAMS|0x8089023E|Hay no hay más secuencias en absoluto después de aplicar los filtros.|
|MPE_FILTER_LIVE_BACKOFF_OVER_DVRWINDOW|0x8089023F|El retroceso en directo está más allá de la ventana de DVR.|
|MPE_FILTER_LIVE_BACKOFF_OVER_PRESENTATION_WINDOW|0x80890262|El retroceso en directo es mayor que la ventana de presentación.|
|MPE_FILTER_COMPOSITION_FILTER_COUNT_OVER_LIMIT|0x80890246|Habían superado filtros predeterminado permitido máximo de diez (10).|
|MPE_FILTER_COMPOSITION_MULTIPLE_FIRST_QUALITY_OPERATOR_NOT_ALLOWED|0x80890248|No se permite varios primer operador de calidad de vídeo en los filtros de solicitud combinada.|
|MPE_FILTER_FIRST_QUALITY_ATTRIBUTE_INVALID|0x80890249|El número de atributos de velocidad de bits de primera calidad debe ser uno (1).|
|MPE_HLS_SEGMENT_TOO_LARGE|0x80890243|Duración del segmento HLS debe ser menores que retroceder un tercio de la ventana DVR y HLS.|
|MPE_KEY_FRAME_INTERVAL_TOO_LARGE|0x808901FE|Duraciones de fragmentos deben ser menor o igual a aproximadamente 20 segundos o los niveles de calidad de entrada no están alineado de tiempo.|
|MPE_DTS_RESERVEDBOX_EXPECTED|0x80890105|Error de DTS específico, no se encuentra el ReservedBox cuando debe presentar en el DTSSpecficBox durante el análisis de cuadro DTS.|
|MPE_DTS_INVALID_CHANNEL_COUNT|0x80890106|Error específico de DTS, ningún canal se encuentra en el DTSSpecficBox durante el análisis de cuadro DTS.|
|MPE_DTS_SAMPLETYPE_MISMATCH|0x80890107|Error específico de DTS, error de coincidencia de tipo de ejemplo en el DTSSpecficBox.|
|MPE_DTS_MULTIASSET_DTSH_MISMATCH|0x80890108|Error específico del DTS, varios activos se establece, pero la discordancia de ejemplo DTSH.|
|MPE_DTS_INVALID_CORESTREAM_SIZE|0x80890109|Error específico del DTS, tamaño de la secuencia principal no es válido.|
|MPE_DTS_INVALID_SAMPLE_RESOLUTION|0x8089010A|Error de DTS específico, resolución de ejemplo no es válida.|
|MPE_DTS_INVALID_SUBSTREAM_INDEX|0x8089010B|Error de DTS específico, índice de la extensión de secuencia secundaria no es válido.|
|MPE_DTS_INVALID_BLOCK_NUM|0x8089010C|Error de DTS específico, número de bloque de flujo secundario no es válido.|
|MPE_DTS_INVALID_SAMPLING_FREQUENCE|0x8089010D|Error de DTS específico, frecuencia de muestreo no es válido.|
|MPE_DTS_INVALID_REFCLOCKCODE|0x8089010E|Error de DTS específico, el código de reloj de referencia en la extensión de flujo secundario no es válido.|
|MPE_DTS_INVALID_SPEAKERS_REMAP|0x8089010F|Error de DTS específico, el número de conjunto de reasignación de los altavoces no es válido.|

Para artículos de cifrado y ejemplos, vea:

- [Concepto: protección de contenido](content-protection-overview.md)
- [Concepto: Directivas de clave de contenido](content-key-policy-concept.md)
- [Concepto: Directivas de transmisión por secuencias](streaming-policy-concept.md)
- [Ejemplo: proteger con cifrado AES](protect-with-aes128.md)
- [Ejemplo: proteger con DRM](protect-with-drm.md)

Para obtener instrucciones de filtro, vea:

- [Concepto: manifiestos dinámicos](filters-dynamic-manifest-overview.md)
- [Concepto: filtros](filters-concept.md)
- [Ejemplo: creación de filtros con las API de REST](filters-dynamic-manifest-rest-howto.md)
- [Ejemplo: creación de filtros con .NET](filters-dynamic-manifest-dotnet-howto.md)
- [Ejemplo: crear filtros con la CLI](filters-dynamic-manifest-cli-howto.md)

Para artículos en vivo y ejemplos, vea:

- [Concepto: información general de streaming en vivo](live-streaming-overview.md)
- [Concepto: Eventos en directo y salidas en vivo](live-events-outputs-concept.md)
- [Ejemplo: tutorial de transmisión por secuencias en directo](stream-live-tutorial-with-api.md)

## <a name="416-range-not-satisfiable"></a>416 de intervalo no se puede satisfacer

|Código de error|Valor hexadecimal |Descripción del error|
|---|---|---|
|MPE_STORAGE_INVALID_RANGE|0x808900F1|Error de operación de almacenamiento, devolvió un error http 416, intervalo no válido.|

## <a name="500-internal-server-error"></a>Error de servidor interno 500

Durante el procesamiento de la solicitud, Media Services encuentra algún error que impide que el procesamiento continúe.  

|Código de error|Valor hexadecimal |Descripción del error|
|---|---|---|
|MPE_STORAGE_SOCKET_TIMEOUT|0x808900F4|Recibido y se convierten desde Winhttp código de error ERROR_WINHTTP_TIMEOUT (0x00002ee2).|
|MPE_STORAGE_SOCKET_CONNECTION_ERROR|0x808900F5|Recibido y se convierten desde Winhttp código de error ERROR_WINHTTP_CONNECTION_ERROR (0x00002efe).|
|MPE_STORAGE_SOCKET_NAME_NOT_RESOLVED|0x808900F6|Recibido y se convierten desde Winhttp código de error ERROR_WINHTTP_NAME_NOT_RESOLVED (0x00002ee7).|
|MPE_STORAGE_INTERNAL_ERROR|0x808900E6|Error de operación de almacenamiento, InternalError general de uno de los errores HTTP 500.|
|MPE_STORAGE_OPERATION_TIMED_OUT|0x808900E7|Error de operación de almacenamiento, OperationTimedOut general de uno de los errores HTTP 500.|
|MPE_STORAGE_FAILURE|0x808900F2|Error de operación de almacenamiento, otros errores HTTP 500 Error interno o OperationTimedOut.|

## <a name="503-service-unavailable"></a>Servicio no disponible 503

El servidor actualmente no puede recibir solicitudes. Este error puede deberse a solicitudes excesivas en el servicio. El mecanismo de limitación de Media Services restringe el uso de recursos en las aplicaciones que realizan un número excesivo de solicitudes al servicio.

> [!NOTE]
> Compruebe el mensaje de error y la cadena de error para obtener más información detallada sobre la razón por la que tiene el error 503. Este error no siempre implica limitación.
> 

|Código de error|Valor hexadecimal |Descripción del error|
|---|---|---|
|MPE_STORAGE_SERVER_BUSY|0x808900E8|Error de operación de almacenamiento, recibe el error de disponibilidad de servidor HTTP 503.|

## <a name="ask-questions-give-feedback-get-updates"></a>Formule preguntas, realice comentarios y obtenga actualizaciones

Consulte el artículo [Comunidad de Azure Media Services](media-services-community.md) para ver diferentes formas de formular preguntas, enviar comentarios y obtener actualizaciones de Media Services.

## <a name="see-also"></a>Vea también

- [Códigos de error de codificación](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode)
- [Conceptos de Azure Media Services](concepts-overview.md)
- [Cuotas y limitaciones](limits-quotas-constraints.md)

## <a name="next-steps"></a>Pasos siguientes

[Ejemplo: obtener acceso a código de error y el mensaje desde ApiException con .NET](configure-connect-dotnet-howto.md#connect-to-the-net-client)
