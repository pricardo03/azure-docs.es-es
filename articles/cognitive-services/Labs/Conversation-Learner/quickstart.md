---
title: Creación de un modelo de Conversation Learner mediante Node.js - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Obtenga información sobre cómo crear un modelo de Conversation Learner mediante Node.js.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: b98b528d2d78d2285977dec49da18081df47e9d6
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53166139"
---
# <a name="create-a-conversation-learner-model-using-nodejs"></a>Creación de un modelo de Conversation Learner mediante Node.js

Conversation Learner reduce la complejidad que supone la creación de bots. Habilita un flujo de trabajo de desarrollo híbrido para permitir que el código escrito a mano y el aprendizaje automático se encarguen de reducir la cantidad de código requerido para escribir bots. De todos modos, aún se pueden codificar ciertas partes fijas de su modelo como, por ejemplo, la opción de comprobar si el usuario tiene la sesión iniciada, o realizar una solicitud de API para comprobar el inventario del almacén. Sin embargo, se pueden aprender otros cambios en la selección de estado y acción a partir de diálogos de ejemplo que haya proporcionado el experto o desarrollador del dominio.

## <a name="invitation-required"></a>Es necesario tener una invitación

*Es necesario tener una invitación para obtener acceso a Project Conversation Learner.*

Project Conversation Learner consiste en un SDK que se agrega al bot, y un servicio en la nube al que el SDK accede para realizar el proceso de aprendizaje automático.  Hoy en día, el acceso al servicio en la nube de Project Conversation Leaner requiere una invitación.  Si todavía no lo han invitado, [solicite una invitación](https://aka.ms/conversation-learner-request-invite).  Si no ha recibido una invitación, no podrá acceder a la API en la nube.

## <a name="prerequisites"></a>Requisitos previos

- Nodo 8.5.0 o superior y npm 5.3.0 o superior. Instalar desde [https://nodejs.org](https://nodejs.org).
  
- Clave de creación LUIS:

  1. Inicie sesión en [http://www.luis.ai](http://www.luis.ai).

  2. Haga clic en su nombre en la esquina superior derecha y, a continuación, en "configuración".

  3. La clave de creación se muestra en la página resultante.

  (Su clave de creación LUIS sirve para 2 roles.  En primer lugar, servirá como la clave de creación de Conversation Learner.  En segundo lugar, Conversation Learner usará LUIS para la extracción de entidades; la clave de creación LUIS se usa para crear modelos LUIS en su nombre).

- Explorador web Google Chrome. Instalar desde [https://www.google.com/chrome/index.html](https://www.google.com/chrome/index.html).

- git. Instalar desde [https://git-scm.com/downloads](https://git-scm.com/downloads).

- VSCode Instalar desde [https://code.visualstudio.com/](https://code.visualstudio.com/). Esto es recomendable, pero no es obligatorio.

## <a name="quick-start"></a>Inicio rápido 

1. Instalar y compilar:

    ```bash    
    git clone https://github.com/Microsoft/ConversationLearner-Samples cl-bot-01
    cd cl-bot-01
    npm install
    npm run build
    ```

    > [!NOTE]
    > Durante `npm install`, puede ignorar este error si ocurre: `gyp ERR! stack Error: Can't find Python executable`

2. Configurar:

   En el directorio `.env`, cree un archivo denominado `cl-bot-01`.  El contenido del archivo debería ser como este:

   ```
   NODE_ENV=development
   LUIS_AUTHORING_KEY=<your LUIS authoring key>
   ```

3. Bot de inicio:

    ```
    npm start
    ```

    Esta opción ejecuta el bot vacío genérico en `cl-bot-01/src/app.ts`.

3. Ejecutar la interfaz de usuario de Conversation Learner:

    ```bash
    [open second command prompt window]
    cd cl-bot-01
    npm run ui
    ```

4. Abra el explorador en http://localhost:5050. 

Ya está utilizando Conversation Learner y puede crear y aplicar métodos de aprendizaje mediante un modelo de Conversation Learner.  

> [!NOTE]
> Al iniciarlo, Project Conversation Learner estará disponible si tiene una invitación.  Si http://localhost:5050 muestra un error HTTP `403`, significa que su cuenta no ha sido invitada.  [Solicite una invitación](https://aka.ms/conversation-learner-request-invite).

## <a name="tutorials-demos-and-switching-between-bots"></a>Tutoriales, demostraciones y cambiar de bot

Las instrucciones anteriores le permitieron iniciar el robot vacío genérico.  Para ejecutar un tutorial o bot de demostración, debe realizar lo siguiente:

1. Si tiene abierta la interfaz de usuario web de Conversation Learner, vuelva a la lista de modelos en http://localhost:5050/home.
    
2. Si se está ejecutando otro bot (como `npm start` o `npm run demo-pizza`), deténgalo.  No necesita detener el proceso de la interfaz de usuario o cerrar el explorador web.

3. Ejecute un bot de demostración desde la línea de comandos (paso 2 anterior).  Las demostraciones incluyen:

  ```bash
  npm run tutorial-general
  npm run tutorial-entity-detection
  npm run tutorial-session-callbacks
  npm run tutorial-api-calls
  npm run tutorial-hybrid
  npm run demo-password
  npm run demo-pizza
  npm run demo-storage
  npm run demo-vrapp
  ```

4. Si aún no lo ha hecho, cambie a la interfaz de usuario web de Conversation Learner en Chrome; para ello, debe cargar http://localhost:5050/home. 

5. Haga clic en "Importar tutoriales" (solo debe hacerse una vez).  Esto llevará aproximadamente un minuto y copiará los modelos de Conversation Learner de todos los tutoriales en su cuenta de Conversation Learner.

6. Haga clic en el modelo de demostración en la interfaz de usuario de Conversation Learner que corresponde a la demostración que inició.

Los archivos de origen de las demostraciones están en `cl-bot-01/src/demos`.

## <a name="create-a-bot-which-includes-back-end-code"></a>Crear un bot que incluya código de back-end

1. Si tiene abierta la interfaz de usuario web de Conversation Learner, vuelva a la lista de modelos en http://localhost:5050/home.
    
2. Si se está ejecutando un bot (como `npm run demo-pizza`), deténgalo.  No necesita detener el proceso de la interfaz de usuario o cerrar el explorador web.

3. Si lo desea, edite el código en `cl-bot-01/src/app.ts`.

4. Reconstruya y reinicie el bot:

    ```bash    
    npm run build
    npm start
    ```

5. Si aún no lo ha hecho, cambie a la interfaz de usuario web de Conversation Learner en Chrome; para ello, debe cargar http://localhost:5050/home. 

6. Cree un nuevo modelo de Conversation Learner en la interfaz de usuario, y comience a aplicar los métodos de aprendizaje.

7. Para hacer cambios de código en `cl-bot-01/src/app.ts`, repita los pasos anteriores, comenzando desde el paso 2.

## <a name="vscode"></a>VSCode

En VSCode, hay configuraciones de ejecución para cada demostración y para el "bot vacío" en `cl-bot-01/src/app.ts`.  Abra la carpeta `cl-bot-01` en VSCode.

## <a name="advanced-configuration"></a>Configuración avanzada

Hay un archivo de plantilla `.env.example` que muestra las variables de entorno que puede establecer para configurar las muestras.

Puede ajustar estos puertos para evitar conflictos entre otros servicios que se ejecutan en su máquina, al agregar un archivo `.env` a la raíz del proyecto:

```bash
cp .env.example .env
```

Esta opción utiliza la configuración estándar, que le permite ejecutar su bot de forma local, y comenzar a usar Conversation Learner.  (Más adelante, para implementar su bot en Bot Framework, deberá hacer algunos cambios en este archivo).

## <a name="support"></a>Soporte técnico

- Etiquetar preguntas en [Stack Overflow](https://stackoverflow.com) con el valor "microsoft cognitive".
- Solicitar una característica en nuestra [página de UserVoice](https://aka.ms/conversation-learner-uservoice).
- Abra una incidencia en nuestro [repositorio de GitHub](https://github.com/Microsoft/ConversationLearner-Samples).

## <a name="contributing"></a>Contribuir

Este proyecto ha adoptado el [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Código de conducta de código abierto de Microsoft). Para más información, consulte las [preguntas más frecuentes del código de conducta](https://opensource.microsoft.com/codeofconduct/faq/) o póngase en contacto con [opencode@microsoft.com](mailto:opencode@microsoft.com) si tiene cualquier otra pregunta o comentario.

## <a name="source-repositories"></a>Repositorios de origen

- [conversationlearner-samples](https://github.com/Microsoft/ConversationLearner-Samples)
- [conversationlearner-sdk](https://github.com/Microsoft/ConversationLearner-SDK)
- [conversationlearner-models](https://github.com/Microsoft/ConversationLearner-Models)
- [conversationlearner-ui](https://github.com/Microsoft/ConversationLearner-UI)
- [conversationlearner-webchat](https://github.com/Microsoft/ConversationLearner-WebChat)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Hello world](./tutorials/1-hello-world.md)
