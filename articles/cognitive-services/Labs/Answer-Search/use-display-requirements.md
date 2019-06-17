---
title: 'Requisitos de uso y visualización: Project Answer Search'
titlesuffix: Azure Cognitive Services
description: Requisitos de uso y visualización del punto de conexión de Project Answer Search.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: answer-search
ms.topic: conceptual
ms.date: 04/13/2018
ms.author: rosh
ms.openlocfilehash: 085cb20e4dad92ed55b5ba0914c677aa50f3ac97
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60345760"
---
# <a name="project-answer-search-use-and-display-requirements"></a>Requisitos de uso y visualización de Project Answer Search

Los requisitos de uso y visualización se aplican a cualquier implementación del contenido y la información asociada (por ejemplo, relaciones, metadatos y otras señales) disponible a través de llamadas a las API Bing Knowledge Search, Bing Custom Search, Entity Search, Image Search, News Search, Video Search, Visual Search, Web Search, Spell Check y Autosuggest. Los detalles de implementación relacionados con estos requisitos se pueden encontrar en la documentación de las características y resultados específicos.

## <a name="1-bing-spell-check-and-bing-autosuggest-api"></a>1. Bing Spell Check API y Bing Autosuggest API.

No:

- copiar, almacenar ni almacenar en caché los datos que reciba de las API Bing Spell Check o Bing Autosuggest;
- usar datos que reciba desde las API Bing Spell Check o Bing Autosuggest como parte de cualquier aprendizaje automático o actividad algorítmica similar para entrenar, evaluar o mejorar los servicios nuevos o existentes que usted o terceros pueden ofrecer.

## <a name="2-definitions"></a>2. Definiciones

- "respuesta" hace referencia a una categoría de resultados devueltos en una respuesta. Por ejemplo, una respuesta de Bing Web Search API puede incluir respuestas en las categorías de los resultados de página web, imágenes, vídeo y noticias;
- "respuesta" significa una y todas las respuestas y los datos asociados recibidos en respuesta a una sola llamada a una API de búsqueda;
- "resultado" hace referencia a un elemento de información en una respuesta. Por ejemplo, el conjunto de datos conectados con un único artículo de noticias es un resultado en una respuesta de noticias.
- El término "API de búsqueda" hace referencia de manera colectiva a estas API: Bing Custom Search, Entity Search, Image Search, News Search, Video Search, Visual Search y Web Search. 


## <a name="3-search-apis"></a>3. API de búsqueda

Los requisitos de esta sección 3 se aplican a las API de búsqueda.

**A. Experiencia de búsqueda en Internet.** Todos los datos devueltos en las respuestas solo pueden usarse en experiencias de búsqueda en Internet. Una experiencia de búsqueda en Internet significa que el contenido presentado, según corresponda: 
- Es pertinente y responde a la consulta directa del usuario final u otra indicación del interés y la intención de búsqueda del usuario (por ejemplo, una consulta de búsqueda indicada por el usuario). 
- Ayuda a los usuarios a buscar y navegar a los orígenes de datos (por ejemplo, las direcciones URL proporcionadas se implementan como hipervínculos para que el contenido o la atribución sean un vínculo interactivo que se muestre claramente con los datos); o bien, para Bing Entity Search API, mostrar un vínculo a la dirección URL de bing.com proporcionada en la respuesta que permite al usuario navegar a los resultados de búsqueda de la consulta relevante en bing.com;
- Incluye varios resultados para que el usuario final seleccione entre ellos (por ejemplo, se presentan varios resultados de la respuesta de noticias o todos los resultados si se devuelven muy pocos). 
- Está limitado a una cantidad apropiada para atender al propósito de búsqueda (por ejemplo, las imágenes en miniatura se muestran en un tamaño miniatura proporcional a la pantalla del usuario). 
- Incluye una indicación visible para el usuario final que informa de que el contenido se compone de resultados de la búsqueda de Internet (por ejemplo, una instrucción que indica que el contenido es "de la Web").
- Incluye cualquier otra combinación de las medidas necesarias para garantizar que el uso de los datos recibidos de las API de búsqueda no incumple ninguna ley aplicable, ni vulnera los derechos de terceros (por ejemplo, si depende de una licencia de Creative Commons, el cumplimiento de los términos de licencia aplicables). Consulte con sus asesores legales para determinar las medidas que pueden ser adecuadas.
La única excepción a los requisitos de experiencia de búsqueda de Internet es para la detección de direcciones URL tal como se describe en la sección 3E (Detección de direcciones URL fuera de presentación) a continuación. 

**B. Restricciones.** No:

- copiar, almacenar ni almacenar en caché ningún dato de las respuestas (excepto la retención en la medida permitida por la sección "Continuidad del servicio" más adelante); 
- usar datos recibidos de las API de búsqueda como parte de cualquier actividad de Machine Learning o actividad algorítmica similar para entrenar, evaluar o mejorar los servicios nuevos o existentes que usted o terceros puedan ofrecer;
- modificar el contenido de los resultados (si no es para volver a formatearlos de manera que no infrinja ningún otro requisito) a menos que así lo exija la ley o se acuerde con Microsoft; 
- omitir la atribución y las direcciones URL asociadas al contenido de resultado;
- volver a ordenar, incluidos por omisión, los resultados que se muestran en una respuesta cuando se proporciona un orden o una clasificación (para Bing Custom Search API, esta regla no se aplica a la reordenación implementada a través del portal de customsearch.ai), salvo que la ley lo exija o Microsoft lo acuerde;
- presentar otro contenido dentro de cualquier parte de una respuesta de forma que pueda dar lugar a que un usuario final crea que el otro contenido es parte de la respuesta; 
- mostrar publicidad que no proporciona Microsoft en cualquier página que muestre cualquier parte de una respuesta; mostrar cualquier publicidad con respuestas (i) de las API Bing Image Search, Bing News Search o Bing Video Search; o (ii) que se filtran o limitan principalmente (o exclusivamente) en resultados de imagen, noticias o vídeo.

**C. Avisos y personalización de marca.** 

- Debe incluir en un lugar destacado un hipervínculo funcional a la declaración de privacidad de Microsoft, disponible en https://go.microsoft.com/fwlink/?LinkId=521839, cerca de cada punto en la experiencia del usuario que ofrezca a este la posibilidad de introducir una consulta de búsqueda. Etiquete el vínculo como "declaración de privacidad de Microsoft".
- Debe incluir en un lugar destacado la marca Bing, de forma coherente con las directrices disponibles en https://go.microsoft.com/fwlink/?linkid=833278, cerca de cada punto en la experiencia del usuario que ofrezca a este la posibilidad de introducir una consulta de búsqueda.  Esta marca debe indicar claramente al usuario que es Microsoft quien ofrece la experiencia de búsqueda en Internet.
- Puede atribuir cada respuesta (o parte de una respuesta) de las que se reciben de las API Bing Web, Image, News y Video a Microsoft, como se describe en https://go.microsoft.com/fwlink/?linkid=833278, a menos que Microsoft especifique lo contrario por escrito para su uso. 
- No atribuya respuestas (o partes de respuestas) de las que se reciben de Bing Custom Search API a Microsoft, a menos que Microsoft especifique lo contrario por escrito para su uso particular.


**D. Transferencia de las respuestas.** Si permite a un usuario transferir una respuesta de una API de búsqueda a otro usuario, por ejemplo, a través de una aplicación de mensajería o de su publicación en redes sociales, se aplica lo siguiente: 
- Las respuestas transferidas tienen que:
  - constar de contenido que no se haya modificado en relación al contenido de las respuestas que se presentaron al usuario que realiza la transferencia (los cambios de formato están permitidos);
  - no incluir ningún dato en formato de metadatos;
  - para las respuestas de las API Bing Web, Image, News y Video, presentar texto que indique que la respuesta se obtuvo a través de una experiencia de búsqueda de Internet usando la tecnología de Bing (por ejemplo, "Con tecnología de Bing", "Obtener más información sobre esta imagen en Bing" o mediante el uso del logotipo de Bing);
  - para las respuestas que se reciben de Bing Custom Search API, mostrar texto que indique que la respuesta se obtuvo a través de una experiencia de búsqueda en Internet (por ejemplo, "Obtener más información sobre este resultado de la búsqueda");
  - presentar de forma destacada la consulta completa utilizada para generar la respuesta; e
  - Incluir un vínculo destacado o atribución similar con el origen subyacente de la respuesta, ya sea directamente o a través del motor de búsqueda (bing.com, m.bing.com o su propio servicio de búsqueda personalizado, según sea aplicable).
- No puede automatizar la transferencia de las respuestas. Una transferencia tiene que iniciarse a partir de una acción del usuario que evidencie claramente un intento de transferir una respuesta.
- Solo puede permitir que un usuario transfiera las respuestas que haya obtenido como resultado de la consulta del usuario para realizar la transferencia.

**E. Continuidad del servicio.** No copie, almacene o almacene en caché ningún dato de las respuestas de las API de búsqueda. Sin embargo, para habilitar la continuidad del acceso al servicio y la representación de datos, puede conservar los resultados únicamente con las siguientes condiciones:

**Dispositivo.** Se puede permitir a los usuarios finales conservar los resultados en un dispositivo durante (i) 24 horas desde el momento de la consulta o (ii) hasta que un usuario final envía otra consulta para obtener resultados actualizados, lo que ocurra primero, y siempre que los resultados retenidos se utilicen solamente:

- para permitir que el usuario final acceda a los resultados devueltos anteriormente a ese usuario final en ese dispositivo (por ejemplo, en caso de interrupción del servicio); o
- para almacenar los resultados devueltos para la consulta proactiva personalizada en previsión de las necesidades del usuario final basadas en las señales del usuario final (por ejemplo, en caso de interrupción prevista del servicio).

**Servidor.** Puede conservar los resultados específicos de un solo usuario final de forma segura en un servidor bajo su control y presentar los resultados retenidos solo:

- para permitir al usuario final tener acceso a un informe histórico de resultados devueltos anteriormente para ese usuario en la solución, siempre que los resultados (i) no se conserven más de 21 días desde el momento de la consulta inicial del usuario final y (ii) no se presenten en respuesta a una consulta nueva o repetida de usuario final; o
- para almacenar los resultados devueltos en una consulta proactiva personalizada en previsión de las necesidades de un usuario final basadas en las señales del usuario final durante (i) 24 horas desde el momento de la consulta o (ii) hasta que un usuario final envía otra consulta para obtener resultados actualizados, lo que ocurra primero.

Cuando se conserven, los resultados para un usuario específico no se pueden entremezclar con los resultados de otro usuario, es decir, los resultados de cada usuario tiene que conservarse y entregarse por separado.

**General.** Para cualquier presentación de los resultados retenidos, tiene que:

- incluir un aviso claro y visible de la hora en la que se envió la consulta,
- presentar al usuario un botón o medio similar para volver a consultar y obtener resultados actualizados, 
- conservar la personalización de marca de Bing en la presentación de los resultados y
- eliminar (y actualizar con una consulta nueva si es necesario) los resultados almacenados dentro de los períodos de tiempo especificados.

**F. Detección de direcciones URL fuera de presentación.** Solo puede usar las respuestas de búsqueda en una experiencia de búsqueda sin conexión a Internet con el único fin de detectar direcciones URL de los orígenes de la información, en respuesta a una consulta desde el usuario o el cliente. Puede copiar estas direcciones URL en un informe o una respuesta similar que usted proporcione (i) solo a ese usuario o cliente, en respuesta a esa consulta y (ii) que incluya contenido significativo adicional que tenga valor y sea pertinente para la consulta. Los requisitos de las secciones de 3A a 3E de estos requisitos de uso y visualización no se aplican a este uso fuera de presentación, excepto en los siguientes casos: 

- No almacenará en caché, copiará ni almacenará ningún dato o contenido de, o derivado de, la respuesta de búsqueda, que no sea la copia limitada de dirección URL descrita anteriormente.
- Tiene que garantizar que el uso de los datos (incluidas las direcciones URL) recibidos de las API de búsqueda no incumple ninguna ley aplicable ni vulnera los derechos de terceros.
- No debe usar datos (incluidas las direcciones URL) recibidos desde las API de búsqueda como parte de cualquier índice de búsqueda o aprendizaje automático o actividad algorítmica similar para crear, entrenar, evaluar, o mejorar los servicios que usted o terceros puedan ofrecer.

## <a name="next-steps"></a>Pasos siguientes
[Información general de Answer Search](overview.md)
