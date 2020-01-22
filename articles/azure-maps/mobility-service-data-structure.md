---
title: Estructuras de datos del servicio Mobility en Azure Maps | Microsoft Azure Maps
description: En este artículo, aprenderá sobre los campos y estructuras de datos comunes que devuelve el servicio Mobility de Microsoft Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 06/05/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 213910ee2439fa958b9f1d4926883eb8e066ba41
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2020
ms.locfileid: "75910720"
---
# <a name="data-structures-in-azure-maps-mobility-service"></a>Estructuras de datos de Mobility Service en Azure Maps

En este artículo se presenta el concepto de área metropolitana en [Mobility Service de Azure Maps](https://aka.ms/AzureMapsMobilityService) y algunos de los campos habituales que se devuelven mediante los servicios cuando se consultan las paradas y las líneas de transporte público. Se recomienda consultar este artículo antes de empezar a usar las API Mobility Service. A continuación se tratarán estos campos típicos.

## <a name="metro-area"></a>Área metropolitana

Los datos de Mobility Service se dividen en las áreas metropolitanas admitidas. Las áreas metropolitanas no tienen límites urbanos, pueden contener varias ciudades, como una densamente poblada y las ciudades de los alrededores; un país o una región también pueden constituir un área metropolitana. 

`metroID` es el identificador de un área metropolitana que puede usarse para llamar a la [Get Metro Area Info API](https://aka.ms/AzureMapsMobilityMetroAreaInfo) para solicitar tipos de transporte admitidos y otros detalles del área metropolitana, como empresas de transporte y alertas activas. Puede usar Get Metro API de Azure Maps para solicitar las áreas metropolitanas admitidas y sus identificadores. Los identificadores de área metropolitana están sujetos a cambios.

**metroID:** 522   **Nombre:** Seattle-Tacoma-Bellevue

![Área metropolitana de Seattle](./media/mobility-service-data-structure/seattle-metro.png)

## <a name="stop-ids"></a>Identificadores de parada

Se puede hacer referencia a las paradas de transporte con dos tipos de identificador, la [especificación de la fuente de tráfico general (GFTS)](https://gtfs.org/) (denominada stopKey) y el identificador de parada de Azure Maps (denominado stopId). Para hacer referencia a las paradas a lo largo del tiempo, se sugiere usar el identificador de parada de Azure Maps, ya que es más estable y no cambiará en tanto en cuanto la parada física siga existiendo. La especificación GTFS se actualiza con más frecuencia, por ejemplo, en caso de que el proveedor necesite cambiar la GTFS o que se publique una nueva versión, aunque la parada física no cambie.

Para empezar, puede solicitar las paradas próximas mediante [Get Nearby Transit API](https://aka.ms/AzureMapsMobilityNearbyTransit).

## <a name="line-groups-and-lines"></a>Líneas y grupos de líneas

Mobility Service usa un modelo de datos paralelo para las líneas y los grupos de líneas para encajar mejor los cambios derivados del modelo de datos de trayectos y rutas [GTFS](https://gtfs.org/).


### <a name="line-groups"></a>Grupos de líneas

Un grupo de líneas es una entidad que reúne todas las líneas que lógicamente forman parte del mismo grupo. Normalmente, un grupo de líneas contiene dos líneas, una que va desde el punto A al punto B y la otra que vuelve del punto B al punto A (ambas pertenecen a la misma empresa de transporte público y tienen el mismo número de línea). Sin embargo, puede haber casos en los que un grupo de líneas tenga más de dos o solo una línea.


### <a name="lines"></a>Líneas

Como se explicó anteriormente, cada grupo de líneas se compone de un conjunto de líneas. A menudo, cada línea describe una dirección y cada grupo de líneas está compuesto de dos. Sin embargo, en ocasiones un grupo de líneas lo componen más, por ejemplo, si hay una línea que a veces se desvía por un barrio y otras, no y en ambos casos opera con el mismo número de línea; en otros casos, el grupo puede formarlo solo una línea, por ejemplo, una circular con dirección única.

Para empezar, puede solicitar grupos de líneas mediante [Get Transit Line API](https://aka.ms/AzureMapsMobilityTransitLine) y después explorar las líneas.


## <a name="next-steps"></a>Pasos siguientes

Aprenda a solicitar datos de tránsito mediante Mobility Service:

> [!div class="nextstepaction"]
> [Cómo solicitar datos de tránsito](how-to-request-transit-data.md)

Aprenda a solicitar datos en tiempo real mediante Mobility Service:

> [!div class="nextstepaction"]
> [Cómo solicitar datos en tiempo real](how-to-request-real-time-data.md)

Explore la documentación de la API Mobility Service de Azure Maps.

> [!div class="nextstepaction"]
> [Documentación de Mobility Service API](https://aka.ms/AzureMapsMobilityService)
