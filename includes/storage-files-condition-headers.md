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
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66159858"
---
## <a name="error-conditionheadersnotsupported-from-a-web-application-using-azure-files-from-browser"></a>Error ConditionHeadersNotSupported desde una aplicación Web con Azure Files desde Explorador

Al tener acceso al contenido hospedado en Azure Files a través de una aplicación que hace que el uso de encabezados condicionales, como un explorador web, se produce un error en acceso, mostrar un error de ConditionHeadersNotSupported.

![Error de ConditionHeaderNotSupported](media/storage-files-condition-headers/conditionalerror.png)

### <a name="cause"></a>Causa

Encabezados condicionales no se admiten aún. Su implementación de aplicaciones deberá solicitar el archivo completo cada vez que se accede al archivo.

### <a name="workaround"></a>Solución alternativa

Cuando se carga un archivo nuevo, la propiedad de control de caché predeterminada es "no-cache". Para forzar la aplicación para solicitar el archivo cada vez, la propiedad del archivo de control de caché debe actualizarse desde "no-cache" a "no almacenar en caché, no-store, must-revalidate". Esto puede lograrse mediante [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/).

![Modificación de caché de contenido del explorador de almacenamiento](media/storage-files-condition-headers/storage-explorer-cache.png)