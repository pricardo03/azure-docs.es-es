---
title: 'Administración de datos de usuario con Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo administrar los datos de usuario con Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 83c7e808e48733487e84d668236cdb327c21c44c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60688545"
---
# <a name="managing-user-data"></a>Administración de datos de usuario

En esta página se describe qué registra el servicio en la nube Conversation Learner al mantener diálogos con usuarios finales.  También se describe cómo asociar los registros de Conversation Learner con identificadores de usuario, para que pueda recuperar o eliminar todos los registros asociados con un usuario particular.

## <a name="overview-of-end-user-data-logging"></a>Información general del registro de datos del usuario final

De forma predeterminada, el servicio en la nube Conversation Learner registra las interacciones entre los usuarios finales y el bot.  Estos registros son importantes para mejorar el bot, permitirle identificar casos donde el bot extrajo la entidad incorrecta o seleccionó la acción incorrecta.  Estos errores pueden corregirse en la página "Log Dialogs" (Diálogos de registro) de la interfaz de usuario, realizando correcciones y almacenando este diálogo corregido como un nuevo diálogo de entrenamiento. Para más información, vea el tutorial sobre "Diálogos de registro".

## <a name="how-to-disable-logging"></a>Deshabilitar registros

Puede controlar si las conversaciones con los usuarios finales están en la página "Configuración" del modelo de Conversation Learner.  Hay una casilla para "Log Conversations" (Conversaciones de registro).  Si desactiva esta casilla, las conversaciones con los usuarios finales no se registrarán.

## <a name="what-is-logged"></a>¿Qué se registra? 

En los cuadros de diálogo de registro, Conversation Learner almacena la entrada de usuario, los valores de entidad, las acciones seleccionadas y las marcas de tiempo de cada turno.  Estos registros se almacenan durante un período de tiempo y luego se descartan (vea la página de Ayuda en "Valor y límites predeterminados" para obtener información detallada).  

Conversation Learner crea un identificador único para cada diálogo registrado.  Conversation Learner *no* almacena un identificador de usuario con diálogos registrados.  

## <a name="associating-logged-dialogs-with-a-user-id"></a>Asociación de diálogos registrados con un identificador de usuario

A menudo es importante poder asociar los diálogos registrados con el identificador del usuario; por ejemplo, para poder recuperar o eliminar diálogos registrados de un usuario particular.  Puesto que Conversation Learner no almacena un identificador de usuario, esta asociación necesita mantenerse con el código del desarrollador.  

Para crear esta asociación, obtenga el identificador del diálogo registrado en `EntityDetectionCallback`; a continuación, en el almacenamiento del bot, almacene la asociación entre el identificador de usuario y este diálogo registrado.  

```
cl.EntityDetectionCallback(async (text: string, memoryManager: ClientMemoryManager): Promise<void> => {

    // Get user and session info
    var sessionData = memoryManager.SessionInfo();

    // sessionData.sessionId is the ID of this logged dialog.
    // In your bot-specific data store, store an association
    // between your user identifier and this session ID.
    console.log(sessionData.logDialogId)

    // sessionData.userId and sessionData.userName are the 
    // user ID and user name as defined by the channel the user
    // is on (eg, Skype, Teams, Facebook Messenger, etc.).
    // For some channels, this will be undefined.
    // This information is NOT stored with the logged dialog.
    console.log(sessionData.userId);
    console.log(sessionData.userName);

})
```

## <a name="headers-for-http-calls"></a>Encabezados para llamadas HTTP

En cada una de las llamadas HTTP siguientes, agregue el encabezado siguiente:

```
Ocp-Apim-Subscription-Key=<LUIS_AUTHORING_KEY>
```

donde `<LUIS_AUTHORING_KEY>` es la clave de creación de LUIS utilizada para acceder a las aplicaciones Conversation Learner.

## <a name="how-to-obtain-raw-data-for-a-logged-dialog"></a>Cómo obtener los datos sin procesar para un diálogo registrado

Para obtener los datos sin procesar para un diálogo registrado, puede usar esta llamada HTTP:

```
GET https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/logdialog/<logDialogId>
```

Donde `<appId>` es el GUID para este modelo de Conversation Learner y `<logDialgoId>` es el identificador del diálogo registrado que desea recuperar.  

> [!NOTE]
> El desarrollador puede editar los diálogos registrados y luego almacenarlos como diálogos de entrenamiento.  Cuando esto sucede, Conversation Learner almacena el identificador del diálogo registrado de "origen" con el diálogo de entrenamiento.  Además, un diálogo de entrenamiento puede "bifurcarse" en la interfaz de usuario; si un diálogo de entrenamiento tiene un identificador de diálogo registrado de origen asociado, las bifurcaciones de dicho diálogo de entrenamiento se marcarán con el mismo identificador de diálogo registrado.

Para obtener todos los diálogos de entrenamiento derivados de un diálogo registrado, siga estos pasos.

En primer lugar, recupere todos los diálogos de entrenamiento:

```
GET https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/traindialogs
```

Donde `<appId>` es el GUID para este modelo de Conversation Learner.  

Esto devuelve todos los diálogos de entrenamiento.  Busque esta lista para el `sourceLogDialogId` asociado y anote el `trainDialogId` asociado. 

Para un único diálogo de entrenamiento por identificador:

```
GET https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/traindialog/<trainDialogId>
```

Donde `<appId>` es el GUID para este modelo de Conversation Learner y `<trainDialogId>` es el identificador del cuadro de diálogo de entrenamiento que desea recuperar.  

## <a name="how-to-delete-a-logged-dialog"></a>Eliminación de un diálogo registrado

Si desea eliminar un diálogo registrado por su identificador, puede usar esta llamada HTTP:

```
DELETE https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/logdialog/<logDialogId>
```

Donde `<appId>` es el GUID para este modelo de Conversation Learner y `<logDialogId>` es el identificador del cuadro de diálogo de registro que desea eliminar. 

Si desea eliminar un diálogo de entrenamiento por su identificador, puede usar esta llamada HTTP:

```
DELETE https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/app/<appId>/traindialog/<trainDialogId>
```

Donde `<appId>` es el GUID para este modelo de Conversation Learner y `<trainDialogId>` es el identificador del cuadro de diálogo de entrenamiento que desea eliminar. 
