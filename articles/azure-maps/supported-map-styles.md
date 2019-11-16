---
title: Estilos de mapa admitidos en Azure Maps | Microsoft Docs
description: Estilos de mapa admitidos por Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 05/06/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: cd16ee87b213c2bfa4c541c4d3c453a4499855d0
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2019
ms.locfileid: "73887725"
---
# <a name="azure-maps-supported-map-styles"></a>Estilos de mapa admitidos en Azure Maps
Azure Maps es compatible con varios estilos de mapa integrados, tal como se describe a continuación.

## <a name="road"></a>carreteras
Un mapa de **carreteras** es un mapa estándar que muestra las carreteras y características naturales y artificiales con las etiquetas de esas características.

![carreteras](./media/supported-map-styles/road.png)

**API correspondientes:**
* [Imagen de mapa](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Mosaico de mapa](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Control de mapa de SDK web
* Control de mapa de Android

## <a name="blank-and-blank_accessible"></a>blank y blank_accessible

Los estilos de mapa **blank** y **blank_accessible** proporcionan un lienzo en blanco en el que visualizar los datos. El estilo**estilo** seguirá proporcionando actualizaciones del lector de pantalla con detalles de la ubicación en que se encuentra el mapa, aunque no se muestre el mapa base.

> [!Note]
> En el SDK web puede cambiar el color de fondo del mapa estableciendo el estilo CSS `background-color` del elemento DIV del mapa.

**API correspondientes:**
* Control de mapa de SDK web

## <a name="satellite"></a>satélite 
El mapa estilo **satélite** es una combinación de imágenes aéreas o por satélite.

![satélite](./media/supported-map-styles/satellite.png)

**API correspondientes:**
* [Mosaico de satélite](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytilepreview)
* Control de mapa de SDK web
* Control de mapa de Android

## <a name="satellite_road_labels"></a>satellite_road_labels
Este estilo de mapa es un híbrido de carreteras y etiquetas que se superpone a imágenes aéreas o por satélite.

![satellite_road_labels](./media/supported-map-styles/satellite_road_labels.png)

**API correspondientes:**
* Control de mapa de SDK web
* Control de mapa de Android

## <a name="grayscale_dark"></a>grayscale_dark
**escala de grises oscuros** es una versión oscura del estilo de mapa de carreteras.

![gray_scale](./media/supported-map-styles/grayscale_dark.png)

**API correspondientes:**
* Control de mapa de SDK web 
* Control de mapa de Android


## <a name="grayscale_light"></a>grayscale_light
**escala de grises claros** es una versión clara del estilo de mapa de carreteras.

![escala de grises claros](./media/supported-map-styles/grayscale_light.png)

**API correspondientes:**
* Control de mapa de SDK web
* Control de mapa de Android


## <a name="night"></a>noche
**noche** es una versión oscura del estilo de mapa de carreteras con símbolos y carreteras en color.

![noche](./media/supported-map-styles/night.png)

**API correspondientes:**
* Control de mapa de SDK web
* Control de mapa de Android

## <a name="road_shaded_relief"></a>road_shaded_relief
**relieve sombreado del camino** es un estilo principal de Azure Maps completado con los contornos de la tierra.

![relieve sombreado](./media/supported-map-styles/shaded-relief.png)

**API correspondientes:**
* [Mosaico de mapa](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Control de mapa de SDK web
* Control de mapa de Android


## <a name="next-steps"></a>Pasos siguientes

Aprenda a establecer un estilo de mapa en Azure Maps:

> [!div class="nextstepaction"]
> [Elección de un estilo de mapa](https://docs.microsoft.com/azure/azure-maps/choose-map-style)