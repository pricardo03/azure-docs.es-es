---
title: 'Asociación de un recurso de Cognitive Services con un conjunto de aptitudes: Azure Search'
description: Instrucciones para asociar una suscripción todo en uno de Cognitive Services a una canalización de enriquecimiento cognitivo en Azure Search.
manager: nitinme
author: LuisCabrer
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: luisca
ms.openlocfilehash: 113286f829b628d4740fbba34e7279741a934aef
ms.sourcegitcommit: 3f22ae300425fb30be47992c7e46f0abc2e68478
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "71265923"
---
# <a name="attach-a-cognitive-services-resource-with-a-skillset-in-azure-search"></a>Asociación de un recurso de Cognitive Services con un conjunto de aptitudes en Azure Search 

Los algoritmos de inteligencia artificial impulsan las [canalizaciones de indexación cognitivas](cognitive-search-concept-intro.md) usadas para el enriquecimiento de documentos en Azure Search. Estos algoritmos se basan en recursos de Cognitive Services, incluidos [Computer Vision](https://azure.microsoft.com/services/cognitive-services/computer-vision/) para el análisis de imágenes y el reconocimiento óptico de caracteres (OCR) y [Text Analytics](https://azure.microsoft.com/services/cognitive-services/text-analytics/) para el reconocimiento de entidades, la extracción de frases clave y otros enriquecimientos. Tal como los utiliza Azure Search con fines de enriquecimiento de documentos, los algoritmos se incluyen en una *aptitud*, ubicada en un *conjunto de aptitudes* y mencionada por un *indexador* durante la indexación.

Puede enriquecer un número limitado de documentos de manera gratuita, o bien puede asociar un recurso de Cognitive Services facturable a un *conjunto de aptitudes* para cargas de trabajo más grandes y más frecuentes. En este artículo, aprenderá a asociar un recurso de Cognitive Services facturable para enriquecer documentos durante la [indexación](search-what-is-an-index.md) de Azure Search.

> [!NOTE]
> Los eventos facturables incluyen llamadas a Cognitive Services APIs y la extracción de imágenes como parte de la fase de descifrado de documentos en Azure Search. No se aplica ningún cargo por la extracción de texto de documentos o por las aptitudes que no llaman a Cognitive Services.
>
> La ejecución de aptitudes facturables tiene lugar al [precio de pago por uso de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/). Para ver los precios de la extracción de imágenes, consulte la [página de precios de Azure Search](https://go.microsoft.com/fwlink/?linkid=2042400).

## <a name="same-region-requirement"></a>Requisito de la misma región

Se requiere la existencia de Azure Search y Azure Cognitive Services en la misma región. De lo contrario, recibirá este mensaje en tiempo de ejecución: `"Provided key is not a valid CognitiveServices type key for the region of your search service."` 

No existe forma alguna de migrar un servicio entre regiones. Si recibe este error, debe crear un nuevo recurso de Cognitive Services en la misma región que Azure Search.

> [!NOTE]
> Algunas habilidades integradas se basan en una instancia de Cognitive Services no regional (por ejemplo, la [habilidad de traducción de texto](cognitive-search-skill-text-translation.md)). Tenga en cuenta que si agrega cualquiera de estas habilidades a su conjunto de aptitudes, no se garantiza que los datos permanezcan en la misma región que el recurso de Azure Search o Cognitive Services. Consulte la [página de estado del servicio](https://aka.ms/allinoneregioninfo) para obtener más detalles.

## <a name="use-free-resources"></a>Uso de recursos gratis

Puede usar una opción de procesamiento limitada y gratuita para completar los ejercicios del tutorial y la guía de inicio rápido de Cognitive Search.

Los recursos gratuitos (enriquecimientos limitados) se restringen a 20 documentos al día, por suscripción.

1. Abra el Asistente para la importación de datos:

   ![Abra el Asistente para importar datos](media/search-get-started-portal/import-data-cmd.png "Open the Import data wizard")

1. Elija un origen de datos y siga a **Agregar Cognitive Search (opcional)** . Para ver un tutorial paso a paso de este asistente, consulte el artículo sobre cómo [importar, indexar y consultar con las herramientas del portal](search-get-started-portal.md).

1. Expanda **Adjuntar Cognitive Services** y seleccione **Gratis (enriquecimientos limitados)** :

   ![Sección Adjuntar Cognitive Services expandida](./media/cognitive-search-attach-cognitive-services/attach1.png "Expanded Attach Cognitive Services section")

1. Vaya al paso siguiente, **Agregar enriquecimientos**. Para una descripción de las aptitudes disponibles en el portal, consulte [Paso 2: Agregar conocimientos cognitivos](cognitive-search-quickstart-blob.md#create-the-enrichment-pipeline) en el inicio rápido de Cognitive Search.

## <a name="use-billable-resources"></a>Uso de recursos facturables

Para cargas de trabajo que creen más de 20 enriquecimientos al día, asegúrese de asociar un recurso facturable de Cognitive Services. Se recomienda asociar siempre un recurso facturable de Cognitive Services, incluso si nunca ha querido llamar a las API de Cognitive Services. Al asociar un recurso se invalida el límite diario.

Solo se aplican cargos por las aptitudes que llaman a Cognitive Services APIs. No se le facturará por las [aptitudes personalizadas](cognitive-search-create-custom-skill-example.md) o aptitudes como la de [combinación de texto](cognitive-search-skill-textmerger.md), la de [división de texto](cognitive-search-skill-textsplit.md) y la de [conformador](cognitive-search-skill-shaper.md), que no están basadas en API.

1. Abra el Asistente para la importación de datos, elija un origen de datos y siga a **Agregar Cognitive Search (opcional)** .

1. Expanda **Adjuntar Cognitive Services** y, a continuación, seleccione **Crear nuevo recurso de Cognitive Services**. Se abre una nueva pestaña para poder crear el recurso:

   ![Creación de un recurso de Cognitive Services](./media/cognitive-search-attach-cognitive-services/cog-services-create.png "Create a Cognitive Services resource")

1. En la lista **Ubicación**, seleccione la región donde se encuentra su instancia de Azure Search. Asegúrese de usar esta región por motivos de rendimiento. Al usar esta región, también se anulan los cobros por ancho de banda saliente entre regiones.

1. En la lista **Plan de tarifa**, seleccione **S0** para obtener la colección todo en uno de características de Cognitive Services, incluidas las características Visión e Idioma utilizadas para respaldar las aptitudes predefinidas que usa Azure Search.

   Para el nivel S0, encontrará tarifas para cargas de trabajo específicas en la [página de precios de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/).
  
   + En la lista **Select Offer** (Seleccionar oferta), asegúrese de que **Cognitive Services** está seleccionado.
   + En las características **Idioma**, las tarifas para **Text Analytics Standard** se aplican a la indexación de inteligencia artificial.
   + En las características **Visión**, se aplican las tarifas para **Computer Vision S1**.

1. Seleccione **Crear** para aprovisionar el nuevo recurso de Cognitive Services.

1. Vuelva a la pestaña anterior, donde se encuentra el Asistente para la importación de datos. Seleccione **Actualizar** para mostrar el recurso de Cognitive Services y, luego, seleccione el recurso:

   ![Selección del recurso de Cognitive Services](./media/cognitive-search-attach-cognitive-services/attach2.png "Select the Cognitive Services resource")

1. Expanda la sección **Agregar enriquecimientos** para seleccionar las aptitudes cognitivas específicas que desea ejecutar en los datos. Realice los pasos restantes del asistente. Para una descripción de las aptitudes disponibles en el portal, consulte [Paso 2: Agregar conocimientos cognitivos](cognitive-search-quickstart-blob.md#create-the-enrichment-pipeline) en el inicio rápido de Cognitive Search.

## <a name="attach-an-existing-skillset-to-a-cognitive-services-resource"></a>Asociación de un conjunto de aptitudes existentes con un recurso de Cognitive Services

Si ya tiene un conjunto de aptitudes, puede asociarlo a un recurso de Cognitive Services nuevo o distinto.

1. En la página **Información general del servicio**, seleccione **Conjuntos de aptitudes**:

   ![Pestaña Conjuntos de aptitudes](./media/cognitive-search-attach-cognitive-services/attach-existing1.png "Skillsets tab")

1. Seleccione el nombre del conjunto de aptitudes y un recurso existente o cree uno nuevo. Seleccione **Aceptar** para confirmar los cambios.

   ![Lista de recursos del conjunto de aptitudes](./media/cognitive-search-attach-cognitive-services/attach-existing2.png "Lista de recursos del conjunto de aptitudes")

   Recuerde que **Gratis (enriquecimientos limitados)** se limita a 20 documentos diarios y que puede usar **Crear nuevo recurso de Cognitive Services** para aprovisionar un recurso facturable nuevo. Si crea un nuevo recurso, seleccione **Actualizar** para actualizar la lista de recursos de Cognitive Services y, a continuación, seleccione el recurso.

## <a name="attach-cognitive-services-programmatically"></a>Asociación de Cognitive Services mediante programación

Cuando defina un conjunto de aptitudes mediante programación, agregue una sección `cognitiveServices` al conjunto de aptitudes. En la sección, incluya la clave del recurso de Cognitive Services que desea asociar con el conjunto de aptitudes. Recuerde que el recurso debe estar en la misma región que su recurso de Azure Search. Además incluya `@odata.type` y establézcalo en `#Microsoft.Azure.Search.CognitiveServicesByKey`.

En el siguiente ejemplo se muestra este patrón. Observe la sección `cognitiveServices` al final de la definición.

```http
PUT https://[servicename].search.windows.net/skillsets/[skillset name]?api-version=2019-05-06
api-key: [admin key]
Content-Type: application/json
```
```json
{
    "name": "skillset name",
    "skills": 
    [
      {
        "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
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
        "key": "<your key goes here>"
    }
}
```

## <a name="example-estimate-costs"></a>Ejemplo: cálculo de los costos

Para calcular los costos asociados con la indexación de búsqueda cognitiva, empiece con una idea de cómo debe verse un documento promedio para que pueda ejecutar algunos números. Un cálculo aproximado, por ejemplo, sería:

+ 1000 archivos PDF.
+ Seis páginas cada uno.
+ Una imagen por página (6000 imágenes).
+ 3000 caracteres por página.

Supongamos que hay una canalización que consta del descifrado de cada documento PDF con extracción de imágenes y texto, reconocimiento óptico de caracteres (OCR) de las imágenes y reconocimiento de entidad de las organizaciones.

Los precios mostrados en este artículo son hipotéticos. Se usan para ilustrar el proceso de estimación. Sus costos podrían ser más bajos. Para ver los precios reales de las transacciones, consulte [Precios de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services).

1. Para el descifrado de documentos con contenido de texto e imagen, la extracción de texto actualmente es gratis. Para 6000 imágenes, suponga 1 USD por cada 1000 imágenes extraídas. Eso es un costo de 6 USD para este paso.

2. Para realizar el reconocimiento óptico de caracteres (OCR) de 6000 imágenes en inglés, la aptitud de reconocimiento de OCR usa el mejor algoritmo (DescribeText). Suponiendo un precio de 2,50 USD por cada 1000 imágenes que se analizan, tendríamos que pagar 15,00 USD en este paso.

3. Para la extracción de entidades, tendríamos un total de tres registros de texto por página. Cada registro tiene 1000 caracteres. Tres registros de texto por página multiplicados por 6000 páginas equivale a 18 000 registros de texto. Suponiendo un costo de 2,00 USD por cada 1,000 registros de texto, en este paso tendríamos un costo de 36,00 USD.

En resumen, pagaría unos 57 USD por la ingesta de 1000 documentos PDF de este tipo con el conjunto de aptitudes descrito.

## <a name="next-steps"></a>Pasos siguientes
+ [Página de precios de Azure Search](https://azure.microsoft.com/pricing/details/search/)
+ [Definición de un conjunto de aptitudes](cognitive-search-defining-skillset.md)
+ [Crear un conjunto de aptitudes (REST)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Cómo asignar campos enriquecidos](cognitive-search-output-field-mapping.md)
