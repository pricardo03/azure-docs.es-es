---
title: 'Entidad Pattern.any: LUIS'
titleSuffix: Azure Cognitive Services
description: Use la entidad Pattern.any para extraer datos de expresiones con el formato correcto y donde el final de los datos se pueda confundir fácilmente con el resto de las palabras de la expresión.
services: cognitive-services
ms.custom: seodec18
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 06/12/2019
ms.author: diberry
ms.openlocfilehash: 165163d0a889e0bd5f1faef8d7f6a17d28c2b4f7
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68563281"
---
# <a name="tutorial-extract-free-form-data-with-patternany-entity"></a>Tutorial: Extracción de datos de forma libre con una entidad Pattern.any

En este tutorial, use la entidad Pattern.any para extraer datos de expresiones con el formato correcto y donde el final de los datos se puede confundir fácilmente con el resto de las palabras de la expresión. 

**En este tutorial, aprenderá a:**

> [!div class="checklist"]
> * Importar la aplicación de ejemplo
> * Agregar expresiones de ejemplo a la intención existente
> * Crear la entidad Pattern.any
> * Crear el patrón
> * Train
> * Probar el patrón nuevo

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="using-patternany-entity"></a>Uso de la entidad Pattern.any

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
|¿Quién creó el formulario **"Request relocation from employee new to the company 2018 version 5"** ?|
|¿El formulario **Request relocation from employee new to the company 2018 version 5** se publicó en francés?|

La diferencia de longitud incluye palabras que pueden confundir a LUIS con respecto a dónde finaliza la entidad. Si usa una entidad Pattern.any en un patrón, puede especificar el principio y el final del nombre del formulario para que LUIS extraiga correctamente el nombre del formulario.

|Ejemplo de expresión de plantilla|
|--|
|¿Dónde está {NombreDelFormulario}[?]|
|¿Quién ha creado {NombreDelFormulario}[?]|
|¿{NombreDelFormulario} se ha publicado en francés[?]|

## <a name="import-example-app"></a>Importar la aplicación de ejemplo

1. Descargue y guarde el [archivo JSON de la aplicación](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/custom-domain-pattern-roles-HumanResources.json).

1. En el [portal de LUIS](https://www.luis.ai), en la página **Mis aplicaciones**, importe el archivo JSON a una nueva aplicación.

1. Desde la sección **Manage** (Administrar), en la pestaña **Versions** (Versiones), clone la versión y asígnele el nombre `patt-any`. La clonación es una excelente manera de trabajar con distintas características de LUIS sin que afecte a la versión original. Dado que el nombre de la versión se usa como parte de la ruta de la dirección URL, el nombre no puede contener ningún carácter que no sea válido en una dirección URL.

## <a name="add-example-utterances"></a>Incorporación de expresiones de ejemplo 

1. Seleccione **Build** (Compilación) en el menú de navegación superior y seleccione **Intents** (Intenciones) en el menú de navegación de la izquierda.

1. Seleccione **FindForm** en la lista de las intenciones.

1. Agregue algunas expresiones de ejemplo:

    |Expresión de ejemplo|
    |--|
    |¿Dónde está el formulario **What to do when a fire breaks out in the Lab** y quién tiene que firmarlo después de que yo lo lea?|
    |¿Dónde está el formulario **Request relocation from employee new to the company** en el servidor?|
    |¿Quién creó el formulario "**Health and wellness requests on the main campus**" y cuál es la versión más actual?|
    |Estoy buscando el formulario "**Office move request including physical assets**". |

    Sin una entidad Pattern.any, sería difícil que LUIS entendiera dónde finaliza el título del formulario debido a las muchas variaciones de nombres de formulario.

## <a name="create-a-patternany-entity"></a>Creación de una entidad Pattern.any
La entidad Pattern.any extrae entidades de diferente longitud. Solo funciona en un patrón porque este marca el principio y el final de la entidad.  

1. Haga clic en **Entidades** en el panel de navegación izquierdo.

1. Seleccione **Create new entity** (Crear nueva entidad), escriba el nombre `FormName` y seleccione **Pattern.any** como el tipo. Seleccione **Listo**. 

    No puede etiquetar la entidad en las experiencias de ejemplo de una intención, porque Pattern.any solo es válido en un patrón. 

    Si desea que los datos extraídos incluyan otras entidades, como number o datetimeV2, deberá crear una entidad compuesta que incluya la entidad Pattern.any, así como también number y datetimeV2.

## <a name="add-a-pattern-that-uses-the-patternany"></a>Agregar un modelo que usa la entidad Pattern.any

1. Haga clic en **Patrones** en el panel de navegación izquierdo.

1. Haga clic en la intención **FindForm**.

1. Escriba las expresiones de plantilla siguientes que usan la entidad nueva:

    |Expresiones de plantilla|
    |--|
    |[¿]Dónde está el formulario ["]{FormName}["] y quién tiene que firmarlo después de que yo lo lea[?]|
    |[¿]Dónde está el formulario ["]{FormName}["] en el servidor[?]|
    |[¿]Quién creó el formulario ["]{FormName}["] y cuál es la versión más actual[?]|
    |Estoy buscando el formulario ["]{FormName}["][.]|

    Si desea tener en cuenta las variaciones del formulario, como las comillas simples en lugar de comillas dobles o un punto en lugar de un signo de interrogación, cree un patrón nuevo para cada variación.

## <a name="train-the-luis-app"></a>Entrenamiento de la aplicación de LUIS

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="test-the-new-pattern-for-free-form-data-extraction"></a>Probar el nuevo patrón para extraer datos de forma libre
1. Haga clic en **Probar** en la barra superior para abrir el panel de pruebas. 

1. Escriba la expresión siguiente: 

    `Where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?`

1. Haga clic en **Inspeccionar** debajo del resultado para ver los resultados de la prueba de la entidad y la intención.

    Primero se encuentra la entidad `FormName` y luego se encuentra el patrón, que determina la intención. Si tiene un resultado de la prueba en el que no se detectan las entidades y, por tanto, no se encuentra el patrón, debe agregar más expresiones de ejemplo en la intención (no en el patrón).

1. Cierre el panel de pruebas; para ello, haga clic en el botón **Probar** situado en el panel de navegación superior.

## <a name="using-an-explicit-list"></a>Uso de una lista explícita

Si descubre que el patrón, cuando incluye una entidad Pattern.any, extrae entidades de forma incorrecta, use una [lista explícita](luis-concept-patterns.md#explicit-lists) para corregir este problema.


## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial se han agregado expresiones de ejemplo a una intención existente y después se ha creado una entidad Pattern.any para el nombre del formulario. Después, en el tutorial se ha creado un patrón para la intención existente con las nuevas expresiones de ejemplo y la entidad. Las pruebas interactivas mostraron que el patrón y su intención se predijeron porque se encontró la entidad. 

> [!div class="nextstepaction"]
> [Información sobre cómo usar roles con un patrón](luis-tutorial-pattern-roles.md)
