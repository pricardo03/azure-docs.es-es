---
title: Uso de la fuente de cambios de Azure Cosmos DB con Azure Functions
description: Utilice la fuente de cambios de Azure Cosmos DB con Azure Functions
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/12/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 35639dac0eacd5eae04b7848bdbbc1bc30fbf214
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59680781"
---
# <a name="serverless-event-based-architectures-with-azure-cosmos-db-and-azure-functions"></a>Arquitecturas sin servidor basada en eventos con Azure Cosmos DB y Azure Functions

Azure Functions proporciona la manera más sencilla para conectarse a la [fuente de cambios](). Puede crear pequeñas reactivo funciones de Azure que se desencadenará automáticamente en cada nuevo evento en la fuente de cambios de su contenedor de Azure Cosmos.

![Funciones sin servidor basado en eventos, trabajar con el desencadenador de Azure Cosmos DB](./media/change-feed-functions/functions.png)

Con el [desencadenador de Azure Cosmos DB](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger), puede aprovechar la [procesadores de fuente de cambios](./change-feed-processor.md)escalado y la funcionalidad de detección de eventos confiable sin necesidad de mantener cualquiera [trabajo infraestructura](./change-feed-processor.md#implementing-the-change-feed-processor-library). Centrarse en la lógica de la función de Azure sin preocuparse por el resto de la canalización de abastecimiento de eventos. Incluso puede combinar el desencadenador con cualquier otro [enlaces de Azure Functions](../azure-functions/functions-triggers-bindings.md#supported-bindings).

> [!NOTE]
> Actualmente, el desencadenador de Azure Cosmos DB se puede usar con el núcleo (API de SQL) solo.

## <a name="requirements"></a>Requisitos

Para implementar un flujo basado en eventos sin servidor, necesita:

* **El contenedor supervisado**: Contenedor supervisado es el contenedor de Azure Cosmos supervisado, y almacena los datos desde el que se genera la fuente de cambios. Todas las inserciones y cambios (por ejemplo, CRUD) al contenedor supervisado se reflejan en la fuente de cambios del contenedor.
* **El contenedor de concesión**: El contenedor de concesión mantiene el estado entre varios y las instancias de función dinámica de Azure sin servidor y permite el escalado dinámico. Este contenedor de concesión puede manual o automáticamente crearse mediante el Trigger.To de Azure Cosmos DB automáticamente cree el contenedor de la concesión, establezca la *CreateLeaseCollectionIfNotExists* marca en el [configuración](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger---configuration). Contenedores de concesión con particiones se deben tener un `/id` definición de la clave de partición.

## <a name="create-your-azure-cosmos-db-trigger"></a>Crear el desencadenador de Azure Cosmos DB

Ahora se admite la creación de la función de Azure con un desencadenador de Azure Cosmos DB en todos los IDE de las funciones de Azure y las integraciones de CLI:

* [Extensión de Visual Studio](../azure-functions/functions-develop-vs.md) para usuarios de Visual Studio.
* [Extensión de Visual Studio Core](https://code.visualstudio.com/tutorials/functions-extension/create-function) para los usuarios de Visual Studio Code.
* Y, finalmente, [herramientas CLI Core](../azure-functions/functions-run-local.md#create-func) para una experiencia independiente del IDE multiplataforma.

## <a name="run-your-azure-cosmos-db-trigger-locally"></a>Ejecutar el desencadenador de Azure Cosmos DB localmente

Puede ejecutar su [Azure Functions localmente](../azure-functions/functions-develop-local.md) con el [emulador de Azure Cosmos DB](./local-emulator.md) para crear y desarrollar sus flujos basado en eventos sin servidor sin una suscripción de Azure ni incurrir en ningún gasto.

Si desea probar escenarios en vivo en la nube, puede [probar Cosmos DB gratis](https://azure.microsoft.com/try/cosmosdb/) sin ninguna tarjeta de crédito o una suscripción de Azure necesarios.

## <a name="next-steps"></a>Pasos siguientes

Ahora puede continuar obtener más información acerca de la fuente de cambios en los siguientes artículos:

* [Introducción a la fuente de cambios](change-feed.md)
* [Maneras de leer la fuente de cambios](read-change-feed.md)
* [Uso de la biblioteca de procesadores de fuente de cambios](change-feed-processor.md)
* [Trabajo con la biblioteca de procesadores de fuente de cambios](change-feed-processor.md)
* [Informática de base de datos sin servidor con Azure Cosmos DB y Azure Functions](serverless-computing-database.md)
