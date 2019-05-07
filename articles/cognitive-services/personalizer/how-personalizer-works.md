---
title: Funcionamiento de Personalizer (Personalizer)
titleSuffix: Azure Cognitive Services
description: Personalizer utiliza el aprendizaje automático para detectar qué acción se debe usar en un contexto. Cada bucle de aprendizaje tiene un modelo que está entrenado exclusivamente con los datos que le ha enviado a través de llamadas a Rank y a Reward. Todos los bucles de aprendizaje son completamente independientes entre sí.
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/07/2019
ms.author: edjez
ms.openlocfilehash: 6b2237f27fba5eaf952932cd6592052649400b96
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025497"
---
# <a name="how-personalizer-works"></a>Funcionamiento de Personalizer

Personalizer utiliza el aprendizaje automático para detectar qué acción se debe usar en un contexto. Cada bucle de aprendizaje tiene un modelo que está entrenado exclusivamente con los datos que le ha enviado a través de llamadas a **Rank** y **Reward**. Todos los bucles de aprendizaje son completamente independientes entre sí. Cree un bucle de aprendizaje para cada parte o comportamiento de la aplicación que desea personalizar.

En cada bucle, **llame a la API Rank** en función del contexto actual, con:

* Una lista de posibles acciones: elementos de contenido entre los que seleccionar la acción superior.
* Una lista de datos contextualmente relevantes de las [características contexto](concepts-features.md), como el usuario, contenido y contexto.

La API **Rank** decide usar:

* _Explotación_: el modelo actual para decidir la mejor acción según los datos anteriores.
* _Exploración_: seleccione una acción diferente, en lugar de la acción superior.

La API **Reward**:

* Recopila datos para entrenar el modelo mediante la grabación de las características y las puntuaciones de recompensa de cada llamada a Rank.
* Utiliza esos datos para actualizar el modelo, en función de la configuración especificada en la _directiva de aprendizaje_.

## <a name="architecture"></a>Arquitectura

La siguiente imagen muestra el flujo arquitectónico de realizar las llamadas a Rank y Reward:

![alt text](./media/how-personalizer-works/personalization-how-it-works.png "Funcionamiento de Personalization")

1. Personalizer utiliza un modelo de inteligencia artificial interno para determinar la clasificación de la acción.
1. El servicio decide si va a explorar el modelo actual o a explorar nuevas opciones para el modelo.  
1. El resultado de la clasificación se envía a EventHub.
1. Cuando Personalizer recibe la recompensa, esta se envía a EventHub. 
1. La clasificación y la recompensa están correlacionadas.
1. El modelo de inteligencia artificial se actualiza en función de los resultados de la correlación.
1. El motor de inferencia se actualiza con el nuevo modelo. 

## <a name="research-behind-personalizer"></a>La investigación que hay detrás de Personalizer

Personalizer se basa en una ciencia e investigación vanguardistas en el área del [aprendizaje de refuerzo](concepts-reinforcement-learning.md) que incluyen documentos, actividades de investigación o áreas de exploración en curso de Microsoft Research.

## <a name="terminology"></a>Terminología

* **Ruta de aprendizaje**: puede crear un bucle de aprendizaje para todas las partes de la aplicación que pueden beneficiarse de la personalización. Si tiene varias experiencias de personalización, cree un bucle para cada una de ellas. 

* **Acciones**: las acciones son los elementos de contenido, como productos o promociones, entre los que se eligen. Personalizer elige la acción superior para mostrar a los usuarios, conocida como la _acción de Reward_, mediante la API Rank. Todas las acciones pueden tener características enviadas con la solicitud de Rank.

* **Context**: para proporcionar una clasificación más precisa, especifique información acerca del contexto, como por ejemplo:
    * Su usuario.
    * El dispositivo en el que están. 
    * La hora actual.
    * Otros datos acerca d la situación actual.
    * Datos históricos acerca del usuario o contexto.

    Su aplicación específica puede tener otra información de contexto. 

* **[Características](concepts-features.md)**: una unidad de información acerca de un elemento de contenido o un contexto de usuario.

* **Recompensa**: una medida de la forma en que el usuario respondió a la acción devuelta por API Rank, en forma de puntuación entre 0 y 1. El valor de 0 a 1 lo establece la lógica de negocios, en función de la forma en que la elección ha ayudado a lograr los objetivos empresariales de personalización. 

* **Exploración**: el servicio Personalizer está explorando cuando, en lugar de devolver la mejor acción, elige otra acción para el usuario. El servicio Personalizer evita el desfase, estancamiento y puede adaptarse al comportamiento del usuario en curso mediante la exploración. 

* **Duración del experimento**: La cantidad de tiempo que el servicio Personalizer espera una recompensa, desde el momento en que se realizó la llamada de Rank en el evento.

* **Eventos inactivos**: Un evento inactivo es aquel en el que llamó a Rank, pero no está seguro de que el usuario vea alguna vez el resultado, debido a decisiones de la aplicación cliente. Los eventos inactivos le permiten crear y almacenar resultados de la personalización y, después, decidir descartarlos más adelante sin que ello afecte al modelo de Machine Learning.

* **Modelo**: los modelos de Personalizer capturan todos los datos aprendidos acerca del comportamiento del usuario y obtiene los datos de entrenamiento de la combinación de los argumentos que se envían a las llamadas a Rank y a Reward, y con un comportamiento de entrenamiento determinado por la directiva de aprendizaje. 

## <a name="next-steps"></a>Pasos siguientes

Información acerca de [dónde se puede usar Personalizer](where-can-you-use-personalizer.md).