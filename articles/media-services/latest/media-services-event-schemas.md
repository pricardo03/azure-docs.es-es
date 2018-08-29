---
title: Esquemas de Azure Event Grid para eventos de Media Services
description: Describe las propiedades que se proporcionan para los eventos de Media Services con Azure Event Grid
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: reference
ms.date: 08/17/2018
ms.author: juliako
ms.openlocfilehash: a6a6c459e170627d26aa1445f4f4dd193965fe70
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2018
ms.locfileid: "42145004"
---
# <a name="azure-event-grid-schemas-for-media-services-events"></a>Esquemas de Azure Event Grid para eventos de Media Services

En este artículo se proporcionan los esquemas y las propiedades para los eventos de Media Services.

Para obtener una lista de scripts de ejemplo y los tutoriales, consulte [Orígenes de eventos de Media Services](../../event-grid/event-sources.md#azure-subscriptions).

## <a name="available-event-types"></a>Tipos de eventos disponibles

Media Services emite los siguientes tipos de evento:

| Tipo de evento | DESCRIPCIÓN |
| ---------- | ----------- |
| Microsoft.Media.JobStateChange| Cambios en el estado actual del trabajo. |
| Microsoft.Media.LiveEventConnectionRejected | Se rechazó el intento de conexión del codificador. |
| Microsoft.Media.LiveEventEncoderConnected | El codificador se conectó al evento en directo. |
| Microsoft.Media.LiveEventEncoderDisconnected | El codificador se desconecta. |
| Microsoft.Media.LiveEventIncomingDataChunkDropped | El servidor multimedia elimina el fragmento de datos porque es demasiado tarde o porque tiene una marca de tiempo superpuesta (la marca de tiempo del nuevo fragmento de datos es menor que el tiempo de finalización del fragmento de datos anterior). |
| Microsoft.Media.LiveEventIncomingStreamReceived | El servidor multimedia recibe el primer fragmento de datos de cada pista en la transmisión o la conexión. |
| Microsoft.Media.LiveEventIncomingStreamsOutOfSync | El servidor multimedia detecta que las transmisiones de audio y vídeo no están sincronizadas. Se usa como advertencia porque la experiencia del usuario no se verá afectada. |
| Microsoft.Media.LiveEventIncomingVideoStreamsOutOfSync | El servidor multimedia detecta que cualquiera de las dos transmisiones de vídeo provenientes del codificador externo no está sincronizada. Se usa como advertencia porque la experiencia del usuario no se verá afectada. |
| Microsoft.Media.LiveEventIngestHeartbeat | Se publica cada 20 segundos para cada pista cuando se está ejecutando el evento en directo. Proporciona resumen de mantenimiento de la ingesta. |
| Microsoft.Media.LiveEventTrackDiscontinuityDetected | El servidor multimedia detecta una discontinuidad en la pista entrante. |

Existen dos categorías para los eventos en **directo**: eventos a nivel de transmisión y eventos a nivel de pista. 

Los eventos de nivel de transmisión se generan en función de la transmisión o conexión. Cada evento tiene un parámetro `StreamId` que identifica la conexión o la transmisión. Cada transmisión o conexión tiene una o más pistas de diferentes tipos. Por ejemplo, una conexión de un codificador puede tener una pista de audio y cuatro pistas de vídeo. Los tipos de evento de transmisión son:

* LiveEventConnectionRejected
* LiveEventEncoderConnected
* LiveEventEncoderDisconnected

Los eventos de nivel de seguimiento se generan por pista. Los tipos de evento de pista son:

* LiveEventIncomingDataChunkDropped
* LiveEventIncomingStreamReceived
* LiveEventIncomingStreamsOutOfSync
* LiveEventIncomingVideoStreamsOutOfSync
* LiveEventIngestHeartbeat
* LiveEventTrackDiscontinuityDetected

## <a name="jobstatechange"></a>JobStateChange

En el ejemplo siguiente, se muestra el esquema de un evento **JobStateChange**: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "transforms/VideoAnalyzerTransform/jobs/<job-id>",
    "eventType": "Microsoft.Media.JobStateChange",
    "eventTime": "2018-04-20T21:26:13.8978772",
    "id": "b9d38923-9210-4c2b-958f-0054467d4dd7",
    "data": {
      "previousState": "Processing",
      "state": "Finished"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

El objeto data tiene las siguientes propiedades:

| Propiedad | Escriba | DESCRIPCIÓN |
| -------- | ---- | ----------- |
| previousState | string | El estado del trabajo antes del evento. |
| state | string | El nuevo estado del trabajo que se notifica en este evento. Por ejemplo, "En cola: el trabajo está a la espera de recursos" o "Programado: el trabajo está listo para empezar".|

Donde el estado del trabajo puede ser uno de estos valores: *Queued* (En cola), *Scheduled* (Programado), *Processing* (En proceso), *Finished* (Finalizado), *Error*, *Canceled* (Cancelado), *Canceling* (Cancelándose).

## <a name="liveeventconnectionrejected"></a>LiveEventConnectionRejected

En el ejemplo siguiente, se muestra el esquema de un evento **LiveEventConnectionRejected**: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaServices/<account-name>",
    "subject": "/LiveEvents/MyLiveEvent1",
    "eventType": "Microsoft.Media.LiveEventConnectionRejected",
    "eventTime": "2018-01-16T01:57:26.005121Z",
    "id": "b303db59-d5c1-47eb-927a-3650875fded1",
    "data": { 
      "StreamId":"Mystream1",
      "IngestUrl": "http://abc.ingest.isml",
      "EncoderIp": "118.238.251.xxx",
      "EncoderPort": 52859,
      "ResultCode": "MPE_INGEST_CODEC_NOT_SUPPORTED"
    },
    "dataVersion": "1.0"
  }
]
```

El objeto data tiene las siguientes propiedades:

| Propiedad | Escriba | DESCRIPCIÓN |
| -------- | ---- | ----------- |
| StreamId | string | Identificador de la transmisión o la conexión. El codificador o el cliente es responsable de agregar este id. en la URL de introducción. |  
| IngestUrl | string | Introducir la dirección URL que proporcionó el evento en directo. |  
| EncoderIp | string | Dirección IP del codificador. |
| EncoderPort | string | Puerto del codificador de donde procede esta transmisión. |
| ResultCode | string | El motivo por el que se rechazó la conexión. En la tabla siguiente se muestran los códigos de resultados: |

Los códigos de resultados son:

| Código de resultado | DESCRIPCIÓN |
| ----------- | ----------- |
| MPE_RTMP_APPID_AUTH_FAILURE | Dirección URL de introducción incorrecta. |
| MPE_INGEST_ENCODER_CONNECTION_DENIED | El codificador de IP no está presente en la lista de direcciones IP permitidas configurada. |
| MPE_INGEST_RTMP_SETDATAFRAME_NOT_RECEIVED | El codificador no envió los metadatos acerca de la transmisión. |
| MPE_INGEST_CODEC_NOT_SUPPORTED | No se admite el códec especificado. |
| MPE_INGEST_DESCRIPTION_INFO_NOT_RECEIVED | Se recibió un fragmento antes de recibir el encabezado de esa transmisión. |
| MPE_INGEST_MEDIA_QUALITIES_EXCEEDED | El número de calidades especificado supera el límite máximo permitido. |
| MPE_INGEST_BITRATE_AGGREGATED_EXCEEDED | La velocidad de bits agregada supera el límite máximo permitido. |
| MPE_RTMP_FLV_TAG_TIMESTAMP_INVALID | La marca de tiempo de vídeo o audio FLVTag no es válida en el codificador de RTMP. |

## <a name="liveeventencoderconnected"></a>LiveEventEncoderConnected

En el ejemplo siguiente, se muestra el esquema de un evento **LiveEventEncoderConnected**: 

```json
[
  { 
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventEncoderConnected",
    "eventTime": "2018-08-07T23:08:09.1710643",
    "id": "<id>",
    "data": {
      "ingestUrl": "http://mle1-amsts03mediaacctgndos-ts031.channel.media.azure-test.net:80/ingest.isml",
      "streamId": "15864-stream0",
      "encoderIp": "131.107.147.xxx",
      "encoderPort": "27485"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

El objeto data tiene las siguientes propiedades:

| Propiedad | Escriba | DESCRIPCIÓN |
| -------- | ---- | ----------- |
| StreamId | string | Identificador de la transmisión o la conexión. El codificador o el cliente es responsable de proporcionar este id. en la URL de introducción. |
| IngestUrl | string | Introducir la dirección URL que proporcionó el evento en directo. |
| EncoderIp | string | Dirección IP del codificador. |
| EncoderPort | string | Puerto del codificador de donde procede esta transmisión. |

## <a name="liveeventencoderdisconnected"></a>LiveEventEncoderDisconnected

En el ejemplo siguiente, se muestra el esquema de un evento **LiveEventEncoderDisconnected**: 

```json
[
  { 
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventEncoderDisconnected",
    "eventTime": "2018-08-07T23:08:09.1710872",
    "id": "<id>",
    "data": {
      "ingestUrl": "http://mle1-amsts03mediaacctgndos-ts031.channel.media.azure-test.net:80/ingest.isml",
      "streamId": "15864-stream0",
      "encoderIp": "131.107.147.xxx",
      "encoderPort": "27485",
      "resultCode": "S_OK"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

El objeto data tiene las siguientes propiedades:

| Propiedad | Escriba | DESCRIPCIÓN |
| -------- | ---- | ----------- |
| StreamId | string | Identificador de la transmisión o la conexión. El codificador o el cliente es responsable de agregar este id. en la URL de introducción. |  
| IngestUrl | string | Introducir la dirección URL que proporcionó el evento en directo. |  
| EncoderIp | string | Dirección IP del codificador. |
| EncoderPort | string | Puerto del codificador de donde procede esta transmisión. |
| ResultCode | string | La razón por la cual el codificador se desconecta. Podría ser un cierre estable o un error. En la tabla siguiente se muestran los códigos de resultados: |

Los códigos de resultado del error son:

| Código de resultado | DESCRIPCIÓN |
| ----------- | ----------- |
| MPE_RTMP_SESSION_IDLE_TIMEOUT | La sesión de RTMP agotó el tiempo de espera después de estar inactiva durante el límite de tiempo permitido. |
| MPE_RTMP_FLV_TAG_TIMESTAMP_INVALID | La marca de tiempo de vídeo o audio FLVTag no es válida en el codificador de RTMP. |
| MPE_CAPACITY_LIMIT_REACHED | El codificador envía datos demasiado rápido. |
| Códigos de error desconocido | Estos códigos de error pueden variar desde un error de memoria hasta entradas duplicadas en el mapa de hash. |

Los códigos de resultado del cierre estable son:

| Código de resultado | DESCRIPCIÓN |
| ----------- | ----------- |
| S_OK | El codificador se desconectó correctamente. |
| MPE_CLIENT_TERMINATED_SESSION | Codificador desconectado (RTMP). |
| MPE_CLIENT_DISCONNECTED | Codificador desconectado (FMP4). |
| MPI_REST_API_CHANNEL_RESET | Se recibió el comando de restablecimiento de canal. |
| MPI_REST_API_CHANNEL_STOP | Se recibió el comando de parada de canal. |
| MPI_REST_API_CHANNEL_STOP | Canal en mantenimiento. |
| MPI_STREAM_HIT_EOF | La transmisión EOF que envía el codificador. |

## <a name="liveeventincomingdatachunkdropped"></a>LiveEventIncomingDataChunkDropped

En el ejemplo siguiente, se muestra el esquema de un evento **LiveEventIncomingDataChunkDropped**: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaServices/<account-name>",
    "subject": "/LiveEvents/MyLiveEvent1",
    "eventType": "Microsoft.Media.LiveEventIncomingDataChunkDropped",
    "eventTime": "2018-01-16T01:57:26.005121Z",
    "id": "03da9c10-fde7-48e1-80d8-49936f2c3e7d",
    "data": { 
      "TrackType": "Video",
      "TrackName": "Video",
      "Bitrate": 300000,
      "Timestamp": 36656620000,
      "Timescale": 10000000,
      "ResultCode": "FragmentDrop_OverlapTimestamp"
    },
    "dataVersion": "1.0"
  }
]
```

El objeto data tiene las siguientes propiedades:

| Propiedad | Escriba | DESCRIPCIÓN |
| -------- | ---- | ----------- |
| TrackType | string | Tipo de pista (audio/vídeo). |
| TrackName | string | Nombre de la pista. |
| Bitrate | integer | Velocidad de bits de la pista. |
| Timestamp | string | Marca de tiempo del fragmento de datos proporcionado. |
| Escala de tiempo | string | Escala temporal de la marca de tiempo. |
| ResultCode | string | Razón del fragmento de datos proporcionado. **FragmentDrop_OverlapTimestamp** o **FragmentDrop_NonIncreasingTimestamp**. |

## <a name="liveeventincomingstreamreceived"></a>LiveEventIncomingStreamReceived

En el ejemplo siguiente, se muestra el esquema de un evento **LiveEventIncomingStreamReceived**: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventIncomingStreamReceived",
    "eventTime": "2018-08-07T23:08:10.5069288Z",
    "id": "7f939a08-320c-47e7-8250-43dcfc04ab4d",
    "data": {
      "ingestUrl": "http://mle1-amsts03mediaacctgndos-ts031.channel.media.azure-test.net:80/ingest.isml/Streams(15864-stream0)15864-stream0",
      "trackType": "video",
      "trackName": "video",
      "bitrate": 2962000,
      "encoderIp": "131.107.147.xxx",
      "encoderPort": "27485",
      "timestamp": "15336831655032322",
      "duration": "20000000",
      "timescale": "10000000"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

El objeto data tiene las siguientes propiedades:

| Propiedad | Escriba | DESCRIPCIÓN |
| -------- | ---- | ----------- |
| TrackType | string | Tipo de pista (audio/vídeo). |
| TrackName | string | Nombre de la pista (lo puede proporcionar el codificador o, en el caso de RTMP, el servidor se genera en formato *TrackType_Bitrate*). |
| Bitrate | integer | Velocidad de bits de la pista. |
| IngestUrl | string | Introducir la dirección URL que proporcionó el evento en directo. |
| EncoderIp | string  | Dirección IP del codificador. |
| EncoderPort | string | Puerto del codificador de donde procede esta transmisión. |
| Timestamp | string | Primera marca de tiempo del fragmento de datos recibido. |
| Escala de tiempo | string | Escala temporal en la que se representa la marca de tiempo. |

## <a name="liveeventincomingstreamsoutofsync"></a>LiveEventIncomingStreamsOutOfSync

En el ejemplo siguiente, se muestra el esquema de un evento **LiveEventIncomingStreamsOutOfSync**: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventIncomingStreamsOutOfSync",
    "eventTime": "2018-08-10T02:26:20.6269183Z",
    "id": "b9d38923-9210-4c2b-958f-0054467d4dd7",
    "data": {
      "minLastTimestamp": "319996",
      "typeOfStreamWithMinLastTimestamp": "Audio",
      "maxLastTimestamp": "366000",
      "typeOfStreamWithMaxLastTimestamp": "Video"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

El objeto data tiene las siguientes propiedades:

| Propiedad | Escriba | DESCRIPCIÓN |
| -------- | ---- | ----------- |
| MinLastTimestamp | string | Mínimo de las últimas marcas de tiempo entre todas las pistas (audio o vídeo). |
| TypeOfTrackWithMinLastTimestamp | string | Tipo de pista (audio o vídeo) con la última marca de tiempo mínima. |
| MaxLastTimestamp | string | Máximo de las todas las marcas de tiempo entre todas las pistas (audio o vídeo). |
| TypeOfTrackWithMaxLastTimestamp | string | Tipo de pista (audio o vídeo) con la última marca de tiempo máxima. |

## <a name="liveeventincomingvideostreamsoutofsync"></a>LiveEventIncomingVideoStreamsOutOfSync

En el ejemplo siguiente, se muestra el esquema de un evento **LiveEventIncomingVideoStreamsOutOfSync**: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaServices/<account-name>",
    "subject": "/LiveEvents/LiveEvent1",
    "eventType": "Microsoft.Media.LiveEventIncomingVideoStreamsOutOfSync",
    "eventTime": "2018-01-16T01:57:26.005121Z",
    "id": "6dd4d862-d442-40a0-b9f3-fc14bcf6d750",
    "data": {
      "FirstTimestamp": "2162058216",
      "FirstDuration": "2000",
      "SecondTimestamp": "2162057216",
      "SecondDuration": "2000"
    },
    "dataVersion": "1.0"
  }
]
```

El objeto data tiene las siguientes propiedades:

| Propiedad | Escriba | DESCRIPCIÓN |
| -------- | ---- | ----------- |
| FirstTimestamp | string | Marca de tiempo recibida para una de las pistas o niveles de calidad de tipo de vídeo. |
| FirstDuration | string | Duración del fragmento de datos con la primera marca de tiempo. |
| SecondTimestamp | string  | Marca de tiempo recibida para algún otro nivel de pista o calidad del tipo de vídeo. |
| SecondDuration | string | Duración del fragmento de datos con la segunda marca de tiempo. |

## <a name="liveeventingestheartbeat"></a>LiveEventIngestHeartbeat

En el ejemplo siguiente, se muestra el esquema de un evento **LiveEventIngestHeartbeat**: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventIngestHeartbeat",
    "eventTime": "2018-08-07T23:17:57.4610506",
    "id": "7f450938-491f-41e1-b06f-c6cd3965d786",
    "data": {
      "trackType": "audio",
      "trackName": "audio",
      "bitrate": 160000,
      "incomingBitrate": 155903,
      "lastTimestamp": "15336837535253637",
      "timescale": "10000000",
      "overlapCount": 0,
      "discontinuityCount": 0,
      "nonincreasingCount": 0,
      "unexpectedBitrate": false,
      "state": "Running",
      "healthy": true
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

El objeto data tiene las siguientes propiedades:

| Propiedad | Escriba | DESCRIPCIÓN |
| -------- | ---- | ----------- |
| TrackType | string | Tipo de pista (audio/vídeo). |
| TrackName | string | Nombre de la pista (lo puede proporcionar el codificador o, en el caso de RTMP, el servidor se genera en formato *TrackType_Bitrate*). |
| Bitrate | integer | Velocidad de bits de la pista. |
| IncomingBitrate | integer | Velocidad de bits calculada en función de los fragmentos de datos procedentes del codificador. |
| LastTimestamp | string | Última marca de tiempo recibida para una pista en los últimos 20 segundos. |
| Escala de tiempo | string | Escala temporal en la que se expresan las marcas de tiempo. |
| OverlapCount | integer | Número de fragmentos de datos que tienen marcas de tiempo superpuestas en los últimos 20 segundos. |
| DiscontinuityCount | integer | Número de interrupciones observada en los últimos 20 segundos. |
| NonIncreasingCount | integer | Número de fragmentos de datos con marcas de tiempo en el pasado que se recibieron en los últimos 20 segundos. |
| UnexpectedBitrate | booleano | Si las velocidades de bits esperadas y reales superan el límite permitido en los últimos 20 segundos. Es "true" si, y solo si, la velocidad de bits IncomingBitrate >= 2* O IncomingBitrate <= bitrate/2 O IncomingBitrate = 0. |
| Estado | string | Estado del evento en directo. |
| Healthy | booleano | Indica si la ingesta está en buen estado según los recuentos y las marcas. Estará en buen estado si este tiene el valor "true" y si OverlapCount = 0 && DiscontinuityCount = 0 && NonIncreasingCount = 0 && UnespectedBitrate = false. |

## <a name="liveeventtrackdiscontinuitydetected"></a>LiveEventTrackDiscontinuityDetected

En el ejemplo siguiente, se muestra el esquema de un evento **LiveEventTrackDiscontinuityDetected**: 

```json
[
  {
    "topic": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Media/mediaservices/<account-name>",
    "subject": "liveEvent/mle1",
    "eventType": "Microsoft.Media.LiveEventTrackDiscontinuityDetected",
    "eventTime": "2018-08-07T23:18:06.1270405Z",
    "id": "5f4c510d-5be7-4bef-baf0-64b828be9c9b",
    "data": {
      "trackName": "video",
      "previousTimestamp": "15336837615032322",
      "trackType": "video",
      "bitrate": 2962000,
      "newTimestamp": "15336837619774273",
      "discontinuityGap": "575284",
      "timescale": "10000000"
    },
    "dataVersion": "1.0",
    "metadataVersion": "1"
  }
]
```

El objeto data tiene las siguientes propiedades:

| Propiedad | Escriba | DESCRIPCIÓN |
| -------- | ---- | ----------- |
| TrackType | string | Tipo de pista (audio/vídeo). |
| TrackName | string | Nombre de la pista (lo puede proporcionar el codificador o, en el caso de RTMP, el servidor se genera en formato *TrackType_Bitrate*). |
| Bitrate | integer | Velocidad de bits de la pista. |
| PreviousTimestamp | string | Marca de tiempo del fragmento anterior. |
| NewTimestamp | string | Marca de tiempo del fragmento actual. |
| DiscontinuityGap | string | Intervalo entre las dos marcas de tiempo anteriores. |
| Escala de tiempo | string | Escala temporal en la que se representan tanto la marca de tiempo como el intervalo de discontinuidad. |

## <a name="common-event-properties"></a>Propiedades personalizadas del evento

Un evento tiene los siguientes datos de nivel superior:

| Propiedad | Escriba | DESCRIPCIÓN |
| -------- | ---- | ----------- |
| topic | string | El tema EventGrid. Esta propiedad tiene el id. de recurso de la cuenta de Media Services. |
| subject | string | La ruta de acceso del recurso del canal de Media Services en la cuenta de Media Services. Al concatenar los temas, obtendrá la identificación del recurso para el trabajo. |
| eventType | string | Uno de los tipos de eventos registrados para este origen de eventos. Por ejemplo, "Microsoft.Media.JobStateChange". |
| eventTime | string | La hora de generación del evento en función de la hora UTC del proveedor. |
| id | string | Identificador único para el evento |
| data | objeto | Datos del evento de Media Services. |
| dataVersion | string | Versión del esquema del objeto de datos. El publicador define la versión del esquema. |
| metadataVersion | string | Versión del esquema de los metadatos del evento. Event Grid define el esquema de las propiedades de nivel superior. Event Grid proporciona este valor. |

## <a name="next-steps"></a>Pasos siguientes

[Regístrese para los eventos de cambio de estado del trabajo](job-state-events-cli-how-to.md)