---
title: Información general sobre Durable Functions en Azure
description: Introducción a la extensión Durable Functions de Azure Functions.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: overview
ms.date: 12/22/2018
ms.author: azfuncdf, glenga
ms.openlocfilehash: caeeaae4dca99a2522d767a5a4a18f0818a2fa7f
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54044117"
---
# <a name="what-is-durable-functions"></a>¿Qué es Durable Functions?

*Durable Functions* es una extensión de [Azure Functions](../functions-overview.md) que le permite escribir funciones con estado en un entorno sin servidor. La extensión administra el estado, establece puntos de control y reinicia en su nombre.

## <a name="benefits"></a>Ventajas

La extensión le permite definir flujos de trabajo con estado mediante una [ *función de orquestador*](durable-functions-types-features-overview.md#orchestrator-functions), que puede proporcionar las siguientes ventajas:

* Puede definir los flujos de trabajo en el código. No se necesitan esquemas JSON ni diseñadores.
* Se pueden llamar a otras funciones de forma sincrónica y asincrónica. La salida de las funciones llamadas puede guardarse en variables locales.
* El progreso se controla automáticamente cuando la función espera. El estado local nunca se pierde si el proceso se recicla o se reinicia la máquina virtual.

## <a name="application-patterns"></a>Patrones de aplicación

El caso de uso principal para Durable Functions es simplificar los requisitos de coordinación con estado complejos en las aplicaciones sin servidor. A continuación, se describen algunos patrones de aplicación típicos que se pueden beneficiar de Durable Functions:

* [Encadenamiento](durable-functions-concepts.md#chaining)
* [Distribución ramificada de salida y de entrada](durable-functions-concepts.md#fan-in-out)
* [API de HTTP asincrónico](durable-functions-concepts.md#async-http)
* [Supervisión](durable-functions-concepts.md#monitoring)
* [Interacción humana](durable-functions-concepts.md#human)

## <a name="language-support"></a>

Durable Functions admite actualmente los siguientes idiomas:

* **C#**: tanto las [bibliotecas de clases precompiladas](../functions-dotnet-class-library.md) como [script C#](../functions-reference-csharp.md).
* **F#**: tanto las bibliotecas de clases precompiladas como script F#. El script F# solo es compatible con la versión 1.x del entorno de ejecución de Azure Functions.
* **JavaScript**: solo es compatible con la versión 2.x del entorno de ejecución de Azure Functions. Requiere la versión 1.7.0 de la extensión Durable Functions, o una posterior. 

Durable Functions tiene el objetivo de admitir todos los [idiomas de Azure Functions](../supported-languages.md). Consulte en la [lista de problemas de Durable Functions](https://github.com/Azure/azure-functions-durable-extension/issues) en qué punto se encuentra la compatibilidad con idiomas adicionales.

Al igual que con Azure Functions, existen plantillas que le ayudarán a desarrollar Durable Functions mediante [Visual Studio 2017](durable-functions-create-first-csharp.md), [Visual Studio Code](quickstart-js-vscode.md) y [Azure Portal](durable-functions-create-portal.md).

## <a name="billing"></a>Facturación

Durable Functions se factura igual que Azure Functions. Para más información, consulte los [precios de Azure Functions](https://azure.microsoft.com/pricing/details/functions/).

## <a name="jump-right-in"></a>Comenzar de inmediato

Puede empezar a trabajar con Durable Functions en menos de 10 minutos completando uno de estos tutoriales de inicio rápido específicos del idioma:

* [C# con Visual Studio 2017](durable-functions-create-first-csharp.md)
* [JavaScript con Visual Studio Code](quickstart-js-vscode.md)

En ambos inicios rápidos, podrá crear y probar localmente una función durable de "hello world". Luego, publicará el código de función en Azure. La función que crea organiza y encadena llamadas a otras funciones.

## <a name="learn-more"></a>Más información

El siguiente vídeo resalta las ventajas de Durable Functions:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Durable-Functions-in-Azure-Functions/player] 

Durable Functions es una extensión avanzada para [Azure Functions](../functions-overview.md) que no es adecuada para todas las aplicaciones. Para más información acerca de Durable Functions, consulte [Patrones de Durable Functions y conceptos técnicos](durable-functions-concepts.md). Para obtener una comparación con otras tecnologías de orquestación de Azure, consulte [Comparativa entre Azure Functions y Azure Logic Apps](../functions-compare-logic-apps-ms-flow-webjobs.md#compare-azure-functions-and-azure-logic-apps).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Patrones de Durable Functions y conceptos técnicos](durable-functions-concepts.md)
