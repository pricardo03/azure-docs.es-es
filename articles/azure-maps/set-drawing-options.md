---
title: Establecer opciones de dibujo en Azure Maps | Microsoft Docs
description: Cómo establecer datos de opciones de dibujo mediante Azure Maps SDK Web
author: walsehgal
ms.author: v-musehg
ms.date: 09/04/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: d85525274db7818737b62ad4e9ea2026b8aef3b2
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70309716"
---
# <a name="set-drawing-options"></a>Establecimiento de las opciones de dibujo

Este artículo muestra cómo las diferentes opciones del [administrador de dibujos](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest#setoptions-drawingmanageroptions-) cambian la experiencia del usuario. Puede especificar opciones para el administrador de dibujo al crear una instancia de ella, o bien usar la función **drawingManager.setOptions()** para establecer opciones.


## <a name="set-drawing-mode"></a>Establecimiento del modo de dibujo

En el código siguiente crea una instancia del administrador de dibujos y establece la opción **modo** de dibujo. 

```Javascript
//Create an instance of the drawing manager and set drawing mode.
drawingManager = new atlas.drawing.DrawingManager(map,{
    mode: "draw-polygon"
});
```

El código siguiente es un ejemplo de ejecución completo de cómo establecer un modo de dibujo del administrador de dibujos. Haga clic en el mapa para empezar a dibujar un polígono.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Dibuje un polígono" src="//codepen.io/azuremaps/embed/YzKVKRa/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/YzKVKRa/'>Dibujar un polígono</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="set-interaction-type"></a>Establezca el tipo de interacción

El siguiente código establece el tipo de interacción de dibujo al que debe adherirse el administrador de dibujo. 

```Javascript
//Create an instance of the drawing manager and set drawing mode.
drawingManager = new atlas.drawing.DrawingManager(map,{
    mode: "draw-polygon",
    interactionType: "freehand"
});
```

A continuación se muestra el ejemplo de código que implementa la funcionalidad que le permite dibujar libremente el mapa, mientras mantiene presionado el botón izquierdo del mouse y lo arrastra. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Dibujo a mano alzada" src="//codepen.io/azuremaps/embed/ZEzKoaj/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/ZEzKoaj/'>Dibujo a mano alzada</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="customizing-drawing-options"></a>Personalizar las opciones de dibujo

En los ejemplos anteriores se ha mostrado cómo personalizar las opciones de dibujo al crear una instancia del administrador de dibujos. También puede establecer las opciones del administrador de dibujos con la función **drawingManager.setOptions ()** . A continuación se muestra una herramienta para probar la personalización de todas las opciones del administrador de dibujos mediante la función setOptions.

<br/>

<iframe height="685" title="Personalizar el administrador de dibujos" src="//codepen.io/azuremaps/embed/LYPyrxR/?height=600&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true" style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/LYPyrxR/'>Obtención de datos de forma</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Administrador de dibujo](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest)

> [!div class="nextstepaction"]
> [Barra de herramientas de dibujo](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest)
