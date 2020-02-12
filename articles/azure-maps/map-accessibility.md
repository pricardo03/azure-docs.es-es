---
title: Creación de una aplicación accesible de mapa con Azure Maps| Microsoft Azure Maps
description: En este artículo, obtendrá información sobre cómo crear una aplicación con características de accesibilidad mediante Microsoft Azure Maps.
services: azure-maps
author: rbrundritt
ms.author: richbrun
ms.date: 12/10/2019
ms.topic: conceptual
ms.service: azure-maps
manager: cpendleton
ms.openlocfilehash: e298aad6dc43f85f5b6c344eec56f5d8b37980d4
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2020
ms.locfileid: "76933297"
---
# <a name="building-an-accessible-application"></a>Creación de una aplicación accesible

Más del 20 % de los usuarios de Internet necesitan aplicaciones web accesibles. Por lo tanto, es importante asegurarse de que la aplicación está diseñada de forma que cualquier usuario pueda utilizarla fácilmente. En lugar de pensar en la accesibilidad como un conjunto de tareas que hay que completar, piense en ella como parte de la experiencia global del usuario. Cuanto más accesible sea su aplicación, más personas podrán usarla. 

Cuando se trata de contenido interactivo enriquecido como un mapa, algunas consideraciones de accesibilidad comunes son:
- Ser compatible con lectores de pantalla para los usuarios que tengan dificultades para ver la aplicación web.
- Disponer de varios métodos para interactuar y navegar por la aplicación web, como mouse, función táctil y teclado.
- Asegurarse de que el contraste de color sea tal que los colores no se mezclen entre sí y se vuelvan difíciles de distinguir. 

El SDK web de Azure Maps viene precompilado con muchas características de accesibilidad tales como:
- Descripciones del lector de pantalla cuando se mueve el mapa y cuando el usuario se centra en un control o un elemento emergente.
- Compatibilidad con mouse, entrada táctil y teclado.
- Compatibilidad con el contraste de color accesible en el estilo de mapa de carreteras.

[Aquí](https://cloudblogs.microsoft.com/industry-blog/government/2018/09/11/accessibility-conformance-reports/) se pueden encontrar detalles de cumplimiento de accesibilidad completos para todos los productos de Microsoft. Busque "web de Azure Maps" para buscar el documento específicamente para el SDK web de Azure Maps. 

## <a name="navigating-the-map"></a>Navegación por el mapa
Hay varias maneras de ampliar, desplazar lateralmente, girar e inclinar el mapa. A continuación se detallan todas las formas de navegar por el mapa.

**Acercamiento/alejamiento del mapa**
- Con un mouse, haga doble clic en el mapa para acercarse un nivel.
- Con un mouse, desplácese por la rueda para acercar el mapa.
- Con una pantalla táctil, toque el mapa con dos dedos y acérquelos para alejarse o aléjelos para acercarse.
- Con una pantalla táctil, pulse dos veces el mapa para acercarse un nivel.
- Con el mapa centrado, use el signo más (`+`) o el signo igual* (`=`) para acercarse un nivel.
- Con el mapa centrado, use el signo menos, el guion (`-`) o el carácter de subrayado (`_`) para alejarse un nivel.
- Use el control de zoom con el mouse, la entrada táctil o las teclas de tabulación/ingreso del teclado.
- Mantenga presionado el botón `Shift` y presione el botón izquierdo del mouse sobre el mapa y arrastre para dibujar un área para aplicar zoom al mapa.

**Desplazamiento lateral del mapa**
- Con el mouse: presione con el botón izquierdo del mouse sobre el mapa y arrastre en cualquier dirección.
- Con una pantalla táctil: toque el mapa y arrastre en cualquier dirección.
- Con el mapa centrado: utilice las teclas de dirección para desplazar el mapa.

**Giro del mapa**
- Con el mouse: presione con el botón derecho del mouse sobre el mapa y arrastre a la izquierda o la derecha. 
- Con una pantalla táctil: toque el mapa con dos dedos y gira.
- Con el mapa centrado: use la tecla Mayús y las teclas de dirección izquierda o derecha.
- Use el control de rotación con el mouse, la entrada táctil o las teclas de tabulación/ingreso del teclado.

**Inclinación del mapa**
- Con el mouse: presione con el botón derecho del mouse sobre el mapa y arrastre hacia arriba o hacia abajo. 
- Con una pantalla táctil: toque el mapa con dos dedos y arrastre hacia arriba o hacia abajo.
- Con el mapa centrado, use la tecla Mayús y las teclas de dirección arriba o abajo. 
- Use el control de inclinación con el mouse, la entrada táctil o las teclas de tabulación/ingreso del teclado.

**Cambio del estilo de mapa** no todos los desarrolladores querrán que todos los estilos de mapa posibles estén disponibles en su aplicación. El desarrollador puede establecer y cambiar el estilo de mapa mediante programación según sea necesario. Si el desarrollador muestra el control selector de estilo de mapa, el usuario podrá cambiar el estilo de mapa mediante el mouse, la entrada táctil o el teclado mediante las teclas de tabulación/ingreso. El desarrollador puede especificar los estilos de mapa que quiere que estén disponibles en el control selector de estilo de mapa. 

## <a name="keyboard-shortcuts"></a>Accesos directos del teclado

El mapa tiene una serie de métodos abreviados de teclado integrados que facilitan el uso del mapa. Estos métodos abreviados de teclado funcionan cuando el mapa tiene el foco.

| Clave      | Acción                            |
|----------|-----------------------------------|
| `Tab` | Navegue por los controles y los elementos emergentes del mapa. |
| `ESC` | Desplace el foco de cualquier elemento del mapa al elemento de mapa de nivel superior. |
| `Ctrl` + `Shift` + `D` | Alternancia del nivel de detalle del lector de pantalla.  |
| Tecla de flecha izquierda | Desplazamiento de 100 píxeles a la izquierda del mapa |
| Tecla de flecha derecha | Desplazamiento del mapa a la derecha de 100 píxeles |
| Flecha abajo | Desplazamiento del mapa hacia abajo de 100 píxeles |
| Tecla de flecha arriba | Desplazamiento del mapa hacia arriba de 100 píxeles |
| `Shift` + flecha arriba | Aumento de la inclinación del mapa en 10 grados |
| `Shift` + flecha abajo | Reducción de la inclinación del mapa en 10 grados |
| `Shift` + flecha derecha | Giro del mapa 15 grados en el sentido de las agujas del reloj |
| `Shift` + flecha izquierda | Giro del mapa 15 grados en el sentido contrario a las agujas del reloj |
| Signo más (`+`) o <sup>*</sup>signo igual (`=`) | Acercamiento |
| Signo menos, guion (`-`) o <sup>*</sup>carácter de subrayado (`_`) | Alejamiento | 
| `Shift` + arrastre del mouse en el mapa al área de dibujo | Acercamiento al área |

<sup>*</sup> Normalmente, estos métodos abreviados de teclado comparten la misma tecla en un teclado. Estos métodos abreviados se han incorporado para mejorar la experiencia del usuario. En estos métodos abreviados, no importa si el usuario utiliza la tecla Mayús.

## <a name="screen-reader-support"></a>Compatibilidad con el lector de pantalla

Los usuarios pueden navegar por el mapa con el teclado. Si se está ejecutando un lector de pantalla, el mapa notificará al usuario los cambios en su estado. Por ejemplo, los usuarios reciben notificaciones de cambios en el mapa cuando este se amplía o desplaza lateralmente. De forma predeterminada, el mapa ofrece descripciones simplificadas que excluyen el nivel de zoom y las coordenadas del centro del mapa. El usuario puede alternar el nivel de detalle de estas descripciones mediante el uso del método abreviado de teclado `Ctrl` + `Shift` + `D`.

Cualquier información adicional que se coloque en el mapa base debería tener la información textual correspondiente para los usuarios de lectores de pantalla. Asegúrese de agregar los atributos [Accessible Rich Internet Applications (ARIA)](https://www.w3.org/WAI/standards-guidelines/aria/), Alt y Title cuando sea apropiado. 

## <a name="make-popups-keyboard-accessible"></a>Creación de elementos emergentes accesibles con el teclado

A menudo se usa un marcador o un símbolo para representar una ubicación en el mapa. Normalmente, se muestra información adicional sobre la ubicación en un elemento emergente cuando el usuario interactúa con el marcador. En la mayoría de las aplicaciones, cuando un usuario hace clic o pulsa en un marcador, aparece un elemento emergente; sin embargo, esto requiere que el usuario use un mouse o una pantalla táctil. Una buena práctica es hacer que los elementos emergentes sean accesibles cuando se usa un teclado. Esta funcionalidad puede conseguirse creando un elemento emergente en cada punto de datos y agregándolo al mapa. 

En el ejemplo siguiente se cargan puntos de interés en el mapa mediante una capa de símbolos y se agrega un elemento emergente al mapa para cada punto de interés. En las propiedades de cada punto de datos se guarda una referencia de cada elemento emergente. También puede recuperarse en el caso de los marcadores; por ejemplo, al hacer clic en un marcador. Cuando está centrado en el mapa, al presionar la tecla de tabulación, el usuario podrá recorrer cada elemento emergente del mapa.

<br/>

<iframe height='500' scrolling='no' title='Crear una aplicación accesible' src='//codepen.io/azuremaps/embed/ZoVyZQ/?height=504&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Vea el lápiz <a href='https://codepen.io/azuremaps/pen/ZoVyZQ/'>Crear una aplicación accesible</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>. </iframe>

<br/>

## <a name="additional-accessibility-tips"></a>Sugerencias de accesibilidad adicionales

Estos son algunos otros consejos para que la aplicación de mapas web resulte más accesible.

- Si aparecen muchos datos de puntos interactivos en el mapa, considere la posibilidad de usar la agrupación en clústeres para poner un poco de orden. 
- Asegúrese de que la relación de contraste de color entre el texto y los símbolos y los colores de fondo es de 4,5:1 o más.
- Mantenga los mensajes del lector de pantalla (los atributos ARIA, Alt y Title) breves, descriptivos y significativos. Evite terminología y acrónimos innecesarios.
- Intente optimizar los mensajes enviados al lector de pantalla para ofrecer información breve y significativa que sea fácil de resumir para el usuario. Por ejemplo, si desea actualizar el lector de pantalla con una frecuencia elevada (por ejemplo, cuando el mapa se esté moviendo), podría hacer lo siguiente:
    - Espere hasta que el mapa termine de moverse para actualizar el lector de pantalla.
    - Limite las actualizaciones a una vez cada pocos segundos. 
    - Combine los mensajes de manera lógica. 
- Evite usar el color como único medio para transmitir información. Use texto, iconos o patrones para complementar o reemplazar el color. Algunas consideraciones:
    - Si usa una capa de burbujas para mostrar el valor relativo entre los puntos de datos, considere la posibilidad de escalar el radio de cada burbuja además de o como alternativa a colorearlos. 
    - Considere la posibilidad de usar una capa de símbolos con diferentes iconos para distintas categorías de métricas, como triángulos, estrellas y cuadrados. La capa de símbolos también admite el escalado del tamaño del icono. También se puede mostrar una etiqueta de texto.
    - Si se muestran datos de línea, el ancho se puede usar para representar el peso o el tamaño. Se puede usar un patrón de matriz de guiones para representar diferentes categorías de líneas. Se puede utilizar una capa de símbolos en combinación con una línea para superponer iconos a lo largo de la línea. El uso de un icono de flecha resulta útil para mostrar el flujo o la dirección de la línea.
    - Si se muestran datos de polígono, se puede usar un patrón, por ejemplo, franjas, como alternativa al color. 
- Algunas visualizaciones, como los mapas térmicos, las capas de iconos y las capas de imágenes, no son accesibles para los usuarios con discapacidades visuales. Algunas consideraciones:
    - Haga que el lector de pantalla describa lo que la capa muestra cuando se agrega al mapa. Por ejemplo, si se muestra una capa de mosaico de radar meteorológico, haga que el lector de pantalla indique algo como "datos de radar meteorológico superpuestos en el mapa".
- Limite la cantidad de funciones que requiere un desplazamiento del mouse. Los usuarios que usen un teclado o un dispositivo táctil para interactuar con la aplicación no podrán acceder a estar funcionalidades. Tenga en cuenta que sigue siendo recomendable aplicar el mismo estilo que utiliza el mouse al situarse sobre contenido interactivo, como iconos, vínculos y botones en los que se puede hacer clic.
- Intente navegar por la aplicación con el teclado. Asegúrese de que el orden de tabulación es lógico.
- Si va a crear métodos abreviados de teclado, procure limitarlo a dos teclas o menos. 

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre la accesibilidad en los módulos del SDK web.

> [!div class="nextstepaction"]
> [Accesibilidad de herramientas de dibujo](drawing-tools-interactions-keyboard-shortcuts.md)

Obtenga información sobre el desarrollo de aplicaciones accesibles con Microsoft Learn:

> [!div class="nextstepaction"]
> [Accesibilidad en la ruta de aprendizaje de notificaciones digitales de acción](https://ready.azurewebsites.net/learning/track/2940)

Eche un vistazo a estas herramientas de accesibilidad útiles:
> [!div class="nextstepaction"]
> [Desarrollo de aplicaciones accesibles](https://developer.microsoft.com/windows/accessible-apps)

> [!div class="nextstepaction"]
> [Introducción a WAI-ARIA](https://www.w3.org/WAI/standards-guidelines/aria/)

> [!div class="nextstepaction"]
> [Herramienta de evaluación de accesibilidad web (WAVE)](https://wave.webaim.org/)

> [!div class="nextstepaction"]
> [Comprobador de contraste de color de WebAim](https://webaim.org/resources/contrastchecker/)

> [!div class="nextstepaction"]
> [No Coffee Vision Simulator](https://chrome.google.com/webstore/detail/nocoffee/jjeeggmbnhckmgdhmgdckeigabjfbddl?hl=en-US)
