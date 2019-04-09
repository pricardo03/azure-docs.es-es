---
title: Indexación en Azure Cosmos DB
description: Comprenda cómo funciona la indexación en Azure Cosmos DB.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: rimman
ms.openlocfilehash: ecf53251020ce1b639a5bf8da65f5d31ff699db9
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59265702"
---
# <a name="indexing-in-azure-cosmos-db---overview"></a>Indexación en Azure Cosmos DB: introducción

Azure Cosmos DB es una base de datos independiente del esquema que le permite iterar en la aplicación rápidamente sin tener que tratar con la administración de esquemas o índices. De forma predeterminada, Azure Cosmos DB indexa automáticamente todos los elementos en el contenedor sin necesidad de esquemas ni índices secundarios de los desarrolladores.

## <a name="items-as-trees"></a>Elementos como árboles

Al proyectar elementos en un contenedor como documentos JSON y que se representan como árboles, Azure Cosmos DB normaliza la estructura y los valores de instancia a través de elementos en el concepto unificador de un **codificado dinámicamente la estructura de ruta de acceso** . En esta representación, cada etiqueta de un documento JSON, que incluye los nombres de propiedad y sus valores, se convierte en un nodo del árbol. Las hojas del árbol contienen los valores reales y los nodos intermedios contienen la información de esquema. La siguiente imagen representa los árboles creados para dos elementos (1 y 2) en un contenedor de Azure Cosmos:

![Representación de árbol para dos elementos diferentes de un contenedor de Azure Cosmos](./media/index-overview/indexing-as-tree.png)

Se crea un nodo raíz de pseudo como primario de los nodos reales correspondientes a las etiquetas en el documento JSON debajo. Las estructuras de datos anidadas impulsan la jerarquía del árbol. Los nodos intermedios artificiales etiquetados con valores numéricos (por ejemplo, 0,1,...) se emplean para representar enumeraciones e índices de matrices.

## <a name="index-paths"></a>Rutas de acceso del índice

Azure Cosmos DB proyecta los elementos de un contenedor de Azure Cosmos como documentos JSON y el índice como árboles. A continuación, puede ajustar las directivas de índice para las rutas de acceso dentro del árbol. Puede elegir incluir o excluir rutas de la indexación. Esto puede mejorar el rendimiento de escritura y reducir el almacenamiento necesario para el índice en escenarios en los que se conocen de antemano los patrones de consulta. Para obtener más información, consulte [las rutas de acceso de índice](index-paths.md).

## <a name="indexing-under-the-hood"></a>Indexación: En segundo plano

Se aplica la base de datos de Azure Cosmos *la indexación automática* a los datos, donde se indiza cada ruta de acceso en un árbol, a menos que configure para excluir el determinadas rutas de acceso.

Azure Cosmos DB emplea una estructura de datos de índice invertido para almacenar la información de cada elemento y facilitar una representación eficaz para la realización de consultas. El árbol de índice es un documento que se construye con la unión de todos los árboles que representan los elementos individuales en un contenedor. El árbol de índice aumenta con el tiempo, como los nuevos elementos se agregan o se actualizan los elementos existentes en el contenedor. A diferencia de la indización de base de datos relacional, Azure Cosmos DB no se reinicia la indización de cero, como se introducen nuevos campos. Los nuevos elementos se agregan a la estructura del índice existente. 

Cada nodo del árbol del índice es una entrada de índice que contiene los valores de etiqueta y la posición, llamados el *término*y los identificadores de los elementos, denominados el *registros*. Los registros en las llaves (por ejemplo {1,2}) en la ilustración del índice invertido se corresponden con los elementos como *Document1* y *Document2* que contiene el valor de la etiqueta especificada. Una importante implicación de tratar las etiquetas de esquema y los valores de instancia uniformemente es que todo se empaqueta dentro de un índice grande. Un valor de instancia que está todavía en las hojas no se repite, puede estar en diferentes roles en los elementos, con diferentes etiquetas de esquema, pero sigue siendo el mismo valor. La siguiente imagen muestra la indización invertido para dos elementos diferentes:

![Indexación en segundo plano, índice invertido](./media/index-overview/inverted-index.png)

> [!NOTE]
> El índice invertido puede parecerse a las estructuras de indexación utilizadas en un motor de búsqueda en el dominio de la recuperación de información. Con este método, Azure Cosmos DB le permite buscar en la base de datos cualquier elemento independientemente de su estructura de esquema.

Para la ruta normalizada, el índice codifica toda la ruta de reenvío desde la raíz al valor, junto con la información de tipo del valor. La ruta de acceso y el valor se codifican para proporcionar distintos tipos de indexación como intervalo, espacial, etcetera. La codificación de valores está diseñada para proporcionar un valor único o una composición de un conjunto de rutas.

## <a name="querying-with-indexes"></a>Consultas con índices

El índice invertido permite que una consulta identifique los documentos que coinciden con el predicado de consulta rápidamente. Al tratar el esquema y los valores de instancia uniformemente en cuanto a las rutas de acceso, el índice invertido también es un árbol. Por tanto, el índice y los resultados se pueden serializar en un documento JSON válido y se pueden devolver como documentos en la representación de árbol. Este método permite la recurrencia sobre los resultados a la hora de realizar consultas adicionales. La siguiente imagen muestra un ejemplo de indexación en una consulta de punto:  

![Ejemplo de consulta de punto](./media/index-overview/index-point-query.png)

Para una consulta por rango, *GermanTax* es un [definida por el usuario](stored-procedures-triggers-udfs.md#udfs) ejecutado como parte del procesamiento de consultas. La función definida por el usuario es cualquier función de JavaScript registrada, que puede proporcionar la lógica de programación enriquecida integrada en la consulta. La siguiente imagen muestra un ejemplo de indexación en una consulta por rango:

![Ejemplo de consulta por rango](./media/index-overview/index-range-query.png)

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información acerca de la indexación en los siguientes artículos:

- [Directiva de indexación](index-policy.md)
- [Tipos de índice](index-types.md)
- [Rutas de acceso del índice](index-paths.md)
- [Cómo administrar la directiva de indexación](how-to-manage-indexing-policy.md)
