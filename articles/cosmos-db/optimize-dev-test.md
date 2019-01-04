---
title: Optimización para el desarrollo y pruebas en Azure Cosmos DB
description: En este artículo se explican las opciones gratuitas que ofrece Cosmos DB para el desarrollo y pruebas del servicio.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/20/2018
ms.author: rimman
ms.openlocfilehash: e6ccf70a085950ebd6b520fa4991220981e4f018
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "53263650"
---
# <a name="optimizing-for-development-and-testing-in-azure-cosmos-db"></a>Optimización para el desarrollo y pruebas en Azure Cosmos DB

En este artículo se describen las diferentes opciones para usar Azure Cosmos DB para desarrollo y pruebas de forma gratuita.

## <a name="azure-cosmos-db-emulator-locally-downloadable-version"></a>Emulador de Azure Cosmos DB (versión descargable de forma local)

El [emulador de Azure Cosmos DB](local-emulator.md) es una versión que se puede descargar de forma local y que imita el servicio en la nube de Azure Cosmos DB. Puede escribir y probar el código que usan las API de Azure Cosmos DB incluso si no tiene conexión de red y sin que ello conlleve ningún costo. El emulador de Azure Cosmos DB proporciona un entorno local para propósitos de desarrollo con alta fidelidad al servicio en la nube. Gracias a él, puede desarrollar y probar su aplicación localmente y sin necesidad de crear una suscripción a Azure. Cuando esté listo para implementar su aplicación en la nube, actualice la cadena de conexión para conectarse al punto de conexión de Azure Cosmos DB en la nube; no es necesario que realice otras modificaciones. También puede [configurar una canalización de CI/CD con la tarea de compilación del emulador de Azure Cosmos DB](tutorial-setup-ci-cd.md) en Azure DevOps para ejecutar pruebas. Para comenzar, consulte el artículo referente al [emulador de Cosmos DB](local-emulator.md).

## <a name="try-azure-cosmos-db-for-free"></a>Pruebe gratis Azure Cosmos DB

La opción para [probar Azure Cosmos DB gratis](https://azure.microsoft.com/try/cosmosdb/) le permite crear bases de datos y colecciones, además de experimentar con Azure Cosmos DB en la nube. No tiene que registrarse en Azure ni pagar nada. Las cuentas de prueba de Azure Cosmos DB están disponibles por tiempo limitado (30 días actualmente). Aún así, se pueden renovar en cualquier momento. Las cuentas de prueba de Azure Cosmos DB están disponibles por un tiempo limitado (30 días actualmente). Puede crear una demostración o realizar pruebas unitarias sin incurrir en ningún gasto. Al usar Cosmos DB para obtener cuentas gratuitas, puede evaluar las funcionalidades premium de Cosmos DB de forma gratuita, incluida la distribución global lista para usar, los SLA y los modelos de consistencia. Puede crear una base de datos con un máximo de 25 contenedores de Azure Cosmos y 10 000 RU/s de rendimiento. Puede ejecutar la aplicación de ejemplo sin tener que suscribirse a una cuenta de Azure o con la tarjeta de crédito. Con la opción para probar Cosmos DB de forma gratuita, puede crear una cuenta de Azure Cosmos de varias regiones y ejecutar una aplicación en solo unos minutos. Para comenzar, consulte la página [Probar Azure Cosmos DB de forma gratuita](https://azure.microsoft.com/try/cosmosdb/).

## <a name="azure-free-account"></a>Cuenta gratuita de Azure

Azure Cosmos DB se incluye en la [cuenta gratuita de Azure](https://azure.microsoft.com/free), que ofrece créditos y recursos de Azure de forma gratuita durante un período de tiempo determinado. Específicamente para Azure Cosmos DB, esta cuenta gratuita ofrece 5 GB de almacenamiento y 400 RU de rendimiento aprovisionado para todo el año. Esta experiencia permite a cualquier desarrollador probar fácilmente las características de Azure Cosmos DB o integrarlo con otros servicios de Azure sin ningún costo. Junto con la cuenta gratuita de Azure obtendrá un saldo de 200 USD para gastar durante los primeros 30 días. No se le cobrará nada hasta que decida actualizar la suscripción; incluso si comienza a usar los servicios. Para comenzar, visite la página de la [cuenta gratuita de Azure](https://azure.microsoft.com/free).

## <a name="next-steps"></a>Pasos siguientes

Puede comenzar a usar el emulador o las cuentas gratuitas de Azure Cosmos DB con los siguientes artículos:

* Obtenga más información sobre la [optimización para desarrollo y pruebas](optimize-dev-test.md).
* Obtenga más información sobre [la factura de Azure Cosmos DB](understand-your-bill.md).
* Obtenga más información sobre la [optimización del costo de la capacidad del rendimiento](optimize-cost-throughput.md).
* Obtenga más información sobre la [optimización del costo del almacenamiento](optimize-cost-storage.md).
* Obtenga más información sobre la [optimización del costo de la lectura y la escritura](optimize-cost-reads-writes.md).
* Obtenga más información sobre la [optimización del costo de las consultas](optimize-cost-queries.md).
* Obtenga más información sobre la [optimización del costo de las cuentas de Azure Cosmos de varias regiones](optimize-cost-regions.md).

