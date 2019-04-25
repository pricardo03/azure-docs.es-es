---
title: Lenguajes admitidos en Azure Functions
description: Obtenga información acerca de qué lenguajes se admiten (versión de disponibilidad general) y cuáles son experimentales o están en versión preliminar.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.devlang: dotnet
ms.topic: reference
ms.date: 08/02/2018
ms.author: glenga
ms.openlocfilehash: 5f55122b3bf4bb7160459d524b20dd1303cc0fd8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60325465"
---
# <a name="supported-languages-in-azure-functions"></a>Lenguajes admitidos en Azure Functions

En este artículo se explican los niveles de soporte que se ofrecen para los lenguajes que puede usar con Azure Functions.

## <a name="levels-of-support"></a>Niveles de soporte

Hay tres niveles de soporte:

* **Disponibilidad general (GA)**: totalmente compatible y aprobado para su uso en producción.
* **Versión preliminar**: aún no cuenta con soporte pero se espera que llegue al estado de disponibilidad general en el futuro.
* **Experimental**: no cuenta con soporte y podría abandonarse en el futuro; no se ofrece ninguna garantía de que alcance el estado de versión preliminar o disponibilidad general.

## <a name="languages-in-runtime-1x-and-2x"></a>Lenguajes en el entorno de tiempo de ejecución 1.x y 2.x

[Hay disponibles dos versiones del entorno de tiempo de ejecución de Azure Functions](functions-versions.md). En la tabla siguiente se indica qué lenguajes se admiten en cada versión del sistema de tiempo de ejecución.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

### <a name="experimental-languages"></a>Lenguajes experimentales

En la versión 1.x, los lenguajes experimentales no escalan bien y no admiten todos los enlaces. Por ejemplo, el soporte técnico experimental en la versión 1.x de PowerShell está limitado a la versión 5.1 porque es la que se instala de forma predeterminada en las máquinas virtuales en las que se ejecutan las aplicaciones de función. Si desea ejecutar scripts de PowerShell, considere la posibilidad de usar [Azure Automation](https://azure.microsoft.com/services/automation/).

No use características experimentales para nada importante, porque no se admiten oficialmente. No se deben abrir casos de soporte técnico para problemas con lenguajes experimentales. 

La versión 2.x del sistema de tiempo de ejecución no admite lenguajes experimentales. La compatibilidad para nuevos idiomas solamente se agrega cuando el idioma puede admitirse en producción. 

### <a name="language-extensibility"></a>Extensibilidad de lenguaje

El sistema de tiempo de ejecución 2.x está diseñado para ofrecer [extensibilidad de lenguaje](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Language-Extensibility). Los lenguajes JavaScript y Java en el entorno de ejecución 2.x están incluidos con esta extensibilidad.

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de cómo usar uno de los lenguajes en versión de disponibilidad general o versión preliminar en Azure Functions, consulte los siguientes recursos:

> [!div class="nextstepaction"]
> [C#](functions-reference-csharp.md)

> [!div class="nextstepaction"]
> [F#](functions-reference-fsharp.md)

> [!div class="nextstepaction"]
> [JavaScript](functions-reference-node.md)

> [!div class="nextstepaction"]
> [Java](functions-reference-java.md)

> [!div class="nextstepaction"]
> [Python](functions-reference-python.md)
