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
ms.openlocfilehash: 84c5dbcf5073ba8c0ae662af019cde590a9adf10
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64686642"
---
# <a name="web-sdk-supported-browsers"></a>Exploradores admitidos por el SDK web

El SDK de Web de Azure Maps proporciona una función auxiliar llamada [atlas.isSupported](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas?view=azure-iot-typescript-latest#issupported-boolean-). Esta función detecta si un explorador web tiene el conjunto mínimo de características de WebGL necesarias para admitir la carga y la representación del control de mapa. Este es un ejemplo de cómo usar la función:

```
if(!atlas.isSupported()) {
    alert('Your browser is not supported by Azure Maps');
} else if(!atlas.isSupported(true)) {
    alert('Your browser is supported by Azure Maps, but may have major performance caveats.');
} else {
    // Your browser is supported. Add your map code here.
}
```

## <a name="desktop"></a>Escritorio

El SDK Web de mapas de Azure admite los siguientes exploradores de escritorio:

- Microsoft Edge (versión actual y anterior)
- Google Chrome (versión actual y anterior)
- Mozilla Firefox (versión actual y anterior)
- Apple Safari (Mac OS X) (versión actual y anterior)

Vea también [dirigidas a los exploradores antiguos](#Target-Legacy-Browsers) más adelante en este artículo.

## <a name="mobile"></a>Móvil

El SDK Web de mapas de Azure admite los siguientes exploradores móviles:

- Android
  - Versión actual de Chrome en Android 6.0 y versiones posteriores
  - Chrome WebView en Android 6.0 y versiones posteriores
- iOS
  - Mobile Safari en la versión principal anterior y actual de iOS
  - UIWebView y WKWebView en la versión principal anterior y actual de iOS
  - Versión actual de Chrome para iOS

> [!TIP]
> Si vas a incrustar un mapa dentro de una aplicación móvil mediante el uso de un control WebView, es preferible utilizar la [paquete npm de Azure Maps Web SDK](https://www.npmjs.com/package/azure-maps-control) en lugar de hacer referencia a la versión del SDK que se hospeda en Azure Content Delivery Red. Este enfoque reduce el tiempo de carga porque el SDK esté ya en el dispositivo del usuario y no debe descargarse en tiempo de ejecución.

## <a name="nodejs"></a>Node.js

También se admiten los siguientes módulos de SDK Web de Node.js:

- Módulo de servicios ([documentación](how-to-use-services-module.md) | [módulo npm de](https://www.npmjs.com/package/azure-maps-rest))

## <a name="Target-Legacy-Browsers"></a>Exploradores de destino heredados

Puede tener como destino de los exploradores más antiguos que no son compatibles con WebGL o que solo tienen compatibilidad para él limitada. En tales casos, se recomienda que use servicios de Azure Maps junto con un control de mapa de código abierto como [folleto](https://leafletjs.com/). Este es un ejemplo:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Azure Maps + folleto" src="//codepen.io/azuremaps/embed/GeLgyx/?height=500&theme-id=0&default-tab=html,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/GeLgyx/'>Azure Maps + folleto</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="next-steps"></a>Pasos siguientes

Más información sobre el SDK de Web de Azure Maps:

> [!div class="nextstepaction"]
> [Control de mapa](how-to-use-map-control.md)

> [!div class="nextstepaction"]
> [Módulo de servicios](how-to-use-services-module.md)
