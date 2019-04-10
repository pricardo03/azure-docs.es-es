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
ms.date: 04/04/2019
ms.author: sethm
ms.reviewer: misainat
ms.lastreviewed: 04/04/2019
ms.openlocfilehash: ee98aee0873796c2a06db73d3365e3ff9ef87ccf
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/04/2019
ms.locfileid: "58915890"
---
# <a name="asdk-release-notes"></a>Notas de la versión del Kit de desarrollo de Azure Stack

En este artículo se proporciona información sobre los cambios, correcciones y problemas conocidos del Kit de desarrollo de Azure Stack (ASDK). Si no está seguro de qué versión se está ejecutando, puede usar el [portal de administración](../azure-stack-updates.md#determine-the-current-version).

Para estar al día de las novedades del ASDK, suscríbase a esta [![fuente](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [RSS](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#).

## <a name="build-1903"></a>Compilación 1903

La carga de 1903 no incluye una versión de ASDK.

## <a name="build-11902069"></a>Compilación 1.1902.0.69

### <a name="new-features"></a>Nuevas características

- La compilación 1902 presenta una nueva interfaz de usuario en el portal de administrador de Azure Stack para crear planes, ofertas, cuotas y planes complementarios. Para obtener más información, incluidas las capturas de pantalla, consulte la documentación sobre [Crear planes, ofertas y cuotas](../azure-stack-create-plan.md).

- Para obtener una lista de otros cambios y mejoras de esta versión, consulte [esta sección](../azure-stack-update-1902.md#improvements) de las notas de la versión de Azure Stack.

<!-- ### New features

- For a list of new features in this release, see [this section](../azure-stack-update-1902.md#new-features) of the Azure Stack release notes.

### Fixed and known issues

- For a list of issues fixed in this release, see [this section](../azure-stack-update-1902.md#fixed-issues) of the Azure Stack release notes. For a list of known issues, see [this section](../azure-stack-update-1902.md#known-issues-post-installation).
- Note that [available Azure Stack hotfixes](../azure-stack-update-1902.md#azure-stack-hotfixes) are not applicable to the Azure Stack ASDK. -->

### <a name="known-issues"></a>Problemas conocidos

- Se identificó un problema en el que se descartaron los paquetes de más de 1450 bytes para un equilibrador de carga interno (ILB). El problema se debe a la configuración de MTU en el host, cuyo valor es demasiado bajo para hospedar los paquetes VXLAN encapsulados que atraviesan el rol y que, a partir de la compilación 1901, se movieron al host. Hay al menos dos escenarios que se pueden dar y en los que hemos visto como se manifiesta este problema:

  - Consultas SQL a la solución Always On de SQL que están detrás de un equilibrador de carga interno (ILB) y que tienen más de 660 bytes.
  - Las implementaciones de Kubernetes no se realizan correctamente si intenta habilitar varios maestros.  

  El problema se produce cuando establece comunicación entre una VM y un ILB en la misma red virtual, pero en diferentes subredes. Para solucionar este problema, puede ejecutar los siguientes comandos en un símbolo del sistema con privilegios elevados en el host de ASDK:

  ```shell
  netsh interface ipv4 set sub "hostnic" mtu=1660
  netsh interface ipv4 set sub "management" mtu=1660
  ```

## <a name="build-11901095"></a>Compilación 1.1901.0.95

Consulte la [información importante sobre la compilación en las notas de la versión de Azure Stack](../azure-stack-update-1901.md#build-reference).

### <a name="changes"></a>Cambios

Esta compilación incluye las siguientes correcciones para Azure Stack:

- Los componentes BGP y NAT se implementan ahora en el host físico. Esta acción elimina la necesidad de tener dos direcciones IP públicas o corporativas para la implementación de ASDK y también simplifica la implementación.
- Las copias de seguridad de los sistemas integrados de Azure Stack pueden ahora [validarse](asdk-validate-backup.md) con el script **asdk-installer.ps1** de PowerShell.

### <a name="new-features"></a>Nuevas características

- Para obtener una lista de las nuevas características de esta versión, consulte [esta sección](../azure-stack-update-1901.md#new-features) de las notas de la versión de Azure Stack.

### <a name="fixed-and-known-issues"></a>Problemas conocidos y resueltos

- Para obtener una lista de los problemas resueltos de esta versión, consulte [esta sección](../azure-stack-update-1901.md#fixed-issues) de las notas de la versión de Azure Stack. Para obtener una lista de los problemas conocidos, consulte [esta sección](../azure-stack-update-1901.md#known-issues-post-installation).
- Tenga en cuenta que [las revisiones disponibles de Azure Stack](../azure-stack-update-1901.md#azure-stack-hotfixes) no son aplicables a Azure Stack ASDK.

## <a name="build-118110101"></a>Compilación 1.1811.0.101

### <a name="changes"></a>Cambios

Esta compilación incluye las siguientes correcciones para Azure Stack:  

- Hay un conjunto de requisitos mínimos y recomendados nuevos en hardware y software para el ASDK. Estas nuevas especificaciones recomendadas se documentan en las [Consideraciones de planeación de la implementación de Azure Stack](asdk-deploy-considerations.md). A medida que ha evolucionado la plataforma de Azure Stack, más servicios están ahora disponibles y pueden requerir más recursos. El aumento de especificaciones refleja estas recomendaciones revisadas.

### <a name="new-features"></a>Nuevas características

Para obtener una lista de las nuevas características de esta versión, consulte [esta sección](../azure-stack-update-1811.md#new-features) de las notas de la versión de Azure Stack.

### <a name="fixed-and-known-issues"></a>Problemas conocidos y resueltos

Para obtener una lista de los problemas resueltos de esta versión, consulte [esta sección](../azure-stack-update-1811.md#fixed-issues) de las notas de la versión de Azure Stack. Para obtener una lista de los problemas conocidos, consulte [esta sección](../azure-stack-update-1811.md#known-issues-post-installation).
