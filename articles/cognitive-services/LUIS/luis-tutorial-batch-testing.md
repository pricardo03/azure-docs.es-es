---
title: Pruebas en lote
titleSuffix: Azure Cognitive Services
description: En este tutorial se muestra cómo usar las pruebas por lotes para encontrar problemas de predicción de expresiones en la aplicación y corregirlos.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/29/2019
ms.author: diberry
ms.openlocfilehash: af04ca19961abcfc7ee218824a4a1a804f7ad79c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65146157"
---
# <a name="tutorial-batch-test-data-sets"></a>Tutorial: Pruebas en lote de conjuntos de datos

En este tutorial se muestra cómo usar las pruebas por lotes para encontrar problemas de predicción de expresiones en la aplicación y corregirlos.  

Las pruebas por lotes permiten validar el estado del modelo entrenado activo con un conjunto conocido de entidades y expresiones etiquetadas. En el archivo por lotes con formato JSON, agregue las expresiones y establezca las etiquetas de entidad que necesita predecir dentro de la expresión. 

Requisitos de las pruebas por lotes:

* Máximo de 1000 expresiones por prueba. 
* Sin duplicados. 
* Tipos de entidad permitidos: solo entidades de aprendizaje automático de entidades simples y compuestas. Las pruebas por lotes solo son útiles para las entidades e intenciones de aprendizaje automático.

Cuando use una aplicación que no sea este tutorial, asegúrese de *no* usar las expresiones de ejemplo que ya están agregadas en una intención. 

**En este tutorial, aprenderá a:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Importar la aplicación de ejemplo
> * Crear un archivo de prueba por lotes 
> * Ejecutar una prueba por lotes
> * Revisar los resultados de la prueba
> * Corregir errores 
> * Volver a realizar la prueba por lotes

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="import-example-app"></a>Importar la aplicación de ejemplo

Continúe con la aplicación creada en el último tutorial, denominada **HumanResources**. 

Para ello, siga los pasos que se describen a continuación:

1.  Descargue y guarde el [archivo JSON de la aplicación](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/custom-domain-review-HumanResources.json).

2. Importe el archivo JSON en una aplicación nueva.

3. Desde la sección **Manage** (Administrar), en la pestaña **Versions** (Versiones), clone la versión y asígnele el nombre `batchtest`. La clonación es una excelente manera de trabajar con distintas características de LUIS sin que afecte a la versión original. Dado que el nombre de la versión se usa como parte de la ruta de la dirección URL, el nombre no puede contener ningún carácter que no sea válido en una dirección URL. 

4. Entrene la aplicación.

## <a name="batch-file"></a>Filtro por lotes

1. Cree `HumanResources-jobs-batch.json` en un editor de texto o [descárguelo](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/HumanResources-jobs-batch.json). 

2. En el archivo por lotes con formato JSON, agregue expresiones con la **intención** que quiere predecir en la prueba. 

   [!code-json[Add the intents to the batch test file](~/samples-luis/documentation-samples/tutorials/HumanResources-jobs-batch.json "Add the intents to the batch test file")]

## <a name="run-the-batch"></a>Ejecute el lote

1. Haga clic en **Probar** en la barra de navegación superior. 

2. Seleccione el **panel Prueba por lotes** en el panel de la derecha. 

    [![Captura de pantalla de una aplicación de LUIS con el panel Prueba por lotes resaltado](./media/luis-tutorial-batch-testing/hr-batch-testing-panel-link.png)](./media/luis-tutorial-batch-testing/hr-batch-testing-panel-link.png#lightbox)

3. Seleccione **Importar conjunto de datos**.

    [![Captura de pantalla de una aplicación de LUIS con Importar conjunto de datos resaltado](./media/luis-tutorial-batch-testing/hr-import-dataset-button.png)](./media/luis-tutorial-batch-testing/hr-import-dataset-button.png#lightbox)

4. Elija la ubicación de archivo del archivo `HumanResources-jobs-batch.json`.

5. Asigne un nombre al conjunto de datos `intents only` y seleccione **Done** (Listo).

    ![Selección del archivo](./media/luis-tutorial-batch-testing/hr-import-new-dataset-ddl.png)

6. Haga clic en el botón **Ejecutar**. 

7. Seleccione **Ver resultados**.

8. Revise los resultados en el gráfico y la leyenda.

    [![Captura de pantalla de una aplicación de LUIS con resultados de las pruebas por lotes](./media/luis-tutorial-batch-testing/hr-intents-only-results-1.png)](./media/luis-tutorial-batch-testing/hr-intents-only-results-1.png#lightbox)

## <a name="review-batch-results"></a>Revisar los resultados de lote

El gráfico por lotes muestra cuatro cuadrantes de resultados. Hay un filtro a la derecha del gráfico. De manera predeterminada, el filtro se establece en la primera intención de la lista. El filtro contiene todas las intenciones y solo las entidades simples y compuestas. Cuando se selecciona una [sección del gráfico](luis-concept-batch-test.md#batch-test-results) o un punto dentro del gráfico, las expresiones asociadas se muestra debajo del gráfico. 

Si mantiene el mouse sobre el gráfico y gira la rueda del mouse, puede agrandar o disminuir la visualización del gráfico. Esto resulta útil cuando hay muchos puntos en el gráfico estrechamente agrupados. 

El gráfico está en cuatro cuadrantes, donde dos de las secciones aparecen en rojo. **Estas son las secciones a las que debe prestar atención**. 

### <a name="getjobinformation-test-results"></a>Resultados de las pruebas GetJobInformation

Los resultados de las pruebas **GetJobInformation** que aparecen en el filtro muestran que dos de las cuatro predicciones se realizaron correctamente. Seleccione el nombre **False positive** (Falso positivo) sobre el cuadrante superior derecho para ver las expresiones debajo del gráfico. 

![Expresiones de prueba por lotes de LUIS](./media/luis-tutorial-batch-testing/hr-applyforjobs-false-positive-results.png)

¿Por qué hay dos de las expresiones previstas como **ApplyForJob**, en lugar de la intención correcta **GetJobInformation**? Las dos intenciones están muy estrechamente relacionadas en cuanto a la elección y la disposición de palabras. Además, hay casi tres veces más ejemplos para **ApplyForJob** que para **GetJobInformation**. Esta desigualdad de las expresiones de ejemplo influye a favor de la intención **ApplyForJob**. 

Observe que ambas intenciones tienen la misma cantidad de errores. Una predicción incorrecta en una intención afecta también a la otra intención. Ambas tienen errores porque las expresiones se predijeron de manera incorrecta para una intención y también de manera incorrecta no se predijeron para la otra intención. 

![Errores de filtro de pruebas por lotes de LUIS](./media/luis-tutorial-batch-testing/hr-intent-error-count.png)

Las expresiones que corresponden al punto superior en la sección **False positive** (Falso positivo) son `Can I apply for any database jobs with this resume?` y `Can I apply for any database jobs with this resume?`. Para la primera expresión, la palabra `resume` solo se usó en **ApplyForJob**. Para la segunda expresión, la palabra `apply` solo se usó en la intención **ApplyForJob**.

## <a name="fix-the-app"></a>Corrección de la aplicación

El objetivo de esta sección es predecir correctamente todas las expresiones para **GetJobInformation** mediante la corrección de la aplicación. 

Una corrección aparentemente rápida sería agregar estas expresiones de archivos por lotes a la intención correcta. Sin embargo, eso no es lo que quiere hacer. Quiere que LUIS prediga de manera correcta estas expresiones sin agregarlas como ejemplos. 

Tal vez también se pregunte sobre cómo eliminar expresiones de **ApplyForJob** hasta que la cantidad de expresiones sea igual a **GetJobInformation**. Eso puede corregir los resultados de las pruebas, pero impediría que LUIS pueda predecir dicha intención con precisión la próxima vez. 

La primera corrección consiste en agregar más expresiones a **GetJobInformation**. La segunda corrección consiste en reducir el peso de palabras como `resume` y `apply` respecto de la intención **ApplyForJob**. 

### <a name="add-more-utterances"></a>Adición de más declaraciones

1. Seleccione el botón **Probar** que se encuentra en el panel de navegación superior para cerrar el panel de pruebas por lotes. 

2. Seleccione **GetJobInformation** en la lista de intenciones. 

3. Agregue más expresiones que varíen en longitud, elección de palabras y disposición de palabras, y asegúrese de incluir los términos `resume`, `c.v.` y `apply`:

    |Expresiones de ejemplo para la intención **GetJobInformation**|
    |--|
    |¿Necesito presentar un currículum vítae para el trabajo nuevo de repositor en el almacén?|
    |¿Dónde aparecen hoy los trabajos relacionados con tejados?|
    |Supe que había un trabajo de codificación médica para el que se requiere currículum vítae.|
    |Me gustaría un trabajo que ayudar a los jóvenes universitarios a escribir sus currículum vítae. |
    |Este es mi currículum vítae y uso equipos computacionales para buscar un empleo nuevo en el centro de formación profesional.|
    |¿Qué puestos hay disponibles en el ámbito del cuidado infantil y el cuidado domiciliario?|
    |¿Hay algún puesto interno en el periódico?|
    |Mi currículum vítae indica que soy bueno para analizar adquisiciones, presupuestos y pérdidas de dinero. ¿Hay disponible algún trabajo de este tipo?|
    |¿Dónde están ahora los trabajos relacionados con la perforación de la tierra?|
    |He trabajado ocho años como chofer de ambulancias. ¿Hay algún trabajo nuevo?|
    |¿Es necesario postular a los trabajos nuevos de manipulación de alimentos?|
    |¿Cuánto trabajos de jardinería nuevos hay disponibles?|
    |¿Hay algún empleo nuevo de recursos humanos dirigidos a negociaciones y relaciones laborales?|
    |Tengo un máster en administración de bibliotecas y archivos. ¿Hay algún puesto nuevo?|
    |¿Hay algún trabajo de cuidador de niños para chicos de 13 años hoy en la ciudad?|

    No etiquete la entidad**Job** (Trabajo) en las expresiones. Esta sección del tutorial solo se centra en la predicción de la intención.

4. Seleccione **Train** (Entrenar) en el panel de navegación superior derecho para entrenar la aplicación.

## <a name="verify-the-new-model"></a>Verificación del nuevo modelo

Para comprobar que las expresiones en la prueba por lotes se predicen correctamente, vuelva a ejecutar la prueba por lotes.

1. Haga clic en **Probar** en la barra de navegación superior. Si los resultados por lotes todavía están abiertos, seleccione **Back to list** (Volver a la lista).  

2. Seleccione el botón de puntos suspensivos (***...***) a la derecha del nombre del lote y seleccione **Ejecutar conjunto de datos**. Espere hasta que se realice la prueba por lotes. Tenga en cuenta que el botón **Ver resultados** ahora aparece en color verde. Esto significa que todo el lote se ejecutó correctamente.

3. Seleccione **Ver resultados**. Todas las intenciones deben tener un icono verde a la izquierda de los nombres de la intención. 

    ![Captura de pantalla de LUIS con el botón de los resultados por lotes resaltado](./media/luis-tutorial-batch-testing/hr-batch-test-intents-no-errors.png)

## <a name="create-batch-file-with-entities"></a>Creación de archivo por lotes con entidades 

A fin de comprobar las entidades de una prueba por lotes, es necesario etiquetar las entidades en el archivo JSON por lotes. Se usan solo las entidades de aprendizaje automático: simples y compuestas. No agregue entidades que no son de aprendizaje automático, porque siempre se encuentran ya sea a través de expresiones regulares o de coincidencias de texto explícitas.

La variación de entidades para el número total de palabras ([token](luis-glossary.md#token)) puede afectar a la calidad de la predicción. Asegúrese de que los datos de entrenamiento proporcionados a la intención con expresiones etiquetadas incluyen una variedad de longitudes de entidad. 

Cuando escriba y pruebe archivos por lotes por primera vez, se recomienda empezar con algunas expresiones y entidades que sabe que funcionan, así como con algunas que considere que se pueden predecir de manera incorrecta. Esto le permite centrarse rápidamente en las áreas problemáticas. Después de probar las intenciones **GetJobInformation** y **ApplyForJob** con varios nombres de trabajo distintos, que no se pueden predecir, este archivo de prueba por lotes se desarrolló para ver si hay un problema de predicción con ciertos valores para la entidad **Job** (Trabajo). 

El valor de una entidad **Job** (Trabajo), que se proporciona en las expresiones de prueba, suele ser una o dos palabras, con algunos ejemplos que son más que palabras. Si _su propia_ aplicación de recursos humanos habitualmente tiene nombres de trabajo de muchas palabras, las expresiones de ejemplo etiquetadas con la entidad **Job** (Trabajo) en esta aplicación no funcionarían correctamente.

1. Cree `HumanResources-entities-batch.json` en un editor de texto como [VSCode](https://code.visualstudio.com/) o [descárguelo](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/HumanResources-entities-batch.json).


2. En el archivo por lotes con formato JSON, agregue una matriz de objetos que incluyen expresiones con la **intención** que quiere predecir en la prueba, así como las ubicaciones de las entidades en la expresión. Como una entidad se basa en token, asegúrese de empezar y detener cada entidad en un carácter. No empiece ni detenga la expresión en un espacio. Esto genera un error durante la importación del archivo por lotes.  

   [!code-json[Add the intents and entities to the batch test file](~/samples-luis/documentation-samples/tutorials/HumanResources-entities-batch.json "Add the intents and entities to the batch test file")]


## <a name="run-the-batch-with-entities"></a>Ejecución del lote con entidades

1. Haga clic en **Probar** en la barra de navegación superior. 

2. Seleccione el **panel Prueba por lotes** en el panel de la derecha. 

3. Seleccione **Importar conjunto de datos**.

4. Elija la ubicación del sistema de archivos del archivo `HumanResources-entities-batch.json`.

5. Asigne un nombre al conjunto de datos `entities` y seleccione **Done** (Listo).

6. Haga clic en el botón **Ejecutar**. Espere hasta que se realice la prueba.

7. Seleccione **Ver resultados**.

[!INCLUDE [Entity roles in batch testing - currently not supported](../../../includes/cognitive-services-luis-roles-not-supported-in-batch-testing.md)]

## <a name="review-entity-batch-results"></a>Revisión de los resultados de lote de las entidades

El gráfico se abre con todas las intenciones que se predicen correctamente. Desplácese hacia abajo en el filtro de la derecha para buscar las predicciones de entidades con error. 

1. Seleccione la entidad **Job** (Trabajo) en el filtro.

    ![Predicciones de entidades con error en el filtro](./media/luis-tutorial-batch-testing/hr-entities-filter-errors.png)

    El gráfico cambia para mostrar las predicciones de la entidad. 

2. Seleccione **False Negative** (Falso negativo) en el cuadrante inferior izquierdo del gráfico. A continuación, use el control de combinación de teclado + E para cambiar a la vista del token. 

    [![Vista del token de predicciones de entidad](./media/luis-tutorial-batch-testing/token-view-entities.png)](./media/luis-tutorial-batch-testing/token-view-entities.png#lightbox)
    
    Revisar las expresiones debajo del gráfico muestra un error de coherencia cuando el nombre del trabajo incluye `SQL`. Al revisar las expresiones de ejemplo y la lista de frases de trabajo, SQL solo se usa una vez y solo como parte de un nombre de trabajo más grande, `sql/oracle database administrator`.

## <a name="fix-the-app-based-on-entity-batch-results"></a>Corrección de la aplicación en función de los resultados de lotes

Corregir la aplicación requiere que LUIS determine correctamente las variaciones de los trabajos de SQL. Hay varias opciones para esa corrección. 

* Agregar explícitamente más expresiones de ejemplo, que utilizan SQL, y etiquetar esas palabras como una entidad de trabajo. 
* Agregar explícitamente más trabajos de SQL a la lista de frases.

Estas tareas se dejan para que las haga el usuario.

Agregar un [patrón](luis-concept-patterns.md) antes de que la entidad se prediga correctamente no va a corregir el problema. Esto es porque el patrón no coincidirá hasta que se detecten todas las entidades del patrón. 

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

El tutorial utiliza una prueba por lotes para encontrar problemas en el modelo actual. El modelo se ha corregido y se ha vuelto a probar con el archivo por lotes para comprobar que el cambio ha sido correcto.

> [!div class="nextstepaction"]
> [Información sobre los patrones](luis-tutorial-pattern.md)

