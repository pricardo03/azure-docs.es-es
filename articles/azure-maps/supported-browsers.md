---
title: SDK Web admite los exploradores - Azure Maps | Microsoft Docs
description: Obtenga información sobre los exploradores admitidos para el SDK Web de Azure Maps
author: rbrundritt
ms.author: richbrun
ms.date: 03/25/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendleton
ms.openlocfilehash: bc876fbf0eb15f887d57d4ddcca2301ef7233afa
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2019
ms.locfileid: "58577346"
---
# <a name="web-sdk-supported-browsers"></a>Exploradores compatibles con el SDK Web

El SDK de Web de Azure Maps proporciona una función auxiliar [atlas.isSupported](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas?view=azure-iot-typescript-latest#issupported-boolean-) detectar si un explorador web tiene el mínimo necesario WebGL características para admitir la carga y la representación del control de mapa. 

```
if(!atlas.isSupported()) {
    alert('Your browser is not supported by Azure Maps');
} else if(!atlas.isSupported(true)) {
    alert('Your browser is supported by Azure Maps, but may have major performance caveats.');
} else {
    //Your browser is supported. Add your map code here.
}
```

## <a name="desktop"></a>Escritorio

El SDK Web de mapas de Azure admite los siguientes exploradores de escritorio.

- La versión anterior y actual de Microsoft Edge 
- La versión actual y anterior de Chrome 
- La versión anterior y actual de Firefox 
- La versión anterior y actual de Safari (Mac OS X) 

Vea también [dirigidas a los exploradores antiguos](#Target-Legacy-Browsers).

## <a name="mobile"></a>Móvil

El SDK Web de mapas de Azure admite los siguientes exploradores móviles.

-  Android
    * Versión actual de Chrome en Android 6.0 y versiones posteriores
    * Chrome WebView en Android 6.0 y versiones posteriores
- iOS
    * Mobile Safari en la versión principal anterior y actual de iOS
    * UIWebView y WKWebView en la versión principal anterior y actual de iOS
    * Versión actual de Chrome para iOS

> [!TIP]
> Si va a insertar un mapa dentro de una aplicación móvil con un control WebView, quizás prefiera usar la [paquete npm de Azure Maps Web SDK](https://www.npmjs.com/package/azure-maps-control) en lugar de hacer referencia a la versión alojada en CDN del SDK. Esto reducirá el tiempo de carga como el SDK se esté ya en el dispositivo del usuario y no deben descargarse en tiempo de ejecución.

## <a name="nodejs"></a>Node.js

También se admiten los siguientes módulos de SDK Web de Node.js:

- Módulo de servicios ([documentación](how-to-use-services-module.md) | [módulo npm de](https://www.npmjs.com/package/azure-maps-rest))

## <a name="Target-Legacy-Browsers"></a>Exploradores de destino heredados

Si necesita tener como destino de los exploradores más antiguos que tienen compatibilidad para WebGL limitada o que no admitan, se recomienda utilizar los servicios de Azure Maps en combinación con un control de mapa de código abierto, como [folleto](https://leafletjs.com/). 


<iframe height="500" style="width: 100%;" scrolling="no" title="Azure Maps + folleto" src="//codepen.io/azuremaps/embed/GeLgyx/?height=500&theme-id=0&default-tab=html,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/GeLgyx/'>Azure Maps + folleto</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="next-steps"></a>Pasos siguientes

Más información sobre el SDK de Web de Azure Maps.

> [!div class="nextstepaction"]
> [Control de mapa](how-to-use-map-control.md)

> [!div class="nextstepaction"]
> [Módulo de servicios](how-to-use-services-module.md)
