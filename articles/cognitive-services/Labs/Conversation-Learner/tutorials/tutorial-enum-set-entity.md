---
title: Cuándo usar las entidades ENUM y acciones de la entidad de conjunto con un modelo de aprendiz conversación - Azure Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Obtenga información acerca de cuándo es adecuado usar las entidades ENUM y acciones de la entidad de conjunto con un modelo de aprendiz de conversación.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 1393d0bd1c31a2c9c24652e260ef7f3182d91367
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66476499"
---
# <a name="when-to-use-enum-entities-and-set-entity-actions"></a>Cuándo usar las entidades ENUM y acciones de la entidad de conjunto

En este tutorial se explica cuándo deben usar las entidades de la enumeración (enumeración) y las acciones de SET_ENTITY.

## <a name="video"></a>Vídeo

[![Vista previa de conjunto de entidades Tutorial](https://aka.ms/cl_Tutorial_v3_SetEntity_Preview)](https://aka.ms/cl_Tutorial_v3_SetEntity)



## <a name="what-is-covered"></a>¿Qué se va a tratar?

Este tutorial presentan dos nuevas características. Un nuevo tipo de entidad llamado ENUM (abreviatura de enumeración) y un nuevo tipo de acción llamado SET_ENTITY, que puede hacer referencia a uno de estos valores de enumeración y, como el nombre implica, Establece la entidad a este valor. Tal como aprenderá a continuación, estas nuevas características se usan conjuntamente y se explica qué son y cómo usarlos a continuación. Antes de entrar en detalles, es importante entender qué problema estas características ayudan a resolver.

![enum_entity_type.png](../media/tutorial-enum-set-entity/enum_entity_type.png)
![action_set_entity_type.png](../media/tutorial-enum-set-entity/action_set_entity_type.png)

## <a name="problem"></a>Problema

Hay casos en donde el significado de las palabras depende del contexto de conversaciones.  Normalmente las palabras clave con la etiqueta se aprenden y se extraen mediante un servicio de comprensión del lenguaje, pero en estos casos no pueden ser capaz de aprender con ejemplos etiquetados esos sistemas.

Imagine oír a parte de una conversación entre personas cercanas y sólo oye la palabra "Sí". No sabría qué se acepta el valor "Sí" o confirmar porque no recibimos respuesta la pregunta formularse antes. La pregunta formularse antes que es el contexto, que ofrece un significado para la respuesta. Del mismo modo desde "Sí" es una respuesta tan comunes a muchas preguntas diferentes que no se aprendió proporcionando ejemplos como lo haría con [personalizado entrenado](04-introduction-to-entities.md) entidades porque, a continuación, aprendimos etiquetar cada "Sí" como esa entidad.

### <a name="example"></a>Ejemplo

Aclaremos aún más con el siguiente ejemplo:

Bot: ¿Le gusta Azure Cognitive Services?
User: Sí Bot: ¿Quiere un helado?
User: Sí

En los tutoriales anteriores, analizamos [personalizado entrenado](04-introduction-to-entities.md) entidades y su pensamiento inicial podrían ser crear una entidad denominada "likesCogServices" y etiquetar el primer "Sí" como esta entidad.  Sin embargo, el sistema también podría etiquetar a la segunda "Sí". Al intentar corregir la etiqueta de la segunda "Sí" a "likesIceCream", se crearía un conflicto de dos entradas mismos "Yes" cosas diferentes significado y podría bloquearse.

Es en estos casos que deba usar las entidades ENUM y acciones de SET_ENTITY.

## <a name="when-to-use-enums-or-setentity-actions"></a>Cuándo usar las enumeraciones o acciones SET_ENTITY

Use estas reglas siguientes para saber cuándo usar las entidades ENUM y acciones SET_ENTITY:

- Configuración de entidad o de detección es dependiente del contexto
- Se ha corregido el número de valores posibles (Sí y No sería dos valores)

En otras palabras, usarlas para los mensajes de extremos de cierre como preguntas de confirmación que siempre devuelve Yes o no.

> [!NOTE]
> Actualmente, tenemos la limitación de hasta 5 valores por cada entidad de la enumeración. Cada valor usa una de las ranuras en el límite de 64 actual. Consulte [cl valores y límites](../cl-values-and-boundaries.md)

Ejemplo: Bot: ¿Es el orden correcto?
User: Sí

Cuando los valores posibles de la entidad sin corregir y abiertas, necesitaría utilizar otra característica como [espera entidad](05-expected-entity.md).

Ejemplo: Bot: ¿Cómo te llamas?
User: Matt Bot: ¿Cuál es su color favorito?
User: Silver

Estos mensajes se consideran abiertos ya que podrían recibir respuestas con valores arbitrarios.

## <a name="what"></a>Qué

### <a name="enum-entities"></a>Entidades ENUM

Las entidades de la enumeración se crean al igual que las demás entidades. Al igual que las entidades de "programación", no se puede etiquetar palabras como estas entidades. En su lugar, debe establecerse a través de código o acciones SET_ENTITY.

![enum_entity_creation.png](../media/tutorial-enum-set-entity/enum_entity_creation.png)

### <a name="set-entity-actions"></a>Establezca las acciones de entidad

Como se mencionó anteriormente, las acciones "Establecer la entidad" basta con establecen una entidad en un valor enum conocido. Podría lograr los mismos resultados mediante la creación de una acción de devolución de llamada de API y usando el Administrador de memoria para establecer la entidad en un valor. Por ejemplo, `memory.Set(entityName, entityValue)`. Tener que escribir este código y crear estas acciones se convertiría en una tarea tediosa y difícil de administrar - por lo que el aprendiz de conversación tiene acciones especiales para facilitar este trabajo y generar automáticamente estas acciones cuando se usan. Tener estos elementos como acciones independientes, conserva la capacidad de crear estos sin que se va a junto con otras acciones o código en el bot.

- Establezca las acciones de entidad solo pueden crearse cuando se hace referencia a un valor de una entidad de la enumeración, por lo que debe crear primero una entidad de la enumeración.
- Establezca las acciones de entidad son también "no-await" porque no tienen ningún resultado visible y debe ir seguida una acción "wait" el usuario puede ver.
- Establezca las acciones de entidad son inmutables, lo que significa que no puede modificar después de la creación.

![action_set_entity_creation.png](../media/tutorial-enum-set-entity/action_set_entity_creation.png)

### <a name="automatic-action-generation"></a>Generación automática de acción

Si existe una entidad de la enumeración en el modelo, el aprendiz de conversación se crear acciones para cada uno de los posibles valores de marcador de posición y ponerlos a disposición seleccionar durante el entrenamiento. Tras la selección, la acción se crearía automáticamente para usted.

Por ejemplo, si creo una entidad de enumeración con los valores "Yes" y "No":

![enum_entity_order_confirmation.png](../media/tutorial-enum-set-entity/enum_entity_order_confirmation.png)

Incluso sin crear explícitamente las acciones para esta nueva enumeración vería dos nuevas acciones disponibles durante el entrenamiento:

![action_set_entity_sample.png](../media/tutorial-enum-set-entity/action_set_entity_sample.png)


## <a name="create-a-bot-using-these-new-features"></a>Creación de un Bot con estas nuevas características

### <a name="requirements"></a>Requisitos

Para poder realizar este tutorial debe ejecutar el bot del tutorial general

    npm run tutorial-general

Creamos un bot para simular los pedidos de comida rápida. Tendrá valores discretos para tamaños de las bebidas y papas fritas (pequeño/mediano/grande), y preguntas de confirmación con sí / NO respuestas. Las dos de estas entidades cumplen las dos reglas anteriores de ser dependiente del contexto respuestas y valores fijos.

### <a name="create-the-model"></a>Crear el modelo

1. En la interfaz de usuario web, haga clic en "Importar"
2. Seleccione el tutorial denominado "Tutorial-Enum-Set-Entity"

Esto le remitirá a la página de administración de modelo.
Tenga en cuenta que el modelo ya contiene varias entidades de enum y establecer las acciones de la entidad.

### <a name="create-the-first-dialog"></a>Creación del primer cuadro de diálogo

1. En el panel de navegación izquierdo, haga clic en "Cuadros de diálogo de Train" y luego haga clic en el botón "Nuevo"Train "Dialog".
2. Como el usuario escriba, "Hola, me gustaría pedir Coca cola y papas fritas.".
3. Haga clic en el botón "Puntuación acciones"

   > El usuario no ha especificado la bebida tamaños o papas fritas, por lo que necesitamos para pedirles.

4. Seleccione la acción de respuesta: "¿Qué bebida tamaño desearía?"
5. Como el usuario escriba, "grande"
6. Haga clic en el botón "Puntuación acciones"
7. Seleccione la acción SET_ENTITY - "drinkSize: LARGE"
8. Seleccione la acción de respuesta: "¿Qué tamaño papas fritas ¿le gustaría?"
9. Como el tipo de uso de "mensajería unificada, que esos un medio.
10. Haga clic en el botón "Puntuación acciones"
11. Seleccione la acción SET_ENTITY - "friesSize: MEDIANO"
12. Seleccione la acción de respuesta: "¿Desea que los condimentos?"
13. Como el tipo de uso en "Sí"
14. Haga clic en el botón "Puntuación acciones"
15. Seleccione la acción SET_ENTITY - "condimentsConfirmation: YES"
16. Seleccione la acción de respuesta: "Bueno, tengo un pedido de una bebida grande y medianas fritas. ¿Es correcto?"
17. Como el tipo de uso en "Sí"
18. Haga clic en el botón "Puntuación acciones"
19. Seleccione la acción SET_ENTITY - "orderConfirmation: YES"
20. Seleccione la acción de respuesta: "Bien, el número de pedido es 58. Espere allí."
21. Haga clic en "Guardar" para cerrar el cuadro de diálogo

Acaba de crear el primer cuadro de diálogo con las entidades ENUM y acciones SET_ENTITY. Puede hacer muchas más combinaciones del usuario especificando la información parcial o experimentar con otros tipos de preguntas que ha finalizado el cierre.

> [!NOTE]
> Durante el entrenamiento, verá marcadores de posición para las acciones de SET_ENTITY como
>
> ![action_set_entity_training.png](../media/tutorial-enum-set-entity/action_set_entity_training.png)
>
> pero cuando crear inician cuadros de diálogo o mediante implementado bots usuarios no verán estos.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Entradas alternativas](./10-alternative-inputs.md)
