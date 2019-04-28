---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: 80503ad154a9fc4d01614ffd2816f9d5fd497fdb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60309531"
---
<a name="paths"></a>
## <a name="paths"></a>Rutas de acceso

<a name="anomalydetection-post"></a>
### <a name="detect-anomaly-points-for-the-time-series-data-points-requested"></a>Detección de puntos de anomalías para los puntos de datos de serie temporal solicitados
```
POST /anomalydetection
```


#### <a name="parameters"></a>Parámetros

|Type|NOMBRE|DESCRIPCIÓN|Esquema|
|---|---|---|---|
|**Cuerpo**|**body**  <br>*requerido*|Los puntos de datos de serie temporal y el período, si es necesario.|[Solicitud](#request)|


#### <a name="responses"></a>Respuestas

|Código HTTP|DESCRIPCIÓN|Esquema|
|---|---|---|
|**200**|Operación correcta.|< [response](#response) > array|
|**400**|No se puede analizar la solicitud JSON.|Ningún contenido|
|**403**|El servidor no acepta el certificado proporcionado.|Ningún contenido|
|**405**|Método no permitido.|Ningún contenido|
|**500**|Error interno del servidor.|Ningún contenido|


#### <a name="consumes"></a>Consume

* `application/json`


#### <a name="produces"></a>Produce

* `application/json`


#### <a name="tags"></a>Etiquetas

* anomalydetection



