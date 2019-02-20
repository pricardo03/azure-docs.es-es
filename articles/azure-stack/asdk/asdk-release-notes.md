---
title: Notas de la versión del Kit de desarrollo de Microsoft Azure Stack | Microsoft Docs
description: Mejoras, correcciones y problemas conocidos del Kit de desarrollo de Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2019
ms.author: sethm
ms.reviewer: misainat
ms.lastreviewed: 02/09/2019
ms.openlocfilehash: 485d88a4765d7cedcb171a5b325fe5f366fff1f9
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2019
ms.locfileid: "56004783"
---
# <a name="asdk-release-notes"></a>Notas de la versión del Kit de desarrollo de Azure Stack

En este artículo se proporciona información sobre los cambios, correcciones y problemas conocidos del Kit de desarrollo de Azure Stack (ASDK). Si no está seguro de qué versión se está ejecutando, puede usar el [portal de administración](../azure-stack-updates.md#determine-the-current-version).

Para estar al día de las novedades del ASDK, suscríbase a esta [![fuente](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [RSS](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#).

## <a name="build-11901095"></a>Compilación 1.1901.0.95

### <a name="changes"></a>Cambios

Esta compilación incluye las siguientes correcciones para Azure Stack:

- Los componentes BGP y NAT se implementan ahora en el host físico. Esta acción elimina la necesidad de tener dos direcciones IP públicas o corporativas para la implementación de ASDK y también simplifica la implementación.
- Las copias de seguridad de los sistemas integrados de Azure Stack pueden ahora [validarse](asdk-validate-backup.md) con el script **asdk-installer.ps1** de PowerShell.

### <a name="new-features"></a>Nuevas características

- Para obtener una lista de las nuevas características de esta versión, consulte [esta sección](../azure-stack-update-1901.md#new-features) de las notas de la versión de Azure Stack.

### <a name="fixed-and-known-issues"></a>Problemas conocidos y resueltos

- Para obtener una lista de los problemas corregidos de esta versión, consulte [esta sección](../azure-stack-update-1901.md#fixed-issues) de las notas de la versión de Azure Stack. Para obtener una lista de los problemas conocidos, consulte [esta sección](../azure-stack-update-1901.md#known-issues-post-installation).
- Tenga en cuenta que [las revisiones disponibles de Azure Stack](../azure-stack-update-1901.md#azure-stack-hotfixes) no son aplicables a Azure Stack ASDK.

## <a name="build-118110101"></a>Compilación 1.1811.0.101

### <a name="changes"></a>Cambios

Esta compilación incluye las siguientes correcciones para Azure Stack:  

- Hay un conjunto de requisitos mínimos y recomendados nuevos en hardware y software para el ASDK. Estas nuevas especificaciones recomendadas se documentan en las [Consideraciones de planeación de la implementación de Azure Stack](asdk-deploy-considerations.md). A medida que ha evolucionado la plataforma de Azure Stack, más servicios están ahora disponibles y pueden requerir más recursos. El aumento de especificaciones refleja estas recomendaciones revisadas.

### <a name="new-features"></a>Nuevas características

Para obtener una lista de las nuevas características de esta versión, consulte [esta sección](../azure-stack-update-1811.md#new-features) de las notas de la versión de Azure Stack.

### <a name="fixed-and-known-issues"></a>Problemas conocidos y resueltos

Para obtener una lista de los problemas resueltos de esta versión, consulte [esta sección](../azure-stack-update-1811.md#fixed-issues) de las notas de la versión de Azure Stack. Para obtener una lista de los problemas conocidos, consulte [esta sección](../azure-stack-update-1811.md#known-issues-post-installation).

## <a name="build-11809090"></a>Compilación 1.1809.0.90

### <a name="new-features"></a>Nuevas características

Para obtener una lista de las nuevas características de esta versión, consulte [esta sección](../azure-stack-update-1809.md#new-features) de las notas de la versión de Azure Stack.

### <a name="fixed-issues"></a>Problemas corregidos

Para obtener una lista de los problemas corregidos de esta versión, consulte [esta sección](../azure-stack-update-1809.md#fixed-issues).

### <a name="known-issues"></a>Problemas conocidos

Para obtener una lista de los problemas conocidos de esta versión, consulte [esta sección](../azure-stack-update-1809.md#known-issues-post-installation).