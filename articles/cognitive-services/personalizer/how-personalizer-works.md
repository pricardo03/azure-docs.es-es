---
title: Funcionamiento de Personalizer (Personalizer)
titleSuffix: Azure Cognitive Services
description: Personalizer utiliza el aprendizaje automático para detectar qué acción se debe usar en un contexto. Cada bucle de aprendizaje tiene un modelo que está entrenado exclusivamente con los datos que le ha enviado a través de llamadas a Rank y a Reward. Todos los bucles de aprendizaje son completamente independientes entre sí.
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 06/07/2019
ms.author: edjez
ms.openlocfilehash: d0a0a6101d876493188426d19f2fa845d20ee274
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67055461"
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

* **[Características](concepts-features.md)** : una unidad de información acerca de un elemento de contenido o un contexto de usuario.

* **Recompensa**: una medida de la forma en que el usuario respondió a la acción devuelta por API Rank, en forma de puntuación entre 0 y 1. El valor de 0 a 1 lo establece la lógica de negocios, en función de la forma en que la elección ha ayudado a lograr los objetivos empresariales de personalización. 

* **Exploración**: el servicio Personalizer está explorando cuando, en lugar de devolver la mejor acción, elige otra acción para el usuario. El servicio Personalizer evita el desfase, estancamiento y puede adaptarse al comportamiento del usuario en curso mediante la exploración. 

* **Duración del experimento**: La cantidad de tiempo que el servicio Personalizer espera una recompensa, desde el momento en que se realizó la llamada de Rank en el evento.

* **Eventos inactivos**: Un evento inactivo es aquel en el que llamó a Rank, pero no está seguro de que el usuario vea alguna vez el resultado, debido a decisiones de la aplicación cliente. Los eventos inactivos le permiten crear y almacenar resultados de la personalización y, después, decidir descartarlos más adelante sin que ello afecte al modelo de Machine Learning.

* **Modelo**: los modelos de Personalizer capturan todos los datos aprendidos acerca del comportamiento del usuario y obtiene los datos de entrenamiento de la combinación de los argumentos que se envían a las llamadas a Rank y a Reward, y con un comportamiento de entrenamiento determinado por la directiva de aprendizaje. 

## <a name="example-use-cases-for-personalizer"></a>Ejemplos de casos de uso para Personalizer

* Aclaración de intenciones y desambiguación: Para ayudar a los usuarios a tener una mejor experiencia cuando su intención no sea clara, brinda una opción que esté personalizada para cada usuario.
* Sugerencias predeterminadas para menús y opciones: Haz que el bot sugiera el elemento más probable de una manera personalizada como primer paso, en lugar de presentar un menú impersonal o una lista de alternativas.
* Rasgos y tono del: Para los bots que pueden variar el tono, el nivel de detalle y el estilo de escritura, piensa en la posibilidad de variar estos rasgos de maneras personalizadas.
* Contenido de notificaciones y alertas: Decide qué texto usar para las alertas con el fin de llamar más la atención de los usuarios.
* Tiempos de notificaciones y alertas: Haz un aprendizaje personalizado de cuándo enviar notificaciones a los usuarios para atraerlos más.

## <a name="checklist-for-applying-personalizer"></a>Lista de comprobación para aplicar Personalizer

Puede aplicar Personalizer en situaciones donde:

* Tiene un objetivo de negocio o de usabilidad para la aplicación.
* Tiene un lugar en la aplicación donde tomar una decisión contextual sobre qué mostrar a los usuarios para mejorar ese objetivo.
* La mejor opción puede y debe aprenderse del comportamiento colectivo de los usuarios y de la puntuación total de recompensa.
* El uso del aprendizaje automático para la personalización sigue las [directrices de uso responsable](ethics-responsible-use.md) y las opciones para tu equipo.
* La decisión puede expresarse como la clasificación de la mejor opción ([acción](concepts-features.md#actions-represent-a-list-of-options)) de un conjunto limitado de opciones.
* Cuán bien funcione esa opción puede calcularse mediante la lógica de negocios, al medir algún aspecto del comportamiento del usuario, y expresarla en un número comprendido entre -1 y 1.
* La puntuación de recompensa no aporta demasiados factores de confusión o externos, específicamente la duración del experimento es lo suficientemente baja como para que la puntuación de recompensa pueda calcularse mientras sea pertinente.
* Puedes expresar el contexto para la clasificación como un diccionario de al menos 5 características que, en tu opinión, ayudarían a tomar la decisión correcta, y que no incluya información de identificación personal.
* Tienes información sobre cada acción como un diccionario de al menos 5 atributos o características que, en tu opinión, ayudarán a Personalizer a tomar la decisión correcta.
* Puedes retener datos durante el tiempo suficiente para acumular un historial de al menos 100 000 interacciones.

## <a name="machine-learning-considerations-for-applying-personalizer"></a>Consideraciones de aprendizaje automático para aplicar Personalizer

Personalizer se basa en el aprendizaje de refuerzo, un enfoque del aprendizaje automático que se enseña mediante los comentarios que le haces. 

Personalizer aprenderá mejor en situaciones en las que:
* Hay suficientes eventos para estar al tanto de la personalización óptima si el problema se desvía con el tiempo (como las preferencias en las noticias o la moda). Personalizer se adaptará al cambio continuo en el mundo real, pero los resultados no serán óptimos si no hay suficientes eventos y datos de los que aprender para detectar y establecer nuevos patrones. Debe elegir un caso de uso que ocurra con suficiente frecuencia. Considere la posibilidad de buscar casos de uso que ocurran al menos 500 veces al día.
* El contexto y las acciones tienen características suficientes para facilitar el aprendizaje.
* Hay menos de 50 acciones para clasificar por llamada.
* La configuración de retención de datos permite a Personalizer recopilar suficientes datos para realizar evaluaciones sin conexión y optimizar las directivas. Por lo general, se trata de al menos 50 000 puntos de datos.

## <a name="how-to-use-personalizer-in-a-web-application"></a>Cómo usar Personalizer en una aplicación web

Agregar un bucle a una aplicación web incluye:

* Decide qué experiencia personalizar, qué acciones y características tienes, qué características de contexto quieres usar y qué recompensas vas a definir.
* Agrega una referencia al SDK de Personalization en la aplicación.
* Llama a Rank API cuando estés listo para personalizar.
* Almacena el eventId. Enviarás una recompensa con Reward API más adelante.
1. Llamar a Activate para el evento una vez que estés seguro de que el usuario ha visto tu página personalizada.
1. Espera a que el usuario seleccione el contenido clasificado. 
1. Llama a Reward API para especificar qué tan bien funcionó la salida de Rank API.

## <a name="how-to-use-personalizer-with-a-chat-bot"></a>Cómo usar Personalizer con un bot de chat

En este ejemplo, verás cómo usar Personalization para hacer una sugerencia predeterminada en lugar de enviar al usuario una serie de menús u opciones cada vez.

* Obtén el [código](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/tree/master/samples/ChatbotExample) para este ejemplo.
* Configura la solución del bot. Asegúrate de publicar la aplicación de LUIS. 
* Administrar las llamadas a Rank y Reward API para el bot.
    * Agrega código para administrar el procesamiento de intenciones de LUIS. Si se devuelve **None** como intención superior o la puntuación de la intención superior está por debajo de tu umbral de lógica de negocio, envía la lista de intenciones a Personalizer para clasificar las intenciones.
    * Muestra la lista de intención al usuario como vínculos seleccionables, donde la primera intención sea la intención de rango superior de la respuesta de Rank API.
    * Captura la selección del usuario y envíala en la llamada de Reward API. 

### <a name="recommended-bot-patterns"></a>Patrones recomendados para bots

* Haz llamadas de Rank API de Personalizer cada vez que se necesite una desambiguación, en lugar de almacenar en caché los resultados para cada usuario. El resultado de la intención de desambiguación puede cambiar con el tiempo para una persona, por lo que permitir que Rank API explore variaciones acelerará el aprendizaje en general.
* Elige una interacción que sea común con muchos usuarios, de modo que tengas suficientes datos para personalizar. Por ejemplo, las preguntas introductorias pueden servir mejor que las aclaraciones más detalladas más adelante en el grafo de la conversación, al que solo es posible que lleguen unos pocos usuarios.
* Usa llamadas a Rank API para habilitar conversaciones "la primera sugerencia es correcta", donde al usuario se le pregunta "¿Quieres X?" o "¿Quisiste decir X?" y el usuario solo puede confirmar; a diferencia de proporcionar opciones al usuario donde este debe elegir entre un menú. Por ejemplo, - Usuario: "Me gustaría pedir un café". - Bot: "¿Quieres un expreso doble?". De este modo, la señal de recompensa también es fuerte, ya que pertenece directamente a una sugerencia.

## <a name="how-to-use-personalizer-with-a-recommendation-solution"></a>Cómo usar Personalizer con una solución de recomendaciones

Usa tu motor de recomendaciones para filtrar un catálogo grande a unos cuantos elementos que luego puedan presentarse como 30 posibles acciones que se envíen a Rank API.

Puedes usar motores de recomendaciones con Personalizer:

* Configura la [solución de recomendaciones](https://github.com/Microsoft/Recommenders/). 
* Al mostrar una página, invoca el modelo de recomendaciones para obtener una breve lista de recomendaciones.
* Llama a Personalization para clasificar el resultado de solución de recomendaciones.
* Envía tus comentarios sobre la acción del usuario con la llamada de Reward API.


## <a name="pitfalls-to-avoid"></a>Errores que hay que evitar

* No use Personalizer donde el comportamiento personalizado no sea algo que se pueda detectar a través de todos los usuarios, sino algo que deba recordarse para usuarios específicos o que provenga de una lista de alternativas específica para cada usuario. Por ejemplo, usar Personalizer para sugerir un primer pedido de pizza en una lista de 20 elementos de menú posibles es útil, pero a qué contacto llamar de la lista de contactos del usuario al pedir ayuda con el cuidado de los niños (por ejemplo, "Abuela") no es algo que pueda personalizarse a través de la base de usuarios.


## <a name="adding-content-safeguards-to-your-application"></a>Agregar a la aplicación medidas de seguridad para contenido

Si la aplicación permite grandes variaciones del contenido que se muestra a los usuarios, y algunos de estos contenidos pueden no ser seguros o apropiados para algunos usuarios, debes planear con antelación para asegurarte de poner en práctica las medidas de seguridad correctas a para evitar que los usuarios vean contenido inaceptable. El mejor patrón para implementar medidas de seguridad es el siguiente: El mejor patrón para implementar medidas de seguridad es el siguiente:
    * Obtén la lista de acciones para clasificar.
    * Filtra las que no sean viables para el público.
    * Clasifica solo estas acciones viables.
    * Muestra al usuario la acción con clasificación superior.

En algunas arquitecturas, la secuencia anterior puede ser difícil de implementar. En ese caso, existe un enfoque alternativo a la implementación de medidas de seguridad después de la clasificación, pero debes establecer salvedades para que las acciones que excluidas por la medida de seguridad no se usen para entrenar el modelo de Personalizer.

* Obtén la lista de acciones para clasificar, con aprendizaje desactivado.
* Clasifica las acciones.
* Comprueba si la acción superior es viable.
    * Si la acción superior es viable, activa el aprendizaje para esta clasificación y, luego, muéstrala al usuario.
    * Si la acción superior no es viable, no actives el aprendizaje para esta clasificación y decide mediante tu propia lógica o enfoques alternativos qué mostrar al usuario. Incluso si usas la segunda mejor opción clasificada, no actives el aprendizaje para esta clasificación.

## <a name="verifying-adequate-effectiveness-of-personalizer"></a>Verificar la eficacia adecuada de Personalizer

Para supervisar la eficacia de Personalizer periódicamente, haz las [evaluaciones sin conexión](how-to-offline-evaluation.md).

## <a name="next-steps"></a>Pasos siguientes

Información acerca de [dónde se puede usar Personalizer](where-can-you-use-personalizer.md).