---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 05/17/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 0486b595bffd18b06d54e8377b24deab04e2aa93
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186383"
---
## <a name="error-conditionheadersnotsupported-from-a-web-application-using-azure-files-from-browser"></a>Error ConditionHeadersNotSupported de una aplicación web al usar Azure Files desde el explorador

Al acceder a contenido hospedado en Azure Files a través de una aplicación en la que se usan encabezados condicionales, como un explorador web, se produce un error en el acceso, y se muestra un error ConditionHeadersNotSupported.

![Error ConditionHeaderNotSupported](media/storage-files-condition-headers/conditionalerror.png)

### <a name="cause"></a>Causa

Los encabezados condicionales todavía no se admiten. Las aplicaciones que los implementen tendrán que solicitar el archivo completo cada vez que se accede al archivo.

### <a name="workaround"></a>Solución alternativa

Cuando se carga un archivo nuevo, la propiedad cache-control predeterminada es "no-cache". Para forzar que la aplicación solicite el archivo cada vez, la propiedad cache-control del archivo se debe actualizar de "no-cache" a "no-cache, no-store, must-revalidate". Esto se puede lograr mediante el [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/).

![Modificación de la caché de contenido en el Explorador de Storage](media/storage-files-condition-headers/storage-explorer-cache.png)