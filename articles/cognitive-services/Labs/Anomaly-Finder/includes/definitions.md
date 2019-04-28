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
ms.openlocfilehash: 5ad589c4adb60369f81979e214935f73d9eb0755
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60309540"
---
<a name="definitions"></a>
## <a name="definitions"></a>Definiciones

<a name="point"></a>
### <a name="point"></a>Punto

|NOMBRE|DESCRIPCIÓN|Esquema|
|---|---|---|
|**Timestamp**  <br>*opcional*|La marca de tiempo del punto de datos. Asegúrese de que coincide con la medianoche y que usa una cadena de tiempo de fecha UTC; por ejemplo, 2017-08-01T00:00:00Z.|string (date-time)|
|**Valor**  <br>*opcional*|Un valor de medida de datos.|número (doble)|


<a name="request"></a>
### <a name="request"></a>Solicitud

|NOMBRE|DESCRIPCIÓN|Esquema|
|---|---|---|
|**Período**  <br>*opcional*|El período de los puntos de datos. Si el valor es null o no está presente, la API determinará el período automáticamente.|número (doble)|
|**Puntos**  <br>*opcional*|Los puntos de datos de serie temporal. Los datos se deben ordenar de forma ascendente en función de su marca de tiempo para coincidir con el resultado de la anomalía. Si los datos no están ordenados correctamente o si la marca de tiempo está duplicada, la API detectará los puntos de anomalía correctamente, pero podrían no establecerse coincidencias correctas de los puntos devueltos con la entrada. En este caso, se agrega un mensaje de advertencia en la respuesta.|< [punto](#point) > matriz|


<a name="response"></a>
### <a name="response"></a>Response

|NOMBRE|DESCRIPCIÓN|Esquema|
|---|---|---|
|**ExpectedValues**  <br>*opcional*|El valor de predicción del modelo basado en el aprendizaje. Si los puntos de datos de entrada se ordenan de manera ascendente en función de su marca temporal, el índice de la matriz puede utilizarse para asignar el valor esperado y el valor original.|< número (doble) > matriz|
|**IsAnomaly**  <br>*opcional*|El resultado que indica si los puntos de datos son anomalías o no en ambas direcciones (picos o DIP). True significa que el punto es anómalo, mientras que false significa que el punto no presenta anomalías. Si los puntos de datos de entrada se ordenan de manera ascendente en función de su marca temporal, el índice de la matriz puede utilizarse para asignar el valor esperado y el valor original.|< booleano > matriz|
|**IsAnomaly_Neg**  <br>*opcional*|El resultado que indica si los puntos de datos son anomalías en una dirección negativa (DIP). True significa que la dirección de la anomalía es negativa. Si los puntos de datos de entrada se ordenan de manera ascendente en función de su marca temporal, el índice de la matriz puede utilizarse para asignar el valor esperado y el valor original.|< booleano > matriz|
|**IsAnomaly_Pos**  <br>*opcional*|El resultado que indica si los puntos de datos son anomalías en una dirección positiva (picos). True significa que la dirección de la anomalía es positiva. Si los puntos de datos de entrada se ordenan de manera ascendente en función de su marca temporal, el índice de la matriz puede utilizarse para asignar el valor esperado y el valor original.|< booleano > matriz|
|**LowerMargin**  <br>*opcional*|(ExpectedValue - LowerMargin) determina el límite inferior en que el punto de datos aún se considera normal. Si los puntos de datos de entrada se ordenan de manera ascendente en función de su marca temporal, el índice de la matriz puede utilizarse para asignar el valor esperado y el valor original.|< número (doble) > matriz|
|**Período**  <br>*opcional*|El período que la API usó para detectar los puntos de anomalías.|número (flotante)|
|**UpperMargin**  <br>*opcional*|La suma de ExpectedValue y UpperMargin determina el límite superior en que el punto de datos aún se considera normal. Si los puntos de datos de entrada se ordenan de manera ascendente en función de su marca temporal, el índice de la matriz puede utilizarse para asignar el valor esperado y el valor original.|< número (doble) > matriz|
|**WarningText**  <br>*opcional*|Si los puntos de datos de entrada proporcionados no siguen la regla que la API requiere y la API aún puede detectar los datos, esta analizará los datos y anexará la información de advertencia en este campo.|string|



