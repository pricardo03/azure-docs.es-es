---
title: Cree una clave de partición sintética en Azure Cosmos DB para distribuir la carga de trabajo y los datos uniformemente.
description: Aprenda a usar claves de partición sintéticas en los contenedores de Azure Cosmos
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: rimman
ms.openlocfilehash: 1fd436746dcd2e93a1699ac5c68965213c74580e
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65978877"
---
# <a name="create-a-synthetic-partition-key"></a>Creación de una clave de partición sintética

Es la práctica recomendada para tener una clave de partición con muchos valores distintos, por ejemplo, cientos o miles. El objetivo es distribuir los datos y la carga de trabajo uniformemente entre los elementos asociados con estos valores de clave de partición. Si esta propiedad no existe en los datos, puede construir un *clave de partición sintéticas*. Este documento describe varias técnicas básicas para generar una clave de partición sintética para el contenedor de Cosmos.

## <a name="concatenate-multiple-properties-of-an-item"></a>Concatenación de varias propiedades de un elemento

Puede formar una clave de partición mediante la concatenación de varios valores de propiedades en una única propiedad artificial `partitionKey`. Estas claves se conocen como claves sintéticas. Por ejemplo, considere el siguiente documento de ejemplo:

```JavaScript
{
"deviceId": "abc-123",
"date": 2018
}
```

Para el documento anterior, una opción es establecer /deviceId o /date como clave de partición. Use esta opción, si desea dividir el contenedor en función de los valores de Id. de dispositivo o de fecha. Otra opción es concatenar estos dos valores en una propiedad `partitionKey` sintética que se usa como clave de partición.

```JavaScript
{
"deviceId": "abc-123",
"date": 2018,
"partitionKey": "abc-123-2018"
}
```

En escenarios en tiempo real, puede tener miles de elementos en una base de datos. En lugar de agregar manualmente la clave sintética, definir la lógica del lado cliente para concatenar los valores e inserte la clave sintética en los elementos de los contenedores de Cosmos.

## <a name="use-a-partition-key-with-a-random-suffix"></a>Uso de una clave de partición con un sufijo aleatorio

Otra posible estrategia para distribuir más uniformemente la carga de trabajo es anexar un número aleatorio al final del valor de la clave de partición. La distribución de los elementos de esta forma permite realizar operaciones de escritura en paralelo entre las particiones.

Por ejemplo, si una clave de partición representa una fecha, Puede elegir un número aleatorio entre 1 y 400 y concatenarlo como sufijo a la fecha. Este método da como resultado valores de clave de partición como `2018-08-09.1`,`2018-08-09.2`, y así sucesivamente, a través de `2018-08-09.400`. Dado que la clave de partición se genera aleatoriamente, las operaciones de escritura del contenedor cada día se reparten uniformemente entre varias particiones. Este método mejora el paralelismo y el rendimiento general.

## <a name="use-a-partition-key-with-pre-calculated-suffixes"></a>Usar una clave de partición con sufijos calculados previamente 

La estrategia de sufijo aleatorio puede mejorar considerablemente el rendimiento de escritura, pero resulta difícil de leer un elemento específico. No conoce el valor de sufijo que se usó cuando se escribió el artículo. Para que sea más fácil de leer los elementos individuales, use la estrategia de sufijos calculados previamente. En lugar de usar un número aleatorio para distribuir los elementos entre las particiones, use un número que se calcula basándose en un elemento que desee consultar.

Considere el ejemplo anterior, donde utiliza un contenedor de una fecha como clave de partición. Ahora suponga que cada elemento tiene un `Vehicle-Identification-Number` (`VIN`) el atributo que se va a tener acceso. Además, suponga que a menudo se ejecutan consultas para buscar elementos por el `VIN`, además de fecha. Antes de que la aplicación escriba el elemento en el contenedor, puede calcular un sufijo hash según el número de bastidor y anexarlo a la fecha de la clave de partición. El cálculo podría generar un número entre 1 y 400 que se distribuye uniformemente. Este resultado es similar a los resultados producidos por el método de la estrategia de sufijo aleatorio. El valor de la clave de partición será entonces la fecha concatenada con el resultado calculado.

Con esta estrategia, las escrituras se distribuyen uniformemente entre los valores de clave de partición y entre todas las particiones. Puede leer fácilmente un elemento determinado y la fecha, porque puede calcular el valor de clave de partición para un determinado `Vehicle-Identification-Number`. La ventaja de este método es que puede evitar la creación de una clave única partición activa, es decir, una clave de partición que toma la carga de trabajo. 

## <a name="next-steps"></a>Pasos siguientes

Puede obtener más información sobre el concepto de la creación de particiones en los siguientes artículos:

* Más información sobre las [particiones lógicas](partition-data.md).
* Obtenga más información sobre el [aprovisionamiento del rendimiento de contenedores y bases de datos de Azure Cosmos](set-throughput.md).
* Obtenga más información sobre el [aprovisionamiento del rendimiento de un contenedor de Azure Cosmos](how-to-provision-container-throughput.md).
* Obtenga más información sobre el [aprovisionamiento del rendimiento de una base de datos de Azure Cosmos](how-to-provision-database-throughput.md).
