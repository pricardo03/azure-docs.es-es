---
title: 'Inicio rápido: Entrenamiento de un modelo y extracción de datos de formularios mediante cURL (Form Recognizer)'
titleSuffix: Azure Cognitive Services
description: En este inicio rápido, usará la API de REST de Form Recognizer con cURL para entrenar un modelo y extraer datos de formularios.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: quickstart
ms.date: 04/15/2019
ms.author: pafarley
ms.openlocfilehash: 351cb7ba2d7a55300a0ace999792a498cf72ebbb
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66475266"
---
# <a name="quickstart-train-a-form-recognizer-model-and-extract-form-data-by-using-the-rest-api-with-curl"></a>Inicio rápido: Entrenamiento de un modelo de Form Recognizer y extracción de los datos del formulario mediante la API de REST con cURL

En este inicio rápido, usará la API de REST de Azure Form Recognizer con cURL para entrenar y puntuar formularios para extraer tablas y pares clave-valor.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos
Para completar este inicio rápido, debe cumplir los siguientes requisitos:
- Tener acceso a la versión preliminar de acceso limitado de Form Recognizer. Para acceder a la versión preliminar, rellene y envíe el formulario [Solicitud de acceso de Form Recognizer](https://aka.ms/FormRecognizerRequestAccess).
- [cURL](https://curl.haxx.se/windows/) instalado.
- Un conjunto de al menos cinco formularios del mismo tipo. Estos datos se usarán para entrenar el modelo. Para este inicio rápido puede usar un [conjunto de datos de ejemplo](https://go.microsoft.com/fwlink/?linkid=2090451). Cargue los datos en la raíz de una cuenta de Azure Blob Storage.

## <a name="create-a-form-recognizer-resource"></a>Creación de un recurso de Form Recognizer

Cuando se le conceda acceso para usar Form Recognizer, recibirá un correo electrónico de bienvenida con varios vínculos y recursos. Use el vínculo de "Azure Portal" de dicho mensaje para abrir Azure Portal y crear un recurso de Form Recognizer. En el panel **Crear**, proporcione la siguiente información:

|    |    |
|--|--|
| **Nombre** | Un nombre descriptivo para su recurso. Se recomienda usar un nombre descriptivo, como *MyNameFormRecognizer*. |
| **Suscripción** | Seleccione la suscripción de Azure a la que se le ha concedido acceso. |
| **Ubicación** | Ubicación de la instancia de Cognitive Services. Las diferentes ubicaciones pueden crear latencias, pero no tienen ningún impacto en la disponibilidad del tiempo de ejecución del recurso. |
| **Plan de tarifa** | El costo del recurso depende el plan de tarifa elegido y del uso. Para obtener más información, consulte los [detalles de los precios](https://azure.microsoft.com/pricing/details/cognitive-services/).
| **Grupos de recursos** | [Grupo de recursos de Azure](https://docs.microsoft.com/azure/architecture/cloud-adoption/governance/resource-consistency/azure-resource-access#what-is-an-azure-resource-group) que contendrá su recurso. Puede crear un nuevo grupo o agregarlo a uno ya existente. |

> [!IMPORTANT]
> Normalmente, al crear un recurso de Cognitive Services en Azure Portal, tiene la opción de crear una clave de suscripción para varios servicios (que se usa en varias instancias de Cognitive Services) o una clave de suscripción para un solo servicio (que se usa solo con una instancia específica de Cognitive Services). Sin embargo, dado que Form Recognizer se encuentra en versión preliminar, no se incluye en la suscripción para varios servicios, y no puede crear la suscripción para un solo servicio a menos que use el vínculo proporcionado en el correo electrónico de bienvenida.

Cuando el recurso de Form Recognizer termine la implementación, búsquelo y selecciónelo en la lista **Todos los recursos** del portal. A continuación, seleccione la pestaña **Claves** para ver las claves de la suscripción. Cualquiera de las claves dará a la aplicación acceso al recurso. Copie el valor de **CLAVE 1**. La usará en la sección siguiente.

## <a name="train-a-form-recognizer-model"></a>Entrenamiento de un modelo de Form Recognizer

En primer lugar, se necesitará un conjunto de datos de entrenamiento de un blob de Azure Storage. Debe tener un mínimo de cinco formularios de ejemplo (documentos PDF o imágenes) de la misma estructura y tipo que los datos de entrada principales. O bien, puede usar un formulario vacío con dos formularios rellenos. El nombre de archivo del primero formulario debe incluir la palabra "empty".

Para entrenar un modelo de Form Recognizer mediante los documentos del contenedor de blobs de Azure, llame a la API **Train** mediante la ejecución del comando de cURL siguiente. Antes de ejecutar el comando, realice estos cambios:

1. Reemplace `<Endpoint>` por el punto de conexión que obtuvo de la clave de suscripción de Form Recognizer. Lo encontrará en la pestaña **Información general** del recurso Form Recognizer.
1. Reemplace `<SAS URL>` por una dirección URL de la firma de acceso compartido (SAS) del contenedor de Azure Blob Storage compartido de la ubicación en la que se encuentran los datos de entrenamiento.  
1. Reemplace `<subscription key>` con la clave de suscripción que copió en el paso anterior.

```bash
curl -X POST "https://<Endpoint>/formrecognizer/v1.0-preview/custom/train" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <subscription key>" --data-ascii "{ \"source\": \""<SAS URL>"\"}"
```

Recibirá una respuesta `200 (Success)` con la siguiente salida JSON:

```json
{
  "modelId": "59e2185e-ab80-4640-aebc-f3653442617b",
  "trainingDocuments": [
    {
      "documentName": "Invoice_1.pdf",
      "pages": 1,
      "errors": [],
      "status": "success"
    },
    {
      "documentName": "Invoice_2.pdf",
      "pages": 1,
      "errors": [],
      "status": "success"
    },
    {
      "documentName": "Invoice_3.pdf",
      "pages": 1,
      "errors": [],
      "status": "success"
    },
    {
      "documentName": "Invoice_4.pdf",
      "pages": 1,
      "errors": [],
      "status": "success"
    },
    {
      "documentName": "Invoice_5.pdf",
      "pages": 1,
      "errors": [],
      "status": "success"
    }
  ],
  "errors": []
}
```

Tome nota del valor de `"modelId"`. Lo necesitará en los pasos siguientes.
  
## <a name="extract-key-value-pairs-and-tables-from-forms"></a>Extraiga los pares clave-valor y las tablas de los formularios

A continuación, analizará un documento y extraerá de él tanto los pares clave-valor como las tablas. Llame a la API **Model - Analyze** mediante la ejecución del siguiente comando de cURL. Antes de ejecutar el comando, realice estos cambios:

1. Reemplace `<Endpoint>` por el punto de conexión que obtuvo de la clave de suscripción de Form Recognizer. Lo encontrará en la pestaña **Información general** del recurso Form Recognizer.
1. Reemplace `<modelID>` por el identificador de modelo que recibió en la sección anterior.
1. Reemplace `<path to your form>` por la ruta de acceso al archivo del formulario. Por ejemplo c:\temp\file.pdf. 
1. Reemplace `<file type>` por el tipo de archivo. Tipos compatibles: pdf, image/jpeg e image/png.
1. Reemplace `<subscription key>` por la clave de suscripción.


```bash
curl -X POST "https://<Endpoint>/formrecognizer/v1.0-preview/custom/models/<modelID>/analyze" -H "Content-Type: multipart/form-data" -F "form=@\"<path to your form>\";type=application/<file type>" -H "Ocp-Apim-Subscription-Key: <subscription key>"
```

### <a name="examine-the-response"></a>Examen de la respuesta

Se devuelve una respuesta correcta en JSON. Representa las tablas y los pares clave-valor extraídos del formulario:

```bash
{
  "status": "success",
  "pages": [
    {
      "number": 1,
      "height": 792,
      "width": 612,
      "clusterId": 0,
      "keyValuePairs": [
        {
          "key": [
            {
              "text": "Address:",
              "boundingBox": [
                57.4,
                683.1,
                100.5,
                683.1,
                100.5,
                673.7,
                57.4,
                673.7
              ]
            }
          ],
          "value": [
            {
              "text": "1 Redmond way Suite",
              "boundingBox": [
                57.4,
                671.3,
                154.8,
                671.3,
                154.8,
                659.2,
                57.4,
                659.2
              ],
              "confidence": 0.86
            },
            {
              "text": "6000 Redmond, WA",
              "boundingBox": [
                57.4,
                657.1,
                146.9,
                657.1,
                146.9,
                645.5,
                57.4,
                645.5
              ],
              "confidence": 0.86
            },
            {
              "text": "99243",
              "boundingBox": [
                57.4,
                643.4,
                85,
                643.4,
                85,
                632.3,
                57.4,
                632.3
              ],
              "confidence": 0.86
            }
          ]
        },
        {
          "key": [
            {
              "text": "Invoice For:",
              "boundingBox": [
                316.1,
                683.1,
                368.2,
                683.1,
                368.2,
                673.7,
                316.1,
                673.7
              ]
            }
          ],
          "value": [
            {
              "text": "Microsoft",
              "boundingBox": [
                374,
                687.9,
                418.8,
                687.9,
                418.8,
                673.7,
                374,
                673.7
              ],
              "confidence": 1
            },
            {
              "text": "1020 Enterprise Way",
              "boundingBox": [
                373.9,
                673.5,
                471.3,
                673.5,
                471.3,
                659.2,
                373.9,
                659.2
              ],
              "confidence": 1
            },
            {
              "text": "Sunnayvale, CA 87659",
              "boundingBox": [
                373.8,
                659,
                479.4,
                659,
                479.4,
                645.5,
                373.8,
                645.5
              ],
              "confidence": 1
            }
          ]
        }
      ],
      "tables": [
        {
          "id": "table_0",
          "columns": [
            {
              "header": [
                {
                  "text": "Invoice Number",
                  "boundingBox": [
                    38.5,
                    585.2,
                    113.4,
                    585.2,
                    113.4,
                    575.8,
                    38.5,
                    575.8
                  ]
                }
              ],
              "entries": [
                [
                  {
                    "text": "34278587",
                    "boundingBox": [
                      38.5,
                      547.3,
                      82.8,
                      547.3,
                      82.8,
                      537,
                      38.5,
                      537
                    ],
                    "confidence": 1
                  }
                ]
              ]
            },
            {
              "header": [
                {
                  "text": "Invoice Date",
                  "boundingBox": [
                    139.7,
                    585.2,
                    198.5,
                    585.2,
                    198.5,
                    575.8,
                    139.7,
                    575.8
                  ]
                }
              ],
              "entries": [
                [
                  {
                    "text": "6/18/2017",
                    "boundingBox": [
                      139.7,
                      546.8,
                      184,
                      546.8,
                      184,
                      537,
                      139.7,
                      537
                    ],
                    "confidence": 1
                  }
                ]
              ]
            },
            {
              "header": [
                {
                  "text": "Invoice Due Date",
                  "boundingBox": [
                    240.5,
                    585.2,
                    321,
                    585.2,
                    321,
                    575.8,
                    240.5,
                    575.8
                  ]
                }
              ],
              "entries": [
                [
                  {
                    "text": "6/24/2017",
                    "boundingBox": [
                      240.5,
                      546.8,
                      284.8,
                      546.8,
                      284.8,
                      537,
                      240.5,
                      537
                    ],
                    "confidence": 1
                  }
                ]
              ]
            },
            {
              "header": [
                {
                  "text": "Charges",
                  "boundingBox": [
                    341.3,
                    585.2,
                    381.2,
                    585.2,
                    381.2,
                    575.8,
                    341.3,
                    575.8
                  ]
                }
              ],
              "entries": [
                [
                  {
                    "text": "$56,651.49",
                    "boundingBox": [
                      387.6,
                      546.4,
                      437.5,
                      546.4,
                      437.5,
                      537,
                      387.6,
                      537
                    ],
                    "confidence": 1
                  }
                ]
              ]
            },
            {
              "header": [
                {
                  "text": "VAT ID",
                  "boundingBox": [
                    442.1,
                    590,
                    474.8,
                    590,
                    474.8,
                    575.8,
                    442.1,
                    575.8
                  ]
                }
              ],
              "entries": [
                [
                  {
                    "text": "PT",
                    "boundingBox": [
                      447.7,
                      550.6,
                      460.4,
                      550.6,
                      460.4,
                      537,
                      447.7,
                      537
                    ],
                    "confidence": 1
                  }
                ]
              ]
            }
          ]
        }
      ]
    }
  ],
  "errors": []
}
```

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha utilizado la API de REST de Form Recognizer con cURL para entrenar un modelo y ejecutarlo en un escenario de ejemplo. A continuación, consulte la documentación de referencia para explorar la API de Form Recognizer con mayor profundidad.

> [!div class="nextstepaction"]
> [Documentación de referencia de API REST](https://aka.ms/form-recognizer/api)
