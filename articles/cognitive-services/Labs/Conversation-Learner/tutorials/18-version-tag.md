---
title: 'Uso del etiquetado de versiones con un modelo de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Obtenga información sobre cómo usar el control de versiones y el etiquetado con un modelo de Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 99b4712373423d30dc83408cc80eccc93b342c10
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55206007"
---
# <a name="how-to-use-version-tagging"></a>Uso del etiquetado de versiones

En este tutorial se ilustra cómo etiquetar versiones del modelo de Conversation Learner y definir qué versión está "activa".  

## <a name="requirements"></a>Requisitos
Este tutorial requiere usar Bot Framework Emulator para crear diálogos de registro, no la interfaz de usuario web de diálogos de registros.  

Para poder realizar este tutorial debe ejecutar el bot de tutorial general:

    npm run tutorial-general

## <a name="details"></a>Detalles

Las versiones etiquetadas del modelo son estáticas; no puede editarlas ni cambiarlas. Al editar el modelo, siempre editará la versión principal. Cuando se agrega una nueva etiqueta, Conversation Learner captura una instantánea del modelo en ese momento dado. 

El bot usará la versión del modelo que ha seleccionado como la versión "activa", pero se podrán ver las conversaciones que tenga solo cuando "Editing Tag" (Etiqueta de edición) se haya establecido como "Master" (Principal). Si se establece la propiedad "Editing Tag" (Etiqueta de edición) del modelo en un valor distinto de "Master" (Principal), se podrá ver la instantánea del modelo, pero no podrá cambiarse de ninguna manera.

## <a name="steps"></a>Pasos

### <a name="install-the-bot-framework-emulator"></a>Instalación de Bot Framework Emulator

1. Vaya a [https://github.com/Microsoft/BotFramework-Emulator](https://github.com/Microsoft/BotFramework-Emulator).
2. Descargue e instale el emulador.

### <a name="create-a-model"></a>Creación de un modelo

1. En la página principal de lista de modelos, haga clic en el botón `New Model`.
2. En el tipo de campo `Name`, "Tutorial-18-Versioning", pulse Entrar.
4. En el panel izquierdo, haga clic en "Settings" (Configuración).
5. Copie el contenido del campo CONVERSATION_LEARNER_MODEL_ID en el Portapapeles.

### <a name="configure-the-emulator"></a>Configuración del emulador

1. En la carpeta raíz de Conversation Learner, abra el archivo ".env".
2. Agregue al archivo ".env" una línea similar a la siguiente:
    - `CONVERSATION_LEARNER_MODEL_ID=[paste-model-id-from-clipboard-here]`
3. Reinicie el servicio Conversation Learner cerrando el símbolo del sistema y volviendo a ejecutar:
    - `npm run tutorial-general`
4. En Bot Framework Emulator, cree una nueva configuración de bot y establezca la dirección URL del punto de conexión en `http://localhost:3978/api/messages`.

### <a name="version-1"></a>versión 1

Vamos a crear una única acción para la versión 1.

1. En el panel izquierdo de la interfaz de usuario web, haga clic en "Actions" (Acciones) y, a continuación, haga clic en el botón `New Action`.
2. En el campo "Bot's response" (Respuesta del bot), escriba "hola (versión 1)".
3. Haga clic en el botón `Save`.

Ahora lo etiquetaremos como "Versión 1" del modelo.

4. En el panel izquierdo, haga clic en "settings" (configuración) y, a continuación, haga clic en el icono ![](../media/tutorial18_version_tags.PNG)"Version Tags" (Etiquetas de versión) para que aparezca el botón `New Tag` en el cual se debe hacer clic.
    - Asígnele el nombre "Versión 1"
4. En la lista desplegable "Live Tag" (Etiqueta activa) seleccione "Versión 1".  
    - Ahora, los canales que usan este bot usarán la "Versión 1" de nuestro modelo.
    - Ya no se pueden cambiar los diálogos de entrenamiento, entidades y acciones de este modelo Versión 1.
    - Si selecciona "Versión 1" como "Editing Tag" (Etiqueta de edición) SOLO podrá ver el modelo, no podrá editarlo.
    - Deje "Editing Tag" (Etiqueta de edición) establecido como "Master" (Principal), es la única versión del modelo que se puede editar.

Ahora verá "Version 1" en la cuadrícula "Version Tags" (Etiquetas de versión).

### <a name="version-2"></a>versión 2

Ahora editaremos nuestro modelo para distinguirlo del modelo Versión 1.

1. En el panel izquierdo, haga clic en "Actions" (Acciones).
2. En la cuadrícula de acciones, haga clic en "hola (versión 1)".
3. En el campo "Bot's response" (Respuesta del bot), escriba "hola (versión 2)".
4. Haga clic en el botón `Save`.
5. Haga clic en el botón `New Action`.
6. En el campo "Bot's response" (Respuesta del bot), escriba "adiós (versión 2)".

### <a name="confirm-bot-framework-emulator-is-using-version-1"></a>Confirmación de que Bot Framework Emulator usa la Versión 1

1. En Bot Framework Emulator, escriba el mensaje "Hola".
2. Observe que el bot responde "hola (versión 1)".
    - Esto verifica que la Versión 1 está "activa".

### <a name="view-the-conversation-logs-in-conversation-learner-web-ui"></a>Visualización de los registros de conversación en la interfaz de usuario web de Conversation Learner

1. En el panel izquierdo, haga clic en "Log Dialogs" (Diálogos de registro).
    - Si no ve los diálogos, haga clic en el botón de actualización.
2. Observe la etiqueta "Versión 1" en la cuadrícula.
3. En la cuadrícula, haga clic en "hola (versión 1)".

> [!NOTE]
> Podemos hacer correcciones mediante la elección de toda la funcionalidad de Conversation Learner disponible actualmente; sin embargo, estas modificaciones se realizarán en "Master" (Principal), no en "Versión 1".

Ya ha visto cómo funciona el control de versiones y cómo puede interactuar con el bot mediante Bot Framework Emulator.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Demostración: restablecimiento de contraseña](./demo-password-reset.md)
