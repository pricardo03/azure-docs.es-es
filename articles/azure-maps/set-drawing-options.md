---
title: Uso del módulo de herramientas de dibujo de Azure Maps | Microsoft Docs
description: Cómo establecer datos de opciones de dibujo mediante Azure Maps SDK Web
author: walsehgal
ms.author: v-musehg
ms.date: 09/04/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 0ac9bc775798a14e6431718bc602d8ff41288c10
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75408150"
---
# <a name="use-the-drawing-tools-module"></a>Uso del módulo de herramientas de dibujo

El SDK web de Azure Maps proporciona un *módulo de herramientas de dibujo*. Este módulo facilita el dibujo y la edición de formas en el mapa mediante un dispositivo de entrada, como un mouse en la pantalla táctil. La clase principal de este módulo es el [administrador de dibujos](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest#setoptions-drawingmanageroptions-) que proporciona todas las funcionalidades necesarias para dibujar y editar formas en el mapa. El administrador de dibujos puede usarse directamente e integrarse con una interfaz de usuario personalizada de la barra de herramientas, o se puede usar la clase [barra de herramientas de dibujo](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest) integrada. 

## <a name="loading-the-drawing-tools-module-in-a-webpage"></a>Carga del módulo de herramientas de dibujo en una página web

1. Cree un archivo HTML e [implemente el mapa como de costumbre](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control).
2. Cargue el módulo de herramientas de dibujo de Azure Maps. Puede realizar ese procedimiento de alguna de estas dos formas:
    - Utilizar la versión de Azure Content Delivery Network hospedada globalmente del módulo de servicios de Azure Maps. Agregue una referencia a la hoja de estilos de JavaScript y CSS en el elemento `<head>` del archivo:

        ```html
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/drawing/0.1/atlas-drawing.min.css" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/javascript/drawing/0.1/atlas-drawing.min.js"></script>
        ```

    - Otra alternativa es cargar localmente el módulo de herramientas de dibujo para el código fuente del SDK web de Azure Maps mediante el paquete npm [azure-maps-rest](https://www.npmjs.com/package/azure-maps-drawing-tools) y, luego, hospedarlo con la aplicación. Este paquete también incluye las definiciones de TypeScript. Use este comando:
    
        > **npm install azure-maps-drawing-tools**
    
        Luego, agregue una referencia a la hoja de estilos de JavaScript y CSS en el elemento `<head>` del archivo:

         ```html
        <link rel="stylesheet" href="node_modules/azure-maps-drawing-tools/dist/atlas-drawing.min.css" type="text/css" />
        <script src="node_modules/azure-maps-drawing-tools/dist/atlas-drawing.min.js"></script>
         ```

## <a name="use-the-drawing-manager-directly"></a>Uso del administrador de dibujos directamente

Ahora que el módulo de herramientas de dibujo se ha cargado en la aplicación, puede usar el [administrador de dibujo](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest#setoptions-drawingmanageroptions-) para habilitar funcionalidades de dibujo y edición dentro del mapa. Puede especificar opciones para el administrador de dibujo al crear una instancia de él, o bien usar la función `drawingManager.setOptions()`.

### <a name="set-the-drawing-mode"></a>Establecimiento del modo de dibujo

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


### <a name="set-the-interaction-type"></a>Establecimiento del tipo de interacción

El administrador de dibujo admite tres formas diferentes de interactuar con el mapa para dibujar formas.

* `click`: se agregan coordenadas cuando se hace clic en el mouse o en la entrada táctil.
* `freehand `: se agregan coordenadas cuando se arrastra el mouse o la entrada táctil en el mapa. 
* `hybrid`: se agregan coordenadas cuando se hace clic o se arrastra el mouse o la entrada táctil.

El código siguiente habilita el modo de dibujo de polígonos y establece el tipo de interacción de dibujo que debe seguir el administrador de dibujo en `freehand`. 

```Javascript
//Create an instance of the drawing manager and set drawing mode.
drawingManager = new atlas.drawing.DrawingManager(map,{
    mode: "draw-polygon",
    interactionType: "freehand"
});
```

A continuación se muestra el código de ejemplo que implementa la funcionalidad que le permite dibujar libremente en el mapa un polígono, mientras mantiene presionado el botón izquierdo del mouse y arrastra. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Dibujo a mano alzada" src="//codepen.io/azuremaps/embed/ZEzKoaj/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/ZEzKoaj/'>Dibujo a mano alzada</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>


### <a name="customizing-drawing-options"></a>Personalizar las opciones de dibujo

En los ejemplos anteriores se ha mostrado cómo personalizar las opciones de dibujo al crear una instancia del administrador de dibujos. También puede establecer las opciones del administrador de dibujo mediante la función `drawingManager.setOptions()`. A continuación se muestra una herramienta para probar la personalización de todas las opciones del administrador de dibujos mediante la función setOptions.

<br/>

<iframe height="685" title="Personalizar el administrador de dibujos" src="//codepen.io/azuremaps/embed/LYPyrxR/?height=600&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true" style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/LYPyrxR/'>Obtención de datos de forma</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="next-steps"></a>Pasos siguientes

Aprenda a usar características adicionales del módulo de herramientas de dibujo:

> [!div class="nextstepaction"]
> [Incorporación de una barra de herramientas de dibujo](map-add-drawing-toolbar.md)

> [!div class="nextstepaction"]
> [Obtención de datos de forma](map-get-shape-data.md)

> [!div class="nextstepaction"]
> [Reacción a eventos de dibujo](drawing-tools-events.md)

> [!div class="nextstepaction"]
> [Tipos de interacción y métodos abreviados de teclado](drawing-tools-interactions-keyboard-shortcuts.md)

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Administrador de dibujo](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest)

> [!div class="nextstepaction"]
> [Barra de herramientas de dibujo](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest)
