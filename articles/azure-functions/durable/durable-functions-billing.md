---
title: 'Facturación de Durable Functions: Azure Functions'
description: Obtenga información sobre los comportamientos internos de Durable Functions y cómo afectan a la facturación de Azure Functions.
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: overview
ms.date: 08/31/2019
ms.author: azfuncdf
ms.openlocfilehash: b79d50adf932bd5c316fbda9d0964eea7c0484ca
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2019
ms.locfileid: "70935463"
---
# <a name="durable-functions-billing"></a>Facturación de Durable Functions

[Durable Functions](durable-functions-overview.md) se factura igual que Azure Functions. Para más información, consulte los [precios de Azure Functions](https://azure.microsoft.com/pricing/details/functions/). Al ejecutar funciones de orquestador en el [plan de consumo](../functions-scale.md#consumption-plan) de Azure Functions, hay algunos comportamientos de facturación que deben tenerse en cuenta. En las secciones siguientes se describen estos comportamientos y su impacto con más detalle.

## <a name="orchestrator-function-replay-billing"></a>Facturación de la reproducción de una función de orquestador

Las [funciones de orquestador](durable-functions-orchestrations.md) pueden reproducirse varias veces a lo largo de la duración de la orquestación. El entorno de ejecución de Azure Functions considera cada reproducción como una invocación de función distinta. Por esta razón, en el plan de consumo de Azure Functions se le facturará por cada reproducción de la función de orquestador. Otros tipos de planes no cobran la reproducción de la función de orquestador.

## <a name="awaiting-and-yielding-in-orchestrator-functions"></a>Espera y suspensión de funciones de orquestador

Cuando una función de orquestador espera a que se complete una acción asincrónica mediante `await` (C#) o `yield` (JavaScript), el entorno de ejecución considera que se ha completado esa ejecución en particular. La facturación de la función de orquestador se detiene en ese momento y no se reanuda hasta la siguiente reproducción de la función de orquestador. No se le facturará por el tiempo dedicado a la espera o la suspensión de una función de orquestador.

> [!NOTE]
> Algunas personas consideran que las funciones que llaman a otras funciones son un antipatrón. Esto se debe a un problema conocido como _doble facturación_. Cuando una función llama directamente a otra función, ambas se ejecutan al mismo tiempo. La función llamada ejecuta código activamente mientras la función que llama espera una respuesta. En este caso, debe pagar por el tiempo que la función que llama invierte en esperar a que se ejecute la función llamada. Las funciones de orquestador no sufren esta doble facturación porque la facturación de la función de orquestador se detiene mientras espera el resultado de una función de actividad (o suborquestación).

## <a name="durable-http-polling"></a>Sondeo HTTP duradero

Las funciones de orquestador pueden realizar llamadas HTTP de ejecución prolongada a puntos de conexión externos, como se describe en el artículo [Características de HTTP](durable-functions-http-features.md). El método `CallHttpAsync` (C#) y el método `callHttp` (JavaScript) pueden sondear internamente un punto de conexión HTTP según el [patrón 202 asincrónico](durable-functions-http-features.md#http-202-handling). Actualmente no se realiza una facturación directa por las operaciones de sondeo HTTP internas. Sin embargo, el sondeo interno puede hacer que la función de orquestador se reproduzca periódicamente y se le facturarán cargos estándar por estas reproducciones internas de la función.

## <a name="azure-storage-transactions"></a>Transacciones de Azure Storage

Durable Functions usa Azure Storage de forma predeterminada para conservar el estado, procesar los mensajes y administrar las particiones mediante concesiones de blobs. Esta cuenta de almacenamiento es propiedad del usuario, por lo que los costos de las transacciones se facturan en la suscripción de Azure. Para más información sobre los artefactos de Azure Storage que usa Durable Functions, consulte el artículo [Centrales de tareas](durable-functions-task-hubs.md).

Hay varios factores que contribuyen a los costos de Azure Storage reales en los que incurre la aplicación de Durable Functions.

* Una sola aplicación de funciones está asociada a una única central de tareas, que comparte un conjunto de recursos de Azure Storage. Estos recursos son utilizados por todas las funciones duraderas de una aplicación de funciones. El número real de funciones de la aplicación de funciones no tiene ningún impacto en los costos de las transacciones de Azure Storage.
* Cada instancia de la aplicación de funciones sondea internamente varias colas de la cuenta de almacenamiento mediante un algoritmo de sondeo de retroceso exponencial. Una instancia de una aplicación inactiva sondeará las colas con menos frecuencia que una aplicación activa, con lo que se reduce el costo de las transacciones. Para más información sobre el comportamiento del sondeo de colas de Durable Functions, consulte la [sección de sondeo de colas del artículo Rendimiento y escalado](durable-functions-perf-and-scale.md#queue-polling).
* Cuando se ejecuta en el plan de consumo o el plan Premium de Azure Functions, el [controlador de escalado de Azure Functions](../functions-scale.md#how-the-consumption-and-premium-plans-work) sondea regularmente todas las colas de la central de tareas en segundo plano. En una escala de ligera a moderada, solo una instancia del controlador de escalado sondeará estas colas. Si la aplicación de funciones se escala horizontalmente a un gran número de instancias, es posible que se agreguen más instancias del controlador de escalado. Estas instancias adicionales del controlador de escalado pueden aumentar el costo total de las transacciones de colas.
* Cada instancia de la aplicación de funciones compite por un conjunto de concesiones de blobs. Estas instancias realizarán llamadas periódicamente a Azure Blob service para renovar las concesiones retenidas o intentar adquirir nuevas concesiones. El número de concesiones de blobs viene determinado por el recuento de particiones configurado de la central de tareas. Escalar horizontalmente a un número mayor de instancias de la aplicación de funciones probablemente aumentará los costos de las transacciones de Azure Storage asociadas a estas operaciones de concesión.

Puede encontrar más información sobre los precios de Azure Storage en la documentación [Precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre los precios de Azure Functions](https://azure.microsoft.com/pricing/details/functions/)
