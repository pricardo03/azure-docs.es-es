---
title: Descarga y notas de la versión del emulador de Azure Cosmos
description: Lea las notas de la versión del emulador de Azure Cosmos y descárguelas.
ms.service: cosmos-db
ms.topic: tutorial
author: markjbrown
ms.author: mjbrown
ms.date: 06/20/2019
ms.openlocfilehash: 03163d75681bee7434e2b7576a678b1094d47b94
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2019
ms.locfileid: "71973711"
---
# <a name="azure-cosmos-emulator---release-notes-and-download-information"></a>Emulador de Azure Cosmos: notas de la versión e información de descarga.

En este artículo se muestran las notas de la versión del emulador de Azure Cosmos con una lista de actualizaciones de las características que se realizaron en cada versión. También muestra la versión más reciente del emulador que se va a descargar y usar.

## <a name="download"></a>Descargar

| | |
|---------|---------|
|**Descarga de MSI**|[Centro de descarga de Microsoft](https://aka.ms/cosmosdb-emulator)|
|**Primeros pasos**|[Uso del emulador de Azure Cosmos para desarrollo y pruebas locales](local-emulator.md)|

## <a name="release-notes"></a>Notas de la versión

### <a name="246"></a>2.4.6

- Esta versión proporciona paridad con las características del servicio Azure Cosmos a partir de julio de 2019, con las excepciones indicadas en [Uso del emulador de Azure Cosmos para desarrollo y pruebas locales](local-emulator.md). También corrige varios errores relacionados con el cierre del emulador cuando se invoca mediante la línea de comandos y la dirección IP interna reemplaza los clientes del SDK mediante la conectividad en modo directo.

### <a name="243"></a>2.4.3

- Deshabilitación del inicio del servicio de MongoDB de forma predeterminada. Solo el punto de conexión SQL está habilitado de forma predeterminada. El usuario debe iniciar el punto de conexión manualmente mediante la opción de línea de comandos "/EnableMongoDbEndpoint" del emulador. Ahora es similar a todos los demás puntos de conexión de servicio, como Gremlin, Cassandra y Table.
- Corrección de un error en el emulador al empezar con “/AllowNetworkAccess”, donde los puntos de conexión Gremlin, Cassandra y Table no controlaban correctamente las solicitudes de clientes externos.
- Incorporación de puertos de conexión directa a la configuración de reglas de firewall.

### <a name="240"></a>2.4.0

- Corrección de un problema con el emulador, que no se puede iniciar mientras aplicaciones de supervisión de red, como Pulse Client, están presentes en el equipo host.
