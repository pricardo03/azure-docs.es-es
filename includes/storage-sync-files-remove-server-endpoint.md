---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 05/31/2018
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: 33dff710d83bd12a8db343a89c6e4576d1397ba7
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186387"
---
No: eliminar un punto de conexión de servidor no es como reiniciar un servidor. Eliminar y volver a crear el punto de conexión del servidor casi nunca es una solución adecuada para solucionar problemas de sincronización, niveles de nube u otros aspectos de Azure File Sync. Eliminar un punto de conexión de servidor es una operación destructiva y puede provocar la pérdida de datos en el caso de que existan archivos en capas fuera del espacio de nombres del punto de conexión del servidor (consulte [¿Por qué los archivos en capas se encuentran fuera del espacio de nombres del punto de conexión de servidor?](../articles/storage/files/storage-files-faq.md#afs-tiered-files-out-of-endpoint) para más información), o en archivos en capas inaccesibles que existan dentro del espacio de nombres del punto de conexión del servidor. Estos problemas no se resolverán si se vuelve a crear el punto de conexión del servidor. Puede haber archivos en capas en el espacio de nombres del punto de conexión de un servidor aunque nunca se hayan habilitado los niveles en la nube. Por tanto, le recomendamos que no elimine el punto de conexión del servidor a menos que desee dejar de usar Azure File Sync con esta carpeta en concreto o un ingeniero de Microsoft le haya solicitado expresamente que lo haga así. Para más información sobre la eliminación de puntos de conexión del servidor, consulte [Eliminación de un punto de conexión de servidor](../articles/storage/files/storage-sync-files-server-endpoint.md#remove-a-server-endpoint).    