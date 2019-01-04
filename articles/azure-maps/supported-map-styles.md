---
title: Estilos de mapa admitidos en Azure Maps | Microsoft Docs
description: Estilos de mapa admitidos por Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 10/02/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 2f426ab85d9ab15dd080a487337b8d48358ed040
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "52888467"
---
# <a name="azure-maps-supported-map-styles"></a>Estilos de mapa admitidos en Azure Maps
Azure Maps es compatible con varios estilos de mapa integrados diferentes, tal como se describe a continuación.

## <a name="road"></a>carreteras
Un mapa de **carreteras** es un mapa estándar que muestra las carreteras y características naturales y artificiales con las etiquetas de esas características.

![carreteras](./media/supported-map-styles/road.png)

**API correspondientes:**
* [Imagen de mapa](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Mosaico de mapa](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* Control de mapa JS

## <a name="satellite"></a>satélite 
El mapa estilo **satélite** es una combinación de imágenes aéreas o por satélite.

![satélite](./media/supported-map-styles/satellite.png)

**API correspondientes:**
* [Mosaico de satélite](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytilepreview)
* Control de mapa JS

## <a name="satelliteroadlabels"></a>satellite_road_labels
Este estilo de mapa es un híbrido de carreteras y etiquetas que se superpone a imágenes aéreas o por satélite.

![satellite_road_labels](./media/supported-map-styles/satellite_road_labels.png)

**API correspondientes:**
* Control de mapa JS

## <a name="grayscaledark"></a>grayscale_dark
**escala de grises oscuros** es una versión oscura del estilo de mapa de carreteras.

![gray_scale](./media/supported-map-styles/grayscale_dark.png)

**API correspondientes:**
* Control de mapa JS 

## <a name="night"></a>noche
**noche** es una versión oscura del estilo de mapa de carreteras con símbolos y carreteras en color.

![noche](./media/supported-map-styles/night.PNG)

**API correspondientes:**
* Control de mapa JS 