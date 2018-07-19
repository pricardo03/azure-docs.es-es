---
title: Introducción a las versiones de tiempo de ejecución de Azure Functions
description: Azure Functions admite varias versiones del runtime. Conozca las diferencias entre ellas y cómo elegir la más adecuada en su caso.
services: functions
documentationcenter: ''
author: ggailey777
manager: cfowler
editor: ''
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: glenga
ms.openlocfilehash: 8ba12f21d76d8deded047f40489c46657c9380b8
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38602124"
---
# <a name="azure-functions-runtime-versions-overview"></a>Introducción a las versiones de tiempo de ejecución de Azure Functions

 Hay dos versiones principales del entorno de ejecución de Azure Functions: 1.x y 2.x. 1.x solo está aprobado para usarse en producción. En este artículo se explica cuáles son las novedades en la versión 2.x, que se encuentra en versión preliminar.

| Tiempo de ejecución | Status |
|---------|---------|
|1.x|Disponible con carácter general|
|2.x|Vista previa|

> [!NOTE] 
> Este artículo se refiere al servicio en la nube de Azure Functions. Para obtener información acerca del producto que le permite ejecutar Azure Functions de forma local, consulte la [Introducción al entorno de ejecución de Azure Functions](functions-runtime-overview.md).

## <a name="cross-platform-development"></a>Desarrollo multiplataforma

El tiempo de ejecución 1.x admite solo desarrollo y hospedaje en el portal o en Windows. El tiempo de ejecución 2.x funciona en .NET Core, lo que significa que se puede ejecutar en todas las plataformas admitidas por .NET Core, incluidos macOS y Linux. Esto hace posible escenarios de desarrollo y hospedaje multiplataforma que no son posibles con 1.x.

## <a name="languages"></a>Lenguajes

El tiempo de ejecución 2.x emplea un nuevo modelo de extensibilidad de lenguajes. Inicialmente, JavaScript y Java aprovechan las ventajas de este nuevo modelo. Los lenguajes experimentales de 1.x de Azure Functions no se han actualizado para usar el nuevo modelo, así que no se admiten en 2.x. En la tabla siguiente se indican qué lenguajes de programación se admiten en cada versión del runtime.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

Para más información, consulte [Lenguajes admitidos](supported-languages.md).

## <a name="bindings"></a>Enlaces 

El tiempo de ejecución 2.x usa un nuevo [modelo de extensibilidad de enlaces](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) que ofrece estas ventajas:

* Compatibilidad con extensiones de enlace de terceros.
* Desacoplamiento de tiempo de ejecución y enlaces. Esto permite el control de versiones y la publicación de las extensiones de enlace de forma independiente. Por ejemplo, puede elegir actualizar a una versión de una extensión que se basa en una versión más reciente de un SDK subyacente.
* Un entorno de ejecución más ligero, donde solo se conocen y se cargan en tiempo de ejecución los enlaces que están en uso.

Todos los enlaces integrados de Azure Functions han adoptado este modelo y ya no se incluyen de forma predeterminada, excepto los desencadenadores Timer y HTTP. Esas extensiones se instalan automáticamente al crear las funciones a través de herramientas como Visual Studio o el portal.

En la tabla siguiente se indica qué enlaces se admiten en cada versión de tiempo de ejecución.

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

## <a name="known-issues-in-2x"></a>Problemas conocidos de 2.x

Para información acerca de la compatibilidad con los enlaces y otros espacios funcionales en la versión 2.x, consulte los [problemas conocidos del Runtime 2.0](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Azure-Functions-runtime-2.0-known-issues).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, consulte los siguientes recursos:

* [Codificación y comprobación de Azure Functions en un entorno local](functions-run-local.md)
* [Cómo elegir las versiones del entorno de ejecución de Azure Functions](set-runtime-version.md)
* [Notas de la versión](https://github.com/Azure/azure-functions-host/releases)