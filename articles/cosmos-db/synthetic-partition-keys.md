---
title: Claves de partición sintéticas en Azure Cosmos DB
description: Obtenga información sobre cómo usar claves de partición sintéticas en los contenedores de Azure Cosmos DB.
author: aliuy
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: andrl
ms.openlocfilehash: f6323549c0ecf8f0196d327779f7f7cb67c6e03f
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51263350"
---
# <a name="create-a-synthetic-partition-key"></a>Creación de una clave de partición sintética

Se recomienda tener una clave de partición con muchos valores distintos, como centenares o miles. El objetivo es distribuir los datos y la carga de trabajo uniformemente entre los elementos asociados con estos valores de clave de partición. Si esta propiedad no existe en los datos, se puede crear una clave de partición sintética. En las secciones siguientes se describen varias técnicas básicas para generar una clave de partición sintética para el contenedor.

## <a name="concatenating-multiple-properties-of-an-item"></a>Concatenación de varias propiedades de un elemento

Puede formar una clave de partición mediante la concatenación de varios valores de propiedades en una única propiedad artificial `partitionKey`. Estas claves se conocen como claves sintéticas. Por ejemplo, considere el siguiente documento de ejemplo:

```JavaScript
{
"deviceId": "abc-123",
"date": 2018
}
```

Para el documento anterior, una opción es establecer /deviceId o /date como clave de partición, si desea dividir el contenedor en función de los valores identificador de dispositivo o fecha. Otra opción es concatenar estos dos valores en una propiedad `partitionKey` sintética que se usa como clave de partición.

```JavaScript
{
"deviceId": "abc-123",
"date": 2018,
"partitionKey": "abc-123-2018"
}
```

En escenarios en tiempo real, puede tener miles de documentos en una base de datos, por lo que, en lugar de agregar la clave sintética manualmente, debe definir la lógica del lado cliente para concatenar los valores e insertar la clave sintética en los documentos.

## <a name="using-a-partition-key-with-random-suffix"></a>Uso de una clave de partición con un sufijo aleatorio

Otra posible estrategia para distribuir más uniformemente la carga de trabajo es anexar un número aleatorio al final del valor de la clave de partición. La distribución de los elementos de esta forma permite realizar operaciones de escritura en paralelo entre las particiones.

Por ejemplo, si una clave de partición representa una fecha, puede elegir un número aleatorio entre 1 y 400 y concatenarlo como sufijo a la fecha. Este método da como resultado valores de clave de partición como 2018-08-09.1, 2018-08-09.2, etc., hasta 2018-08-09.400. Dado que la clave de partición se genera aleatoriamente, las operaciones de escritura del contenedor cada día se reparten uniformemente entre varias particiones. Este método mejora el paralelismo y el rendimiento general.

## <a name="using-a-partition-key-with-pre-calculated-suffixes"></a>Uso de una clave de partición con sufijos calculados previamente 

Aunque la estrategia de aleatoriedad puede mejorar considerablemente el rendimiento de escritura, es difícil leer un elemento específico, puesto que desconoce el valor de sufijo que se usa al escribir el elemento. Para que sea más fácil leer los elementos individuales, puede usar la estrategia de sufijos calculados previamente. En lugar de usar un número aleatorio para distribuir los elementos entre las particiones, use un número calculado en función de algo que desea consultar.

Considere el ejemplo anterior, donde un contenedor utiliza una fecha en la clave de partición. Ahora suponga que cada elemento tiene un atributo de bastidor (número de identificación del vehículo) accesible y que a menudo se ejecutan consultas para buscar elementos por el número de bastidor, además de la fecha. Antes de que la aplicación escriba el elemento en el contenedor, puede calcular un sufijo hash según el número de bastidor y anexarlo a la fecha de la clave de partición. El cálculo podría generar un número entre 1 y 400 que se distribuya de manera uniforme, de manera similar a los resultados generados con el método de la estrategia aleatoria. El valor de la clave de partición será entonces la fecha concatenada con el resultado calculado.

Con esta estrategia, las escrituras se distribuyen uniformemente entre los valores de clave de partición y entre todas las particiones. Puede leer fácilmente un elemento y una fecha concretos, porque puede calcular el valor de la clave de partición para un número de bastidor específico. La ventaja de este método es que puede evitar la creación de una única clave de partición frecuente (la clave de partición que soporta toda la carga de trabajo). 

## <a name="next-steps"></a>Pasos siguientes

Puede obtener más información sobre el concepto de la creación de particiones en los siguientes artículos:

* Más información sobre las [particiones lógicas](partition-data.md)
* Más información sobre el [aprovisionamiento del rendimiento de contenedores y bases de datos de Cosmos](set-throughput.md)
* Más información sobre [cómo aprovisionar rendimiento en un contenedor de Cosmos](how-to-provision-container-throughput.md)
* Más información sobre [cómo aprovisionar rendimiento en una base de datos de Cosmos](how-to-provision-database-throughput.md)
