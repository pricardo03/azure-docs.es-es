---
title: Adición de intenciones
titleSuffix: Language Understanding - Azure Cognitive Services
description: Agregue intenciones a la aplicación de LUIS para identificar los grupos de preguntas o comandos que tienen las mismas intenciones.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.component: language-understanding
ms.topic: article
ms.date: 10/24/2018
ms.author: diberry
ms.service: cognitive-services
ms.openlocfilehash: 3e6064b4c202c36e4b63d6e06edfbf3149f6665f
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53139879"
---
# <a name="add-intents"></a>Adición de intenciones 

Agregue [intenciones](luis-concept-intent.md) a la aplicación de LUIS para identificar los grupos de preguntas o comandos que tienen la misma intención. 

Las intenciones se administran desde la barra de navegación superior de la sección **Build** (Compilar), desde el panel izquierdo **Intents** (Intenciones). 

## <a name="create-an-app"></a>Creación de una aplicación

1. Inicie sesión en el portal de [LUIS](https://www.luis.ai).

1. Seleccione **Create new app** (Crear nueva aplicación). 

1. Asigne el nombre `MyHumanResourcesApp` a la nueva aplicación. Seleccione la referencia cultural **English** (Inglés). La descripción es opcional. 

1. Seleccione **Listo**. 

## <a name="add-intent"></a>Agregar intención

1. La aplicación se abre en la lista **Intents** (Intenciones).

1. En la página **Intents** (Intenciones), seleccione **Create new intent** (Crear intención).

1. En el cuadro de diálogo **Create new intent** (Crear nueva intención), escriba el nombre de la intención `GetEmployeeInformation` y haga clic en **Done** (Listo).

    ![Agregar intención](./media/luis-how-to-add-intents/Addintent-dialogbox.png)

## <a name="add-an-example-utterance"></a>Incorporación de una expresión de ejemplo

Las expresiones de ejemplo son ejemplos de texto de preguntas de los usuarios o de comandos. Para entrenar el servicio Language Understanding (LUIS), debe agregar expresiones de ejemplo a una intención.

1. En la página de detalles de la intención **GetEmployeeInformation**, escriba una expresión pertinente que espere de los usuarios, como `Does John Smith work in Seattle?`, en el cuadro de texto situado debajo del nombre de la intención y presione Entrar.
 
    ![Captura de pantalla de la página de detalles de las intenciones, con la expresión resaltada](./media/luis-how-to-add-intents/add-new-utterance-to-intent.png) 

    LUIS pasa todas las expresiones a minúsculas y agrega espacios alrededor de los tokens como guiones.

## <a name="intent-prediction-discrepancy-errors"></a>Errores de discrepancia de predicción de intenciones 

Es posible que en una intención una expresión tenga una discrepancia de predicción de intenciones entre la intención seleccionada y la puntuación de predicción. LUIS indica esta discrepancia con un cuadro rojo alrededor del valor de **Labeled intent** (Intención etiquetada) en la fila de la expresión de ejemplo. 

![Captura de pantalla de la página de detalles de las intenciones, con errores de discrepancia en la predicción de la expresión](./media/luis-how-to-add-intents/prediction-discrepancy-intent.png) 

Seleccione **Train** (Entrenar) en la barra de navegación superior. La discrepancia de predicción habrá desaparecido.

## <a name="add-a-custom-entity"></a>Incorporación de una entidad personalizada

Una vez agregada una expresión a una intención, podrá seleccionar texto de ella para crear una entidad personalizada. Una entidad personalizada es una manera de etiquetar texto para la extracción, junto con la intención correcta. 

1. Seleccione la palabra `Seattle` en la expresión. Aparecerán unos corchetes alrededor del texto y un menú desplegable. 

    ![Captura de pantalla de la página de detalles Intents (Intenciones), creación de una entidad personalizada](./media/luis-how-to-add-intents/create-custom-entity.png) 

    En este ejemplo se selecciona una sola palabra para marcarla como entidad. Puede marcar simples palabras y frases como entidades.

1. En el cuadro de texto superior del menú, escriba `Location` y seleccione **Create new entity** (Crear nueva entidad). 

    ![Captura de pantalla de la página de detalles Intents (Intenciones), creación del nombre de entidad personalizada](./media/luis-how-to-add-intents/create-custom-entity-name.png) 

1. En la ventana emergente **What type of entity do you want to create?** (¿Qué tipo de entidad desea crear?), valide que el **nombre de entidad** sea _Location_ (Ubicación) y el **tipo de entidad** , _Simple_ (Sencilla). Seleccione **Listo**.

## <a name="entity-prediction-discrepancy-errors"></a>Errores de discrepancia de predicción de entidades 

La entidad está subrayada en rojo para indicar una [discrepancia de predicción de entidades](luis-how-to-add-example-utterances.md#entity-status-predictions). Puesto que es la primera aparición de una entidad, no hay suficientes ejemplos de LUIS para garantizar que el texto se haya etiquetado con la entidad correcta. Esta discrepancia se quita cuando se entrena la aplicación. 

![Captura de pantalla de la página de detalles Intents (Intenciones), nombre de entidad personalizada resaltado en azul](./media/luis-how-to-add-intents/create-custom-entity-name-blue-highlight.png) 

El texto se resalta en azul, lo cual indica una entidad.  

## <a name="add-a-prebuilt-entity"></a>Adición de una entidad precompilada

Para más información, consulte la sección sobre [entidades precompiladas](luis-how-to-add-entities.md#add-prebuilt-entity).

## <a name="using-the-contextual-toolbar"></a>Uso de la barra de herramientas contextual

Cuando se selecciona una o más expresiones de ejemplo en la lista, al activar la casilla a la izquierda de una expresión, la barra de herramientas de encima de la lista de expresiones permite realizar las siguientes acciones:

* Reasignar intenciones: mover expresiones a distintas intenciones
* Eliminar expresiones
* Filtros de entidad: mostrar solo las expresiones con entidades filtradas
* Mostrar todo/solo errores: mostrar expresiones con errores de predicción o mostrar todas las expresiones
* Vista de entidades/tokens: mostrar la vista de las entidades con nombres de entidad o mostrar texto sin formato de las expresiones
* Lupa: buscar expresiones que contengan texto específico

## <a name="working-with-an-individual-utterance"></a>Trabajo con una expresión individual

Las siguientes acciones se pueden realizar en una expresión individual a partir del menú de los puntos suspensivos a la derecha de esta:

* Editar: cambiar el texto de la expresión
* Eliminar: quitar la expresión de la intención Si desea mantener la expresión, un método mejor es moverla a la intención **None** (Ninguna). 
* Agregar un patrón: los patrones permiten tomar una expresión común y marcar el texto reemplazable y el irrelevante, lo que reduce la necesidad de más expresiones en la intención. 

La columna **Labeled intent** (Intención etiquetada) permite cambiar la intención de la expresión.

## <a name="train-your-app-after-changing-model-with-intents"></a>Entrenar la aplicación después de cambiar el modelo con intenciones

Después de agregar, editar o quitar intenciones, [entrene](luis-how-to-train.md) y [publique](luis-how-to-publish-app.md) la aplicación para que los cambios se apliquen a las consultas de punto de conexión. 

## <a name="next-steps"></a>Pasos siguientes

Más información sobre la incorporación de [expresiones de ejemplo](luis-how-to-add-example-utterances.md) con entidades. 
