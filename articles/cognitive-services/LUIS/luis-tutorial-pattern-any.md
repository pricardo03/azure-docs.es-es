---
title: 'Tutorial 5: Entidad Pattern.any para texto de forma libre'
titleSuffix: Azure Cognitive Services
description: Use la entidad Pattern.any para extraer datos de expresiones con el formato correcto y donde el final de los datos se pueda confundir fácilmente con el resto de las palabras de la expresión.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: 9038947ad20f63638af1830ca660de8cac802d9b
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "51282097"
---
# <a name="tutorial-5-extract-free-form-data"></a>Tutorial 5: Extracción de datos de forma libre

En este tutorial, use la entidad Pattern.any para extraer datos de expresiones con el formato correcto y donde el final de los datos se puede confundir fácilmente con el resto de las palabras de la expresión. 

La entidad Pattern.any permite buscar datos de forma libre en los que la redacción de la entidad dificulte determinar el final de la entidad del resto de la expresión. 

Esta aplicación de recursos humanos ayuda a que los empleados encuentren formularios de la empresa. 

|Expresión|
|--|
|¿Dónde está el formulario **HRF-123456**?|
|¿Quién creó el formulario **HRF-123234**?|
|¿El formulario **HRF-456098** se publicó en francés?|

Sin embargo, cada formulario tiene un nombre con formato, el que se usa en la tabla anterior, además de un nombre descriptivo, como `Request relocation from employee new to the company 2018 version 5`. 

Las expresiones con el nombre de formato descriptivo tienen el aspecto siguiente:

|Expresión|
|--|
|¿Dónde está el formulario "**Request relocation from employee new to the company 2018 version 5**"?|
|¿Quién creó el formulario **"Request relocation from employee new to the company 2018 version 5"**?|
|¿El formulario **Request relocation from employee new to the company 2018 version 5** se publicó en francés?|

La diferencia de longitud incluye palabras que pueden confundir a LUIS con respecto a dónde finaliza la entidad. Si usa una entidad Pattern.any en un patrón, puede especificar el principio y el final del nombre del formulario para que LUIS extraiga correctamente el nombre del formulario.

|Ejemplo de expresión de plantilla|
|--|
|¿Dónde está {NombreDelFormulario}[?]|
|¿Quién ha creado {NombreDelFormulario}[?]|
|¿{NombreDelFormulario} se ha publicado en francés[?]|

**En este tutorial, aprenderá a:**

> [!div class="checklist"]
> * Usar la aplicación del tutorial existente
> * Agregar expresiones de ejemplo a la intención existente
> * Crear la entidad Pattern.any
> * Crear el patrón
> * Train
> * Probar el patrón nuevo

[!INCLUDE[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Uso de una aplicación existente
Continúe con la aplicación creada en el último tutorial, denominada **HumanResources**. 

Si no tiene la aplicación HumanResources del tutorial anterior, siga estos pasos:

1.  Descargue y guarde el [archivo JSON de la aplicación](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-roles-HumanResources.json).

2. Importe el archivo JSON en una aplicación nueva.

3. Desde la sección **Manage** (Administrar), en la pestaña **Versions** (Versiones), clone la versión y asígnele el nombre `patt-any`. La clonación es una excelente manera de trabajar con distintas características de LUIS sin que afecte a la versión original. Dado que el nombre de la versión se usa como parte de la ruta de la dirección URL, el nombre no puede contener ningún carácter que no sea válido en una dirección URL.

## <a name="add-example-utterances"></a>Incorporación de expresiones de ejemplo 
Quite la entidad keyPhrase precompilada si resulta difícil compilar y etiquetar la entidad FormName. 

1. Seleccione **Build** (Compilación) en el menú de navegación superior y seleccione **Intents** (Intenciones) en el menú de navegación de la izquierda.

2. Seleccione **FindForm** en la lista de las intenciones.

3. Agregue algunas expresiones de ejemplo:

    |Expresión de ejemplo|
    |--|
    |¿Dónde está el formulario **What to do when a fire breaks out in the Lab** y quién tiene que firmarlo después de que yo lo lea?|
    |¿Dónde está el formulario **Request relocation from employee new to the company** en el servidor?|
    |¿Quién creó el formulario "**Health and wellness requests on the main campus**" y cuál es la versión más actual?|
    |Estoy buscando el formulario "**Office move request including physical assets**". |

    Sin una entidad Pattern.any, sería difícil que LUIS entendiera dónde finaliza el título del formulario debido a las muchas variaciones de nombres de formulario.

## <a name="create-a-patternany-entity"></a>Creación de una entidad Pattern.any
La entidad Pattern.any extrae entidades de diferente longitud. Solo funciona en un patrón porque este marca el principio y el final de la entidad. Si descubre que el patrón, cuando incluye una entidad Pattern.any, extrae entidades de forma incorrecta, use una [lista explícita](luis-concept-patterns.md#explicit-lists) para corregir este problema. 

1. Haga clic en **Entidades** en el panel de navegación izquierdo.

2. Seleccione **Create new entity** (Crear nueva entidad), escriba el nombre `FormName` y seleccione **Pattern.any** como el tipo. Seleccione **Listo**. 

    No se puede etiquetar la entidad en la intención porque una entidad Pattern.any solo es válida en un patrón. 

    Si desea que los datos extraídos incluyan otras entidades, como number o datetimeV2, deberá crear una entidad compuesta que incluya la entidad Pattern.any, así como también number y datetimeV2.

## <a name="add-a-pattern-that-uses-the-patternany"></a>Agregar un modelo que usa la entidad Pattern.any

1. Haga clic en **Patrones** en el panel de navegación izquierdo.

2. Haga clic en la intención **FindForm**.

3. Escriba las expresiones de plantilla siguientes que usan la entidad nueva:

    |Expresiones de plantilla|
    |--|
    |[¿]Dónde está el formulario ["]{FormName}["] y quién tiene que firmarlo después de que yo lo lea[?]|
    |[¿]Dónde está el formulario ["]{FormName}["] en el servidor[?]|
    |[¿]Quién creó el formulario ["]{FormName}["] y cuál es la versión más actual[?]|
    |Estoy buscando el formulario ["]{FormName}["][.]|

    Si desea tener en cuenta las variaciones del formulario, como las comillas simples en lugar de comillas dobles o un punto en lugar de un signo de interrogación, cree un patrón nuevo para cada variación.

4. Si quitó la entidad keyPhrase, vuelva a agregarla a la aplicación. 

## <a name="train-the-luis-app"></a>Entrenamiento de la aplicación de LUIS

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="test-the-new-pattern-for-free-form-data-extraction"></a>Probar el nuevo patrón para extraer datos de forma libre
1. Haga clic en **Probar** en la barra superior para abrir el panel de pruebas. 

2. Escriba la expresión siguiente: 

    `Where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?`

3. Haga clic en **Inspeccionar** debajo del resultado para ver los resultados de la prueba de la entidad y la intención.

    Primero se encuentra la entidad `FormName` y luego se encuentra el patrón, que determina la intención. Si tiene un resultado de la prueba en el que no se detectan las entidades y, por tanto, no se encuentra el patrón, debe agregar más expresiones de ejemplo en la intención (no en el patrón).

4. Cierre el panel de pruebas; para ello, haga clic en el botón **Probar** situado en el panel de navegación superior.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial se han agregado expresiones de ejemplo a una intención existente y después se ha creado una entidad Pattern.any para el nombre del formulario. Después, en el tutorial se ha creado un patrón para la intención existente con las nuevas expresiones de ejemplo y la entidad. Las pruebas interactivas mostraron que el patrón y su intención se predijeron porque se encontró la entidad. 

> [!div class="nextstepaction"]
> [Información sobre cómo usar roles con un patrón](luis-tutorial-pattern-roles.md)