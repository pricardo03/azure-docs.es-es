---
title: Configuración de alertas de seguridad para los roles de recurso de Azure en PIM | Microsoft Docs
description: Aprenda a configurar alertas de seguridad para los roles de recurso de Azure en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 2fa63cf2fa05f2cde4558f0bea38bfd7f17df3ae
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2018
ms.locfileid: "43342069"
---
# <a name="configure-security-alerts-for-azure-resource-roles-in-pim"></a>Configuración de alertas de seguridad para los roles de recurso de Azure en PIM
Privileged Identity Management (PIM) para los recursos de Azure genera alertas cuando existen actividades sospechosas o no seguras en su entorno. Cuando se desencadena una alerta, se muestra en la página de alertas. 

![Página de alertas](media/azure-pim-resource-rbac/RBAC-alerts-home.png)

## <a name="review-alerts"></a>Revisar alertas
Seleccione una alerta para ver un informe que enumera los usuarios o roles que desencadenaron la alerta, así como consejos de solución.

![Informe de alertas](media/azure-pim-resource-rbac/rbac-alert-info.png)

## <a name="alerts"></a>Alertas
| Alerta | Gravedad | Desencadenador | Recomendación |
| --- | --- | --- | --- |
| **Hay demasiados propietarios asignados a un recurso** |Mediano |Demasiados usuarios tienen el rol de propietario. |Revise los usuarios de la lista y vuelva a asignar algunos a roles con menos privilegios. |
| **Hay demasiados propietarios permanentes asignados a un recurso** |Mediano |Demasiados usuarios están asignados permanentemente a un rol. |Revise los usuarios de la lista y vuelva a asignar algunos para que tengan que solicitar la activación para usar el rol. |
| **Se ha creado un rol duplicado** |Mediano |Varios roles tienen los mismos criterios. |Use solo uno de estos roles. |


### <a name="severity"></a>Gravedad
* **Alta**: requiere acción inmediata debido a la infracción de una directiva. 
* **Media**: no requiere acción inmediata, pero indica una posible infracción de una directiva.
* **Baja**: no requiere acción inmediata pero sugiere un cambio de directiva preferida.

## <a name="configure-security-alert-settings"></a>Configuración de alertas de seguridad
En la página de alertas, vaya a **Configuración**.
![Configuración](media/azure-pim-resource-rbac/rbac-navigate-settings.png)

Personalice la configuración de las diferentes alertas para que encajen con su entorno y con sus objetivos de seguridad.
![Personalizar la configuración](media/azure-pim-resource-rbac/rbac-alert-settings.png)

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de alertas de seguridad para los roles de recurso de Azure en PIM](pim-resource-roles-configure-alerts.md)
