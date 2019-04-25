---
title: 'Asociación de un recurso de Cognitive Services con un conjunto de aptitudes: Azure Search'
description: Instrucciones para asociar una suscripción todo en uno de Cognitive Services a una canalización de enriquecimiento cognitivo en Azure Search.
manager: cgronlun
author: LuisCabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 04/14/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 09695f764ff71b274e125e90835f5314eb25c980
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60344553"
---
# <a name="attach-a-cognitive-services-resource-with-a-skillset-in-azure-search"></a>Asociación de un recurso de Cognitive Services con un conjunto de aptitudes en Azure Search 

Unidad de algoritmos de inteligencia artificial el [cognitivas canalizaciones indexación](cognitive-search-concept-intro.md) utiliza para procesar los datos no estructurados en Azure Search. Estos algoritmos se basan en [recursos de Cognitive Services](https://azure.microsoft.com/services/cognitive-services/), incluidos [Computer Vision](https://azure.microsoft.com/services/cognitive-services/computer-vision/), para el análisis de imágenes y el reconocimiento óptico de caracteres (OCR), y [Text Analytics](https://azure.microsoft.com/services/cognitive-services/text-analytics/), para el reconocimiento de entidades, la extracción de frases clave y otros enriquecimientos.

Puede enriquecer un número limitado de documentos de manera gratuita o bien puede asociar un recurso de Cognitive Services facturable para cargas de trabajo más grandes y más frecuentes. En este artículo, aprenderá a asociar un recurso de Cognitive Services con el conjunto de aptitudes cognitivas para enriquecer los datos durante el [indexado de Azure Search](search-what-is-an-index.md).

Aunque la canalización conste de aptitudes no relacionadas con Cognitive Services APIs, aún deberá asociar un recurso de Cognitive Services. De este modo, se invalida el recurso **gratuito** que establece un límite de una pequeña cantidad de enriquecimientos al día. No se aplica ningún cargo por las aptitudes que no están relacionadas con Cognitive Services APIs. Entre estas aptitudes se incluyen las siguientes: [aptitudes personalizadas](cognitive-search-create-custom-skill-example.md), [combinación de texto](cognitive-search-skill-textmerger.md), [división de texto](cognitive-search-skill-textsplit.md) y [conformador](cognitive-search-skill-shaper.md).

> [!NOTE]
> A medida que expande el ámbito aumentando la frecuencia de procesamiento, agregando más documentos o agregando más algoritmos de inteligencia artificial, tendrá que asociar un recurso facturable de Cognitive Services. Los cargos se acumulan cuando se llama a las API de Cognitive Services y para la extracción de imágenes como parte de la fase de descifrado de documentos en Azure Search. No hay ningún cargo por la extracción de texto de documentos.
>
> Ejecución de [integrados habilidades cognitivas](cognitive-search-predefined-skills.md) ejecución se cobra a la [Cognitive Services paga a medida que vaya precio](https://azure.microsoft.com/pricing/details/cognitive-services), en la misma velocidad como si hubiera realiza la tarea directamente. Extracción de la imagen es un cargo de Azure Search, se reflejan en el [página de precios de Azure Search](https://go.microsoft.com/fwlink/?linkid=2042400).


## <a name="use-free-resources"></a>Uso de recursos gratis

Puede usar una opción de procesamiento limitada y gratuita para completar los ejercicios del tutorial y la guía de inicio rápido de Cognitive Search. 

**Gratis (Enriquecimientos limitado)** están restringidos a los 20 documentos al día por suscripción.

1. Abra el Asistente para la **importación de datos**.

   ![Comando de importación de datos](media/search-get-started-portal/import-data-cmd2.png "Comando de importación de datos")

1. Elija un origen de datos y siga a **Agregar Cognitive Search (opcional)**. Para ver un tutorial paso a paso de este asistente, consulte el artículo sobre cómo [importar, indexar y consultar con las herramientas del portal](search-get-started-portal.md).

1. Expanda **Adjuntar Cognitive Services** y seleccione **Gratis (enriquecimientos limitados)**.

   ![Sección Adjuntar Cognitive Services expandida](./media/cognitive-search-attach-cognitive-services/attach1.png "Expanded Attach Cognitive Services")

Vaya al paso siguiente, **Agregar enriquecimientos**. Para una descripción de las aptitudes disponibles en el portal, consulte ["Paso 2: Agregar conocimientos cognitivos"](cognitive-search-quickstart-blob.md#create-the-enrichment-pipeline) en el inicio rápido de Cognitive Search.

## <a name="use-billable-resources"></a>Uso de recursos facturables

Para cargas de trabajo que superen los 20 enriquecimientos diarios, deberá asociar un recurso facturable de Cognitive Services. 

Solo se aplican cargos por las aptitudes que llaman a Cognitive Services APIs. No se aplicarán cargos por las aptitudes que no están basadas en API, como las [aptitudes personalizadas](cognitive-search-create-custom-skill-example.md), la de [combinación de texto](cognitive-search-skill-textmerger.md), la de [división de texto](cognitive-search-skill-textsplit.md) y la de [conformador](cognitive-search-skill-shaper.md).

1. Abra el **importar datos** asistente, elija un origen de datos y seguir **Agregar búsqueda cognitiva (opcional)**. 

1. Expanda **adjuntar Cognitive Services** y, a continuación, seleccione **crear nuevo recurso de Cognitive Services**. Se abrirá una nueva pestaña para que pueda crear el recurso. 

   ![Crear un recurso de Cognitive Services](./media/cognitive-search-attach-cognitive-services/cog-services-create.png "crear un recurso de Cognitive Services")

1. En la ubicación, elija la misma región que Azure Search para evitar cargos de ancho de banda saliente entre regiones.

1. En el nivel de precios, elija **S0** para obtener la colección all-in-one de características de Cognitive Services, incluidas las funciones de visión y el idioma que respaldan las habilidades predefinidas usadas por Azure Search. 

   Para el nivel S0, puede encontrar las tarifas para cargas de trabajo específicas en el [página de precios de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/).
  
   + En **seleccione ofrecen**, asegúrese de que *Cognitive Services* está seleccionada.
   + En las características del lenguaje, las tasas de *estándar de análisis de texto* se aplican a la indexación de inteligencia artificial. 
   + En las funciones de visión, las tasas de *equipo Vision S1* se aplican.

1. Haga clic en **Crear** para aprovisionar el nuevo recurso de Cognitive Services. 

1. Vuelva a la pestaña anterior donde se encuentra el Asistente para la **importación de datos**. Haga clic en **Actualizar** para mostrar el recurso de Cognitive Services y, luego, seleccione el recurso.

   ![Recurso de Cognitive Services seleccionado](./media/cognitive-search-attach-cognitive-services/attach2.png "Selected Cognitive Service Resource")

1. Expanda la sección **Agregar enriquecimiento** para seleccionar las aptitudes cognitivas específicas que desea ejecutar en los datos y continuar con el resto del flujo. Para una descripción de las aptitudes disponibles en el portal, consulte ["Paso 2: Agregar conocimientos cognitivos"](cognitive-search-quickstart-blob.md#create-the-enrichment-pipeline) en el inicio rápido de Cognitive Search.

## <a name="attach-an-existing-skillset-to-a-cognitive-services-resource"></a>Asociación de un conjunto de aptitudes existentes con un recurso de Cognitive Services

Si ya tiene un conjunto de aptitudes, puede asociarlo a un recurso de Cognitive Services nuevo o distinto.

1. En la página **Service overview** (Introducción al servicio), haga clic en **Conjuntos de aptitudes**.

   ![Pestaña Conjuntos de aptitudes](./media/cognitive-search-attach-cognitive-services/attach-existing1.png "Skillsets tab")

1. Haga clic en el nombre del conjunto de aptitudes y seleccione un recurso existente o cree uno nuevo. Haga clic en **Aceptar** para confirmar los cambios. 

   ![Lista de recursos del conjunto de aptitudes](./media/cognitive-search-attach-cognitive-services/attach-existing2.png "Lista de recursos del conjunto de aptitudes")

Recuerde que **Gratis (enriquecimientos limitados)** se limita a 20 documentos diarios y que **Crear nuevo recurso de Cognitive Services** se usa para aprovisionar un recurso facturable nuevo. Si crea un nuevo recurso, seleccione **Actualizar** para actualizar la lista de recursos de Cognitive Services y, a continuación, seleccione el recurso.

## <a name="attach-cognitive-services-programmatically"></a>Asociación de Cognitive Services mediante programación

Cuando defina un conjunto de aptitudes mediante programación, agregue una sección `cognitiveServices` al conjunto de aptitudes. En la sección, incluya la clave del recurso de Cognitive Services que desea asociar con el conjunto de aptitudes. Recuerde que el recurso debe estar en la misma región que Azure Search. Además incluya `@odata.type` y establézcalo en `#Microsoft.Azure.Search.CognitiveServicesByKey`. 

En el siguiente ejemplo se muestra este patrón. Observe la sección cognitiveServices que está en la parte inferior de la definición.

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
        "key": "<your key goes here>"
    }
}
```

## <a name="example-estimate-costs"></a>Ejemplo: cálculo de los costos

Para calcular los costos asociados con el indexado de Cognitive Search, empiece con una idea de cómo debe verse un documento promedio para que pueda ejecutar algunos números. Por ejemplo, para realizar el cálculo, podría tomar como un cálculo aproximado:

+ 1000 archivos PDF
+ Seis páginas cada uno
+ Una imagen por página (6000 imágenes)
+ 3000 caracteres por página

Supongamos que hay una canalización que consta del descifrado de cada documento PDF con extracción de imágenes y texto, reconocimiento óptico de caracteres (OCR) de las imágenes y reconocimiento de entidad con nombre de las organizaciones. 

En este ejercicio, usamos el mayor precio por transacción. Los costos reales podrían ser menores debido a los precios escalonados. Consulte los [precios de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services).

1. Para el descifrado de documentos con contenido de texto e imagen, la extracción de texto actualmente es gratis. Para 6000 imágenes, suponga 1 USD por cada 1000 imágenes extraídas, con un costo de 6 USD por este paso.

2. Para realizar el reconocimiento óptico de caracteres (OCR) de 6000 imágenes en inglés, la aptitud de reconocimiento de OCR usa el mejor algoritmo (DescribeText). Suponiendo un precio de 2,50 USD por cada 1000 imágenes que se analizan, tendríamos que pagar 15,00 USD en este paso.

3. Para la extracción de entidades, tendríamos un total de 3 registros de texto por página. Cada registro tiene 1000 caracteres. Tres registros de texto por página * 6000 páginas = 18 000 registros de texto. Suponiendo un costo de 2,00 USD por cada 1,000 registros de texto, en este paso tendríamos un costo de 36,00 USD.

En resumen, pagaría unos 57 USD por la ingesta de 1000 documentos PDF de esta naturaleza con el conjunto de aptitudes descrito. 

## <a name="next-steps"></a>Pasos siguientes
+ [Página de precios de Azure Search](https://azure.microsoft.com/pricing/details/search/)
+ [Definición de un conjunto de aptitudes](cognitive-search-defining-skillset.md)
+ [Crear un conjunto de aptitudes (REST)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Cómo asignar campos enriquecidos](cognitive-search-output-field-mapping.md)
