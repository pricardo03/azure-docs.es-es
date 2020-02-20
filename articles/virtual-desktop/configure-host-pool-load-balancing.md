---
title: 'Configuración del equilibrio de carga de Windows Virtual Desktop: Azure'
description: Cómo configurar el método de equilibrio de carga de un entorno de Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 08/29/2019
ms.author: helohr
ms.openlocfilehash: 1b341732dd844ba172c7e682fb7089c98ca9c165
ms.sourcegitcommit: f97f086936f2c53f439e12ccace066fca53e8dc3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2020
ms.locfileid: "77367611"
---
# <a name="configure-the-windows-virtual-desktop-load-balancing-method"></a>Configuración del método de equilibrio de carga de Windows Virtual Desktop

La configuración del método de equilibrio de carga de un grupo de hosts le permite ajustar el entorno de Windows Virtual Desktop para satisfacer mejor sus necesidades.

>[!NOTE]
> Esto no se aplica a un grupo de hosts de escritorio persistente, ya que los usuarios siempre tienen una asignación de 1:1 a un host de sesión dentro del grupo de hosts.

## <a name="configure-breadth-first-load-balancing"></a>Configuración del equilibrio de carga en amplitud

El equilibrio de carga en amplitud es la configuración predeterminada de los nuevos grupos de hosts no persistentes. El equilibrio de carga en amplitud distribuye nuevas sesiones de usuario entre todos los hosts de sesión disponibles del grupo de hosts. Al configurar el equilibrio de carga en amplitud, puede establecer un límite de sesiones máximo por host de sesión en el grupo de hosts.

En primer lugar y, si aún no lo ha hecho, [descargue e importe el módulo de PowerShell para Windows Virtual Desktop](/powershell/windows-virtual-desktop/overview/) que se usará en la sesión de PowerShell. Después, ejecute el siguiente cmdlet para iniciar sesión en su cuenta:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Para configurar un grupo de hosts para realizar el equilibrio de carga en amplitud sin ajustar el límite de sesiones máximo, ejecute el siguiente cmdlet de PowerShell:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -BreadthFirstLoadBalancer
```

Para configurar un grupo de hosts para realizar el equilibrio de carga en amplitud y usar un nuevo límite de sesiones máximo, ejecute el siguiente cmdlet de PowerShell:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -BreadthFirstLoadBalancer -MaxSessionLimit ###
```

## <a name="configure-depth-first-load-balancing"></a>Configuración del equilibrio de carga en profundidad

El equilibrio de carga en profundidad distribuye nuevas sesiones de usuario a un host de sesión disponible con el máximo número de conexiones, pero que no ha alcanzado su umbral del límite de sesiones máximo. Al configurar el equilibrio de carga en profundidad, **debe** establecer un límite de sesiones máximo por host de sesión en el grupo de hosts.

Para configurar un grupo de hosts para realizar el equilibrio de carga en profundidad, ejecute el siguiente cmdlet de PowerShell:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -DepthFirstLoadBalancer -MaxSessionLimit ###
```
