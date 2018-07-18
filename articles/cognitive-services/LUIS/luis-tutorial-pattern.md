---
title: 'Tutorial de uso de patrones para mejorar las predicciones de LUIS: Azure | Microsoft Docs'
titleSuffix: Azure
description: En este tutorial, use patrones para intenciones a fin de mejorar las predicciones de intenciones y entidades de LUIS.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr;
ms.openlocfilehash: ff5572366be548132b28e5ce03b9595e7f98128c
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265323"
---
# <a name="tutorial-use-patterns-to-improve-predictions"></a>Tutorial: Usar patrones para mejorar las predicciones

En este tutorial, use patrones para mejorar la predicción de intenciones y entidades.  

> [!div class="checklist"]
* Cómo identificar que un patrón ayudaría a la aplicación
* Cómo crear un patrón 
* Cómo usar entidades creadas previamente y personalizadas en un patrón 
* Cómo comprobar las mejoras de la predicción de patrones
* Cómo agregar un rol a una entidad para buscar entidades basadas en el contexto
* Cómo agregar un Pattern.any para buscar entidades de forma libre

Para este artículo, necesita una cuenta de [LUIS][LUIS] gratuita para crear la aplicación de LUIS.

## <a name="import-humanresources-app"></a>Importar la aplicación HumanResources
Este tutorial importa una aplicación HumanResources. La aplicación tiene tres intenciones: None, GetEmployeeOrgChart y GetEmployeeBenefits. La aplicación tiene dos entidades: Prebuilt number y Employee. La entidad Employee es una entidad simple para extraer el nombre de un empleado. 

1. Cree un archivo de aplicación de LUIS y asígnele el nombre `HumanResources.json`. 

2. Copie la siguiente definición de aplicación en el archivo:

   [!code-json[Add the LUIS model](~/samples-luis/documentation-samples/tutorial-patterns/HumanResources.json?range=1-164 "Add the LUIS model")]

3. En la página **Aplicaciones** de LUIS, haga clic en **Import new app** (Importar aplicación nueva). 

4. En el cuadro de diálogo **Import new app** (Importar aplicación nueva), seleccione el archivo `HumanResources.json` que ha creado en el paso 1.

5. Seleccione la intención **GetEmployeeOrgChart** y cambie de **Entities view** (Vista de entidades) a **Tokens view** (Vista de tokens). Se muestran varias expresiones de ejemplo. Cada expresión contiene un nombre, que es una entidad Employee. Tenga en cuenta que cada nombre es diferente y que la organización de las palabras es diferente en cada expresión. Esta diversidad ayuda a que LUIS aprenda una amplia gama de expresiones.

    ![Captura de pantalla de la página Intención con la Vista de entidades activada](media/luis-tutorial-pattern/utterances-token-view.png)

6. Haga clic en **Entrenar** en la barra de navegación superior para entrenar la aplicación. Espere a que aparezca la barra de color verde que indica que se ha realizado correctamente.

7. Haga clic en **Probar** en el panel superior. Escriba `Who does Patti Owens report to?` y presione Entrar. Haga clic en **Inspeccionar** debajo de la expresión para obtener más información sobre la prueba.
    
    El nombre de la empleada, Patti Owens, aún no se ha usado en una expresión de ejemplo. Se trata de una prueba para ver lo bien que ha aprendido LUIS que esta expresión es para la intención `GetEmployeeOrgChart` y la entidad Employee debería ser `Patti Owens`. El resultado debería ser inferior al 50 % (.50) en la intención `GetEmployeeOrgChart`. Aunque la intención es correcta, la puntuación es baja. La entidad Employee también se identifica correctamente como `Patti Owens`. Los patrones aumentan esta puntuación inicial de la predicción. 

    ![Captura de pantalla del panel Probar](media/luis-tutorial-pattern/original-test.png)

8. Cierre el panel de pruebas; para ello, haga clic en el botón **Probar** situado en el panel de navegación superior. 

## <a name="patterns-teach-luis-common-utterances-with-fewer-examples"></a>Los patrones enseñan a LUIS expresiones comunes con menos ejemplos
Dada la naturaleza del dominio de recursos humanos, hay algunas formas habituales de preguntar por relaciones de empleados en las organizaciones. Por ejemplo: 

```
Who does Mike Jones report to?
Who reports to Mike Jones? 
```

Estas expresiones son demasiado cercanas como para determinar la singularidad contextual de cada una de ellas sin proporcionar muchos ejemplos de expresiones. Al agregar un patrón para una intención, LUIS aprende patrones de expresión comunes para una intención sin proporcionar muchos ejemplos de expresiones. 

Entre las expresiones de plantilla de ejemplo de esta intención se incluyen las siguientes:

```
Who does {Employee} report to?
Who reports to {Employee}? 
```

El patrón es una combinación de una coincidencia de expresión regular y el aprendizaje automático. A continuación, proporcione algunos ejemplos de expresión de plantilla para que LUIS aprenda el patrón. Estos ejemplos, junto con las expresiones de la intención, ayudan a que LUIS reconozca mejor qué expresiones se ajustan a la intención y dónde existe la entidad dentro de la expresión. <!--A pattern is specific to an intent. You can't duplicate the same pattern on another intent. That would confuse LUIS, which lowers the prediction score. -->

## <a name="add-the-template-utterances"></a>Agregar las expresiones de plantilla

1. En el panel de navegación izquierdo, debajo de **Improve app performance** (Mejorar el rendimiento de la aplicación), seleccione **Patrones**.

2. Seleccione la intención **GetEmployeeOrgChart** y, después, escriba las siguientes expresiones de plantilla, una de cada vez, y presione Entrar después de cada expresión de plantilla:

    ```
    Does {Employee} have {number} subordinates?
    Does {Employee} have {number} direct reports?
    Who does {Employee} report to?
    Who reports to {Employee}?
    Who is {Employee}'s manager?
    Who are {Employee}'s subordinates?
    ```

    La sintaxis `{Employee}` marca la ubicación de la entidad dentro de la expresión de plantilla, así como qué entidad. 

    ![Captura de pantalla del ingreso de expresiones de plantilla para la intención](./media/luis-tutorial-pattern/enter-pattern.png)

3. Haga clic en **Entrenar** en la barra de navegación superior. Espere a que aparezca la barra de color verde que indica que se ha realizado correctamente.

4. Haga clic en **Probar** en el panel superior. Escriba `Who does Patti Owens report to?` en el cuadro de texto. Presione Entrar. Esta es la misma expresión que ha probado en la sección anterior. El resultado debería ser superior en la intención `GetEmployeeOrgChart`. 

    La puntuación es ahora mucho mejor. LUIS ha aprendido el patrón correspondiente a la intención sin proporcionar muchos ejemplos.

    ![Captura de pantalla del panel Probar con un resultado de puntuación alto](./media/luis-tutorial-pattern/high-score.png)

    Primero se encuentra la entidad y después el patrón, que indica la intención. Si tiene un resultado de la prueba en el que no se detecta la entidad y, por tanto, no se encuentra el patrón, debe agregar más expresiones de ejemplo en la intención (no en el patrón). 

5. Cierre el panel de pruebas; para ello, haga clic en el botón **Probar** situado en el panel de navegación superior.

## <a name="use-an-entity-with-a-role-in-a-pattern"></a>Usar una entidad con un rol en un patrón
La aplicación de LUIS se usa para ayudar a trasladar a los empleados de una ubicación a otra. Una expresión de ejemplo es `Move Bob Jones from Seattle to Los Colinas`. Cada ubicación en la expresión tiene un significado diferente. Seattle es la ubicación de origen y Los Colinas es la ubicación de destino del traslado. Para distinguir estas ubicaciones en el patrón, en las siguientes secciones creará una entidad simple para la ubicación con dos roles: origen y destino. 

### <a name="create-a-new-intent-for-moving-people-and-assets"></a>Crear una intención para trasladar personas y activos
Cree una intención para todas las expresiones que tratan de trasladar a personas o activos.

1. Haga clic en **Intents** (Intenciones) en el panel de navegación izquierdo.
2. Haga clic en **Create new intent** (Crear intención).
3. Escriba el nombre de la nueva intención `MoveAssetsOrPeople`.
4. Agregue expresiones de ejemplo:

    ```
    Move Bob Jones from Seattle to Los Colinas
    Move Dave Cooper from Redmond to Seattle
    Move Jim Smith from Toronto to Vancouver
    Move Jill Benson from Boston to London
    Move Travis Hinton from Portland to Orlando
    ```
    ![Captura de pantalla de expresiones de ejemplo para la intención MoveAssetsOrPeople](./media/luis-tutorial-pattern/intent-moveasserts-example-utt.png)

    El objetivo de las expresiones de ejemplo es proporcionar ejemplos suficientes. Si, más adelante en la prueba, no se detecta la entidad de ubicación y, por tanto, no se detecta el patrón, vuelva a este paso y agregue más ejemplos. Después, vuelva a entrenar y probar. 

5. Marque las entidades en las expresiones de ejemplo con la entidad Employee; para ello, seleccione el nombre y el apellido en una expresión, y luego seleccione la entidad Employee en la lista.

    ![Captura de pantalla de expresiones en MoveAssetsOrPeople marcadas con la entidad Employee](./media/luis-tutorial-pattern/intent-moveasserts-employee.png)

6. Seleccione el texto `portland` en la expresión `move travis hinton from portland to orlando`. En el cuadro de diálogo emergente, escriba el nuevo nombre de la entidad `Location` y haga clic en **Crear nueva entidad**. Elija el tipo de entidad **Simple** y haga clic en **Listo**.

    ![Captura de pantalla de la creación de una entidad de ubicación](./media/luis-tutorial-pattern/create-new-location-entity.png)

    Marque el resto de los nombres de ubicación en las expresiones. 

    ![Captura de pantalla de todas las entidades marcadas](./media/luis-tutorial-pattern/moveasset-all-entities-labeled.png)

    El patrón de elección y orden de las palabras es evidente en la imagen anterior. Si **no** usaba patrones y las expresiones de la intención tienen un patrón claro, es una buena indicación de que debería usar patrones. 

    Si espera una gran variedad de expresiones, en lugar de un patrón, estas serían las expresiones de ejemplo incorrectas. En ese caso, sería aconsejable usar expresiones muy diferentes en lo que a la elección de palabras, la longitud de la expresión y la ubicación de la entidad se refiere. 

<!--TBD: what guidance to move from hier entities to patterns with roles -->
<!--    The [Hierarchical entity quickstart](luis-quickstart-intent-and-hier-entity.md) uses the  same idea of location but uses child entities to find origin and destination locations. 
-->
### <a name="add-role-to-location-entity"></a>Agregar un rol a la entidad de ubicación 
Los roles solo se pueden usar en los patrones. Agregue los roles Origin y Destination a la entidad Location. 

1. Haga clic en **Entidades** en el panel de navegación izquierdo y después en **Location** (Ubicación) en la lista de entidades.

2. Agregue los roles `Origin` y `Destination` a la entidad.

    ![Captura de pantalla de la nueva entidad con roles](./media/luis-tutorial-pattern/location-entity.png)

    Los roles no están marcados en la página de la intención MoveAssetsOrPeople porque no hay roles en las expresiones de la intención. Solo están en las expresiones de plantilla de patrón. 

### <a name="add-template-utterances-that-uses-location-and-destination-roles"></a>Agregar expresiones de plantilla que usan los roles de ubicación y destino
Agregue expresiones de plantilla que usan la nueva entidad.

1. Haga clic en **Patrones** en el panel de navegación izquierdo.

2. Seleccione la intención **MoveAssetsOrPeople**.

3. Escriba una nueva expresión de plantilla con la nueva entidad `Move {Employee} from {Location:Origin} to {Location:Destination}`. La sintaxis de una entidad y un rol dentro de una expresión de plantilla es `{entity:role}`.

    ![Captura de pantalla de la nueva entidad con roles](./media/luis-tutorial-pattern/pattern-moveassets.png)

4. Entrene la aplicación para la intención, la entidad y el patrón nuevos.

### <a name="test-the-new-pattern-for-role-data-extraction"></a>Probar el nuevo patrón para extraer los datos del rol
Valide el nuevo patrón con una prueba.

1. Haga clic en **Probar** en el panel superior. 
2. Escriba la expresión `Move Tammi Carlson from Bellingham to Winthrop`.
3. Haga clic en **Inspeccionar** debajo del resultado para ver los resultados de la prueba de la entidad y la intención.

    ![Captura de pantalla de la nueva entidad con roles](./media/luis-tutorial-pattern/test-with-roles.png)

    Primero se encuentran las entidades y después el patrón, que indica la intención. Si tiene un resultado de la prueba en el que no se detectan las entidades y, por tanto, no se encuentra el patrón, debe agregar más expresiones de ejemplo en la intención (no en el patrón). 

4. Cierre el panel de pruebas; para ello, haga clic en el botón **Probar** situado en el panel de navegación superior.

## <a name="use-a-patternany-entity-to-find-free-form-entities-in-a-pattern"></a>Usar una entidad Pattern.any para buscar entidades de forma libre en un patrón
Esta aplicación HumanResources también ayuda a los empleados a encontrar formularios de la empresa. Muchos de los formularios tienen títulos con diferentes longitudes. La diferencia de longitud incluye frases que pueden confundir a LUIS en relación con dónde finaliza el nombre del formulario. Si usa una entidad **Pattern.any** en un patrón, puede especificar el principio y el final del nombre del formulario para que LUIS extraiga correctamente el nombre del formulario. 

### <a name="create-a-new-intent-for-the-form"></a>Crear una intención para el formulario
Cree una intención para las expresiones que buscan formularios.

1. Haga clic en **Intents** (Intenciones) en el panel de navegación izquierdo.

2. Haga clic en **Create new intent** (Crear intención).

3. Escriba el nombre de la nueva intención `FindForm`.

4. Agregue una expresión de ejemplo.

    ```
    `Where is the form What to do when a fire breaks out in the Lab and who needs to sign it after I read it?`
    ```

    ![Captura de pantalla de la nueva entidad con roles](./media/luis-tutorial-pattern/intent-findform.png)

    El título del formulario es `What to do when a fire breaks out in the Lab`. La expresión pregunta la ubicación del formulario y también quién tiene que firmarlo para validar que el empleado lo ha leído. Sin una entidad Pattern.any, sería difícil entender dónde finaliza el título del formulario y extraerlo como una entidad de la expresión.

### <a name="create-a-patternany-entity-for-the-form-title"></a>Crear una entidad Pattern.any para el título del formulario
La entidad Pattern.any permite entidades de diferente longitud. Solo funciona en un patrón porque este marca el principio y el final de la entidad. Si descubre que el patrón, cuando incluye una entidad Pattern.any, extrae entidades de forma incorrecta, use una [lista explícita](luis-concept-patterns.md#explicit-lists) para corregir este problema. 

1. Haga clic en **Entidades** en el panel de navegación izquierdo.

2. Haga clic en **Crear nueva entidad**. 

3. Denomine a la entidad `FormName` con el tipo **Pattern.any**. Para este tutorial en concreto, no tiene que agregar ningún rol a la entidad.

    ![Imagen del cuadro de diálogo del nombre de la entidad y el tipo de entidad](./media/luis-tutorial-pattern/create-entity-pattern-any.png)

### <a name="add-a-pattern-that-uses-the-patternany"></a>Agregar un modelo que usa la entidad Pattern.any

1. Haga clic en **Patrones** en el panel de navegación izquierdo.

2. Haga clic en la intención **FindForm**.

3. Escriba una expresión de plantilla con la nueva entidad `Where is the form {FormName} and who needs to sign it after I read it?`.

    ![Captura de pantalla de la expresión de plantilla que usa la entidad pattern.any](./media/luis-tutorial-pattern/pattern.any-template-utterance.png)

4. Entrene la aplicación para la intención, la entidad y el patrón nuevos.

### <a name="test-the-new-pattern-for-free-form-data-extraction"></a>Probar el nuevo patrón para extraer datos de forma libre
1. Haga clic en **Probar** en la barra superior para abrir el panel de pruebas. 

2. Escriba la expresión `Where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?`.

3. Haga clic en **Inspeccionar** debajo del resultado para ver los resultados de la prueba de la entidad y la intención.

    ![Captura de pantalla de la expresión de plantilla que usa la entidad pattern.any](./media/luis-tutorial-pattern/test-pattern.any-results.png)

    Primero se encuentra la entidad y después el patrón, que indica la intención. Si tiene un resultado de la prueba en el que no se detectan las entidades y, por tanto, no se encuentra el patrón, debe agregar más expresiones de ejemplo en la intención (no en el patrón).

4. Cierre el panel de pruebas; para ello, haga clic en el botón **Probar** situado en el panel de navegación superior.

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando ya no sea necesaria, elimine la aplicación de LUIS. Para ello, seleccione el menú de tres puntos (...) situado a la derecha del nombre de la aplicación en la lista de aplicaciones y haga clic en **Eliminar**. En el cuadro de diálogo emergente **Delete app?** (¿Eliminar aplicación?), haga clic en **Ok** (Aceptar).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Usar la lista de frases para mejorar la predicción](luis-tutorial-interchangeable-phrase-list.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions