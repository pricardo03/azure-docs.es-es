---
title: Indexación en Azure Cosmos DB
description: Comprenda cómo funciona la indexación en Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/10/2018
ms.author: mjbrown
ms.openlocfilehash: 0333bc5e95b74fc97cfff3d79adbe28aefff5d40
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52834645"
---
# <a name="indexing-in-azure-cosmos-db"></a>Indexación en Azure Cosmos DB

Azure Cosmos DB es una base de datos independiente del esquema que le permite iterar en la aplicación rápidamente sin tener que tratar con la administración de esquemas o índices. De forma predeterminada, Azure Cosmos DB indexa automáticamente todos los elementos en el contenedor sin necesidad de esquemas ni índices secundarios de los desarrolladores.

## <a name="items-as-trees"></a>Elementos como árboles

Si proyectamos los elementos de un contenedor como documentos JSON y los representamos como árboles, Azure Cosmos DB normalizará los valores de estructura y de instancia de los elementos en el concepto unificador de una  **estructura de rutas codificada dinámicamente**. En esta representación, cada etiqueta de un documento JSON, que incluye los nombres de propiedad y sus valores, se convierte en un nodo del árbol. Las hojas del árbol contienen los valores reales y los nodos intermedios contienen la información del esquema. La siguiente imagen representa los árboles creados para dos elementos (1 y 2) de un contenedor:

![Representación de árbol para dos elementos diferentes de un contenedor de Azure Cosmos](./media/index-overview/indexing-as-tree.png)

Se crea un pseudonodo raíz como primario de los nodos reales correspondientes a las etiquetas del documento subyacente. Las estructuras de datos anidadas impulsan la jerarquía del árbol. Los nodos intermedios artificiales etiquetados con valores numéricos (por ejemplo, 0,1,...) se emplean para representar enumeraciones e índices de matrices.

## <a name="index-paths"></a>Rutas de acceso del índice

Azure Cosmos DB proyecta los elementos como documentos JSON y el índice como árboles. Posteriormente, puede ajustar las rutas de acceso a las directivas en el árbol. Puede elegir incluir o excluir rutas de la indexación. Esto puede mejorar el rendimiento de escritura y reducir el almacenamiento necesario para el índice en escenarios en los que se conocen de antemano los patrones de consulta. Para más información, consulte [Rutas de acceso del índice](index-paths.md).

## <a name="indexing-under-the-hood"></a>Indexación: En segundo plano

Azure Cosmos DB aplica la indexación automática a los datos, en la que cada ruta de un árbol se indexa a menos que se haya configurado para excluir determinadas rutas.

Azure Cosmos DB emplea una estructura de datos de índice invertido para almacenar la información de cada elemento y facilitar una representación eficaz para la realización de consultas. El árbol de índice es un documento que se construye con la unión de todos los árboles que representan elementos individuales del contenedor. El árbol de índice crece con el tiempo a medida que se agregan nuevos elementos o se actualizan los ya existentes en el contenedor. A diferencia de la indexación de bases de datos relacionales, Azure Cosmos DB no reinicia la indexación desde el principio cuando se introducen nuevos campos o se agregan nuevos elementos a la estructura existente. 

Cada nodo del árbol de índice es una entrada de índice que contiene los valores de etiqueta y de posición, llamados términos, y los identificadores de los elementos, denominados registros. Los registros entre llaves (por ejemplo {1,2}) en la ilustración del índice invertido se corresponden con los elementos Document1, Document2 que contiene el valor de la etiqueta especificada. Una consecuencia importante de tratar las etiquetas de esquema y los valores de instancia de forma uniforme es que todo se empaqueta en un gran índice. Un valor de instancia que está todavía en las hojas no se repite, puede estar en diferentes roles en los elementos, con diferentes etiquetas de esquema, pero sigue siendo el mismo valor. La siguiente imagen muestra la indexación invertida para distintos elementos:

![Indexación en segundo plano, índice invertido](./media/index-overview/inverted-index.png)

> [!NOTE]
> El índice invertido puede parecerse a las estructuras de indexación utilizadas en un motor de búsqueda en el dominio de la recuperación de información. Con este método, Azure Cosmos DB le permite buscar en la base de datos cualquier elemento independientemente de su estructura de esquema.

Para la ruta normalizada, el índice codifica toda la ruta de reenvío desde la raíz al valor, junto con la información de tipo del valor. La ruta y el valor se codifican para proporcionar distintos tipos de indexación como, por ejemplo, clases de intervalo, espaciales. La codificación de valores está diseñada para proporcionar un valor único o una composición de un conjunto de rutas.

## <a name="querying-with-indexes"></a>Consultas con índices

El índice invertido permite que una consulta identifique los documentos que coinciden con el predicado de consulta rápidamente. Al tratar los valores del esquema y la instancia uniformemente en cuanto a las rutas, el índice invertido también es un árbol. Por tanto, el índice y los resultados se pueden serializar en un documento JSON válido y se pueden devolver como documentos en la representación de árbol. Este método permite la recurrencia sobre los resultados a la hora de realizar consultas adicionales. La siguiente imagen muestra un ejemplo de indexación en una consulta de punto:  

![Ejemplo de consulta de punto](./media/index-overview/index-point-query.png)

Para una consulta por rango, GermanTax es una función definida por el usuario que se ejecuta como parte del procesamiento de consultas. La función definida por el usuario puede ser cualquier función registrada de JavaScript que pueda proporcionar una lógica de programación enriquecida integrada en la consulta. La siguiente imagen muestra un ejemplo de indexación en una consulta por rango:

![Ejemplo de consulta por rango](./media/index-overview/index-range-query.png)

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información acerca de la indexación en los siguientes artículos:

- [Directiva de indexación](index-policy.md)
- [Tipos de índice](index-types.md)
- [Rutas de acceso del índice](index-paths.md)
- [Cómo administrar la directiva de indexación](how-to-manage-indexing-policy.md)
