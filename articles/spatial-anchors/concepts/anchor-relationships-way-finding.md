---
title: Las relaciones de anclaje y búsqueda de manera en los delimitadores espacial de Azure | Microsoft Docs
description: Obtenga información sobre el modelo conceptual detrás de las relaciones de anclaje. Aprenda a conectar los delimitadores dentro de un espacio y para usar la API cercanos para satisfacer un escenario de búsqueda de forma.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.openlocfilehash: 008269a5883750dc8899d896c101c6a05bf7e814
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65969284"
---
# <a name="anchor-relationships-and-way-finding-in-azure-spatial-anchors"></a>Las relaciones de anclaje y búsqueda de manera en los delimitadores espacial de Azure

Mediante el uso de las relaciones de anclaje, puede crear delimitadores conectados en un espacio y, a continuación, hacer preguntas como estas:

* ¿Hay delimitadores cercanas?
* ¿Cuán lejos estén son?

## <a name="examples"></a>Ejemplos

Puede usar delimitadores conectados en estos casos:

* Un trabajador debe completar una tarea que implica visitar varias ubicaciones en una fábrica industrial. El generador tiene delimitadores espaciales en cada ubicación. Una aplicación móvil o HoloLens le ayuda a guiar al trabajo de una ubicación a la siguiente. La aplicación primero pregunta por los delimitadores espaciales cercanos y guías, a continuación, el trabajo a la ubicación siguiente. La aplicación muestra visualmente la dirección general y la distancia a la siguiente ubicación.

* Un museo crea delimitadores espaciales de muestra pública. Juntos, estos delimitadores forman un paseo por una hora monitores esenciales del Museo. En una visualización pública, los visitantes pueden abrir la aplicación de realidad mixta del museo en su dispositivo móvil. A continuación, que señalan a su cámara del teléfono todo el espacio para ver la dirección general y la distancia a las otras pantallas públicas en el paseo. Como un usuario le guía a un público para mostrar, la aplicación actualiza la dirección general y la distancia para ayudar a guiar al usuario.

## <a name="set-up-way-finding"></a>Configurar la búsqueda de forma

Está usando una aplicación que usa la dirección de la línea de visión y la distancia entre los delimitadores para proporcionar orientación *buscar de manera*. Búsqueda de manera es diferente de la navegación por giro. En la navegación por giro, alrededor de las paredes, a través de las puertas y entre los pisos se guía al usuario. Con búsqueda de manera, el usuario obtiene sugerencias acerca de la dirección general del destino. Pero inferencia ni conocimientos del espacio también ayuda al usuario navegar por la estructura en el destino.

Para crear una experiencia de búsqueda de manera, preparar un espacio para la experiencia y desarrollar una aplicación que los usuarios interactuarán con. Estos son los pasos conceptuales:

1. **Planear el espacio de**: Decidir qué ubicaciones dentro del espacio formará parte de la experiencia de búsqueda de forma. En nuestros escenarios, el supervisor de fábrica o el Coordinador de paseo Museo podría decidir qué ubicaciones se debe incluir en la experiencia de búsqueda de forma.
2. **Conectar los anclajes**: Visite las ubicaciones elegidas para crear delimitadores espaciales. Puede hacerlo en un modo de administrador de la aplicación del usuario final o en otra aplicación completamente. Podrá conectarse o se relacionan con cada delimitador de los demás. El servicio mantiene estas relaciones.
3. **Iniciar la experiencia del usuario final**: Los usuarios ejecuten la aplicación para localizar un delimitador, que puede estar en cualquiera de las ubicaciones elegidas. El diseño general debe determinar las ubicaciones donde los usuarios pueden escribir la experiencia.
4. **Encontrar las delimitadores**: Después de que el usuario encuentra un delimitador, la aplicación puede solicitar cercanas delimitadores. Este procedimiento devuelve una postura entre el dispositivo y estos delimitadores.
5. **Guiar al usuario**: La aplicación puede usar la postura a cada uno de estos delimitadores para proporcionar orientación acerca de la dirección general del usuario y la distancia. Por ejemplo, la fuente en la aplicación de la cámara podría mostrar un icono y la flecha para representar cada posible destino, como se muestra en la siguiente imagen.
6. **Refinar las instrucciones**: Cuando el usuario, la aplicación puede calcular periódicamente una postura nueva entre el dispositivo y el delimitador de destino. La aplicación continúa refinar las sugerencias de orientación que ayudan al usuario llega a su destino.

    ![Un ejemplo de cómo una aplicación puede mostrar la orientación de la manera de buscar](./media/meeting-spot.png)

## <a name="connect-anchors"></a>Conectar los delimitadores

Para crear una experiencia de búsqueda de manera, primero deberá colocar los delimitadores en las ubicaciones elegidas. En esta sección, supondremos que el Administrador de la aplicación ya ha finalizado este trabajo.

### <a name="connect-anchors-in-a-single-session"></a>Conectar los anclajes en una única sesión

Para conectar los anclajes de:

1. Guiará a la primera ubicación y crear el delimitador A mediante un CloudSpatialAnchorSession.
2. Recorrido para la segunda ubicación. La plataforma subyacente de MR/AR realiza un seguimiento del movimiento.
3. Crear delimitador B mediante el mismo CloudSpatialAnchorSession. Delimitadores de A y B ahora están conectados. El servicio de delimitadores espacial mantiene esta relación.
4. Siga el procedimiento para los delimitadores restantes.

### <a name="connect-anchors-in-multiple-sessions"></a>Conectar los anclajes en varias sesiones

Puede conectar delimitadores espaciales a través de varias sesiones. Con este método, puede crear y conectarse al mismo tiempo algunos delimitadores y, a continuación, más adelante crear y conectar varios delimitadores. 

Para conectar los anclajes en varias sesiones:

1. La aplicación crea algunos anclajes en uno CloudSpatialAnchorSession. 
2. En otro momento, la aplicación busca uno de estos delimitadores (por ejemplo, ancla A) mediante el uso de un nuevo CloudSpatialAnchorSession.
3. Recorrido para una nueva ubicación. La plataforma subyacente de realidad mixta o realidad aumentada realiza un seguimiento del movimiento.
4. Crear C delimitador con el mismo CloudSpatialAnchorSession. Delimitadores de A, B y C ahora están conectados. El servicio de delimitadores espacial mantiene esta relación.

Puede seguir este procedimiento para delimitadores más y más sesiones con el tiempo.

### <a name="verify-anchor-connections"></a>Compruebe las conexiones de anclaje

La aplicación puede comprobar que dos anclajes están conectados mediante la emisión de una consulta para delimitadores cercanos. Cuando los resultados de la consulta contiene el delimitador de destino, se comprueba la conexión de anclaje. Si los delimitadores no están conectados, la aplicación puede intentar conectarse de nuevo. 

Estos son algunos de los motivos por qué los anclajes podrían no conectarse:

* La plataforma subyacente de realidad mixta o realidad aumentada perdido seguimiento durante el proceso de conexión de delimitadores.
* Debido a un error de red durante la comunicación con el servicio de delimitadores espacial, no se pudo persistir la conexión de anclaje.

### <a name="find-sample-code"></a>Encontrar código de ejemplo

Para buscar el código de ejemplo que se muestra cómo conectar los anclajes y hacer cerca de las consultas, vea [aplicaciones de ejemplo espacial delimitadores](https://github.com/Azure/azure-spatial-anchors-samples).
