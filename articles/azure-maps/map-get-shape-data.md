---
title: Obtenga datos de formas del administrador de dibujos en Azure Maps| Microsoft Docs
description: Cómo obtener datos de formas mediante SDK Web de Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 09/04/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: e7706fba1efad1bd0ce7110e129dcf113689af9a
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70309731"
---
# <a name="get-shape-data"></a>Obtención de datos de forma

Este artículo muestra cómo obtener datos de formas que se han dibujado en el mapa mediante la función [administrador de dibujo](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest#getsource--)  **drawingManager.getSource ()** . Puede haber varios escenarios en los que desee extraer datos de GeoJSON de una forma dibujada y utilizarlos en otro lugar.  


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

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Administrador de dibujo](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest)

> [!div class="nextstepaction"]
> [Barra de herramientas de dibujo](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest)
