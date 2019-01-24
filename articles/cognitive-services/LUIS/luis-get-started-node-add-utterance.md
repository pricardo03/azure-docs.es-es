---
title: Cambiar, entrenar, Node.js
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido de Node.js, agregará expresiones de ejemplo a una aplicación de automatización de dispositivos del hogar y entrenará la aplicación.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 01/17/2019
ms.author: diberry
ms.openlocfilehash: ec56be7bc28dbe90418717d62a098254f36a7970
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2019
ms.locfileid: "54382374"
---
# <a name="quickstart-change-model-using-nodejs"></a>Inicio rápido: Cambio del modelo con Node.js

[!INCLUDE [Quickstart introduction for change model](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [Quickstart prerequisites for changing model](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* Versión de [**Node.js**](https://nodejs.org/en/download/) más reciente con NPM.
* Dependencias de NPM para este artículo: [**request**](https://www.npmjs.com/package/request), [**request-promise**](https://www.npmjs.com/package/request-promise), [**fs-extra**](https://www.npmjs.com/package/fs-extra).  
* [Visual Studio Code](https://code.visualstudio.com/)

[!INCLUDE [Code is available in Azure-Samples GitHub repo](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>Archivo JSON de expresiones de ejemplo

[!INCLUDE [Quickstart explanation of example utterance JSON file](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]

## <a name="create-quickstart-code"></a>Creación de código de inicio rápido 

Agregue las dependencias de NPM al archivo llamado `add-utterances.js`.

   [!code-javascript[NPM Dependencies](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=8-11 "NPM Dependencies")]

Agregue las constantes de LUIS al archivo. Copie el código siguiente y especifique su clave de creación, identificador de aplicación e identificador de versión.

   [!code-javascript[LUIS key and IDs](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=13-22 "LUIS key and IDs")]

Agregue el nombre y la ubicación del archivo de carga que contiene las expresiones. 

   [!code-javascript[Add upload file](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=24-26 "Add upload file")]

Agregue el método y el objeto para la función `addUtterance`.

   [!code-javascript[Add configuration information for adding utterance](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=28-67 "Add configuration information for adding utterance")]

Agregue el método y el objeto para la función `train`.

   [!code-javascript[Add configuration information for training LUIS](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=69-101 "Add configuration information for training LUIS")]

Agregue la función `sendUtteranceToApi` para enviar y recibir llamadas HTTP. 

   [!code-javascript[Send the HTTP request](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=103-119 "Send the HTTP request")]

Agregue el código **principal** que elija qué acción.

   [!code-javascript[Main function](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=121-143 "Main function")]

### <a name="install-dependencies"></a>Instalación de dependencias

Cree un archivo `package.json` con el siguiente texto:

   [!code-json[Package.json](~/samples-luis/documentation-samples/quickstarts/change-model/node/package.json "Package.json")]

En la línea de comandos del directorio que tiene el archivo package.json, instale las dependencias con NPM: `npm install`.

## <a name="run-code"></a>Ejecución del código

Ejecute la aplicación desde una línea de comandos con Node.js.

Al llamar a `npm start` se agregan las expresiones, se realiza el entrenamiento y se obtiene el estado de entrenamiento.

```console
> npm start 
```

Esta línea de comandos muestra el resultado de llamar a la API de adición de expresiones. 

[!INCLUDE [Quickstart response from API calls](../../../includes/cognitive-services-luis-qs-change-model-json-results.md)]


## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con la guía de inicio rápido, quite todos los archivos creados en ella. 

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"] 
> [Creación de una aplicación de LUIS mediante programación](luis-tutorial-node-import-utterances-csv.md)