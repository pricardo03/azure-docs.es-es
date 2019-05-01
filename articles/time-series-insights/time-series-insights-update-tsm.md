---
title: Modelo de serie temporal en Azure Time Series Insights | Microsoft Docs
description: Comprender el modelo de serie temporal de Azure Time Series Insights.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: eeab01146c938ec118deae08a30af85af4186a2e
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64714064"
---
# <a name="time-series-model"></a>Modelo de Time Series

En este artículo se describe la parte del modelo de serie temporal de la versión preliminar de Azure Time Series Insights. Aquí se analiza el modelo en sí, sus capacidades y cómo comenzar a compilar y actualizar su propio modelo.

Tradicionalmente, los datos que se recopilan de los dispositivos de IoT carecen de información contextual, lo que dificulta encontrar y analizar los sensores rápidamente. El principal objetivo del modelo de serie temporal es simplificar la búsqueda y el análisis de datos de IoT. Para poder lograr este objetivo, permite la protección, el mantenimiento y el enriquecimiento de datos de serie temporal para ayudarle a preparar conjuntos de datos listos para el consumidor. 

Los modelos de serie temporal desempeñan un papel vital en las consultas y la navegación, ya que contextualizan tanto las entidades del dispositivo como las que no lo son. Los datos que se conservan en el modelo de serie temporal potencian los cálculos de consulta de serie temporal aprovechando las fórmulas almacenadas en ellos.

![tsm][1]

## <a name="key-capabilities"></a>Principales capacidades

Con el objetivo de simplificar la gestión de la contextualización de serie temporal, el modelo de serie temporal habilita las siguientes capacidades en la versión preliminar de Time Series Insights. Esto le permitirá realizar lo siguiente:

* Crear y administrar cálculos o fórmulas, transformar los datos aprovechando funciones escalares, agregar operaciones, etc.

* Definir relaciones de tipo primario/secundario para habilitar la navegación y las referencias con el fin de proporcionar contexto a la telemetría de series temporales.

* Defina las propiedades que están asociadas con la parte de instancias de los *campos de instancia* y utilícelas para crear jerarquías.

## <a name="times-series-model-key-components"></a>Componentes clave del modelo de serie temporal

El modelo de serie temporal tiene tres componentes principales:

* *Tipos* de modelo de serie temporal
* *Jerarquías* del modelo de serie temporal
* *Instancias* del modelo de serie temporal

## <a name="time-series-model-types"></a>Tipos de modelo de serie temporal

Los *tipos* de modelo de serie temporal le permiten definir variables o fórmulas para realizar cálculos. Además, estos tipos están asociados a una instancia determinada de Time Series Insights. Un tipo puede tener una o más variables. Por ejemplo, una instancia de Time Series Insights podría ser del tipo *Sensor de temperatura*, que consta de las variables *temperatura media*, *temperatura mínima* y *temperatura máxima*. Se crea un tipo predeterminado cuando los datos comienzan a fluir hacia Time Series Insights. El tipo predeterminado se puede recuperar y actualizar desde la configuración del modelo. Los tipos predeterminados tienen una variable que cuenta el número de eventos.

## <a name="time-series-model-type-json-example"></a>Ejemplo JSON del tipo de modelo de serie temporal

Sample:

```JSON
{
    "name": "SampleType",
    "description": "This is sample type",
    "variables": {
        "Avg Temperature": {
            "kind": "numeric",
            "filter": null,
            "value": { "tsx": "$event.temperature.Double" },
            "aggregation": {"tsx": "avg($value)"}
        },
        "Count Temperature": {
            "kind": "aggregate",
            "filter": null,
            "value": null,
            "aggregation": {"tsx": "count()"}
        }
    }
}
```

Para obtener más información sobre los tipos de modelos de serie temporal, consulte la [documentación de referencia](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#types-api).

## <a name="variables"></a>variables

Los tipos de Time Series Insights tienen variables, que son cálculos con nombre que se realizan con los valores de los eventos. Las definiciones de variables de Time Series Insights contienen fórmulas y reglas de cálculo. Las definiciones de variables incluyen la *clase*, el *valor*, el *filtro*, la *reducción* y los *límites*. Las variables se almacenan en la definición de tipo en el modelo de serie temporal y se pueden proporcionar en línea a través de las API de consulta para anular la definición almacenada.

La siguiente matriz funciona como una leyenda para las definiciones de variables:

![table][2]

### <a name="variable-kind"></a>Tipo de variable

Se admiten los siguientes tipos de variable:

* *Numérica*
* *Agregada*

### <a name="variable-filter"></a>Filtro de variables

Los filtros de variables especifican una cláusula de filtro opcional para restringir el número de filas que se tienen en cuenta en el cálculo, en función de las condiciones.

### <a name="variable-value"></a>Valor de la variable

Los valores de las variables son y deben usarse en el cálculo. Esta es la columna que estará en los eventos a los que debemos referirnos.

### <a name="variable-aggregation"></a>Agregación de la variable

La función de agregado de la variable permite realizar parte del cálculo. Time Series Insights admite agregados normales (a saber, *min*, *max*, *avg*, *sum* y *count*).

## <a name="time-series-model-hierarchies"></a>Jerarquías del modelo de serie temporal

Las jerarquías organizan instancias especificando los nombres de las propiedades y sus relaciones. Es posible tener una o varias jerarquías. Asimismo, no necesitan ser una parte actual de los datos, pero cada instancia debe asignarse a una jerarquía. Una instancia del modelo de serie temporal puede asignarse a una sola o a varias jerarquías.

Las jerarquías se definen por el *id. de jerarquía*, el *nombre* y el *origen*. Igualmente, las jerarquías tienen una ruta de acceso, que es un orden descendente de elementos principales y secundarios de la jerarquía que los usuarios quieren crear. Las propiedades de elementos primarios y secundarios asignan *campos de instancia*.

### <a name="time-series-model-hierarchy-json-example"></a>Ejemplo JSON de la jerarquía del modelo de serie temporal

Sample:

```JSON
{
    "id": "4c6f1231-f632-4d6f-9b63-e366d04175e3",
    "name": "Location",
    "source": {
        "instanceFieldNames": [
                "state",
                "city"
            ]
    }
}
```

Para obtener más información sobre las jerarquías del modelo de serie temporal, consulte la [documentación de referencia](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#hierarchies-api).

### <a name="hierarchy-definition-behavior"></a>Comportamiento de la definición de jerarquía

Observe el siguiente ejemplo donde la jerarquía H1 tiene los valores *edificio*, *piso* y *habitación* como parte de su definición:

```plaintext
 H1 = [“building”, “floor”, “room”]
```

Según los *campos de instancia*, los atributos y valores de la jerarquía aparecen tal como se muestra en la siguiente tabla:

| Identificador de serie temporal | Campos de instancia |
| --- | --- |
| ID1 | "edificio" = "1000", "piso" = "10", "habitación" = "55"  |
| ID2 | "edificio" = "1000", "habitación" = "55" |
| ID3 | "piso" = "10" |
| ID4 | "Edificio" = "1000", "piso" = "10"  |
| ID5 | No se establece ningún valor de "compilación", "piso" o "habitación" |

En el ejemplo anterior, ID1 e ID4 se muestran como parte de la jerarquía H1 en el explorador de Azure Time Series Insights, y el resto de los valores se clasifican en *instancias no primarias*, ya que no se ajustan a la jerarquía de datos especificada.

## <a name="time-series-model-instances"></a>Instancias del modelo de serie temporal

Las instancias son la serie temporal en sí mismas. En la mayoría de los casos, *deviceId* o *assetId* funcionan como el identificador único del recurso en el entorno. Las instancias tienen información descriptiva asociada a ellas que se denomina "propiedades de la instancia". Como mínimo, las propiedades de la instancia incluyen información de la jerarquía. También pueden incluir datos útiles y descriptivos como el fabricante, el operador o la última fecha de servicio.

Las instancias se definen en función de los valores de *typeId*, *timeSeriesId*, *name*, *description*, *hierarchyIds* e *instanceFields*. Cada instancia se asigna a solo un *tipo* y a una o más jerarquías. Asimismo, las instancias heredan todas las propiedades de las jerarquías y se pueden agregar valores de *instanceFields* adicionales para obtener una definición de propiedad de instancia adicional.

Los valores de *instanceFields* son propiedades de una instancia y cualquier dato estático que defina esa instancia. Definen valores de jerarquía o propiedades no jerárquicas a la vez que admiten la indexación para realizar operaciones de búsqueda.

La propiedad *name* es opcional y distingue mayúsculas de minúsculas. Si *name* no está disponible, se establecerá de forma predeterminada en el identificador de serie temporal. Si se especifica un valor para *name*, el identificador de serie temporal seguirá estando disponible en la cuadrícula situada bajo los gráficos del explorador. 

## <a name="time-series-model-instance-json-example"></a>Ejemplo JSON de la instancia del modelo de serie temporal

Sample:

```JSON
{
    "typeId": "1be09af9-f089-4d6b-9f0b-48018b5f7393",
    "timeSeriesId": ["sampleTimeSeriesId"],
    "name": "sampleName",
    "description": "Sample Instance",
    "hierarchyIds": [
        "1643004c-0a84-48a5-80e5-7688c5ae9295"
    ],
    "instanceFields": {
        "state": "California",
        "city": "Los Angeles"
    }
}
```

Para obtener más información sobre las instancias del modelo de serie temporal, consulte la [documentación de referencia](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#instances-api).

## <a name="time-series-model-settings-example"></a>Ejemplo de configuración del modelo de serie temporal

Sample:

```JSON
{
    "modelSettings": {
        "name": "DefaultModel",
        "timeSeriesIdProperties": [
            {
                "name": "id",
                "type": "String"
            }
        ],
        "defaultTypeId": "1be09af9-f089-4d6b-9f0b-48018b5f7393"
    }
}
```

Para obtener más información sobre la configuración del modelo de serie temporal, consulte la [documentación de referencia](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#model-settings-api).

## <a name="next-steps"></a>Pasos siguientes

- Consulte [Azure Time Series Insights Preview storage and ingress](./time-series-insights-update-storage-ingress.md) (Almacenamiento y entrada de datos en la versión preliminar de Azure Time Series Insights).

- Consulte el nuevo [Time Series Model](https://docs.microsoft.com/rest/api/time-series-insights/preview-model) (Modelo de serie temporal).

<!-- Images -->
[1]: media/v2-update-tsm/tsm.png
[2]: media/v2-update-tsm/table.png
