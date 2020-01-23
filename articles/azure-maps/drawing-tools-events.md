---
title: Adición de una barra de herramientas de dibujo a un mapa | Microsoft Azure Maps
description: En este artículo, obtendrá información sobre cómo agregar una barra de herramientas de dibujo a un mapa mediante el SDK web de Microsoft Azure Maps
author: rbrundritt
ms.author: richbrun
ms.date: 12/05/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: fd235f3f39d67f86c8387add79ca0dbf17dc5906
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2020
ms.locfileid: "75911680"
---
# <a name="drawing-tool-events"></a>Eventos de la herramienta de dibujo

Cuando se usan herramientas de dibujo en un mapa, suele ser útil reaccionar ante ciertos eventos a medida que el usuario dibuja en el mapa. En la siguiente tabla se muestran todos los eventos que la clase `DrawingManager` admite.

| Evento | Descripción |
|-------|-------------|
| `drawingchanged` | Se activa cuando se ha agregado o cambiado alguna coordenada de una forma. | 
| `drawingchanging` | Se activa cuando se muestra alguna coordenada de vista previa de una forma. Por ejemplo, se activará varias veces mientras se arrastra una coordenada. | 
| `drawingcomplete` | Se activa cuando una forma ha terminado de dibujarse o se saca del modo de edición. |
| `drawingmodechanged` | Se activa cuando ha cambiado el modo de dibujo. El nuevo modo de dibujo se pasa al controlador de eventos. |
| `drawingstarted` | Se activa cuando el usuario comienza a dibujar una forma o coloca una forma en modo de edición.  |

En el código siguiente se muestra cómo funcionan los eventos del módulo Herramientas de dibujo. Dibuje formas en el mapa y observe cómo se activan los eventos.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Eventos de las herramientas de dibujo" src="https://codepen.io/azuremaps/embed/dyPMRWo?height=500&theme-id=default&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/dyPMRWo'>Eventos de las herramientas de dibujo</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

<br/>

## <a name="examples"></a>Ejemplos

A continuación, se muestran ejemplos de algunos escenarios comunes que usan los eventos de las herramientas de dibujo.

### <a name="select-points-in-polygon-area"></a>Selección de puntos en el área de polígono

En el código siguiente se muestra cómo supervisar el dibujo de formas que representan áreas de polígono (polígonos, rectángulos y círculos) y determinar qué puntos de datos del mapa se encuentran dentro del área dibujada. El evento `drawingcomplete` se usa para desencadenar la lógica de selección. En la lógica de selección, todos los puntos de datos del mapa se recorren en bucle y se prueba si presentan una intersección con el área de polígono de la forma dibujada. En este ejemplo se usa la biblioteca de código abierto [Turf.js](https://turfjs.org/) para realizar un cálculo de intersección espacial.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Selección de datos en el área de polígono dibujada" src="https://codepen.io/azuremaps/embed/XWJdeja?height=500&theme-id=default&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/XWJdeja'>Selección de datos en el área de polígono dibujada</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

<br/>

### <a name="draw-and-search-in-polygon-area"></a>Dibujo y búsqueda en el área de polígono

En el código siguiente se muestra cómo realizar una búsqueda de puntos de interés dentro de un área de forma después de que el usuario haya terminado de dibujar la forma. El evento `drawingcomplete` se usa para desencadenar la lógica de búsqueda. Si el usuario dibuja un rectángulo o polígono, se realiza una búsqueda dentro de la geometría. Si se dibuja un círculo, se usa el radio y la posición central para realizar la búsqueda de un punto de interés. El evento `drawingmodechanged` se usa para determinar cuándo el usuario cambia a un modo de dibujo y borra el lienzo de dibujo.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Dibujo y búsqueda en el área de polígono" src="https://codepen.io/azuremaps/embed/eYmZGNv?height=500&theme-id=default&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/eYmZGNv'>Dibujo y búsqueda en el área de polígono</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

<br/>

### <a name="create-a-measuring-tool"></a>Creación de una herramienta de medición

En el código siguiente se muestra cómo se pueden usar los eventos de dibujo para crear una herramienta de medición. El elemento `drawingchanging` se usa para supervisar la forma a medida que se dibuja. A medida que el usuario mueve el mouse, se calculan las dimensiones de la forma. El evento `drawingcomplete` se usa para realizar un cálculo final de la forma después de que se haya dibujado. El evento `drawingmodechanged` se usa para determinar cuándo el usuario cambia a un modo de dibujo y borra el lienzo de dibujo y la información de medición anterior.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Herramienta de medición" src="https://codepen.io/azuremaps/embed/RwNaZXe?height=500&theme-id=default&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/RwNaZXe'>Herramienta de medición</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

<br/>

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo usar características adicionales del módulo de herramientas de dibujo:

> [!div class="nextstepaction"]
> [Obtención de datos de forma](map-get-shape-data.md)

> [!div class="nextstepaction"]
> [Tipos de interacción y métodos abreviados de teclado](drawing-tools-interactions-keyboard-shortcuts.md)

Más información sobre el módulo de servicios:

> [!div class="nextstepaction"]
> [Módulo de servicios](how-to-use-services-module.md)

Consulte más ejemplos de código:

> [!div class="nextstepaction"]
> [Página de ejemplos de código](https://aka.ms/AzureMapsSamples)
