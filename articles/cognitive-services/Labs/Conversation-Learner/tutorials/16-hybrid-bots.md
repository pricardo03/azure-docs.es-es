---
title: 'Uso de Conversation Learner con otras tecnologías de compilación de bots: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo usar Conversation Learner con otras tecnologías de compilación de bots.
services: cognitive-services
author: mattm
manager: larsliden
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 07/13/2018
ms.author: nitinme
ms.openlocfilehash: d6af927e395532e43c7cc51c39665e2e42ac6781
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66389976"
---
# <a name="how-to-use-conversation-learner-with-other-bot-building-technologies"></a>Uso de Conversation Learner con otras tecnologías de compilación de bots

En este tutorial se explica cómo usar Conversation Learner con otras tecnologías de compilación de bots y cómo se puede compartir la memoria (o el estado) entre estas tecnologías. 

## <a name="video"></a>Vídeo

[![Vista previa del tutorial de bots híbridos](https://aka.ms/cl_Tutorial_v3_Hybrid_Applications_Preview)](https://aka.ms/cl_Tutorial_v3_Hybrid_Applications)

## <a name="requirements"></a>Requisitos
Este tutorial requiere usar el emulador de bots para crear diálogos de registro, no la interfaz de usuario web de diálogos de registros. [Aquí](https://docs.microsoft.com/azure/bot-service/bot-service-debug-emulator?view=azure-bot-service-4.0) tiene más información sobre cómo configurar Bot Framework Emulator. 

Para poder realizar este tutorial debe ejecutar el bot de tutorial híbrido:

    npm run tutorial-hybrid

## <a name="details"></a>Detalles

Mientras que el control lo tiene Conversation Learner, todos los estados relacionados con la sesión de Conversation Learner se deben almacenar en el administrador de memoria de Conversation Learner. Esto es necesario porque el aprendizaje automático utiliza el estado para determinar cómo impulsar la conversación. El estado externo se puede pasar a Conversation Learner en el elemento OnSessionStartCallback que se llama cuando se inicia la sesión. OnSessionEndCallback puede devolver el estado interno una vez que finaliza la sesión.

Casi se puede considerar Conversation Learner como una llamada de función que toma algunos valores devueltos o de estado inicial.

En este ejemplo, creará un bot híbrido con dos sistemas distintos:
1. Un modelo de Conversation Learner <br/>
    Usa el modelo de Conversation Learner para determinar la acción siguiente del bot en función de la sesión actual. Esta parte del bot toma una parte del estado inicial `isOpen` (que indica si una tienda está abierta o cerrada) y devuelve otra parte del estado `purchaseItem` (el nombre de un artículo que compra el usuario).

2. Coincidencia de texto <br />
    Simplemente examina el texto entrante en busca de las cadenas específicas y responde. Esta parte del bot administra los otros mecanismos de almacenamiento de bots y es responsable de iniciar la sesión de CL. En concreto, administra tres variables: `usingConversationLearner`, `storeIsOpen` y `purchaseItem`.

Para empezar, echemos un vistazo al modelo que se usa en esta demostración.

### <a name="open-the-demo"></a>Abrir la demostración

En la interfaz de usuario web, haga clic en "Import Tutorials" (Importar tutoriales) y seleccione el modelo denominado "Tutorial-16-HybridBot".

## <a name="entities"></a>Entidades

Abra la página de entidades y observe dos entidades: `isOpen` y `purchaseItem`

Para entender cómo se usan estas entidades, abra el archivo `C:\<installedpath>\src\demos\tutorialHybrid.ts` para observar las devoluciones de llamada.

Tenga en cuenta que el código en `OnSessionStartCallback` copia el valor de `storeIsOpen` desde el almacenamiento de conversación de BotBuilder como el valor de la entidad `isOpen` para que esté disponible para Conversation Learner. Observe el código siguiente:

![](../media/tutorial17_sessionstart.PNG)

Del mismo modo, el código en `OnSessionEndCallback` (si la sesión se terminó debido a una actividad aprendida y no se trata simplemente de un tiempo de espera) copia el valor de la entidad `purchaseItem` a `purchaseItem` de almacenamiento de BotBuilder. Observe el código siguiente:

![](../media/tutorial17_sessionend.PNG)

Echemos ahora un vistazo a las acciones.

## <a name="actions"></a>Acciones

Tenga en cuenta que el modelo tiene cuatro acciones.

Las reglas deseadas para las acciones son las siguientes:

- Si la entidad `isOpen` está establecida, el Bot le preguntará: "¿Qué gustaría comprar?" y lo almacenará en la ranura `puchaseItem`.
- Si `isOpen` no está establecido, el Bot dirá: "Lo siento, está cerrado".
- Las otras dos acciones son del tipo `END_SESSION`.
- La acción END_SESSION indica a ConversationLearner que la conversación finalizó.

### <a name="overall-bot-logic"></a>Lógica general de Bot

En primer lugar, puede ver si se estableció la marca `usingConversationLearner` del estado de Bot y pasamos el control a Conversation Learner. Si no es así, pasamos el control a otra entidad.  En este ejemplo, mostramos la coincidencia de texto simple, pero podría tratarse de cualquier otra tecnología de Bot, como LUIS, QnA Maker e incluso otra instancia de Conversation Learner.

Necesitamos una forma para que el usuario abra y cierre la tienda, por lo que hacemos una cadena se comparan con "open store" (abrir tienda) y "close store" (cerrar tienda) y establecemos la marca "storeIsOpen".

A continuación, necesitamos una manera de desencadenar la administración del control a través del modelo de Conversation Learner. Cuando se coincida con la cadena "shop", haga lo siguiente:
- Establezca la marca `usingConversationLearner` en la memoria del Bot.
- Llame al método "StartSession" en el modelo de Conversation Learner.  Esta acción desencadenará "onSessionStartCallback" que va a inicializar el valor de la entidad `isOpen`

Observe a continuación:

![](../media/tutorial17_useConversationLearner.PNG)

También hacemos una coincidencia de texto con "history" (historial), que mostrará el artículo comprado más reciente.
Por último, si no se escribe nada más, se muestran los comandos disponibles para el usuario.

## <a name="train-dialog"></a>Diálogo de entrenamiento

Para este tutorial, el modelo ya está previamente entrenado.  Probaremos todo el bot para ver en práctica el efecto de las devoluciones de llamada de la sesión inicial y final.

## <a name="testing-the-bot"></a>Prueba del bot

A diferencia de lo que ocurre con los bots únicos de Conversation Learner, no podrá probar esto en la interfaz de usuario de Conversation Learner porque solo puede mostrar lo que controla el modelo de Conversation Learner.

### <a name="install-the-bot-framework-emulator"></a>Instalación de Bot Framework Emulator

- Vaya a [https://github.com/Microsoft/BotFramework-Emulator](https://github.com/Microsoft/BotFramework-Emulator).
- Descargue e instale el emulador.

### <a name="configure-the-emulator"></a>Configuración del emulador

- Abra el emulador y asegúrese de que la dirección URL tiene como destino el mismo puerto en que se ejecuta el bot. Probablemente sea: `http://localhost:3978/api/messages`

### <a name="test"></a>Prueba 

#### <a name="scenario-1-store-is-closed"></a>Escenario 1: La tienda está cerrada
1. Escriba "shop" (tienda). Esto lo controla la coincidencia de texto y proporcionará control para el modelo de Conversation Learner.
2. Escriba "hello" (Hola).  Como el valor de `isOpen` no está establecido, el bot dirá "I’m sorry we’re closed" (Lo siento, está cerrado) y finalizará la sesión.

#### <a name="scenario-2-store-is-open"></a>Escenario 2: La tienda está abierta
1. Escriba "open store" (tienda abierta).  El valor de `isOpen` se establecerá en true.
1. Escriba "shop" (tienda).
1. Escriba "hello" (Hola).  Como el valor de `isOpen` está definido en true, el bot dirá "What would you like to buy?" (¿Qué le gustaría comprar?)
1. Escriba "chair" (silla). "chair" se guardará en la memoria de CL como la entidad `purchaseItem`. Se invoca la devolución de llamada de sesión final que copia este valor en el almacén de la conversación.
1. Escriba "history" (historial).  El bot dirá "You bought chair" (Compró una silla), porque este fue el último `purchaseItem`.

## <a name="conclusion"></a>Conclusión

Con lo que ha aprendido anteriormente, debería poder combinar Conversation Learner con cualquier otra tecnología de compilación de bots.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Crear ramas y deshacer](./17-branch-undo.md)
