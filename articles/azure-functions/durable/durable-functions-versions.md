---
title: 'Información general sobre las versiones de Durable Functions: Azure Functions'
description: Obtenga información sobre las versiones de Durable Functions.
author: cgillum
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: azfuncdf
ms.openlocfilehash: 789af25cc37183e9eeae253e1e8529615abdd308
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2019
ms.locfileid: "74849809"
---
# <a name="durable-functions-versions-overview"></a>Información general sobre las versiones de Durable Functions

*Durable Functions* es una extensión de [Azure Functions](../functions-overview.md) y [Azure WebJobs](../../app-service/web-sites-create-web-jobs.md) que le permite escribir funciones con estado en un entorno sin servidor. La extensión administra el estado, establece puntos de control y reinicia en su nombre. Si todavía no está familiarizado con Durable Functions, vea la [documentación de introducción](durable-functions-overview.md).

## <a name="new-features-in-2x"></a>Nuevas características de la versión 2.x

En esta sección se describen las características de Durable Functions que se han agregado en la versión 2.x.

### <a name="durable-entities"></a>Entidades duraderas

En Durable Functions 2.x se ha introducido un nuevo concepto de [funciones de entidad](durable-functions-entities.md).

Las funciones de entidad definen las operaciones de lectura y actualización de pequeños fragmentos de estado, denominados *entidades duraderas*. Al igual que las funciones de orquestador, las de entidad son funciones con un tipo especial de desencadenador, el *desencadenador de entidad*. A diferencia de las funciones de orquestador, las funciones de entidad no tienen restricciones de código específicas. Las funciones de entidad también administran el estado de forma explícita, en lugar de representarlo de forma implícita a través del flujo de control.

Para más información, vea el artículo sobre las [entidades duraderas](durable-functions-entities.md).

### <a name="durable-http"></a>Durable HTTP

En Durable Functions 2.x se ha incluido una nueva característica denominada [Durable HTTP](durable-functions-http-features.md#consuming-http-apis) que le permite:

* Llamar directamente a las API HTTP desde las funciones de orquestación (con algunas limitaciones documentadas).
* Implementar sondeos automáticos del estado HTTP 202 en el lado cliente.
* Compatibilidad integrada con [Identidades administradas de Azure](../../active-directory/managed-identities-azure-resources/overview.md).

Para obtener más información, consulte el artículo sobre [características HTTP](durable-functions-http-features.md#consuming-http-apis).

## <a name="migrate-from-1x-to-2x"></a>Migración de 1.x a 2.x

En esta sección se describe cómo migrar la versión 1.x existente de Durable Functions a la versión 2.x para aprovechar las ventajas de las nuevas características.

### <a name="upgrade-the-extension"></a>Actualización de la extensión

Instale la versión 2.x de la [extensión de enlace Durable Functions](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask) en el proyecto. Para más información, vea [Registro de las extensiones de enlace de Azure Functions](../functions-bindings-register.md).

### <a name="update-your-code"></a>Actualización del código

En Durable Functions 2.x se han presentado varios cambios importantes. Las aplicaciones de Durable Functions 1.x no son compatibles con Durable Functions 2.x sin cambios de código. En esta sección se enumeran algunos de los cambios que debe realizar al actualizar las funciones de la versión 1.x a la versión 2.x.

#### <a name="hostjson-schema"></a>Esquema de host.json

En Durable Functions 2.x se usa un nuevo esquema de host.json. Los principales cambios con respecto a la versión 1.x incluyen:

* `"storageProvider"` (y la subsección `"azureStorage"`) para la configuración específica del almacenamiento.
* `"tracing"` para la configuración de seguimiento y registro.
* `"notifications"` (y la subsección `"eventGrid"`) para la configuración de notificaciones de Event Grid.

Vea la [Documentación de referencia para el archivo host.json de Durable Functions](durable-functions-bindings.md#durable-functions-2-0-host-json) para obtener más información.

#### <a name="public-interface-changes-net-only"></a>Cambios en la interfaz pública (solo para .NET)

En la versión 1.x, los distintos objetos de _contexto_ admitidos por Durable Functions tenían clases base abstractas destinadas a usarse en pruebas unitarias. Como parte de Durable Functions 2.x, estas clases base abstractas se han reemplazado con interfaces.

En la tabla siguiente se representan los cambios principales:

| 1.x | 2.x |
|----------|----------|
| `DurableOrchestrationClientBase` | `IDurableOrchestrationClient` o `IDurableClient` |
| `DurableOrchestrationContext` o `DurableOrchestrationContextBase` | `IDurableOrchestrationContext` |
| `DurableActivityContext` o `DurableActivityContextBase` | `IDurableActivityContext` |
| `OrchestrationClientAttribute` | `DurableClientAttribute` |

Si una clase base abstracta contiene métodos virtuales, estos métodos virtuales se han reemplazado por los métodos de extensión definidos en `DurableContextExtensions`.

#### <a name="functionjson-changes-javascript-and-c-script"></a>Cambios en function.json (JavaScript y Script de C#)

En Durable Functions 1.x, el enlace del cliente de orquestación usa un `type` de `orchestrationClient`. En la versión 2.x se usa `durableClient` en su lugar.
