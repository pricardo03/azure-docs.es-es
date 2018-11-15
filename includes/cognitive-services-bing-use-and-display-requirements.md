---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: MikeDodaro
ms.service: cognitive-services
ms.topic: include
ms.custom: include file
ms.date: 04/19/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: f166ceac1ae848565f861a94781ce0500c24747e
ms.sourcegitcommit: 02ce0fc22a71796f08a9aa20c76e2fa40eb2f10a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "51289162"
---
Los requisitos de uso y visualización se aplican a todas las implementaciones del contenido y a la información asociada. Por ejemplo, los requisitos se aplican a las relaciones, los metadatos y otras señales. Estas pueden estar disponibles a través de llamadas a las siguientes API:

- Bing Custom Search
- Bing Entity Search
- Bing Image Search
- Bing News Search
- Bing Video Search
- Bing Visual Search
- Bing Web Search
- Bing Spell Check
- Bing Autosuggest

Puede encontrar detalles de implementación relacionados con estos requisitos en la documentación de las características y resultados específicos.     

## <a name="bing-spell-check-and-bing-autosuggest-apis"></a>Bing Spell Check API y Bing Autosuggest API

No:

- Copie, almacene o almacene en caché los datos que reciba de Bing Spell Check API o de Bing Autosuggest API.
- Use los datos que reciba de Bing Spell Check API o de Bing Autosuggest API como parte de cualquier actividad de aprendizaje automático u otra actividad algorítmica similar. No utilice estos datos para entrenar, evaluar o mejorar los servicios nuevos o existentes que pueda ofrecer usted u otros terceros.

## <a name="definitions"></a>Definiciones

- *Respuesta* hace referencia a una categoría de resultados devueltos en una respuesta. Por ejemplo, una respuesta de Bing Web Search API puede incluir respuestas en las categorías de resultados de página web, imágenes, vídeos, objetos visuales y noticias.   
- *Respuesta* significa una y todas las respuestas y los datos asociados recibidos en respuesta a una sola llamada a una API de búsqueda.
- *Resultado* hace referencia a un elemento de información en una respuesta. Por ejemplo, el conjunto de datos conectados con un único artículo de noticias es un resultado en una respuesta de noticias.
- El término *API de búsqueda* hace referencia de manera colectiva a estas API: Bing Custom Search, Entity Search, Image Search, News Search, Video Search, Visual Search y Web Search. 


## <a name="search-apis"></a>API de búsqueda

Los requisitos de esta sección se aplican a todas las API de búsqueda. Las API de búsqueda no incluyen Bing Spell Check ni Bing Autosuggest. Los requisitos de esas dos API se describen en la sección anterior.

### <a name="internet-search-experience"></a>Experiencia de búsqueda en Internet

Todos los datos devueltos en las respuestas solo pueden usarse en experiencias de búsqueda en Internet. Una experiencia de búsqueda en Internet significa que el contenido presentado, según corresponda: 
- Es pertinente y responde a la consulta directa del usuario final u otra indicación del interés y propósito de búsqueda del usuario (por ejemplo, una consulta de búsqueda indicada por el usuario). 
- Ayuda a los usuarios a encontrar los orígenes de los datos y a desplazarse hasta ellos (por ejemplo, las direcciones URL proporcionadas se implementan como hipervínculos por lo que el contenido o la atribución es un vínculo interactivo que se presenta claramente con los datos). O, si se usa Bing Entity Search API, hay un vínculo visible a la dirección URL de bing.com en la respuesta que permite al usuario desplazarse a los resultados de búsqueda obtenidos con la consulta pertinente en bing.com.
- Incluye varios resultados para que el usuario seleccione entre ellos (por ejemplo, se presentan varios resultados de la respuesta de noticias o todos los resultados si se devuelven muy pocos). 
- Está limitado a una cantidad apropiada para atender al propósito de búsqueda (por ejemplo, las imágenes en miniatura se muestran en un tamaño miniatura proporcional a la pantalla del usuario). 
- Incluye una indicación visible para el usuario que informa de que el contenido se compone de resultados de búsqueda de Internet (por ejemplo, una afirmación de que el contenido es "de la Web").
- Incluye cualquier otra combinación de las medidas necesarias para garantizar que el uso de los datos recibidos de las API de búsqueda no incumple ninguna ley aplicable, ni vulnera los derechos de terceros. Por ejemplo, si se depende de una licencia de Creative Commons, cumplirá con los términos de licencia aplicables. Consulte con sus asesores legales para determinar las medidas que pueden ser adecuadas.
La única excepción a los requisitos de experiencia de búsqueda de Internet es para la detección de direcciones URL tal como se describe más abajo en este artículo. 

### <a name="restrictions"></a>Restricciones

No:

- Copiar, almacenar o almacenar en caché ningún dato de las respuestas (excepto la retención en la medida permitida por la sección "Continuidad del servicio" más adelante en este artículo). 
- Utilizar los datos recibidos de las API de búsqueda como parte de cualquier actividad de aprendizaje automático u otra actividad algorítmica similar. No utilice estos datos para entrenar, evaluar o mejorar los servicios nuevos o existentes que pueda ofrecer usted u otros terceros.
- Modificar el contenido de los resultados (en otra forma que no sea el volver a darles un formato que no infrinja cualquier otro requisito) a menos que así lo exija la ley o un acuerdo con Microsoft. 
- Omitir la atribución y las direcciones URL asociadas al contenido de resultado.
- Volver a ordenar (también por omisión) los resultados que se presentan en una respuesta cuando se proporciona un orden o una clasificación, a menos que así lo exija la ley o un acuerdo con Microsoft. (Para Bing Custom Search API, esta regla no se aplica a la reordenación implementada a través del portal customsearch.ai).
- Presentar otro contenido dentro de cualquier parte de una respuesta de forma que pueda dar lugar a que un usuario crea que el otro contenido es parte de la respuesta. 
- Presentar publicidad que no proporcione Microsoft en cualquier página que muestre cualquier parte de una respuesta. 
- Presentar publicidad con respuestas (i) de las API Bing Image, News Search, Video Search o Visual Search APIs, o bien (ii) que se filtran o limitan principalmente (o exclusivamente) en resultados de imagen, noticias o vídeo.

### <a name="notices-and-branding"></a>Avisos y personalización de marca 

- Debe incluir en un lugar destacado un hipervínculo funcional a la [declaración de privacidad de Microsoft](https://go.microsoft.com/fwlink/?LinkId=521839), cerca de cada punto en la experiencia del usuario que ofrezca a este la posibilidad de introducir una consulta de búsqueda. Etiquete el vínculo como **declaración de privacidad de Microsoft**.
- Debe incluir en un lugar destacado la marca Bing, de forma coherente con lo indicado en [Bing Trademark Usage Guidelines](https://go.microsoft.com/fwlink/?linkid=833278) (Directrices de uso de la marca comercial Bing), cerca de cada punto en la experiencia del usuario que ofrezca a este la posibilidad de introducir una consulta de búsqueda. Esta marca debe indicar claramente al usuario que es Microsoft quien ofrece la experiencia de búsqueda en Internet.
- Puede atribuir cada respuesta (o parte de una respuesta) de las que se reciben de las API Bing Web Search, Image Search, News Search, Video Search y Visual Search a Microsoft, a menos que Microsoft especifique lo contrario por escrito para su uso. Esto se describe en [Bing Trademark Usage Guidelines](https://go.microsoft.com/fwlink/?linkid=833278) (Directrices de uso de la marca comercial Bing). 
- No atribuya respuestas (o partes de respuestas) de las que se reciben de Bing Custom Search API a Microsoft, a menos que Microsoft especifique lo contrario por escrito para su uso particular.

### <a name="transferring-responses"></a>Transferencia de las respuestas

Si permite a un usuario transferir una respuesta de una API de búsqueda a otro usuario, por ejemplo, a través de una aplicación de mensajería o de su publicación en redes sociales, se aplica lo siguiente: 
- Las respuestas transferidas tienen que:
  - Constar de contenido que no se haya modificado en relación al contenido de las respuestas que se presentaron al usuario que realiza la transferencia. Los cambios de formato están permitidos.
  - No incluir ningún dato en formato de metadatos.
  - Para las respuestas que se reciben de las API Bing Web, Image, News, Video y Visual, presente texto que indique que la respuesta se obtuvo a través de una experiencia de búsqueda de Internet usando la tecnología de Bing. Por ejemplo, puede presentar un texto como "Con tecnología de Bing" u "Obtener más información sobre esta imagen en Bing", o puede utilizar el logotipo de Bing.
  - Para las respuestas que se reciben de Bing Custom Search API, presente un texto que indique que la respuesta se obtuvo a través de una experiencia de búsqueda en Internet. Por ejemplo, presente un texto como "Obtener más información sobre este resultado de búsqueda”.
  - Presentar de forma destacada la consulta completa utilizada para generar la respuesta.
  - Incluir un vínculo destacado o atribución similar con el origen subyacente de la respuesta, ya sea directamente o a través del motor de búsqueda (bing.com, m.bing.com o su propio servicio de búsqueda personalizado, según sea aplicable).
- No puede automatizar la transferencia de las respuestas. Una transferencia tiene que iniciarse a partir de una acción del usuario que evidencie claramente un intento de transferir una respuesta.
- Solo puede permitir que un usuario transfiera las respuestas que haya obtenido como resultado de la consulta del usuario para realizar la transferencia.

### <a name="continuity-of-service"></a>Continuidad del servicio 

No copie, almacene o almacene en caché ningún dato de las respuestas de las API de búsqueda. Sin embargo, para habilitar la continuidad del acceso al servicio y la representación de datos, puede conservar los resultados únicamente con las siguientes condiciones:

**Dispositivo.** Se puede permitir a los usuarios conservar los resultados en un dispositivo durante (i) 24 horas desde el momento de la consulta o (ii) hasta que un usuario envía otra consulta para obtener resultados actualizados, lo que ocurra primero, y siempre que los resultados retenidos se utilicen solamente:

- Para permitir que el usuario acceda a los resultados devueltos anteriormente a ese usuario en ese dispositivo (por ejemplo, en caso de interrupción del servicio).
- Para almacenar los resultados devueltos para la consulta proactiva personalizada en previsión de las necesidades del usuario basadas en las señales del usuario (por ejemplo, en caso de interrupción prevista del servicio).

**Servidor.** Puede conservar los resultados específicos de un solo usuario de forma segura en un servidor bajo su control y presentar los resultados retenidos solo:

- Para permitir que el usuario final acceda a un informe histórico de resultados devueltos anteriormente para ese usuario en la solución. Los resultados (i) no se pueden conservar más de 21 días desde el momento de la consulta inicial del usuario final y (ii) no se deben presentar en respuesta a una consulta nueva o repetida de un usuario.
- Para almacenar los resultados devueltos para la consulta proactiva personalizada en previsión de las necesidades del usuario basadas en las señales del usuario. Puede almacenar estos resultados durante (i) 24 horas desde el momento de la consulta o (ii) hasta que un usuario envíe otra consulta para obtener resultados actualizados, el período que resulte menor.

Cuando se conserven, los resultados para un usuario específico no se pueden entremezclar con los resultados de otro usuario. Es decir, los resultados de cada usuario tienen que conservarse y entregarse por separado.

### <a name="general"></a>General 

Para cualquier presentación de los resultados retenidos, tiene que:

- Incluir un aviso claro y visible de la hora en la que se envió la consulta.
- Presentar al usuario un botón o medio similar para volver a consultar y obtener resultados actualizados. 
- Conservar la personalización de marca de Bing en la presentación de los resultados.
- Eliminar (y actualizar con una consulta nueva si es necesario) los resultados almacenados dentro de los períodos de tiempo especificados.

### <a name="non-display-url-discovery"></a>Detección de direcciones URL fuera de presentación 

Solo puede usar las respuestas de búsqueda en una experiencia de búsqueda sin conexión a Internet con el único fin de detectar direcciones URL de los orígenes de la información, en respuesta a una consulta desde el usuario o el cliente. Puede copiar estas direcciones URL en un informe o en una respuesta similar que usted proporcione:

- Solo a ese usuario o cliente, en respuesta a esa consulta.
- Solo si incluye un contenido significativo adicional que tenga valor y sea relevante para la consulta.

Los requisitos de uso y presentación de las secciones anteriores de las API de búsqueda no se aplican a este uso fuera de presentación, excepto en los siguientes casos: 

- No debe almacenar en caché, copiar o almacenar ningún dato o contenido de, o derivado de, la respuesta de búsqueda, que no sea la copia limitada de dirección URL que se ha descrito anteriormente.
- Tiene que garantizar que el uso de los datos (incluidas las direcciones URL) recibidos de las API de búsqueda no incumple ninguna ley aplicable ni vulnera los derechos de terceros.
- No debe usar los datos (incluidas las direcciones URL) recibidos desde las API de búsqueda como parte de cualquier índice de búsqueda o aprendizaje automático o actividad algorítmica similar. No utilice estos datos para entrenar, evaluar o mejorar los servicios que pueda ofrecer usted u otros terceros.

## <a name="gdpr-compliance"></a>Cumplimiento del GDPR  

Con respecto a los datos personales sujetos al Reglamento general de protección de datos (RGPD) y que se procesan en relación con las llamadas a las API Search, Bing Spell Check o Bing Autosuggest, comprende que usted y Microsoft son controladores independientes de los datos sujetos al RGPD. Usted es responsable de manera independiente del cumplimiento del Reglamento general de protección de datos.  

