---
title: Asociación de un recurso de Cognitive Services con un conjunto de aptitudes en Azure Search
description: Instrucciones para asociar una suscripción todo en uno de Cognitive Services a un conjunto de aptitudes cognitivas en Azure Search
manager: cgronlun
author: LuisCabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 52efa685bba330879365f56e547881d62a52a185
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/03/2019
ms.locfileid: "54000162"
---
# <a name="associate-a-cognitive-services-resource-with-a-skillset-in-azure-search"></a>Asociación de un recurso de Cognitive Services con un conjunto de aptitudes en Azure Search 

La búsqueda cognitiva extrae y enriquece datos para que se puedan buscar en Azure Search. A los pasos de extracción y enriquecimiento los llamamos *conocimientos cognitivos*. El conjunto de aptitudes al que se llama durante la indexación de contenido se define en un *conjunto de aptitudes*. Un conjunto de aptitudes puede usar [aptitudes predefinidas](cognitive-search-predefined-skills.md) o aptitudes personalizadas. Para más información, consulte [Ejemplo: creación de una aptitud personalizada](cognitive-search-create-custom-skill-example.md).

En este artículo, aprenderá a asociar el recurso de [Azure Cognitive Services ](https://azure.microsoft.com/services/cognitive-services/) con su conjunto de aptitudes cognitivas.

El recurso de Cognitive Services que seleccione potenciará las aptitudes cognitivas integradas. Este recurso también se usará para fines de facturación. Todos los enriquecimientos que realice mediante las aptitudes cognitivas integradas se facturará en el recurso de Cognitive Services que seleccione. Se le facturará a la misma tarifa que si hubiera realizado la misma tarea mediante un recurso de Cognitive Services. Consulte los [precios de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/).

> [!NOTE]
> A partir del 21 de diciembre de 2018, podrá asociar un recurso de Cognitive Services con un conjunto de aptitudes de Azure Search. Esto nos permite cobrar por la ejecución del conjunto de aptitudes. En esta fecha, también empezamos a cobrar por la extracción de imágenes como parte de la fase de descifrado de documentos. La extracción de texto de documentos continuará ofreciéndose sin costo adicional.
>
> La ejecución de aptitudes integradas se cobra a los [precios de pago por uso de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/). La extracción de imágenes se cobra al precio de la versión preliminar, tal y como se describe en la [página de precios de Azure Search](https://go.microsoft.com/fwlink/?linkid=2042400).

## <a name="limits-when-no-cognitive-services-resource-is-selected"></a>Límites cuando no se selecciona ningún recurso de Cognitive Services
A partir del 1 de febrero de 2019, si no se asocia una suscripción de Cognitive Services con su conjunto de aptitudes, solo podrá enriquecer un pequeño número de documentos de forma gratuita (20 documentos al día). 

## <a name="associate-a-cognitive-services-resource-with-a-new-skillset"></a>Asociación de un recurso de Cognitive Services con un nuevo conjunto de aptitudes

1. Como parte del asistente para [importar datos](search-import-data-portal.md), después de haberse conectado al origen de datos, vaya al paso opcional **Agregar Cognitive Search**. Este es el segundo paso del asistente.

1. Expanda la sección **Adjuntar Cognitive Services**. Este paso permite ver todos los recursos de Cognitive Services que tiene en las mismas regiones como, por ejemplo, el servicio de Azure Search.

   ![Sección Adjuntar Cognitive Services expandida](./media/cognitive-search-attach-cognitive-services/attach1.png "Expanded Attach Cognitive Services")

1. Seleccione un recurso de Cognitive Services existente o seleccione **Crear un nuevo recurso de Cognitive Services**. Si selecciona el **Free (Limited Enrichments)**, solo podrá enriquecer un número pequeño de documentos de forma gratuita (20 documentos al día). Si selecciona **Crear un nuevo recurso de Cognitive Services**, se abrirá una nueva pestaña donde puede crear el recurso. 

1. Si ha creado un nuevo recurso, seleccione **Actualizar** para actualizar la lista de recursos de Cognitive Services y, a continuación, seleccione el recurso. 

   ![Recurso de Cognitive Services seleccionado](./media/cognitive-search-attach-cognitive-services/attach2.png "Selected Cognitive Service Resource")

1. Expanda la sección **Agregar enriquecimiento** para seleccionar las aptitudes cognitivas específicas que desea ejecutar en los datos y continuar con el resto del flujo.

## <a name="associate-a-cognitive-services-resource-with-an-existing-skillset"></a>Asociación de un recurso de Cognitive Services con un conjunto de aptitudes ya existente

1. En la página **Información general del servicio**, seleccione la pestaña **Conjuntos de aptitudes**.

   ![Pestaña Conjuntos de aptitudes](./media/cognitive-search-attach-cognitive-services/attach-existing1.png "Skillsets tab")

1. Seleccione el conjunto de aptitudes que desea modificar. En este paso se abre una hoja donde puede editar un conjunto de aptitudes.

1. Seleccione un recurso de Cognitive Services existente o seleccione **Crear un nuevo recurso de Cognitive Services**. Si selecciona el **Free (Limited Enrichments)**, solo podrá enriquecer un número pequeño de documentos de forma gratuita (20 documentos al día). Si selecciona **Crear un nuevo recurso de Cognitive Services**, se abrirá una nueva pestaña donde puede crear el recurso.

   <n/> 
   <img src="./media/cognitive-search-attach-cognitive-services/attach-existing2.png" width="350">

1. Si ha creado un nuevo recurso, seleccione **Actualizar** para actualizar la lista de recursos de Cognitive Services y, a continuación, seleccione el recurso.

1. Seleccione **Aceptar** para confirmar los cambios.

## <a name="associate-a-cognitive-services-resource-programmatically"></a>Asociación de un recurso de Cognitive Services mediante programación

Cuando defina un conjunto de aptitudes mediante programación, agregue una sección `cognitiveServices`. En la sección, incluya la clave del recurso de Cognitive Services que desea asociar con el conjunto de aptitudes. Además incluya `@odata.type` y establézcalo en `#Microsoft.Azure.Search.CognitiveServicesByKey`. En el siguiente ejemplo se muestra este patrón.

```http
PUT https://[servicename].search.windows.net/skillsets/[skillset name]?api-version=2017-11-11-Preview
api-key: [admin key]
Content-Type: application/json
```
```json
{
    "name": "skillset name",
    "skills": 
    [
      {
        "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
        "categories": [ "Organization" ],
        "defaultLanguageCode": "en",
        "inputs": [
          {
            "name": "text", "source": "/document/content"
          }
        ],
        "outputs": [
          {
            "name": "organizations", "targetName": "organizations"
          }
        ]
      }
    ],
    "cognitiveServices": {
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
        "description": "mycogsvcs",
        "key": "your key goes here"
    }
}
```
## <a name="example-estimate-the-cost-of-document-cracking-and-enrichment"></a>Ejemplo: Estimación del costo de descifrado y enriquecimiento de un documento
Es posible que desee calcular cuánto cuesta enriquecer un tipo de documento. El siguiente ejercicio es solo un ejemplo, pero puede resultarle útil.

Imagine que tiene 1000 archivos PDF. Calcula que, en promedio, cada uno de esos documentos tiene 6 páginas. Cada página tiene 1 imagen. Por término medio hay 3000 caracteres por página. 

Ahora, supongamos que queremos realizar los pasos siguientes como parte de la canalización de enriquecimiento:
1. Como parte del descifrado del documento, extraiga el contenido y las imágenes de este.
1. Como parte del proceso de enriquecimiento, use el reconocimiento óptico de caracteres (OCR) de cada una de las páginas extraídas, combine el texto de todas las páginas y, a continuación, extraiga cada una de las organizaciones del texto combinado de todas las imágenes.

Vamos a calcular cuánto costaría ingerir esos 1000 documentos, paso a paso:

1. Para el descifrado de documentos, extraeríamos un número combinado de 6000 imágenes. Supongamos que cada 1000 imágenes extraídas cuestan 1 USD. Eso nos daría un costo de 6,00 USD.

2. A continuación, extraeríamos el texto de cada una de esas 6000 imágenes. En inglés, la aptitud cognitiva de OCR usa el mejor algoritmo (DescribeText). Suponiendo un precio de 2,50 USD por cada 1000 imágenes que se analizan, tendríamos que pagar 15,00 USD en este paso.

3. Para la extracción de entidades, tendríamos un total de 3 registros de texto por página. (Cada registro tiene 1000 caracteres). Tres registros de texto por página * 6000 páginas = 18 000 registros de texto. Suponiendo un costo de 2,00 USD por cada 1,000 registros de texto, en este paso tendríamos un costo de 36,00 USD.

En resumen, pagaría 57,00 USD por la ingesta de 1,000 documentos PDF de esta naturaleza con el conjunto de aptitudes descrito. En este ejercicio, supusimos el mayor precio por transacción. Podría haber sido inferior debido a la graduación de los precios. Consulte los [precios de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services).



## <a name="next-steps"></a>Pasos siguientes
+ [Página de precios de Azure Search](https://azure.microsoft.com/pricing/details/search/)
+ [Definición de un conjunto de aptitudes](cognitive-search-defining-skillset.md)
+ [Crear un conjunto de aptitudes (REST)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Cómo asignar campos enriquecidos](cognitive-search-output-field-mapping.md)
