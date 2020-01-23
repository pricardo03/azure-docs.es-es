---
title: Obtención de datos a partir de formas en un mapa | Microsoft Azure Maps
description: En este artículo, obtendrá información sobre cómo obtener datos de formas dibujados en un mapa mediante el SDK web de Microsoft Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 09/04/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: ca1b9854b118d359167873313f1fcb11bfecee52
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2020
ms.locfileid: "75911009"
---
# <a name="get-shape-data"></a>Obtención de datos de forma

En este artículo se muestra cómo obtener datos de formas que se han dibujado en el mapa mediante la función [administrador de dibujo](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest#getsource--) **drawingManager.getSource()** . Puede haber varios escenarios en los que desee extraer datos de GeoJSON de una forma dibujada y utilizarlos en otro lugar.  


## <a name="get-data-from-drawn-shape"></a>Obtenga datos de la forma dibujada

La siguiente función obtiene los datos de origen de la forma dibujada y los muestra en la pantalla. 

```Javascript
function getDrawnShapes() {
    var source = drawingManager.getSource();

    document.getElementById('CodeOutput').value = JSON.stringify(source.toJson(), null, '    ');
}
```

A continuación se muestra el ejemplo de código de ejecución completo, donde puede dibujar una forma para probar la funcionalidad:

<br/>

<iframe height="686" title="Obtención de datos de forma" src="//codepen.io/azuremaps/embed/xxKgBVz/?height=265&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true" style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/xxKgBVz/'>Obtención de datos de forma</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="next-steps"></a>Pasos siguientes

Aprenda a usar características adicionales del módulo de herramientas de dibujo:

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
