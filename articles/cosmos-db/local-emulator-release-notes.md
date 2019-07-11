---
title: Descarga y notas de la versión del emulador de Azure Cosmos
description: Lea las notas de la versión del emulador de Azure Cosmos y descárguelas.
ms.service: cosmos-db
ms.topic: tutorial
author: markjbrown
ms.author: mjbrown
ms.date: 06/20/2019
ms.openlocfilehash: 5985d0d82341c76993ee91b8dff6927edd1ed8b4
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2019
ms.locfileid: "67332134"
---
# <a name="use-the-azure-cosmos-emulator-for-local-development-and-testing"></a>Uso del emulador de Azure Cosmos para desarrollo y pruebas locales

En este artículo se muestran las notas de la versión del emulador de Azure Cosmos con una lista de actualizaciones de las características que se realizaron en cada versión. También muestra la versión más reciente del emulador que se va a descargar y usar.

## <a name="download"></a>Descargar

| | |
|---------|---------|
|**Descarga de MSI**|[Centro de descarga de Microsoft](https://aka.ms/cosmosdb-emulator)|
|**Primeros pasos**|[Uso del emulador de Azure Cosmos para desarrollo y pruebas locales](local-emulator.md)|

## <a name="release-notes"></a>Notas de la versión

### <a name="243"></a>2.4.3

- Deshabilitación del inicio del servicio de MongoDB de forma predeterminada. Solo el punto de conexión SQL está habilitado de forma predeterminada. El usuario debe iniciar el punto de conexión manualmente mediante la opción de línea de comandos "/EnableMongoDbEndpoint" del emulador. Ahora es similar a todos los demás puntos de conexión de servicio, como Gremlin, Cassandra y Table.
- Corrección de un error en el emulador al empezar con “/AllowNetworkAccess”, donde los puntos de conexión Gremlin, Cassandra y Table no controlaban correctamente las solicitudes de clientes externos.
- Incorporación de puertos de conexión directa a la configuración de reglas de firewall.

### <a name="240"></a>2.4.0

- Corrección de un problema con el emulador, que no se puede iniciar mientras aplicaciones de supervisión de red, como Pulse Client, están presentes en el equipo host.
