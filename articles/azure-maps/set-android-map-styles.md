---
title: Funcionalidades de estilo de mapa en Azure Maps | Microsoft Docs
description: Obtenga información sobre Azure Maps estilo relacionada con las funcionalidades para Android SDK.
author: walsehgal
ms.author: v-musehg
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 3c8c5d4bae16d8e15c8f2c5b1cc8e00eb14e4ce3
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2019
ms.locfileid: "64870978"
---
# <a name="set-map-style-using-azure-maps-android-sdk"></a>Establecer estilo de mapa mediante el SDK de Android de Azure Maps

En este artículo se muestra dos maneras de establecer estilos de mapa mediante el SDK de Android de Azure Maps. Azure Maps tiene seis estilos diferentes mapas para elegir. Para obtener más información sobre los estilos de mapa compatibles, consulte [admiten estilos de mapa en Azure Maps](./supported-map-styles.md).


## <a name="prerequisites"></a>Requisitos previos

Para completar el proceso en este artículo, necesitará instalar [Android SDK de Azure Maps](https://docs.microsoft.com/azure/azure-maps/how-to-use-android-map-control-library) para cargar un mapa.


## <a name="set-map-style-in-the-layout"></a>Establecer estilo de mapa en el diseño

Puede establecer un estilo de mapa en el archivo de diseño para la clase de actividad. Editar **res > Diseño > activity_main.xml**, por lo que se parece a lo siguiente:

```XML
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    >

    <com.microsoft.azure.maps.mapcontrol.MapControl
        android:id="@+id/mapcontrol"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:mapcontrol_centerLat="47.602806"
        app:mapcontrol_centerLng="-122.329330"
        app:mapcontrol_zoom="12"
        app:mapcontrol_style="grayscale_dark"
        />

</FrameLayout>
```

El `mapcontrol_style` atributo anterior establece el estilo de mapa a **grayscale_dark**. 

<center>

![style-grayscale_dark](./media/set-android-map-styles/grayscale-dark.png)</center>

## <a name="set-map-style-in-the-activity-class"></a>Establecer estilo de mapa en la clase de actividad

Estilo de mapa se puede establecer en la clase de actividad. Copie el siguiente fragmento de código en el **onCreate()** método de su `MainActivity.java` clase. Esto establecerá el estilo de mapa **satellite_road_labels**.

```Java
    mapControl.onReady(map -> {
    //Set the camera of the map.
    map.setCamera(center(47.64, -122.33), zoom(14));

    //Set the style of the map.
    map.setStyle(style(MapStyle.SATELLITE));
});
```

<center>

![style-satellite-road-labels](./media/set-android-map-styles/satellite-road-labels.png)</center>