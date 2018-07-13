---
title: 'Uso de entidades negables con una aplicación Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información acerca de cómo usar entidades negables con una aplicación Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 3d65376c9c43ee1407468f3e8bf3e058048bd556
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381802"
---
# <a name="how-to-use-negatable-entities-with-a-conversation-learner-application"></a>Uso de entidades negables con una aplicación Conversation Learner

En este tutorial se explica la propiedad "negable" de las entidades.

## <a name="requirements"></a>Requisitos
Para poder realizar este tutorial debe ejecutar el bot de tutorial general.

    npm run tutorial-general

## <a name="details"></a>Detalles
Marque una acción como "negable" si el usuario puede "borrar" un valor de entidad, como en "No, I do not want $entity" (No, no quiero $entity) o "no, not $entity" (No, $entity no). Por ejemplo, "no, I do not want to leave from Boston" (No, no quiero dejar Boston).

Concretamente, si se establece la propiedad "negable" de una entidad:

- Al etiquetar menciones de entidad, puede etiquetar instancias normales (positivas) de una entidad e instancias "negativas" de la entidad.
- LUIS aprende dos modelos de entidad: uno para instancias positivas y otro para instancias negativas.
- El efecto de una instancia negativa de una entidad es borrar el valor de la variable de entidad (si existe).

## <a name="steps"></a>Pasos

### <a name="create-the-application"></a>Creación de la aplicación

1. En la interfaz de usuario web, haga clic en Nueva aplicación.
2. En Name (Nombre), escriba NegatableEntity. A continuación, haga clic en Crear.

### <a name="create-an-entity"></a>Crear una entidad

1. Haga clic en Entidades y, a continuación, en Nueva entidad.
2. En Entity Name (Nombre de entidad), escriba el nombre.
3. Active Negatable (Negable).
    - Indica que el usuario podrá proporcionar un valor para la entidad o decir algo que *no* es el valor de la entidad. En este último caso, se eliminará un valor coincidente de la entidad.
3. Haga clic en Crear.

![](../media/tutorial5_entities.PNG)

### <a name="create-two-actions"></a>Crear dos acciones

1. Haga clic en Actions (Acciones) y, a continuación, en New Action (Nueva acción).
2. En Response (Respuesta), escriba "I don't know your name" (No sé cómo se llama).
3. En Disqualifying Entities (Entidades descalificadas), escriba el nombre.
3. Click Create

A continuación, cree la segunda acción.

1. Haga clic en Actions (Acciones) y en New Action (Nueva acción) para crear una segunda acción.
3. En Response (Respuesta), escriba "I know your name. It is $name" (Sé su nombre. Se llama $name).
4. Click Create

Ahora tiene dos acciones.

![](../media/tutorial5_actions.PNG)

### <a name="train-the-bot"></a>Entrenamiento del bot

1. Haga clic en Train Dialogs (Diálogos de entrenamiento) y, a continuación, en New Train Dialog (Nuevo diálogo de entrenamiento).
2. Escriba "hello" (hola).
3. Haga clic en Score Actions (Acciones de puntuación) y seleccione "I don't know your name" (No sé cómo se llama).
    - Tenga en cuenta que esto obtiene una puntuación del 100 % porque es la única acción válida.
2. Escriba "my name is david" (Me llamo David).
3. Seleccione "david" y elija la etiqueta "+name" (+Nombre).
    - Observe que hay dos instancias de "name" (Nombre): "+name" (+Nombre) y "-name" (-Nombre).  El signo más significa que vamos a proporcionar ese valor. Menos significa que vamos a indicar al sistema que algo no es el valor.
5. Haga clic en Score Actions (Acciones de puntuación).
    - Tenga en cuenta que el valor de nombre ahora se encuentra en la memoria del bot.
    - "I know your name. It is $name" (Sé su nombre. Se llama $name) es la única respuesta disponible. 
6. Seleccione "I know your name. It is $name" (Sé su nombre. Se llama $name).

Vamos a intentar borrar la entidad negable:

7. Escriba "my name is not david" (No me llamo David).
    - Observe que "not" (No) se selecciona como nombre enf unción del patrón anterior. Eso es incorrecto.
2. Haga clic en "not" (No) y luego en la x de color rojo. 
3. Haga clic en "david".
    - Ahora se trata de una entidad negativa que comunica que este no es el valor de la entidad de nombre.
2. Seleccione "-name" (-Nombre).
3. Haga clic en Score Actions (Acciones de puntuación).
    - Observe que el valor se ha borrado de la memoria.
2. Seleccione "I don't know your name" (No sé cómo se llama), que es la única acción.

A continuación, se muestra cómo se puede escribir un nuevo valor para el nombre.

3. Escriba "john" como nombre. A continuación, seleccione "john" y haga clic en el nombre.
4. Haga clic en Score Actions (Acciones de puntuación).
5. Seleccione "I know your name. It is $name" (Sé su nombre. Se llama $name).

Ahora intente reemplazar el nombre escrito.

6. Escriba "my name is susan" (Me llamo Susan).
7. Seleccione "I know your name. It is $name" (Sé su nombre. Se llama $name).
7. Haga clic en Score Actions (Acciones de puntuación).
8. Observe que **susan** ha sobrescrito a **john** en los valores de entidad.
9. Escriba "my name is not susan" (No me llamo Susan).
    - Observe que el sistema ha etiquetado esto como una instancia negativa.
2. Haga clic en Score Actions (Acciones de puntuación).
3. Seleccione "I don't know your name" (No sé cómo se llama), que es la única acción.
7. Haga clic en Done Teaching (Aprendizaje completado).

![](../media/tutorial5_dialogs.PNG)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Entidades de varios valores](./6-multi-value-entities.md)
