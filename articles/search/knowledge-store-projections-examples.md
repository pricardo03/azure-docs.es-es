---
title: Definición de proyecciones en un almacén de conocimiento
titleSuffix: Azure Cognitive Search
description: Ejemplos de patrones comunes sobre cómo proyectar documentos enriquecidos en el almacén de información para usarse con Power BI o Azure ML.
manager: eladz
author: vkurpad
ms.author: vikurpad
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/15/2020
ms.openlocfilehash: f29f4b91b85c0027df4be2fd5f26ef8f9749fe33
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77163821"
---
# <a name="knowledge-store-projections-how-to-shape-and-export-enrichments-to-the-knowledge-store"></a>Proyecciones del almacén de conocimiento: cómo conformar y exportar características enriquecidas al almacén de información

> [!IMPORTANT] 
> El almacén de conocimiento está actualmente en versión preliminar pública. La funcionalidad de versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no es aconsejable usarla para cargas de trabajo de producción. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). En la [API REST versión 2019-05-06-Preview](search-api-preview.md) se proporcionan características en versión preliminar. Actualmente hay compatibilidad limitada con el portal y no la hay con el SDK de .NET.

Las proyecciones son la expresión física de los documentos enriquecidos en un almacén de conocimiento. El uso eficaz de los documentos enriquecidos requiere de estructura. En este artículo, explorará la estructura y las relaciones; aprenderá a crear propiedades de proyección y a establecer relaciones entre los datos de los distintos tipos de proyección que cree. 

Para crear una proyección, debe dar forma a los datos mediante una aptitud modeladora para generar un objeto personalizado, o bien mediante la sintaxis de modelado insertado. Una forma de datos contiene todos los datos que quiere proyectar. En este documento se ofrece un ejemplo de cada opción; puede usar cualquiera de las opciones para las proyecciones que creará.


Hay tres tipos de proyecciones:
+ Tablas
+ Objetos
+ Archivos

Las proyecciones de tabla se almacenan en Azure Table Storage. Las proyecciones de objeto y archivo se escriben en el almacenamiento de blobs; las proyecciones de objeto se guardan como archivos JSON y pueden incluir contenido del documento, así como las salidas o características enriquecidas de cualquier aptitud. La canalización de enriquecimiento también puede extraer archivos binarios, como las imágenes. Estos archivos binarios se proyectan como proyecciones de archivos. Cuando un objeto binario se proyecta como proyección de objeto, solo se guardan como blob JSON los metadatos asociados a él. 

Para comprender la relación entre el modelado de datos y las proyecciones, usaremos el siguiente conjunto de aptitudes como base para explorar diversas configuraciones. Este conjunto de aptitudes procesa contenido de texto e imágenes sin procesar. Las proyecciones se definirán a partir del contenido del documento y de los resultados de las aptitudes para los escenarios que queremos admitir.

También puede descargar y usar un [ejemplo de la API REST](https://github.com/Azure-Samples/azure-search-postman-samples/blob/master/projections/Projections%20Docs.postman_collection.json) con todas las llamadas de este tutorial.

> [!IMPORTANT] 
> Al experimentar con las proyecciones, resulta útil [establecer la propiedad de la caché del indizador](search-howto-incremental-index.md) para asegurarse de controlar los costos. La edición de proyecciones dará lugar a que todo el documento se vuelva a enriquecer si no se establece la memoria caché del indizador. Cuando se establece la memoria caché y solo se actualizan las proyecciones, la ejecución del conjunto de aptitudes para documentos enriquecidos previamente no generan costos de Cognitive Services.


```json
{
    "name": "azureblob-skillset",
    "description": "Skillset created from the portal. skillsetName: azureblob-skillset; contentField: merged_content; enrichmentGranularity: document; knowledgeStoreStorageAccount: confdemo;",
    "skills": [
        {
            "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
            "name": "#1",
            "description": null,
            "context": "/document/merged_content",
            "categories": [
                "Person",
                "Quantity",
                "Organization",
                "URL",
                "Email",
                "Location",
                "DateTime"
            ],
            "defaultLanguageCode": "en",
            "minimumPrecision": null,
            "includeTypelessEntities": null,
            "inputs": [
                {
                    "name": "text",
                    "source": "/document/merged_content"
                },
                {
                    "name": "languageCode",
                    "source": "/document/language"
                }
            ],
            "outputs": [
                {
                    "name": "persons",
                    "targetName": "people"
                },
                {
                    "name": "organizations",
                    "targetName": "organizations"
                },
                {
                    "name": "locations",
                    "targetName": "locations"
                },
                {
                    "name": "entities",
                    "targetName": "entities"
                }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Text.KeyPhraseExtractionSkill",
            "name": "#2",
            "description": null,
            "context": "/document/merged_content",
            "defaultLanguageCode": "en",
            "maxKeyPhraseCount": null,
            "inputs": [
                {
                    "name": "text",
                    "source": "/document/merged_content"
                },
                {
                    "name": "languageCode",
                    "source": "/document/language"
                }
            ],
            "outputs": [
                {
                    "name": "keyPhrases",
                    "targetName": "keyphrases"
                }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Text.LanguageDetectionSkill",
            "name": "#3",
            "description": null,
            "context": "/document",
            "inputs": [
                {
                    "name": "text",
                    "source": "/document/merged_content"
                }
            ],
            "outputs": [
                {
                    "name": "languageCode",
                    "targetName": "language"
                }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Text.MergeSkill",
            "name": "#4",
            "description": null,
            "context": "/document",
            "insertPreTag": " ",
            "insertPostTag": " ",
            "inputs": [
                {
                    "name": "text",
                    "source": "/document/content"
                },
                {
                    "name": "itemsToInsert",
                    "source": "/document/normalized_images/*/text"
                },
                {
                    "name": "offsets",
                    "source": "/document/normalized_images/*/contentOffset"
                }
            ],
            "outputs": [
                {
                    "name": "mergedText",
                    "targetName": "merged_content"
                }
            ]
        },
        {
            "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
            "name": "#5",
            "description": null,
            "context": "/document/normalized_images/*",
            "textExtractionAlgorithm": "printed",
            "lineEnding": "Space",
            "defaultLanguageCode": "en",
            "detectOrientation": true,
            "inputs": [
                {
                    "name": "image",
                    "source": "/document/normalized_images/*"
                }
            ],
            "outputs": [
                {
                    "name": "text",
                    "targetName": "text"
                },
                {
                    "name": "layoutText",
                    "targetName": "layoutText"
                }
            ]
        }
    ],
    "cognitiveServices": {
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
        "description": "DemosCS",
        "key": "<COGNITIVE SERVICES KEY>"
    },
    "knowledgeStore": null
}
```

Ahora podemos agregar el objeto `knowledgeStore` y configurar las proyecciones para cada uno de los escenarios según sea necesario. 

## <a name="projecting-to-tables-for-scenarios-like-power-bi"></a>Proyección en tablas para escenarios como Power BI

> [!NOTE] 
> Ya que el almacén de información es una cuenta de Azure Storage, las proyecciones de tabla son tablas de Azure Storage y se rigen por los límites de almacenamiento de las tablas. Para obtener más información al respecto, consulte los [límites de almacenamiento de tablas](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model). Debe saber que el tamaño de la entidad no puede superar 1 MB, y que una sola propiedad no puede ser mayor que 64 KB. Estas restricciones hacen que las tablas sean una buena solución para almacenar un gran número de entidades pequeñas.

Power BI puede leer desde las tablas y detectar relaciones en función de las claves creadas por las proyecciones del almacén de información. Por ello, las tablas son una buena opción para proyectar los datos cuando intenta crear un panel sobre los datos enriquecidos. Suponiendo que queremos crear un panel en el que se puedan visualizar las frases clave extraídas de los documentos como una nube de palabras, se puede agregar una aptitud modeladora al conjunto de aptitudes para generar una forma personalizada que tenga los detalles específicos y frases clave del documento. Agregue la aptitud modeladora al conjunto de aptitudes para crear una nueva característica enriquecida llamada ```pbiShape``` en el ```document```.

### <a name="using-a-shaper-skill-to-create-a-custom-shape"></a>Uso de una aptitud modeladora para crear una forma personalizada

Cree una forma personalizada que se pueda proyectar en el almacenamiento de tablas. Sin una forma personalizada, una proyección solo puede hacer referencia a un solo nodo (una proyección por salida). La creación de una forma personalizada permite agregar varios elementos a un nuevo conjunto lógico que se puede proyectar como una sola tabla, o que se puede segmentar y distribuir en una colección de tablas. En este ejemplo, la forma personalizada combina los metadatos con las entidades y frases clave identificadas. El objeto se denomina pbiShape y tiene como elemento primario a `/document`. 

> [!IMPORTANT] 
> Las rutas de acceso de origen de las características enriquecidas deben ser objetos JSON con el formato correcto antes de que se puedan proyectar. El árbol de características enriquecidas puede representar características enriquecidas que no son código JSON con un formato correcto; por ejemplo, cuando una característica enriquecida tiene como elemento primario un tipo primitivo, como una cadena. Observe cómo `KeyPhrases` y `Entities` están encapsulados en un objeto JSON válido con `sourceContext`. Este formato es obligatorio, ya que `keyphrases` y `entities` son características enriquecidas sobre primitivos y deben convertirse a código JSON válido antes de proyectarse.

```json
{
            "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
            "name": "ShaperForTables",
            "description": null,
            "context": "/document",
            "inputs": [
                {
                    "name": "metadata_storage_content_type",
                    "source": "/document/metadata_storage_content_type",
                    "sourceContext": null,
                    "inputs": []
                },
                {
                    "name": "metadata_storage_name",
                    "source": "/document/metadata_storage_name",
                    "sourceContext": null,
                    "inputs": []
                },
                {
                    "name": "metadata_storage_path",
                    "source": "/document/metadata_storage_path",
                    "sourceContext": null,
                    "inputs": []
                },
                {
                    "name": "metadata_content_type",
                    "source": "/document/metadata_content_type",
                    "sourceContext": null,
                    "inputs": []
                },
                {
                    "name": "keyPhrases",
                    "source": null,
                    "sourceContext": "/document/merged_content/keyphrases/*",
                    "inputs": [
                        {
                            "name": "KeyPhrases",
                            "source": "/document/merged_content/keyphrases/*"
                        }

                    ]
                },
                {
                    "name": "Entities",
                    "source": null,
                    "sourceContext": "/document/merged_content/entities/*",
                    "inputs": [
                        {
                            "name": "Entities",
                            "source": "/document/merged_content/entities/*/name"
                        }

                    ]
                }
            ],
            "outputs": [
                {
                    "name": "output",
                    "targetName": "pbiShape"
                }
            ]
        }
```
Agregue la aptitud modeladora que acabamos de definir a la lista del conjunto de aptitudes. 

Ahora que tenemos todos los datos necesarios para proyectar en tablas, actualice el objeto knowledgeStore con las definiciones de tabla. 

```json

"knowledgeStore" : {
    "storageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<Acct Name>;AccountKey=<Acct Key>;",
    "projections": [
        {
            "tables": [
                {
                    "tableName": "pbiDocument",
                    "generatedKeyName": "Documentid",
                    "source": "/document/pbiShape"
                },
                {
                    "tableName": "pbiKeyPhrases",
                    "generatedKeyName": "KeyPhraseid",
                    "source": "/document/pbiShape/keyPhrases/*"
                },
                {
                    "tableName": "pbiEntities",
                    "generatedKeyName": "Entityid",
                    "source": "/document/pbiShape/Entities/*"
                }
            ],
            "objects": [],
            "files": []
        }
    ]
}
```

Establezca la propiedad ```storageConnectionString``` en una cadena de conexión válida de una cuenta de almacenamiento de uso general V2. En este escenario, se definen tres tablas en el objeto de proyección al establecer las propiedades ```tableName```, ```source``` y ```generatedKeyName```. Ahora puede actualizar el conjunto de aptitudes al emitir la solicitud PUT.

### <a name="slicing"></a>Segmentación 

Al comenzar con una forma consolidada en la que todo el contenido que debe proyectarse se encuentra en una sola forma (o nodo de enriquecimiento), la segmentación proporciona la capacidad de segmentar un solo nodo en varias tablas u objetos. En este caso, el objeto ```pbiShape``` se segmenta en varias tablas. La característica de segmentación permite extraer secciones de la forma, ```keyPhrases``` y ```Entities```, en tablas independientes. Esto resulta útil cuando varias entidades y frases clave están asociadas con cada documento. La segmentación genera de forma implícita una relación entre las tablas primaria y secundaria al usar ```generatedKeyName``` de la tabla primaria para crear una columna con el mismo nombre en la tabla secundaria. 

### <a name="naming-relationships"></a>Nomenclatura de relaciones
Las propiedades ```generatedKeyName``` y ```referenceKeyName``` se usan para establecer relaciones entre los datos de distintas tablas o incluso tipos de proyección. Cada fila de la tabla secundaria o proyección tiene una propiedad que apunta de vuelta al elemento primario. El nombre de la columna o propiedad en el elemento secundario es el ```referenceKeyName``` del elemento primario. Cuando no se proporciona el ```referenceKeyName```, el servicio usa como valor predeterminado el ```generatedKeyName``` del elemento primario. PowerBI usa estas claves generadas para detectar relaciones dentro de las tablas. Si necesita que la columna de la tabla secundaria tenga un nombre diferente, establezca la propiedad ```referenceKeyName``` en la tabla primaria. Como ejemplo, podría establecer ```generatedKeyName``` como id. en la tabla pbiDocument y ```referenceKeyName``` como DocumentID. Como resultado, la columna de las tablas pbiEntities y pbiKeyPhrases que contiene el id. del documento tendría por nombre DocumentID.

Tras guardar el conjunto de aptitudes actualizado y ejecutar el indizador, ahora tendrá una proyección en funcionamiento con tres tablas. La importación de estas tablas en Power BI debe permitir que Power BI detecte automáticamente las relaciones.

## <a name="projecting-to-objects"></a>Proyección en objetos

Las proyecciones de objetos no tienen las mismas limitaciones que las proyecciones de tabla y resultan más adecuadas para proyectar documentos grandes. En este ejemplo, se proyecta todo el documento en una proyección de objeto. Las proyecciones de objeto están limitadas a una sola proyección en un contenedor.
Para definir una proyección de objeto, se usa la matriz ```objects``` en las proyecciones. Puede generar una nueva forma mediante la aptitud modeladora o usar el modelado insertado de la proyección de objetos. Aunque en el ejemplo de tablas se mostró el método para crear una forma y segmentarla, en este ejemplo se muestra el uso del modelado insertado. El modelado insertado es la capacidad de crear una nueva forma en la definición de las entradas para una proyección. El modelado insertado crea un objeto anónimo idéntico a lo que produciría un modelador. El modelado insertado resulta útil si está definiendo una forma que no se va a reutilizar.
La propiedad projections es una matriz y, en este ejemplo, se agregará una nueva instancia de proyección a la matriz. Actualice la definición de knowledgeStore con las proyecciones definidas como insertadas. No se necesita una aptitud modeladora cuando se usan las proyecciones insertadas.

```json
"knowledgeStore" : {
        "storageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<Acct Name>;AccountKey=<Acct Key>;",
        "projections": [
             {
                "tables": [ ],
                "objects": [
                    {
                        "storageContainer": "sampleobject",
                        "source": null,
                        "generatedKeyName": "myobject",
                        "sourceContext": "/document",
                        "inputs": [
                            {
                                "name": "metadata_storage_name",
                                "source": "/document/metadata_storage_name"
                            },
                            {
                                "name": "metadata_storage_path",
                                "source": "/document/metadata_storage_path"
                            },
                            {
                                "name": "content",
                                "source": "/document/content"
                            },
                            {
                                "name": "keyPhrases",
                                "source": "/document/merged_content/keyphrases/*"
                            },
                            {
                                "name": "entities",
                                "source": "/document/merged_content/entities/*/name"
                            },
                            {
                                "name": "ocrText",
                                "source": "/document/normalized_images/*/text"
                            },
                            {
                                "name": "ocrLayoutText",
                                "source": "/document/normalized_images/*/layoutText"
                            }
                        ]

                    }
                ],
                "files": []
            }
        ]
    }
```
## <a name="file-projections"></a>Proyecciones de archivo

Las proyecciones de archivo son imágenes que se extraen del documento de origen o de las salidas de características enriquecidas que se pueden proyectar fuera del proceso de enriquecimiento. Las proyecciones de archivo, de forma similar a las proyecciones de objeto, se implementan como blobs y contienen la imagen. Para generar una proyección de archivo, se usa la matriz ```files``` en el objeto projection. En este ejemplo, se proyectan todas las imágenes extraídas del documento en un contenedor denominado `samplefile`.

```json
"knowledgeStore" : {
        "storageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<Acct Name>;AccountKey=<Acct Key>;",
        "projections": [
            {
                "tables": [ ],
                "objects": [ ],
                "files": [
                    {
                        "storageContainer": "samplefile",
                        "source": "/document/normalized_images/*"
                    }
                ]
            }
        ]
    }
```

## <a name="projecting-to-multiple-types"></a>Proyección en varios tipos

Un escenario más complejo podría requerir que se proyecte contenido en distintos tipos de proyección. Por ejemplo, si necesita proyectar algunos datos como frases clave y entidades en tablas, guardar los resultados de OCR del texto y el texto de diseño como objetos y proyectar las imágenes como archivos. Esta actualización del conjunto de aptitudes hará lo siguiente:

1. Crear una tabla con una fila para cada documento.
2. Crear una tabla relacionada con la tabla de documento, en la que cada frase clave se identificará como una fila en la tabla.
3. Crear una tabla relacionada con la tabla de documento, en la que cada entidad se identificará como una fila en la tabla.
4. Crear una proyección de objeto con el texto de diseño de cada imagen.
5. Crear una proyección de archivo en la que se proyecta cada imagen extraída.
6. Crear una tabla de referencias cruzadas que contenga referencias a la tabla de documentos, la proyección de objeto con el texto de diseño y la proyección de archivo.

Comience por agregar una nueva aptitud modeladora a la matriz de aptitudes que cree un objeto con forma. 

```json
{
            "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
            "name": "ShaperForCross",
            "description": null,
            "context": "/document",
            "inputs": [
                {
                    "name": "metadata_storage_name",
                    "source": "/document/metadata_storage_name",
                    "sourceContext": null,
                    "inputs": []
                },
                {
                    "name": "keyPhrases",
                    "source": null,
                    "sourceContext": "/document/merged_content/keyphrases/*",
                    "inputs": [
                        {
                            "name": "KeyPhrases",
                            "source": "/document/merged_content/keyphrases/*"
                        }

                    ]
                },
                {
                    "name": "entities",
                    "source": null,
                    "sourceContext": "/document/merged_content/entities/*",
                    "inputs": [
                        {
                            "name": "Entities",
                            "source": "/document/merged_content/entities/*/name"
                        }

                    ]
                },
                {
                    "name": "images",
                    "source": null,
                    "sourceContext": "/document/normalized_images/*",
                    "inputs": [
                        {
                            "name": "image",
                            "source": "/document/normalized_images/*"
                        },
                        {
                            "name": "layoutText",
                            "source": "/document/normalized_images/*/layoutText"
                        },
                        {
                            "name": "ocrText",
                            "source": "/document/normalized_images/*/text"
                        }
                        ]
                }
                
            ],
            "outputs": [
                {
                    "name": "output",
                    "targetName": "crossProjection"
                }
            ]
        }
```

```json
"knowledgeStore" : {
        "storageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<Acct Name>;AccountKey=<Acct Key>;",
        "projections": [
             {
                "tables": [
                    {
                        "tableName": "crossDocument",
                        "generatedKeyName": "Id",
                        "source": "/document/crossProjection"
                    },
                    {
                        "tableName": "crossEntities",
                        "generatedKeyName": "EntityId",
                        "source": "/document/crossProjection/entities/*"
                    },
                    {
                        "tableName": "crossKeyPhrases",
                        "generatedKeyName": "KeyPhraseId",
                        "source": "/document/crossProjection/keyPhrases/*"
                    },
                    {
                        "tableName": "crossReference",
                        "generatedKeyName": "CrossId",
                        "source": "/document/crossProjection/images/*"
                    }
                     
                ],
                "objects": [
                    {
                        "storageContainer": "crossobject",
                        "generatedKeyName": "crosslayout",
                        "source": null,
                        "sourceContext": "/document/crossProjection/images/*/layoutText",
                        "inputs": [
                            {
                                "name": "OcrLayoutText",
                                "source": "/document/crossProjection/images/*/layoutText"
                            }
                        ]
                    }
                ],
                "files": [
                     {
                        "storageContainer": "crossimages",
                        "generatedKeyName": "crossimages",
                        "source": "/document/crossProjection/images/*/image"
                    }
                    ]
                
            }
        ]
    }
```

Las proyecciones de objeto requieren un nombre de contenedor para cada proyección; las proyecciones de objeto o archivo no pueden compartir un contenedor. 

### <a name="relationships"></a>Relaciones

En este ejemplo también se resalta otra característica de las proyecciones, ya que se definen varios tipos de proyecciones en el mismo objeto de proyección. Hay una relación expresada dentro y entre los distintos tipos de proyecciones (tablas, objetos, archivos), lo que permite comenzar con una fila de tabla para un documento y buscar todo el texto de OCR de las imágenes dentro de ese documento en la proyección de objeto. Si no quiere que los datos estén relacionados, defina las proyecciones en objetos de proyección diferentes; por ejemplo, el siguiente fragmento de código hará que las tablas estén relacionadas, pero no creará relaciones entre las tablas y las proyecciones de texto de OCR. Los grupos de proyección son útiles cuando se quieren proyectar los mismos datos en diferentes formas con distintas necesidades. Por ejemplo, un grupo de proyección para el panel de Power BI y otro grupo de proyección para usar los datos en el entrenamiento de un modelo de IA para una aptitud.
Al crear proyecciones de diferentes tipos, primero se generan las proyecciones de archivo y objeto y las rutas de acceso se agregan a las tablas.

```json
"knowledgeStore" : {
        "storageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<Acct Name>;AccountKey=<Acct Key>;",
        "projections": [
            {
                "tables": [
                    {
                        "tableName": "unrelatedDocument",
                        "generatedKeyName": "Documentid",
                        "source": "/document/pbiShape"
                    },
                    {
                        "tableName": "unrelatedKeyPhrases",
                        "generatedKeyName": "KeyPhraseid",
                        "source": "/document/pbiShape/keyPhrases"
                    }
                ],
                "objects": [
                    
                ],
                "files": []
            }, 
            {
                "tables": [],
                "objects": [
                    {
                        "storageContainer": "unrelatedocrtext",
                        "source": null,
                        "sourceContext": "/document/normalized_images/*/text",
                        "inputs": [
                            {
                                "name": "ocrText",
                                "source": "/document/normalized_images/*/text"
                            }
                        ]
                    },
                    {
                        "storageContainer": "unrelatedocrlayout",
                        "source": null,
                        "sourceContext": "/document/normalized_images/*/layoutText",
                        "inputs": [
                            {
                                "name": "ocrLayoutText",
                                "source": "/document/normalized_images/*/layoutText"
                            }
                        ]
                    }
                ],
                "files": []
            }
        ]
    }
```

En estos ejemplos se mostraron los patrones comunes de uso de las proyecciones; ahora también debería conocer bien los conceptos relativos a la creación de una proyección para su escenario específico.

## <a name="common-issues"></a>Problemas comunes

Al definir una proyección, hay algunos problemas comunes que pueden generar resultados inesperados.

1. Características enriquecidas de cadena sin modelar. Cuando las cadenas están enriquecidas; por ejemplo, ```merged_content``` enriquecido con frases clave, la propiedad enriquecida se representa como un elemento secundario de "merged_content" dentro del árbol de características enriquecidas. No obstante, en el momento de la proyección, debe transformarse en un objeto JSON válido con un nombre y un valor.
2. Omisión de ```/*``` al final de una ruta de acceso de origen. Si, por ejemplo, el origen de una proyección es ```/document/pbiShape/keyPhrases```, la matriz de frases clave se proyecta como un solo objeto o fila. Al establecer la ruta de acceso de origen en ```/document/pbiShape/keyPhrases/*```, se genera una sola fila u objeto para cada una de las frases clave.
3. Los selectores de ruta de acceso distinguen mayúsculas de minúsculas y pueden producir advertencias de entradas faltantes si no se usan las mayúsculas y minúsculas exactas en el selector.

