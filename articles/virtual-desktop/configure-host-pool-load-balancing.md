---
title: Configurar el método de equilibrio de carga de vista previa de Escritorio Virtual de Windows - Azure
description: Cómo configurar el método de equilibrio de carga para un entorno de Escritorio Virtual de Windows.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 0c4702dada17e759d89c33be99b3155f4b15ad9e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60328891"
---
# <a name="configure-the-windows-virtual-desktop-preview-load-balancing-method"></a>Configurar el método de equilibrio de carga de vista previa de Escritorio Virtual de Windows

Configurar el método de equilibrio de carga para un grupo host le permite ajustar el entorno de Windows Vista previa del escritorio Virtual que se adapte mejor a sus necesidades.

>[!NOTE]
> Esto no se aplica a un grupo host de escritorio persistente porque los usuarios siempre tienen una asignación 1:1 en un host de sesión dentro del grupo host.

## <a name="configure-breadth-first-load-balancing"></a>Configurar el equilibrio de carga de amplitud

Equilibrio de carga de amplitud es la configuración predeterminada para nuevos grupos de host no persistentes. Equilibrio de carga de amplitud, nuevas sesiones de usuario distribuye entre todos los hosts de sesión disponibles en el grupo host. Al configurar el equilibrio de carga de amplitud, puede establecer un límite máximo de la sesión por host de sesión en el grupo host.

En primer lugar y, si aún no lo ha hecho, [descargue e importe el módulo de PowerShell para Windows Virtual Desktop](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) que se usará en la sesión de PowerShell.

Para configurar un grupo host para llevar a cabo sin ajustar el límite máximo de la sesión de equilibrio de carga de amplitud, ejecute el siguiente cmdlet de PowerShell:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -BreadthFirstLoadBalancer
```

Para configurar un grupo host para realizar el equilibrio de carga de la amplitud y utilice un límite máximo de una sesión nueva, ejecute el siguiente cmdlet de PowerShell:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -BreadthFirstLoadBalancer -MaxSessionLimit ###
```

## <a name="configure-depth-first-load-balancing"></a>Configurar el equilibrio de carga de prioridad de profundidad

Equilibrio de carga de prioridad de profundidad distribuye nuevas sesiones de usuario a un host de sesión disponibles con el mayor número de conexiones, pero no ha alcanzado el umbral de límite máximo de la sesión. Al configurar el equilibrio de carga de prioridad de profundidad, **debe** establecer un límite máximo de la sesión por host de sesión en el grupo host.

Para configurar un grupo host para realizar el equilibrio de carga de prioridad de profundidad, ejecute el siguiente cmdlet de PowerShell:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -DepthFirstLoadBalancer -MaxSessionLimit ###
```
