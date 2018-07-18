---
title: Machine Learning - Azure Cognitive Services | Microsoft Docs
description: Un tutorial de aprendizaje automático en Azure Custom Decision Service, una API basada en la nube para la toma de decisiones contextual.
services: cognitive-services
author: slivkins
manager: slivkins
ms.service: cognitive-services
ms.topic: article
ms.date: 05/08/2018
ms.author: slivkins;marcozo;alekh
ms.openlocfilehash: 50814d67ee39c6657954610358462d877843416e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382155"
---
# <a name="machine-learning"></a>Machine Learning

Este tutorial aborda la funcionalidad de aprendizaje automático avanzado en Custom Decision Service. El tutorial consta de dos partes: [caracterización](#featurization-concepts-and-implementation) y [especificación de características](#feature-specification-format-and-apis). Caracterización hace referencia a la representación de datos como "características" para el aprendizaje automático. La especificación de característica cubre el formato JSON y las API complementarias para especificar características.

De forma predeterminada, el aprendizaje automático en Custom Decision Services transparente para el cliente. Las características se extraen automáticamente del contenido y se usa un algoritmo de aprendizaje de refuerzo estándar. La extracción de características usa otros servicios de Azure Cognitive Services: [Entity Linking](../entitylinking/home.md), [Text Analytics](../text-analytics/overview.md), [Emotion](../emotion/home.md) y [Computer Vision](../computer-vision/home.md). Este tutorial puede omitirse si solo se usa la funcionalidad predeterminada.

## <a name="featurization-concepts-and-implementation"></a>Caracterización: conceptos e implementación

Custom Decision Service toma decisiones de una en una. Cada decisión implica elegir entre varias alternativas, también denominadas acciones. En función de la aplicación, la decisión puede elegir una acción única o una (breve) lista clasificada de acciones.

Por ejemplo, personalizar la selección de artículos de la página principal de un sitio web. En este caso, las acciones se corresponden con los artículos, y cada decisión es qué artículos se deben mostrar a un usuario determinado.

Cada acción se representa mediante un vector de propiedades, en adelante denominadas *características*. Puede especificar nuevas características, además de las características que se extraen automáticamente. También puede indicar a Custom Decision Service que registre algunas características, pero que se ignoren para el aprendizaje automático.

### <a name="native-vs-internal-features"></a>Características nativas e internas

Puede especificar características en un formato natural para la aplicación, ya sea un número, una cadena o una matriz. Estas características se denominan "características nativas". Custom Decision Service traduce cada característica nativa en una o más características numéricas denominadas "características internas".

La traducción a las características internas es la siguiente:

- las características numéricas permanecen invariables.
- una matriz numérica se traduce en varias características numéricas, una para cada elemento de la matriz.
- una característica con valor de cadena `"Name":"Value"` se traduce, de forma predeterminada, en una característica con el nombre `"NameValue"` y el valor 1.
- Opcionalmente, una cadena se puede representar como una [bolsa de palabras](https://en.wikipedia.org/wiki/Bag-of-words_model). A continuación, se crea una característica interna para cada palabra de la cadena, cuyo valor es el número de repeticiones de esta palabra.
- Las características internas con valor cero se omiten.

### <a name="shared-vs-action-dependent-features"></a>Características compartidas y dependientes de la acción

Algunas características hacen referencia a toda la decisión y son iguales para todas las acciones. Las llamamos *características compartidas*. Otras características son específicas de una acción concreta. Las llamamos *características dependientes de la acción* (ADF).

En este ejemplo, las características compartidas podrían describir el usuario o el estado del mundo. Características como la geolocalización, la edad y el sexo del usuario y qué eventos importantes se están produciendo ahora mismo. Las características dependientes de la acción pueden describir las propiedades de un artículo determinado, como los temas tratados en este artículo.

### <a name="interacting-features"></a>Características interactivas

A menudo, las características "interactúan": el efecto de una depende de las otras. Por ejemplo, la característica X es si el usuario está interesado en los deportes. La característica Y es si un artículo determinado trata sobre deportes. En ese caso, el efecto de característica Y depende en gran medida de la característica X.

Para tener en cuenta la interacción entre las características X e Y, cree una característica *cuadrática* cuyo valor sea X\*Y. (También la llamamos combinación X e Y). Puede elegir qué pares de características se combinan.

> [!TIP]
> Una característica compartida se debe combinar con características dependientes de la acción para influir en su clasificación. Una característica dependiente de la acción se debe combinar con características compartidas para contribuir a la personalización.

En otras palabras, una característica compartida no combinada con ningún ADF influye en cada acción de la misma manera. Una ADF no combinada con ninguna característica compartida influye también en cada decisión. Estos tipos de características pueden reducir la varianza de las estimaciones de recompensa.

### <a name="implementation-via-namespaces"></a>Implementación a través de espacios de nombres

Puede implementar características combinadas (así como otros conceptos de caracterización) a través de la "línea de comandos VW" en el Portal. La sintaxis se basa en la línea de comandos [Vowpal Wabbit](http://hunch.net/~vw/).

En el centro de la implementación se encuentra el concepto de *espacio de nombres*: un subconjunto de características con nombre. Cada característica pertenece exactamente a un espacio de nombres. El espacio de nombres se puede especificar explícitamente cuando el valor de la característica se proporciona a Custom Decision Service. Es la única manera de hacer referencia a una característica de la línea de comandos de VW.

Un espacio de nombres es "compartido" o "dependiente de la acción", en función de si consta solo de características compartidas, o consta únicamente de características dependientes de la acción de la misma acción.

> [!TIP]
> Es un procedimiento recomendado encapsular características en espacios de nombres especificados explícitamente. Agrupe las características relacionadas en el mismo espacio de nombres.

Si no se proporciona el espacio de nombres, la característica se asigna automáticamente al espacio de nombres predeterminado.

> [!IMPORTANT]
> Las características y los espacios de nombres no necesitan ser coherentes entre acciones. En concreto, un espacio de nombres puede tener características diferentes para distintas acciones. Además, un espacio de nombres concreto puede definirse para algunas acciones y no para otras.

Varias características internas que proceden de la misma característica nativa con valores de cadena se agrupan en el mismo espacio de nombres. Dos características nativas cualesquiera que se encuentren en diferentes espacios de nombres se tratan como distintas, incluso si tienen el mismo nombre de característica.

> [!IMPORTANT]
> Mientras que los identificadores de espacios de nombres largos y descriptivos son comunes, la línea de comandos de VW no distingue entre los espacios de nombres cuyos identificadores empiezan por la misma letra. A continuación, los identificadores de espacio de nombres son solo letras, como `x` e `y`.

Los detalles de implementación son los siguientes:

- Para combinar los espacios de nombres `x` y `y`, escriba `-q xy` o `--quadratic xy`. A continuación, cada característica de `x` se combina con cada característica de `y`. Use `-q x:` para combinar `x` con cada espacio de nombres, y `-q ::` para combinar todos los pares de espacios de nombres.

- Para ignorar todas las características del espacio de nombres `x`, escriba `--ignore x`.

Estos comandos se aplican a cada acción por separado cada vez que se definen los espacios de nombres.

### <a name="estimated-average-as-a-feature"></a>Promedio estimado como característica

Como experimento de pensamiento, ¿cuál sería el promedio de recompensa de una determinada acción si se eligiera para todas las decisiones? Este promedio de recompensa podría utilizarse como medida de "calidad general" de esta acción. No se sabe exactamente cuándo se han elegido otras acciones en su lugar en algunas decisiones. Sin embargo, se puede calcular a través de técnicas de aprendizaje de refuerzo. Normalmente, la calidad de esta estimación mejora con el tiempo.

Puede elegir incluir esta "recompensa de promedio estimada" como una característica para una acción determinada. A continuación, Custom Decision Service actualizaría la estimación cuando llegaran datos nuevos. Esta característica se denomina *característica marginal* de la acción. Las características marginales pueden utilizarse para el aprendizaje automático y para auditorías.

Para agregar características marginales, escriba `--marginal <namespace>` en la línea de comandos de VW. Defina `<namespace>` en JSON como se indica a continuación:

```json
{<namespace>: {"mf_name":1 "action_id":1}
```

Inserte este espacio de nombres junto con otras características dependientes de la acción de una acción determinada. Proporcione la definición de cada decisión, usando el mismo valor de `mf_name` y `action_id` para todas las decisiones.

La característica marginal se agrega para cada acción con `<namespace>`. `action_id` puede ser cualquier nombre de característica que identifique de forma única la acción. El nombre de la característica se define como `mf_name`. En concreto, las características marginales con diferentes valores de `mf_name` se tratan como características distintas: se aprende una ponderación distinta para cada valor de `mf_name`.

El uso predeterminado es que `mf_name` sea el mismo para todas las acciones. A continuación, se aprende una ponderación para todas las características marginales.

También puede especificar varias características marginales para la misma acción, con los mismos valores, pero nombres de características diferentes.

```json
{<namespace>: {"mf_name1":1 "action_id":1 "mf_name2":1 "action_id":1}}
```

### <a name="1-hot-encoding"></a>Codificación one-hot

Puede elegir representar algunas características como vectores de bits, donde cada bit corresponde a un intervalo de valores posibles. Este bit se establece en 1 si y solo si la característica se encuentra en este intervalo. Por lo tanto, hay un bit "en caliente" que se define como 1 y los demás se establecen en 0. Esta representación se suele denominar *codificación one-hot*.

La codificación one-hot es típica de las características de categorías, como "región geográfica", que no tienen ninguna representación numérica inherentemente significativa. También es conveniente para características numéricas cuya influencia en la recompensa es, probablemente, no lineal. Por ejemplo, un artículo determinado podría ser relevante para un determinado grupo de edad e irrelevante para cualquier otra persona más mayor o joven.

Cualquier característica con valores de cadena tiene codificación one-hot de forma predeterminada: se crea una característica interna distinta para cada valor posible. Actualmente, no se proporciona codificación one-hot automática para características numéricas y/o con intervalos personalizados.

> [!TIP]
> Los algoritmos de aprendizaje automático tratan todos los valores posibles de una característica interna concreta de forma uniforme: a través de una "ponderación" común. La codificación one-hot permite una "ponderación" independiente para cada intervalo de valores. Hacer intervalos más pequeños da lugar a mejores premios una vez que se recopilan datos suficientes, pero puede que aumente la cantidad de datos que se deben reunir para conseguir premios mejores.

## <a name="feature-specification-format-and-apis"></a>Especificación de características: formato y API

Puede especificar características a través de varias API auxiliares. Todas las API utilizan un formato JSON común. A continuación se muestran las API y el formato en un nivel conceptual. La especificación se complementa con un esquema de Swagger.

La plantilla básica de JSON para la especificación de la característica es la siguiente:

```json
{
"<name>":<value>, "<name>":<value>, ... ,
"namespace1": {"<name>":<value>, ... },
"namespace2": {"<name>":<value>, ... },
...
}
```

Aquí, `<name>` y `<value>` representan el nombre y el valor de la característica, respectivamente. `<value>` puede ser una cadena, un número entero, un valor float, un valor booleano o una matriz. Si una característica no está encapsulada en un espacio de nombres, se asigna automáticamente al espacio de nombres predeterminado.

Para representar una cadena como una bolsa de palabras, use una sintaxis especial `"_text":"string"`, en lugar de `"<name>":<value>`. De hecho, se crea una característica interna independiente para cada palabra de la cadena. Su valor es el número de repeticiones de esta palabra.

Si `<name>` empieza por "_" (y no es `"_text"`), se ignora la característica.

> [!TIP]
> A veces se combinan características de varios orígenes JSON. Para mayor comodidad, puede representarlas como se indica a continuación:
>
> ```json
> {
> "source1":<features>,
> "source2":<features>,
> ...
> }
> ```

Aquí, `<features>` hace referencia a la especificación de la característica básica definida previamente. También se permiten niveles más profundos de "anidamiento". Custom Decision Service busca automáticamente los objetos JSON "más profundos" que se pueden interpretar como `<features>`.

#### <a name="feature-set-api"></a>Feature Set API

Feature Set API devuelve una lista de características en el formato JSON que se ha descrito anteriormente. Puede usar varios puntos de conexión de Feature Set API. Cada punto de conexión se identifica mediante el identificador de conjunto de características y una dirección URL. La asignación entre las direcciones URL y los identificadores del conjunto de características se establece en el portal.

Llame a Feature Set API insertando el identificador de conjunto de características correspondiente en el lugar adecuado en JSON. Para las características dependientes de la acción, la llamada se parametriza automáticamente mediante el identificador de acción. Puede especificar varios identificadores de conjunto de características para la misma acción.

#### <a name="action-set-api-json-version"></a>Action Set API (versión JSON)

Feature Set API tiene una versión en la que las acciones y las características se especifican en JSON. Las características se pueden especificar explícitamente o a través de instancias de Feature Set API. Las características compartidas pueden especificarse una vez para todas las acciones.

#### <a name="ranking-api-http-post-call"></a>Ranking API (llamada HTTP POST)

Ranking API tiene una versión que usa la llamada HTTP POST. El cuerpo de esta llamada especifica acciones y características a través de una sintaxis JSON flexible.

Las acciones se pueden especificar explícitamente o a través de identificadores de conjuntos de acciones. Cuando se encuentra un identificador de conjunto de acciones, se realiza una llamada al punto de conexión de Action Set API correspondiente.

Para Action Set API, las características se pueden especificar explícitamente o a través de instancias de Feature Set API. Las características compartidas pueden especificarse una vez para todas las acciones.