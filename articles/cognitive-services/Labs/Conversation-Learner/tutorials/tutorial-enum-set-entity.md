---
title: 'Cuándo se usan entidades ENUM y acciones SET ENTITY con un modelo de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información acerca de cuándo es adecuado usar las entidades ENUM y las acciones SET ENTITY con un modelo de Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 1393d0bd1c31a2c9c24652e260ef7f3182d91367
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66476499"
---
# <a name="when-to-use-enum-entities-and-set-entity-actions"></a>Cuándo usar las entidades ENUM y las acciones SET ENTITY

En este tutorial se explica cuándo debe usar las entidades ENUM (enumeración) y las acciones SET_ENTITY.

## <a name="video"></a>Vídeo

[![Tutorial de Set Entity (versión preliminar)](https://aka.ms/cl_Tutorial_v3_SetEntity_Preview)](https://aka.ms/cl_Tutorial_v3_SetEntity)



## <a name="what-is-covered"></a>¿Qué se va a tratar?

En este tutorial se presentan dos características nuevas: un nuevo tipo de entidad llamado ENUM (abreviatura de enumeración) y un nuevo tipo de acción llamado SET_ENTITY, que puede hacer referencia a uno de estos valores de enumeración y, como el nombre indica, establece la entidad en este valor. Tal como aprenderá a continuación, estas nuevas características se usan conjuntamente y a continuación se explica qué son y cómo usarlas. Antes de entrar en detalles, es importante entender qué problema ayudan a resolver.

![enum_entity_type.png](../media/tutorial-enum-set-entity/enum_entity_type.png)
![action_set_entity_type.png](../media/tutorial-enum-set-entity/action_set_entity_type.png)

## <a name="problem"></a>Problema

Hay ciertas conversaciones en las que el significado de las palabras depende del contexto.  Normalmente, las palabras clave con etiqueta se aprenden y se extraen mediante un servicio de comprensión del idioma, pero, en estos casos, esos sistemas pueden no ser capaces de aprender mediante ejemplos etiquetados.

Imagine que oye parte de una conversación entre personas que están cerca y solo escucha la palabra "Sí". No sabría a qué se refiere ese "Sí", si está asintiendo o confirmando, porque no escuchamos qué pregunta se formuló antes. La pregunta respondida antes es el contexto, que ofrece un significado a la respuesta. Del mismo modo, dado que "Sí" es una respuesta común a muchas preguntas diferentes, no se puede aprender proporcionando ejemplos como se haría con entidades con [entrenamiento personalizado](04-introduction-to-entities.md) porque, entonces, se aprendería a etiquetar cada "Sí" como esa entidad.

### <a name="example"></a>Ejemplo

Aclaremos aún más con el siguiente ejemplo:

Bot: ¿Le gusta Azure Cognitive Services?
User: Sí Bot: ¿Le gusta el helado?
User: Sí

En los tutoriales anteriores, analizamos las entidades con [entrenamiento personalizado](04-introduction-to-entities.md) y su pensamiento inicial podría ser crear una entidad denominada "tegustaCogServices" y etiquetar el primer "Sí" como esta entidad.  Sin embargo, el sistema también podría etiquetar el segundo "Sí". Al intentar corregir la etiqueta del segundo "Sí" para "tegustaHelado", se crearía un conflicto de dos entradas "Sí" iguales que tienen significados diferentes y el sistema podría bloquearse.

Es en estos casos cuando debe usar las entidades ENUM y las acciones SET_ENTITY.

## <a name="when-to-use-enums-or-setentity-actions"></a>Cuándo usar acciones SET_ENTITY o ENUM

Use las reglas siguientes para saber cuándo usar entidades ENUM y acciones SET_ENTITY:

- La detección o configuración de las entidades depende del contexto
- El número de valores posibles es fijo (Sí y No serían dos)

En otras palabras, úselos para los mensajes de conclusión como en las preguntas de confirmación que siempre devuelven Sí o No.

> [!NOTE]
> Actualmente, tenemos la limitación de hasta cinco valores por cada entidad de enumeración. Cada valor usa una de las ranuras en el límite de 64 actual. Consulte [cl-values-and-boundaries](../cl-values-and-boundaries.md)

Ejemplo: Bot: ¿Es el orden correcto?
User: Sí

Cuando los valores posibles de la entidad están abiertos y no son fijos, necesitaría utilizar otra característica como la [entidad esperada](05-expected-entity.md).

Ejemplo: Bot: ¿Cómo se llama?
User: Matt Bot: ¿Cuál es su color favorito?
User: Silver

Estos mensajes se consideran abiertos ya que podrían recibir respuestas con valores arbitrarios.

## <a name="what"></a>Qué

### <a name="enum-entities"></a>Entidades ENUM

Las entidades ENUM se crean igual que las demás. Al igual que las entidades de "programación", no se puede etiquetar las palabras como estas entidades. En su lugar, deben establecerse con código o con acciones SET_ENTITY.

![enum_entity_creation.png](../media/tutorial-enum-set-entity/enum_entity_creation.png)

### <a name="set-entity-actions"></a>Acciones Set Entity

Como se mencionó anteriormente, las acciones "Set Entity" simplemente establecen una entidad en un valor de enumeración conocido. Podría lograr los mismos resultados si crea una acción de devolución de llamada de API y usa el administrador de memoria para establecer la entidad en un valor. Por ejemplo, `memory.Set(entityName, entityValue)`. Tener que escribir este código y crear estas acciones se convertiría en una tarea tediosa y difícil de administrar, de modo que Conversation Learner tiene acciones especiales para facilitar este trabajo y generar automáticamente estas acciones cuando se usan. Al tener estos elementos como acciones independientes, se mantiene la capacidad de crearlos sin que acoplarlos a otras acciones o código en el bot.

- Las acciones Set Entity solo pueden crearse cuando se hace referencia a un valor de una entidad de enumeración, por lo que primero debe crear una.
- Estas acciones son también "no-await" (sin espera) porque no tienen ningún resultado visible y deben ir seguidas de una acción "wait" (de espera) que el usuario pueda ver.
- Las acciones Set entity son inmutables, lo que significa que no puede modificarlas una vez creadas.

![action_set_entity_creation.png](../media/tutorial-enum-set-entity/action_set_entity_creation.png)

### <a name="automatic-action-generation"></a>Generación automática de acciones

Si existe una entidad de enumeración en el modelo, Conversation Learner creará acciones de marcador de posición para cada uno de los posibles valores y hará que se puedan seleccionar durante el entrenamiento. Tras la selección, la acción se crearía automáticamente.

Por ejemplo, si crea una entidad de enumeración con los valores "Sí" y "No":

![enum_entity_order_confirmation.png](../media/tutorial-enum-set-entity/enum_entity_order_confirmation.png)

Incluso sin crear explícitamente las acciones para esta nueva enumeración, vería dos nuevas acciones disponibles durante el entrenamiento:

![action_set_entity_sample.png](../media/tutorial-enum-set-entity/action_set_entity_sample.png)


## <a name="create-a-bot-using-these-new-features"></a>Creación de un Bot con estas nuevas características

### <a name="requirements"></a>Requisitos

Para poder realizar este tutorial debe ejecutar el bot del tutorial general

    npm run tutorial-general

Crearemos un bot para simular los pedidos de comida rápida. Tendrá valores discretos para los tamaños de las bebidas y las patatas fritas (PEQUEÑO/MEDIANO/GRANDE), y preguntas de confirmación con respuestas SÍ / NO. Ambas entidades cumplen las dos reglas anteriores de ser respuestas dependientes del contexto y valores fijos.

### <a name="create-the-model"></a>Crear el modelo

1. En la interfaz de usuario web, haga clic en "Importar".
2. Seleccione el tutorial denominado "Tutorial-Enum-Set-Entity"

De esta forma, irá a la página de administración de modelos.
Tenga en cuenta que el modelo ya contiene varias entidades ENUM y acciones SET ENTITY.

### <a name="create-the-first-dialog"></a>Creación del primer cuadro de diálogo

1. En el panel de navegación izquierdo, haga clic en "Train Dialogs" (Diálogos de entrenamiento) y, después, en el botón "New Train Dialog" (Nuevo diálogo de entrenamiento).
2. Como usuario, escriba, "Hola, me gustaría pedir un refresco y patatas fritas".
3. Haga clic en el botón "Score Actions" (Acciones de puntuación).

   > El usuario no ha especificado el tamaño de la bebida ni de las patatas fritas, por lo que tenemos que preguntar.

4. Seleccione la acción con la respuesta: "¿Qué tamaño de bebida desearía?"
5. Como usuario, escriba "grande".
6. Haga clic en el botón "Score Actions" (Acciones de puntuación).
7. Seleccione la acción SET_ENTITY - "drinkSize: LARGE"
8. Seleccione la acción con la respuesta: "¿Qué tamaño de patatas fritas desearía?"
9. Como usuario, escriba "Creo que las medianas.
10. Haga clic en el botón "Score Actions" (Acciones de puntuación).
11. Seleccione la acción SET_ENTITY - "friesSize: MEDIUM"
12. Seleccione la acción con la respuesta: "¿Desea algún condimento?"
13. Como usuario, escriba "Sí"
14. Haga clic en el botón "Score Actions" (Acciones de puntuación).
15. Seleccione la acción SET_ENTITY - "condimentsConfirmation: YES"
16. Seleccione la acción con la respuesta: "Tengo un pedido de una bebida grande y patatas fritas medianas. ¿Es correcto?"
17. Como usuario, escriba "Sí"
18. Haga clic en el botón "Score Actions" (Acciones de puntuación).
19. Seleccione la acción SET_ENTITY - "orderConfirmation: YES"
20. Seleccione la acción con la respuesta: "Su número de pedido es el 58. Por favor, espere allí".
21. Para cerrar el cuadro de diálogo, haga clic en "Save" (Guardar).

Acaba de crear el primer cuadro de diálogo con entidades ENUM y acciones SET_ENTITY. Puede hacer muchas más combinaciones del usuario especificando información parcial o experimentar con otros tipos de preguntas cerradas.

> [!NOTE]
> Durante el entrenamiento, verá marcadores de posición para las acciones SET_ENTITY como:
>
> ![action_set_entity_training.png](../media/tutorial-enum-set-entity/action_set_entity_training.png)
>
> pero, al crear diálogos de registro o usar los usuarios de bots implementados, no los verá.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Entradas alternativas](./10-alternative-inputs.md)
