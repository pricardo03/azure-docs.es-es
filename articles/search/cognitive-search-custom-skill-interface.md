---
title: 'Definición de interfaz para aptitudes personalizadas en Cognitive Search: Azure Search'
description: Interfaz de extracción de datos personalizada para la aptitud personalizada de Web API en la canalización de Cognitive Search en Azure Search.
manager: pablocas
author: luiscabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: e181aab3d92d8111a0a7d1d41bbddac20687a547
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67668881"
---
# <a name="how-to-add-a-custom-skill-to-a-cognitive-search-pipeline"></a>Cómo agregar una aptitud personalizada a una canalización de Cognitive Search

Se puede crear una [canalización de indexación de Cognitive Search](cognitive-search-concept-intro.md) en Azure Search a partir de [aptitudes predefinidas](cognitive-search-predefined-skills.md) y [aptitudes personalizadas](cognitive-search-custom-skill-web-api.md) que puede crear y agregar personalmente a la canalización. En este artículo, obtendrá información sobre cómo crear una aptitud personalizada que exponga una interfaz para que pueda incluirse en una canalización de Cognitive Search. 

Crear una aptitud personalizada le otorga una forma de insertar transformaciones únicas en el contenido. Una aptitud personalizada se ejecuta de forma independiente; además, puede aplicar cualquier paso de enriquecimiento que quiera. Por ejemplo, puede definir entidades personalizadas específicas de campo, crear modelos de clasificación personalizados para diferenciar contratos y documentos comerciales y financieros, o agregar una aptitud de reconocimiento de voz para profundizar en el contenido relevante de los archivos de audio. Para ver un ejemplo paso a paso, consulte [Ejemplo: Creación de una aptitud personalizada de búsqueda cognitiva](cognitive-search-create-custom-skill-example.md).

 Cualquiera que sea la capacidad personalizada que necesite, existe una interfaz simple y clara para conectar una aptitud personalizada al resto de la canalización de enriquecimiento. El único requisito para su inclusión en un [conjunto de aptitudes](cognitive-search-defining-skillset.md), es la capacidad de aceptar entradas y emitir salidas de manera que se puedan consumir dentro del conjunto de aptitudes en conjunto. Este artículo se centra en los formatos de entrada y salida que necesita la canalización de enriquecimiento.

## <a name="web-api-custom-skill-interface"></a>Interfaz de aptitudes personalizadas de Web API

Los puntos de conexión de las aptitudes personalizadas de WebAPI agotarán, de forma predeterminada, el tiempo de expiración si no devuelven una respuesta en un espacio de 30 segundos. La canalización de indexación es sincrónica y la indexación producirá un error de tiempo de expiración si no se recibe una respuesta en ese período.  Es posible configurar el tiempo de expiración para que sea de hasta 90 segundos mediante la configuración del parámetro correspondiente:

```json
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "This skill has a 90 second timeout",
        "uri": "https://[your custom skill uri goes here]",
        "timeout": "PT90S",
```

Actualmente, el único mecanismo para interactuar con una aptitud personalizada es a través de una interfaz de Web API. Web API debe cumplir los requisitos que se describen en esta sección.

### <a name="1--web-api-input-format"></a>1.  Formato de entrada de Web API

Web API debe aceptar la matriz de registros que se va a procesar. Cada registro debe tener un "contenedor de propiedades" que será la entrada proporcionada a Web API. 

Supongamos que quiere crear una opción de enriquecimiento simple que identifique la primera fecha mencionada en el texto de un contrato. En este ejemplo, la aptitud acepta una sola entrada *contractText* como texto del contrato. Asimismo, la aptitud también tiene un único resultado, que es la fecha del contrato. Para que la opción de enriquecimiento sea más interesante, devuelva *contractDate* en la forma de un tipo complejo de varias partes.

Web API debería estar lista para recibir un lote de registros de entrada. Cada miembro de la matriz de *valores* representa la entrada de un registro en particular. Es necesario que cada registro tenga los siguientes elementos:

+ Un miembro *recordId* que sea el identificador único de un registro en particular. Cuando la operación de enriquecimiento devuelva los resultados, debe proporcionar el miembro *recordId* para permitir que el autor de la llamada haga coincidir los resultados del registro con los elementos de entrada.

+ Un miembro *data*, que es esencialmente una colección de campos de entrada de cada registro.

Para ser más concretos, si tomamos de referencia el ejemplo anterior, Web API debe esperar solicitudes que tengan este aspecto:

```json
{
    "values": [
      {
        "recordId": "a1",
        "data":
           {
             "contractText": 
                "This is a contract that was issues on November 3, 2017 and that involves... "
           }
      },
      {
        "recordId": "b5",
        "data":
           {
             "contractText": 
                "In the City of Seattle, WA on February 5, 2018 there was a decision made..."
           }
      },
      {
        "recordId": "c3",
        "data":
           {
             "contractText": null
           }
      }
    ]
}
```
En realidad, es posible llamar al servicio con cientos o miles de registros en lugar de usar solo los tres que se muestran aquí.

### <a name="2-web-api-output-format"></a>2. Formato de salida de Web API

El formato de salida es un conjunto de registros que contiene un miembro *recordId* y un contenedor de propiedades. 

```json
{
  "values": 
  [
      {
        "recordId": "b5",
        "data" : 
        {
            "contractDate":  { "day" : 5, "month": 2, "year" : 2018 }
        }
      },
      {
        "recordId": "a1",
        "data" : {
            "contractDate": { "day" : 3, "month": 11, "year" : 2017 }                    
        }
      },
      {
        "recordId": "c3",
        "data" : 
        {
        },
        "errors": [ { "message": "contractText field required "}   ],  
        "warnings": [ {"message": "Date not found" }  ]
      }
    ]
}
```

Este ejemplo en particular solo tiene una salida, pero puede generar más de una propiedad. 

### <a name="errors-and-warning"></a>Errores y advertencias

Tal como se muestra en el ejemplo anterior, puede devolver mensajes de error y advertencia para cada registro.

## <a name="consuming-custom-skills-from-skillset"></a>Consumir aptitudes personalizadas del conjunto de aptitudes

Cuando crea una opción de enriquecimiento de Web API, puede describir encabezados y parámetros de HTTP como parte de la solicitud. En el siguiente fragmento de código se muestra la manera de describir los parámetros de solicitud y los encabezados HTTP como parte de la definición del conjunto de aptitudes.

```json
{
    "skills": [
      {
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "This skill calls an Azure function, which in turn calls TA sentiment",
        "uri": "https://indexer-e2e-webskill.azurewebsites.net/api/DateExtractor?language=en",
        "context": "/document",
        "httpHeaders": {
            "DateExtractor-Api-Key": "foo"
        },
        "inputs": [
          {
            "name": "contractText",
            "source": "/document/content"
          }
        ],
        "outputs": [
          {
            "name": "contractDate",
            "targetName": "date"
          }
        ]
      }
  ]
}
```

## <a name="next-steps"></a>Pasos siguientes

+ [Ejemplo: Creación de una habilidad personalizada de búsqueda cognitiva](cognitive-search-create-custom-skill-example.md)
+ [Definición de un conjunto de aptitudes](cognitive-search-defining-skillset.md)
+ [Crear un conjunto de aptitudes (REST)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Cómo asignar campos enriquecidos](cognitive-search-output-field-mapping.md)
