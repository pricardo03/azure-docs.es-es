---
title: Las estructuras de datos del servicio de movilidad en Azure Maps | Microsoft Docs
description: Estructuras de datos en el servicio de movilidad de Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 06/05/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 29e8a9d7555ca836b6266879f3b3c1e32ffd3980
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66735562"
---
# <a name="data-structures-in-azure-maps-mobility-service"></a>Estructuras de datos en el servicio de movilidad de Azure Maps

En este artículo se presenta el concepto de área de Metro en [Mobility Service de Azure Maps](https://aka.ms/AzureMapsMobilityService) y algunos de los campos comunes devuelven a través de los servicios, cuando se consulta para tránsito pública deja de líneas. Se recomienda pasar a través de este artículo antes de empezar a usar las API de servicio de movilidad. Trataremos estos campos comunes siguientes.

## <a name="metro-area"></a>Área metropolitana de

Datos del servicio de movilidad se dividen en áreas de metro compatibles. Metropolitana no sigue los límites de la ciudad, un área metropolitana de puede contener varias ciudades, por ejemplo, city densamente poblada y sus ciudades adyacentes; y un país o región puede ser un área metropolitana. 

El `metroID` es identificador de un área metropolitana que puede usarse para llamar a la [obtener Metro área información API](https://aka.ms/AzureMapsMobilityMetroAreaInfo) para tipos de solicitud admitida tránsito y detalles adicionales para el área metropolitana como alertas activas y las agencias de tránsito. Puede usar la API de Azure Maps obtener Metro para solicitar el metropolitana admitido y metroIDs. Id. del área metropolitana está sujetos a cambios.

**metroID:** 522 **nombre:** Seattle-Tacoma-Bellevue

![Área metropolitana de Seattle](./media/mobility-service-data-structure/seattle-metro.png)

## <a name="stop-ids"></a>Detener el Id.

Delimitadores de tránsito pueden hacer referencia a dos tipos de identificadores, el [especificación General de fuente de tránsito (GFTS)](https://gtfs.org/) ID (denominadas stopKey) y el Azure Maps detener ID (denominada stopId). Al hacer referencia a se detiene con el tiempo, se recomienda utilizar el Id. de detención de Azure Maps, como este identificador es más estable y probablemente no cambiará siempre y cuando exista la detención física. El Id. de detención GTFS se actualiza con más frecuencia, por ejemplo, en caso de proveedor GTFS necesita cambiarla o GTFS versión nueva, aunque la detención física no tenía ningún cambio.

Para empezar, puede solicitar deja de tránsito cercano mediante el uso de [obtener API de tránsito cercanos](https://aka.ms/AzureMapsMobilityNearbyTransit).

## <a name="line-groups-and-lines"></a>Grupos de líneas y líneas

El servicio de movilidad usa un modelo de datos en paralelo para las líneas y grupos de líneas para abordar mejor los cambios que se hereda de [GTFS](https://gtfs.org/) modelo de datos de carreras y rutas.


### <a name="line-groups"></a>Grupos de líneas

Un grupo de la línea es una entidad, que agrupa todas las líneas que lógicamente forman parte del mismo grupo. Normalmente, un grupo de líneas contendrá dos líneas, uno va desde el punto al B y otros devolver desde el punto B a, ambas pertenecientes a la misma Agencia de transporte público y tener el mismo número de línea. Sin embargo, puede haber casos en que un grupo de líneas tiene más de dos líneas o una sola línea dentro de él.


### <a name="lines"></a>Líneas

Como se explicó anteriormente, cada grupo de líneas se compone de un conjunto de líneas. A menudo describe cada línea de una dirección y cada línea de grupo se compone de dos líneas. Sin embargo, hay casos en qué líneas más componen un grupo de líneas, por ejemplo existe es una línea que a veces desvía a través de un entorno determinado y, a veces no lo hace y opera en ambos casos, en el mismo número de línea, y hay otros casos en los que una línea g rupo se compone de una sola línea, por ejemplo una línea circular con una dirección única.

Para empezar, puede solicitar los grupos de líneas mediante el [obtener API de línea de tránsito](https://aka.ms/AzureMapsMobilityTransitLine) y posterior vaya a las líneas.


## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo solicitar datos de tránsito mediante el servicio de movilidad:

> [!div class="nextstepaction"]
> [Cómo solicitar datos de tránsito](how-to-request-transit-data.md)

Obtenga información sobre cómo solicitar datos en tiempo real mediante el servicio de movilidad:

> [!div class="nextstepaction"]
> [Cómo solicitar datos en tiempo real](how-to-request-real-time-data.md)

Explore la documentación de API del servicio de movilidad de Azure Maps

> [!div class="nextstepaction"]
> [Documentación de API del servicio de movilidad](https://aka.ms/AzureMapsMobilityService)
