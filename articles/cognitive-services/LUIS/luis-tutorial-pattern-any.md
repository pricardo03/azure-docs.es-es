---
title: 'Tutorial de uso de la entidad pattern.any para mejorar las predicciones de LUIS: Azure | Microsoft Docs'
titleSuffix: Cognitive Services
description: En este tutorial, use la entidad pattern.any para mejorar las predicciones de intenciones y entidades de LUIS.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: 43f169ae11191c2e98c4538189bce781821de980
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44157861"
---
# <a name="tutorial-improve-app-with-patternany-entity"></a>Tutorial: Mejorar la aplicación con la entidad pattern.any

En este tutorial, use la entidad pattern.any para aumentar las predicciones de intenciones y entidades.  

> [!div class="checklist"]
* Obtener información de cómo y cuándo usar la entidad pattern.any
* Crear el patrón que usa la entidad pattern.any
* Comprobar las mejoras de las predicciones

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Antes de empezar
Si no tiene la aplicación de recursos humanos del tutorial sobre [roles de patrón](luis-tutorial-pattern-roles.md), [importe](luis-how-to-start-new-app.md#import-new-app) el archivo JSON a una nueva aplicación en el sitio web de [LUIS](luis-reference-regions.md#luis-website). La aplicación que se va a importar se encuentra en el repositorio de GitHub [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-roles-HumanResources.json).

Si quiere conservar la aplicación original de Recursos humanos, clone la versión en la página [Configuración](luis-how-to-manage-versions.md#clone-a-version) y llámela `patt-any`. La clonación es una excelente manera de trabajar con distintas características de LUIS sin que afecte a la versión original. 

## <a name="the-purpose-of-patternany"></a>El propósito de la entidad pattern.any
La entidad pattern.any permite buscar datos de forma libre en los que la redacción de la entidad dificulte determinar el final de la entidad a partir del resto de la expresión. 

Esta aplicación de recursos humanos ayuda a que los empleados encuentren formularios de la empresa. Los formularios se agregaron en el [tutorial sobre expresiones regulares](luis-quickstart-intents-regex-entity.md). Los nombres de formulario de ese tutorial usaban una expresión regular para extraer el nombre de un formulario con el formato correcto, como los nombres de formato en negrita en la tabla de expresiones siguiente:

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

La diferencia de longitud incluye frases que pueden confundir a LUIS con respecto a dónde finaliza la entidad. Si usa una entidad Pattern.any en un patrón, puede especificar el principio y el final del nombre del formulario para que LUIS extraiga correctamente el nombre del formulario.

**Si bien los patrones permiten proporcionar menos expresiones de ejemplo, si no se detectan las entidades, el patrón no coincide.**

## <a name="add-example-utterances-to-the-existing-intent-findform"></a>Incorporación de expresiones de ejemplo a la intención FindForm existente 
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

> [!div class="nextstepaction"]
> [Información sobre cómo usar roles con un patrón](luis-tutorial-pattern-roles.md)