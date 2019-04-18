---
title: Escenarios de agregación de perímetro y geoespacial con Azure Stream Analytics
description: En este artículo se describe cómo usar Azure Stream Analytics para agregación geoespaciales y el perímetro.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/02/2019
ms.openlocfilehash: cc301855e4cdcb8eb687e753835577399cfe72b0
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "58807171"
---
# <a name="geofencing-and-geospatial-aggregation-scenarios-with-azure-stream-analytics"></a>Escenarios de agregación de perímetro y geoespacial con Azure Stream Analytics

Con las funciones geoespaciales integradas, puede usar Azure Stream Analytics para compilar aplicaciones para escenarios como la administración de flotas, andar compartir, automóviles conectados y seguimiento de activos.

## <a name="geofencing"></a>Geofencing

Azure Stream Analytics admite los cálculos de geovallas en tiempo real de baja latencia en la nube y en el tiempo de ejecución de IoT Edge.

### <a name="geofencing-scenario"></a>Escenario de perímetro

Una empresa de fabricación debe realizar un seguimiento de activos en sus edificios. Están equipados con un GPS todos los dispositivos y desean recibir notificaciones si un dispositivo deja un área concreta.

En este ejemplo utiliza los datos de referencia tienen la información de la geovalla para los edificios y los dispositivos que se permiten en cada uno de los edificios. Recuerde que los datos de referencia podrían ser estáticos o lentos cambiar. Datos de referencia estáticos se usan para este escenario. Un flujo de datos emite continuamente el identificador de dispositivo y su posición actual.

### <a name="define-geofences-in-reference-data"></a>Definir las geovallas en datos de referencia

Un geovalla se puede definir mediante un objeto de GeoJSON. Para los trabajos con compatibilidad 1.2 y versiones posteriores, las geovallas también se pueden definir mediante Well Known Text (WKT) como `NVARCHAR(MAX)`. WKT es una Open Geospatial Consortium (OGC) estándar que se utiliza para representar datos espaciales en formato de texto.

Las funciones geoespaciales integradas pueden usar las geovallas definidas para averiguar si un elemento está dentro o fuera de un polígono geovalla específico.

En la tabla siguiente es un ejemplo de datos de referencia de la geovalla que podrían almacenarse en Azure blob storage o una tabla de SQL Azure. Todos los sitios se representan mediante un polígono geoespacial, y todos los dispositivos se asocia con un identificador de sitio permitido.

|SiteID|SiteName|Geovalla|AllowedDeviceID|
|------|--------|--------|---------------|
|1|"Building 41 de Redmond"|"POLYGON ((-122.1337357922017 47.63782998329432,-122.13373042778369 47.637634793257305,-122.13346757130023 47.637642022530954,-122.13348902897235 47.637508280806806,-122.13361777500506 47.637508280806806,-122.13361241058703 47.63732393354484,-122.13265754417773 47.63730947490855,-122.13266290859576 47.637519124743164,-122.13302232460376 47.637515510097955,-122.13301696018573 47.63764925180358,-122.13272728161212 47.63764925180358,-122.13274873928424 47.63784082716388,-122.13373579220172 47.63782998329432)) "|"B"|
|2|"Redmond edificio 40"|"POLYGON ((-122.1336154507967 47.6366745947009,-122.13361008637867 47.636483015064535,-122.13349206918201 47.636479400347675,-122.13349743360004 47.63636372927573,-122.13372810357532 47.63636372927573,-122.13373346799335 47.63617576323771,-122.13263912671528 47.63616491902258,-122.13264985555134 47.63635649982525,-122.13304682248554 47.636367344000604,-122.13305218690357 47.63650831807564,-122.13276250832996 47.636497473929516,-122.13277323716602 47.63668543881025,-122.1336154507967 47.6366745947009)) "|"A"|
|3|"Building Redmond 22"|"POLYGON ((-122.13611660248233 47.63758544698554,-122.13635263687564 47.6374083293018,-122.13622389084293 47.63733603619712,-122.13622389084293 47.63717699101473,-122.13581619507266 47.63692757827657,-122.13559625393344 47.637046862778135,-122.13569281345798 47.637144458985965,-122.13570890671207 47.637314348246214,-122.13611660248233 47.63758544698554)) "|"C"|

### <a name="generate-alerts-with-geofence"></a>Genere alertas con geovalla

Los dispositivos pueden emitir su identificador y la ubicación de cada minuto a través de una secuencia denominada `DeviceStreamInput`. En la tabla siguiente es una secuencia de entrada.

|Id. de dispositivo|GeoPosition|
|--------|-----------|
|"A"|"POINT(-122.13292341559497 47.636318374032726)"|
|"B"|"POINT(-122.13338475554553 47.63743531308874)"|
|"C"|"POINT(-122.13354001095752 47.63627622505007)"|

Puede escribir una consulta que combina la transmisión del dispositivo con los datos de referencia de la geovalla y genera una alerta cada vez que un dispositivo fuera de un edificio permitido.

```SQL
SELECT DeviceStreamInput.DeviceID, SiteReferenceInput.SiteID, SiteReferenceInput.SiteName 
INTO Output
FROM DeviceStreamInput 
JOIN SiteReferenceInput
ON st_within(DeviceStreamInput.GeoPosition, SiteReferenceInput.Geofence) = 0
WHERE DeviceStreamInput.DeviceID = SiteReferenceInput.AllowedDeviceID
```

La siguiente imagen representa el geovallas. Puede ver que los dispositivos están de acuerdo con los datos de flujo de entrada.

![Geovallas de creación](./media/geospatial-scenarios/building-geofences.png)

Dispositivo "C" se encuentra dentro de la creación de identificador 2, lo cual no está permitido según los datos de referencia. Este dispositivo debe estar dentro de la creación de Id. 3. Ejecutar este trabajo generará una alerta para esta infracción específica.

### <a name="site-with-multiple-allowed-devices"></a>Sitios con varios dispositivos permitidos

Si un sitio permite que varios dispositivos, se puede definir una matriz de identificadores de dispositivo en `AllowedDeviceID` y se puede usar una función definida por el usuario en el `WHERE` cláusula para comprobar si el identificador de dispositivo de la secuencia coincide con cualquier identificador del dispositivo en esa lista. Para obtener más información, consulte el [UDF de Javascript](stream-analytics-javascript-user-defined-functions.md) tutorial para los trabajos en la nube y el [ C# UDF](stream-analytics-edge-csharp-udf.md) tutorial para trabajos de edge.

## <a name="geospatial-aggregation"></a>Agregación de geoespaciales

Azure Stream Analytics admite la agregación de baja latencia geoespaciales en tiempo real en la nube y en el tiempo de ejecución de IoT Edge.

### <a name="geospatial-aggregation-scenario"></a>Escenario de agregación de geoespaciales

Una empresa de cab desea compilar una aplicación en tiempo real para guiar sus controladores de cab buscando andar hacia las áreas de las ciudades experimentando una mayor demanda.

La empresa almacena las regiones de lógicas de la ciudad como datos de referencia. Cada región se define mediante un Id. de región y RegionName Geovalla.

### <a name="define-the-geofences"></a>Definir el geovallas

En la tabla siguiente es un ejemplo de datos de referencia de la geovalla que podrían almacenarse en Azure blob storage o una tabla de SQL Azure. Todas las regiones se representan mediante un polígono geoespaciales, que se usa para correlacionar con las solicitudes procedentes de transmisión de datos.

Estos elementos Polygon es solo para referencia y no representa separaciones lógico o físico de ciudades reales.

|RegionID|RegionName|Geovalla|
|--------|----------|--------|
|1|"SoHo"|"POLYGON ((-74.00279525078275 40.72833625216264,-74.00547745979765 40.721929158663244,-74.00125029839018 40.71893680218994,-73.9957785919998 40.72521409075776,-73.9972377137039 40.72557184584898,-74.00279525078275 40.72833625216264) )"|
|2|"Chinatown"|"POLYGON ((-73.99712367114876 40.71281582267133,-73.9901070123658 40.71336881907936,-73.99023575839851 40.71452359088633,-73.98976368961189 40.71554823078944,-73.99551434573982 40.717337246783735,-73.99480624255989 40.718491949759304,-73.99652285632942 40.719109951574,-73.99776740131233 40.7168005470334,-73.99903340396736 40.71727219249899,-74.00193018970344 40.71938642421256,-74.00409741458748 40.71688186545551,-74.00051398334358 40.71517415773184,-74.0004281526551 40.714377212470005,-73.99849696216438 40.713450141693166,-73.99748845157478 40.71405192594819,-73.99712367114876 40.71281582267133)) "|
|3|"Tribeca"|"POLYGON ((-74.01091641815208 40.72583120006787,-74.01338405044578 40.71436586362705,-74.01370591552757 40.713617702123415,-74.00862044723533 40.711308107057235,-74.00194711120628 40.7194238654018,-74.01091641815208 40.72583120006787))"|

### <a name="aggregate-data-over-a-window-of-time"></a>Agregar datos a través de una ventana de tiempo

La siguiente tabla contiene datos de streaming de "viajes".

|Identificador de usuario|FromLocation|ToLocation|TripRequestedTime|
|------|------------|----------|-----------------|
|"A"|"POINT(-74.00726861389182 40.71610611981975)"|"POINT(-73.98615095917779 40.703107386025835)"|"2019-03-12T07:00:00Z"|
|"B"|"POINT(-74.00249841021645 40.723827238895666)"|"POINT(-74.01160699942085 40.71378884930115)"|"2019-03-12T07:01:00Z"|
|"C"|"POINT(-73.99680120565864 40.716439898624024)"|"POINT(-73.98289663412544 40.72582343969828)"|"2019-03-12T07:02:00Z"|
|"D"|"POINT(-74.00741090068288 40.71615626755086)"|"POINT(-73.97999843120539 40.73477895807408)"|"2019-03-12T07:03:00Z"|

La siguiente consulta combina la transmisión del dispositivo con los datos de referencia de la geovalla y calcula el número de solicitudes por región en un período de tiempo de 15 minutos cada minuto.

```SQL
SELECT count(*) as NumberOfRequests, RegionsRefDataInput.RegionName 
FROM UserRequestStreamDataInput
JOIN RegionsRefDataInput 
ON st_within(UserRequestStreamDataInput.FromLocation, RegionsRefDataInput.Geofence) = 1
GROUP BY RegionsRefDataInput.RegionName, hoppingwindow(minute, 1, 15)
```

Esta consulta genera un recuento de solicitudes cada minuto durante los últimos 15 minutos por cada región dentro de la ciudad. Esta información se puede mostrar fácilmente mediante el panel de Power BI o puede se difunden a todos los controladores como mensajes de texto SMS mediante la integración con servicios como Azure functions.

La imagen siguiente ilustra el resultado de la consulta en el panel de Power BI. 

![Salida de resultados en el panel de Power BI](./media/geospatial-scenarios/power-bi-output.png)


## <a name="next-steps"></a>Pasos siguientes

* [Introducción a las funciones geoespaciales de Stream Analytics](stream-analytics-geospatial-functions.md)
* [Funciones geoespaciales (Azure Stream Analytics)](https://docs.microsoft.com/stream-analytics-query/geospatial-functions)
