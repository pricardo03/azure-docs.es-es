---
title: Directrices para experiencias de anclaje eficaz que usar delimitadores espacial de Azure | Microsoft Docs
description: Directrices y consideraciones que debe crear y busque los anclajes de forma eficaz mediante el uso de delimitadores espacial de Azure.
author: mattwojo
manager: jken
services: azure-spatial-anchors
ms.author: mattwoj
ms.date: 02/24/2019
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.openlocfilehash: 4714ea7aa4bf55f7cbd4500977b09505788233dd
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "57895811"
---
# <a name="create-an-effective-anchor-experience-by-using-azure-spatial-anchors"></a>Crear una experiencia eficaz delimitador mediante delimitadores espacial de Azure

Este artículo proporcionan directrices y consideraciones que le ayudarán a eficazmente creación y buscar delimitadores mediante delimitadores espacial.

## <a name="good-anchors"></a>Delimitadores de buena

Delimitadores espaciales le ayuda a crear delimitadores buena. Es importante invertir tiempo en educación o guiar a los usuarios de la experiencia del usuario (UX) que crear delimitadores buena. Al invertir en la creación de buenas delimitadores por adelantado, ayuda a buscar de forma confiable los anclajes de los usuarios finales:

- En diferentes dispositivos.
- En momentos diferentes.
- En condiciones de iluminación diferentes.
- Desde las perspectivas deseadas dentro del espacio.

## <a name="static-and-dynamic-locations"></a>Ubicaciones estáticas y dinámicas

Parte del diseño de la experiencia de anclaje es elegir las ubicaciones. ¿Las ubicaciones puede estática y definirse por un administrador del espacio? ¿O bien, serán dinámicos y definido por el usuario?

Un administrador de almacén de venta directa es posible que desee una experiencia en tienda estática para convencer a los usuarios visitar. Pero el desarrollador de un juego de tablero de realidad mixta probable que desee permitir a los usuarios elegir dónde desea reproducir.

Para obtener ubicaciones estáticas, puede enseñar administradores dedicar tiempo ya que mantiene el espacio con buena anclajes.

Para obtener ubicaciones dinámicas, debería pensar en cómo enseñar o guiar a los usuarios en la experiencia de usuario para crear los anclajes de buena.

## <a name="stable-visual-features"></a>Características visuales estables

Sistemas de seguimiento visual de que se usan en realidad mixta y dispositivos de realidad aumentada dependen de las características visuales del entorno. Para obtener la experiencia más confiable:  

- *Hacer* crear delimitadores en ubicaciones que tienen las características visuales estables (es decir, las características que no cambian con frecuencia).

- *No* crear anclajes en grandes superficies en blanco que no hay características distintivas.

- *No* crear anclajes en materiales muy brillante.

- *No* crear anclajes en superficies donde el patrón se repite, como la alfombra o papel tapiz.

![Ejemplos de un entorno adecuado para delimitadores y un entorno incorrecto para delimitadores](./media/stable-visual.png)

## <a name="various-viewing-perspectives"></a>Varias perspectivas de visualización

Al crear un delimitador, piense en las personas que posteriormente intenta localizar el delimitador.

Considere, por ejemplo, un delimitador en el medio de una habitación con dos puertas. Es posible que desean permitir a los usuarios a entrar en el salón de cualquier puerta. Al crear el delimitador, deberá examinar su posición de ambas entradas. Modificar perspectivas para capturar datos de entorno alrededor del delimitador para que los usuarios puedan encontrar el delimitador de cualquier puerta.

En general, al crear un delimitador, darle desde las perspectivas de las personas que intentarán encontrarlo. Por lo que si va a colocar contenido virtual en una escultura al aire libre, tiene sentido andar con escultura, al examinar, a medida que cree el delimitador. Si el delimitador se encuentra en la esquina de una sala, hay una única dirección enfocar desde. Al crear este delimitador, puede explorarlo solo desde esta perspectiva.

## <a name="multiple-anchors"></a>Varios delimitadores

Iluminación puede suponer una diferencia en las características visuales que detecta una aplicación. Delimitadores creados en luz natural segura podrían ser difíciles de encontrar en la luz artificial y viceversa.  

Si tiene este problema, puede ayudar a crear dos delimitadores. En el mismo punto, cree un delimitador en el horario de verano y otro de luz artificial. La aplicación, a continuación, puede consultar los dos delimitadores. Cuando se encuentra cualquier delimitador, la aplicación tendrá una postura para el delimitador. 

De forma similar, en entornos donde las características visuales cambian porque la mayoría de los objetos mover, varios delimitadores pueden ayudar a. Cuando se convierte en un delimitador demasiado difícil de encontrar debido a cambios significativos en el entorno, puede reemplazar el delimitador por uno nuevo. Podría hacer esto, por ejemplo, en una tienda donde el diseño se actualiza cada pocos meses.

## <a name="targets-and-rooms"></a>Destinos y las salas

En muchos casos, un delimitador es un punto de entrada a la experiencia de la aplicación. Desea empezar rápidamente a través de este paso y confiable para que los usuarios pueden escribir su experiencia. Dedicar tiempo a cómo los usuarios encontrarán los delimitadores es un paso de diseño importantes. Resulta útil pensar para encontrar los delimitadores en términos de dos escenarios generales: *destinos* y *salas*.

### <a name="targets"></a>Destinos

En el escenario de destino, la ubicación de un delimitador es conocida. Por ejemplo, en una aplicación ficticia realidad mixta de dibujo, un usuario coloca un lienzo virtual en la pared. Indica a los demás usuarios de la sala de reuniones para que apunte a sus dispositivos en el mismo lugar en la pared para buscar el delimitador y comience la experiencia.  

Otro ejemplo de un escenario de destino podría ser un inicio de sesión en una cafetería que diga: "Buscar ofertas". La cafetería ha colocado un delimitador aquí. Como los usuarios examen el inicio de sesión, buscan el delimitador y escriba la experiencia de realidad aumentada para buscar ofertas de café.

En el escenario de destino, pueden ayudar las fotos. Si se muestra a los usuarios una foto del destino deseado en su dispositivo, puede identificar rápidamente qué analizar en el mundo real. Por ejemplo, podría ayudar a los usuarios llegan dentro del área de un objetivo general mediante el uso de GPS. Cuando llega el usuario, la aplicación muestra una foto del destino. El usuario busca todo el espacio, busca el destino y busca el delimitador.

![Ilustración de un delimitador, que muestra una foto del destino en un dispositivo de usuario móviles](./media/start-here-edit.png)

### <a name="rooms"></a>Salas

En el escenario de espacio, los usuarios especificar un espacio con simplemente saber que hay un delimitador de aquí en algún lugar. Los usuarios examen el espacio con su dispositivo y encontrar rápidamente el delimitador.

Esta experiencia normalmente requiere que cree delimitadores bien protegidos, como se describe en [varias perspectivas de visualización](#consider-various-viewing-perspectives). Si ha examinado la sala de muchas perspectivas cuando creó el delimitador, los usuarios pueden analizar prácticamente en cualquier parte cuando intenten buscarla.

![Ilustración de cómo un usuario puede examinar una habitación para buscar un delimitador](./media/scan-room.png)

En esencia, que dedicar más tiempo análisis el espacio al crear el delimitador para que los usuarios posteriores pueden examinar y buscar rápidamente el delimitador. A medida que cree su experiencia, deberá tener en cuenta este equilibrio importante.

El ejemplo de la aplicación de dibujo de realidad mixta que analizamos anteriormente no funciona bien como un escenario de espacio. En este caso, el usuario que coloca el delimitador quiere otros a unirse a la experiencia de rápidamente. Los usuarios no quiere esperar a la experiencia de inicio hasta que también se examina la sala de reuniones. Dado que todos los usuarios saben exactamente dónde para que apunte a su dispositivo para localizar los delimitadores, este ejemplo funciona mejor como un escenario de destino.

## <a name="anchor-location"></a>Ubicación de anclaje

Los sistemas de seguimiento visual dependen de las características visuales en un entorno. Las características más visual que incluye un examen, mayor será la probabilidad de encontrar un delimitador.

Siga las directrices generales de esta sección para crear una experiencia de usuario que animan a un análisis útil del entorno.

En primer lugar, si el usuario no podrá encontrar un delimitador de unos segundos, la aplicación debe anime a los usuarios pasar el dispositivo para capturar más perspectivas. La aplicación también puede animar a los usuarios a sí mismos moverse por el entorno para buscar el delimitador desde varias perspectivas. Las perspectivas más características que ve el dispositivo, mejor.

Para escenarios de destino, pida al usuario a desplazarse por el destino para verlo desde distintas perspectivas. En otras palabras, pida al usuario capturar el destino de nuevas perspectivas hasta que se encuentra el delimitador.

Para escenarios de sala, pida al usuario examinar lentamente la sala. Por ejemplo, pedir al usuario activar para capturar 180 grados o incluso de 360 grados de la sala. O bien, pida al usuario para ver la sala desde una perspectiva de nuevo. 

El método más significativo es buscar en toda la habitación. Un recorrido a través de la sala de captura más características visuales del entorno que un examen de una pared cercano, por ejemplo. Un examen de una pared cercano no capturarán tantas características útiles de visuales del entorno.

No es útil mover varias veces de lado a lado del dispositivo cuando se busca un delimitador. Simplemente captura los mismos puntos desde la perspectiva del mismo.

## <a name="experience-tests"></a>Experiencia de pruebas

En este artículo, hemos hablado de directrices generales. Con los delimitadores espacial, va a escribir aplicaciones que interactúen con el mundo real. Por este motivo, debe dedicar tiempo a probar escenarios de anclaje de la aplicación en entornos reales. Esto es especialmente cierto para los entornos que representan donde se espera que los usuarios para usar la aplicación.