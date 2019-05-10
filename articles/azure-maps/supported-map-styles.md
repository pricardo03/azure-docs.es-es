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
ms.openlocfilehash: db396ad06bf46cbbaf486696b68393a6a4214c2f
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65230833"
---
# <a name="azure-maps-supported-map-styles"></a>Estilos de mapa admitidos en Azure Maps
Azure Maps es compatible con varios estilos de mapa integrados, tal como se describe a continuación.

## <a name="road"></a>carreteras
Un mapa de **carreteras** es un mapa estándar que muestra las carreteras y características naturales y artificiales con las etiquetas de esas características.

![carreteras](./media/supported-map-styles/road.png)

**API correspondientes:**
* [Imagen de mapa](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Mosaico de mapa](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Control de mapa JS
* Control de mapa de Android

## <a name="satellite"></a>satélite 
El mapa estilo **satélite** es una combinación de imágenes aéreas o por satélite.

![satélite](./media/supported-map-styles/satellite.png)

**API correspondientes:**
* [Mosaico de satélite](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytilepreview)
* Control de mapa JS
* Control de mapa de Android

## <a name="satelliteroadlabels"></a>satellite_road_labels
Este estilo de mapa es un híbrido de carreteras y etiquetas que se superpone a imágenes aéreas o por satélite.

![satellite_road_labels](./media/supported-map-styles/satellite_road_labels.png)

**API correspondientes:**
* Control de mapa JS
* Control de mapa de Android

## <a name="grayscaledark"></a>grayscale_dark
**escala de grises oscuros** es una versión oscura del estilo de mapa de carreteras.

![gray_scale](./media/supported-map-styles/grayscale_dark.png)

**API correspondientes:**
* Control de mapa JS 
* Control de mapa de Android


## <a name="grayscalelight"></a>grayscale_light
**escala de grises luz** es una versión ligera del estilo de mapa de carreteras.

![luz de escala de grises](./media/supported-map-styles/grayscale_light.png)

**API correspondientes:**
* Control de mapa JS
* Control de mapa de Android


## <a name="night"></a>noche
**noche** es una versión oscura del estilo de mapa de carreteras con símbolos y carreteras en color.

![noche](./media/supported-map-styles/night.png)

**API correspondientes:**
* Control de mapa JS
* Control de mapa de Android

## <a name="roadshadedrelief"></a>road_shaded_relief
**relieve sombreado del camino** es un estilo principal de Azure Maps completado con los contornos de la tierra.

![relieve sombreado](./media/supported-map-styles/shaded-relief.png)

**API correspondientes:**
* [Mosaico de mapa](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Control de mapa JS
* Control de mapa de Android
