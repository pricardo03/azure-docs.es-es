---
title: Funcionalidades de estilo de mapa en Azure Maps | Microsoft Docs
description: Obtenga información sobre las funcionalidades de Azure Maps relacionadas con el estilo.
author: walsehgal
ms.author: v-musehg
ms.date: 10/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: ffed12b9184c7b6a690c30db9826f031fe6c9f9b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60795902"
---
# <a name="choose-a-map-style-in-azure-maps"></a>Elegir un estilo de mapa en Azure Maps

Azure Maps tiene cuatro estilos de mapas entre los que puede elegir. Para obtener más información sobre los estilos de mapa, consulte [Estilos de mapa admitidos en Azure Maps](./supported-map-styles.md). En este artículo se muestra cómo usar las funcionalidades relacionadas con el estilo para establecer un estilo al cargar un mapa, establecer un estilo nuevo y usar el control de selector de estilos.

## <a name="set-style-on-map-load"></a>Establecimiento del estilo al cargar un mapa

<iframe height='500' scrolling='no' title='Establecer el estilo al cargar un mapa' src='//codepen.io/azuremaps/embed/WKOQRq/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/WKOQRq/'>Setting the style on map load</a> (Establecer el estilo al cargar un mapa) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El bloque de código anterior establece la clave de suscripción y crea un objeto de mapa, con el estilo establecido en grayscale_dark. Consulte [Crear un mapa](./map-create.md) para obtener instrucciones sobre cómo crear un mapa.

## <a name="update-the-style"></a>Actualización del estilo

<iframe height='500' scrolling='no' title='Actualizar el estilo' src='//codepen.io/azuremaps/embed/yqXYzY/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/yqXYzY/'>Updating the style</a> (Actualizar el estilo) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El bloque de código anterior establece la clave de suscripción y crea un objeto de mapa sin estilo establecido previamente. Consulte [Crear un mapa](./map-create.md) para obtener instrucciones sobre cómo crear un mapa.

El segundo bloque de código usa el método [setStyle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) del mapa para establecer el estilo de mapa en satélite.

## <a name="add-the-style-picker"></a>Adición del selector de estilos

<iframe height='500' scrolling='no' title='Agregar el selector de estilos' src='//codepen.io/azuremaps/embed/OwgyvG/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/OwgyvG/'>Adding the style picker</a> (Agregar el selector de estilos) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El primer bloque de código del código anterior establece la clave de suscripción y crea un objeto de mapa, con el estilo del mapa establecido previamente en grayscale_dark. Consulte [Crear un mapa](./map-create.md) para obtener instrucciones sobre cómo crear un mapa.

El segundo bloque de código construye un selector de estilo mediante el constructor de atlas [StyleControl](/javascript/api/azure-maps-control/atlas.control.stylecontrol).

Un selector de estilos habilita la selección de estilo para el mapa. El tercer bloque de código agrega el selector de estilos al mapa mediante el método [controls.add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) del mapa. El selector de estilos está en el **agente de escucha de eventos** del mapa para asegurarse de que se carga después de que el mapa se cargue completamente.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Adición de controles a los mapas:

> [!div class="nextstepaction"]
> [Incorporación de controles de mapa](./map-add-controls.md)

Adición de un anclaje de mapa:

> [!div class="nextstepaction"]
> [Adición de un anclaje](./map-add-pin.md)
