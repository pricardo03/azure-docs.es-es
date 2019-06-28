---
title: Uso de un panel de recursos para realizar una revisión de acceso en PIM - Azure Active Directory | Microsoft Docs
description: Describe cómo usar un panel de recursos para realizar una revisión de acceso en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 03/30/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: e5404d1b85821458aedef64b72ae635ea49aa1ff
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65602474"
---
# <a name="use-a-resource-dashboard-to-perform-an-access-review-in-pim"></a>Uso de un panel de control de recursos para realizar una revisión de acceso en PIM

Puede usar un panel de recursos para realizar una revisión de acceso en Azure Active Directory (Azure AD) Privileged Identity Management (PIM). El panel Vista de administrador consta de tres elementos principales:

- Una representación gráfica de las activaciones de rol de recursos.
- Dos gráficos que muestran la distribución de las asignaciones de roles por tipo de asignación.
- Un área de datos que pertenece a nuevas asignaciones de roles.

![Captura de pantalla del panel de la vista de administrador, que muestra gráficos y tablas](media/pim-resource-roles-overview-dashboards/rbac-overview-top.png)

![Captura de pantalla del panel de la vista de administrador, que muestra listas de datos](media/pim-resource-roles-overview-dashboards/role-settings.png)

La representación gráfica de las activaciones de rol de recursos de los últimos siete días. Estos datos se limitan al recurso seleccionado y muestran las activaciones de los roles más comunes (propietario, colaborador, administrador de acceso de usuario) y de la combinación de todos los roles.

A la derecha del grafo de activaciones, dos gráficos muestran la distribución de las asignaciones de roles por tipo de asignación, tanto de usuario como de grupo. Para cambiar el valor a un porcentaje (o viceversa), seleccione un segmento del gráfico.

Por debajo de los gráficos, verá el número de usuarios y grupos con nuevas asignaciones de roles de los últimos 30 días y una lista de los roles ordenada por asignaciones totales (en orden descendente).

## <a name="next-steps"></a>Pasos siguientes

- [Inicio de una revisión de acceso para los roles de recurso de Azure en PIM](pim-resource-roles-start-access-review.md) 
