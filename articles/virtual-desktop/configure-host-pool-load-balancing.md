---
title: 'Configurar el escritorio de Windows Virtual equilibrio de carga (método) (versión preliminar): Azure'
description: Cómo configurar el método de equilibrio de carga para un entorno de Escritorio Virtual de Windows.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 10a1066b85b16749fe95e373e696d486b0e7bafa
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58318350"
---
# <a name="configure-the-windows-virtual-desktop-load-balancing-method"></a>Configurar el método de equilibrio de carga de Escritorio Virtual de Windows

Configurar el método de equilibrio de carga para un grupo host le permite ajustar el entorno de Escritorio Virtual de Windows (versión preliminar) se adapte mejor a sus necesidades.

>[!NOTE]
> Esto no se aplica a un grupo host de escritorio persistente porque los usuarios siempre tienen una asignación 1:1 en un host de sesión dentro del grupo host.

## <a name="configure-breadth-first-load-balancing"></a>Configurar el equilibrio de carga de amplitud

Equilibrio de carga de amplitud es la configuración predeterminada para nuevos grupos de host no persistentes. Equilibrio de carga de amplitud, nuevas sesiones de usuario distribuye entre todos los hosts de sesión disponibles en el grupo host. Al configurar el equilibrio de carga de amplitud, puede establecer un límite máximo de la sesión por host de sesión en el grupo host.

Primero, [descargar e importar el módulo de Windows PowerShell de Escritorio Virtual](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) usar en la sesión de PowerShell si no lo ha hecho ya.

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
