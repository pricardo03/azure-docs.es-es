---
title: 'Guía de inicio rápido de Java: Cambio del modelo y entrenamiento de la aplicación de LUIS'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido de Java, agregará expresiones de ejemplo a una aplicación de automatización de dispositivos del hogar y entrenará la aplicación. Las expresiones de ejemplo son texto de usuario conversacional que se asigna a una intención. Al proporcionar expresiones de ejemplo para las intenciones, enseñará a LUIS qué clases de texto suministrado por el usuario pertenecen a qué intención.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: bfe4aae25a34799370e52c8677871a62935ab5c2
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50419748"
---
# <a name="quickstart-change-model-using-java"></a>Guía de inicio rápido: Cambio del modelo mediante Java 

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [Quickstart prerequisites for endpoint](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* [JDK SE](https://aka.ms/azure-jdks) (Kit de desarrollo de Java, Standard Edition)
* [Biblioteca GSON JSON de Google](https://github.com/google/gson).

[!INCLUDE [Quickstart note about code repository](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>Archivo JSON de expresiones de ejemplo

[!INCLUDE [Quickstart explanation of example utterance JSON file](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]

## <a name="create-quickstart-code"></a>Creación de código de inicio rápido

1. Agregue las dependencias de Java al archivo llamado `AddUtterances.java`.

   [!code-java[Java Dependencies](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=23-26 "Java Dependencies")]

2. Cree la clase `AddUtterances`. Esta clase contiene todos los fragmentos de código siguientes.

    ```Java
    public class AddUtterances {
        // Insert code here
    }
    ```

3. Agregue las constantes de LUIS a la clase. Copie el código siguiente y especifique su clave de creación, identificador de aplicación e identificador de versión.

   [!code-java[LUIS-based IDs](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=33-44 "LUIS-based IDs")]

4. Agregue el método para llamar en la API de LUIS a la clase AddUtterances. 

   [!code-java[HTTP request to LUIS](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=46-168 "HTTP request to LUIS")]

5. Agregue el método para la respuesta HTTP de la API de LUIS a la clase AddUtterances.

   [!code-java[HTTP response from LUIS](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=170-202 "HTTP response from LUIS")]

6. Agregue el control de excepciones a la clase AddUtterances. 

   [!code-java[Exception Handling](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=205-243 "Exception Handling")]

7. Agregue la función principal a la clase AddUtterances.

   [!code-java[Add main function](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=245-278 "Add main function")]

## <a name="build-code"></a>Compilación del código

Compile AddUtterance con las dependencias

```CMD
> javac -classpath gson-2.8.2.jar AddUtterances.java
```

## <a name="run-code"></a>Ejecución del código
Al llamar a `AddUtterance` sin argumentos, se agregan las expresiones de LUIS a la aplicación sin necesidad de entrenarla.

```CMD
> java -classpath .;gson-2.8.2.jar AddUtterances
```

Esta línea de comandos muestra el resultado de llamar a la API de adición de expresiones. 

[!INCLUDE [Quickstart response from API calls](../../../includes/cognitive-services-luis-qs-change-model-json-results.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando haya terminado con la guía de inicio rápido, quite todos los archivos creados en ella. 

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"] 
> [Creación de una aplicación de LUIS mediante programación](luis-tutorial-node-import-utterances-csv.md) 