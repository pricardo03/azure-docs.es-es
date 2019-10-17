---
title: 'Recursos para desarrolladores: Language Understanding'
titleSuffix: Azure Cognitive Services
description: Los desarrolladores tienen SDK y API REST para Language Understanding.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 8accac7fe6068007180403fdab27013da161b28c
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72437196"
---
# <a name="developer-resources-for-language-understanding"></a>Recursos de desarrolladores para Language Understanding

Los desarrolladores pueden usar SDK y API REST para Language Understanding. 

## <a name="azure-resource-management"></a>Administración de recursos de Azure

Use el nivel de Azure Microsoft Cognitive Services para crear, editar, enumerar y eliminar el recurso Language Understanding o Cognitive Service.

Encuentre la documentación de referencia basada en la herramienta:

* [CLI de Azure](https://docs.microsoft.com/cli/azure/cognitiveservices#az-cognitiveservices-list)

* [Azure RM PowerShell](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/?view=azurermps-4.4.1#cognitive_services)

## <a name="language-understanding-authoring-and-prediction-requests"></a>Solicitudes de creación y predicción de Language Understanding

Al servicio Language Understanding se accede desde un recurso de Azure que tiene que crear. Hay dos recursos: recursos de punto de conexión de predicción y de creación. Ambos recursos le permiten controlar los recursos de LUIS. 

Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).

### <a name="rest-apis"></a>API de REST

Las API de punto de conexión de predicción y de creación están disponibles en las API REST:

* [Documentación de referencia](https://go.microsoft.com/fwlink/?linkid=2092087) de creación
* [Documentación de referencia](https://go.microsoft.com/fwlink/?linkid=2092356) del entorno de ejecución de predicción

### <a name="language-based-sdks"></a>SDK basados en lenguaje

|Idioma |Documentación de referencia|Paquete|Ejemplos|Guías de inicio rápido|
|--|--|--|--|--|
|C#|[Creación](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.authoring?view=azure-dotnet)</br>[Predicción](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime?view=azure-dotnet)|[Creación en NuGet](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.LUIS.Authoring/)<br>[Predicción en NuGet](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime/)|[Ejemplos de SDK de .NET](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/LUIS)|[Creación y administración de una aplicación](sdk-csharp-quickstart-authoring-app.md)<br>[Consulta de un punto de conexión de predicción](sdk-csharp-quickstart-query-prediction-endpoint.md)|
|Go|[Creación y predicción](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/cognitiveservices/v2.0/luis)|[SDK](https://github.com/Azure/azure-sdk-for-go/tree/master/services/cognitiveservices/v2.0/luis)|[Creación](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/quickstarts/change-model/go)<br>[Predicción](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/quickstarts/analyze-text/go)|[Creación con REST](luis-get-started-go-add-utterance.md)<br>[Predicción con REST](luis-get-started-go-get-intent.md)|
|Java|[Creación y predicción](https://docs.microsoft.com/java/api/overview/azure/cognitiveservices/client/languageunderstanding?view=azure-java-stable)|[Autorización con Maven](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-luis-authoring)<br>[Predicción con Maven](https://search.maven.org/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-luis-runtime)|[Creación](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/quickstarts/change-model/java)<br>[Predicción](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/quickstarts/analyze-text/java)|[Creación](luis-get-started-java-add-utterance.md)<br>[Predicción](luis-get-started-java-get-intent.md)
|Node.js|[Creación](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/?view=azure-node-latest)<br>[Predicción](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/?view=azure-node-latest)|[Creación con NPM](https://www.npmjs.com/package/azure-cognitiveservices-luis-authoring)<br>[Predicción con NPM](https://www.npmjs.com/package/azure-cognitiveservices-luis-runtime)|[Creación](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/quickstarts/change-model/node)<br>[Predicción](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/quickstarts/analyze-text/node)|[Creación con REST](https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-node-get-intent)<br>[Predicción con REST](https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-node-add-utterance)|
|Python|[Creación y predicción](sdk-python-quickstart-authoring-app.md)|[Pip](https://pypi.org/project/azure-cognitiveservices-language-luis/)|[Creación](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/LUIS/application_quickstart.py)|[Creación](sdk-python-quickstart-authoring-app.md)<br>[Predicción con REST](luis-get-started-python-get-intent.md)

## <a name="other-tools-and-sdks"></a>Otras herramientas y SDK

Bot Framework está disponible como [un SDK](https://github.com/Microsoft/botframework) en diversos lenguajes y como servicio mediante [Azure Bot Service](https://dev.botframework.com/). 

Bot Framework proporciona [varias herramientas](https://github.com/microsoft/botbuilder-tools) para ayudar con Language Understanding, entre las que se incluyen:

* [LUDown](https://github.com/microsoft/botbuilder-tools/blob/master/packages/Ludown): compile modelos de descripción del lenguaje LUIS con archivos Markdown
* [LUIS Cli](https://github.com/microsoft/botbuilder-tools/blob/master/packages/LUIS): cree y administre sus aplicaciones LUIS.ai
* [Dispatch](https://github.com/microsoft/botbuilder-tools/blob/master/packages/Dispatch): administre aplicaciones principales y secundarias
* [LUISGen](https://github.com/microsoft/botbuilder-tools/blob/master/packages/LUISGen): genere automáticamente clases C#/Typescript de respaldo para sus intenciones y entidades de LUIS.
* [Bot Emulator](https://github.com/Microsoft/BotFramework-Emulator/releases): una aplicación de escritorio que permite que los desarrolladores de bots prueben y depuren los bots mediante el SDK de Bot Framework


## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre los [códigos de error HTTP](luis-reference-response-codes.md) comunes.