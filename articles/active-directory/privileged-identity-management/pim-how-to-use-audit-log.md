---
title: Visualización del historial de auditoría para los roles de directorio de Azure AD en PIM | Microsoft Docs
description: Aprenda a visualizar el historial de auditoría para los roles de directorio de Azure AD en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 02/14/2017
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3b91e2c15e86cdda4daab27892fa7a2b6ed0c0e0
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58008909"
---
# <a name="view-audit-history-for-azure-ad-directory-roles-in-pim"></a>Visualización del historial de auditoría para los roles de directorio de Azure AD en PIM
Puede utilizar el historial de auditoría de Privileged Identity Management (PIM) para ver todas las asignaciones de usuario y las activaciones comprendidas en un período de tiempo determinado para todos los roles con privilegios. Si quiere ver el historial de auditoría completo de actividad en el inquilino, incluido el administrador, el usuario final y la actividad de sincronización, puede usar los [informes de acceso y uso de Azure Active Directory](../reports-monitoring/overview-reports.md)

## <a name="navigate-to-audit-history"></a>Ir al historial de auditoría
En el panel de [Azure Portal](https://portal.azure.com) , seleccione la aplicación **Azure AD Privileged Identity Management** . Desde ahí, acceda al historial de auditoría; para ello, haga clic en **Administrar roles con privilegios** > **Historial de auditoría** en el panel de PIM.

![Historial de auditoría](media/azure-ad-pim-approval-workflow/image021.png)

> [!NOTE]
> Puede ordenar los datos por acción y buscar el texto "Activación aprobada".


## <a name="audit-history-graph"></a>Gráfico del historial de auditoría
Puede utilizar el historial de auditoría para ver el total de activaciones, el número máximo de activaciones por día y el promedio de activaciones por día en un gráfico de líneas.  También puede filtrar los datos por rol si hay más de un rol en el historial de auditoría.

Utilice los botones de **tiempo**, **acción** y **rol** para ordenar el historial.

## <a name="audit-history-list"></a>Lista del historial de auditoría
Las columnas de la lista del historial de auditoría son:

* **Solicitante** : el usuario que solicitó la activación o el cambio del rol.  Si el valor es "Sistema de Azure", compruebe el historial de auditoría de Azure para más información.
* **Usuario** : el usuario que activa el rol o que está asignado al rol.
* **Rol** : el rol asignado al usuario o activado por el usuario.
* **Acción** : las acciones realizadas por el solicitante. Esto puede incluir asignación, desasignación, activación o desactivación.
* **Hora** : momento en que se produjo la acción.
* **Razonamiento** : si durante la activación se escribió texto en el campo de motivo, aparecerá aquí.
* **Caducidad** : solo es importante para la activación de los roles.

## <a name="filter-audit-history"></a>Filtrado del historial de auditoría
Puede filtrar la información que aparece en el historial de auditoría haciendo clic en el botón **Filtrar**.  Aparecerá la hoja **Actualizar parámetros del gráfico** .

Después de configurar los filtros, haga clic en **Actualizar** para filtrar los datos del historial.  Si los datos no aparecen inmediatamente, actualice la página.

### <a name="change-the-date-range"></a>Cambio del intervalo de fechas
Utilice los botones **Hoy**, **Semana anterior**, **Mes anterior** o **Personalizado** para cambiar el intervalo de tiempo del historial de auditoría.

Si elige el botón **Personalizado**, aparece un campo de fecha **Desde** y un campo de fecha **Hasta** para especificar un intervalo de fechas para el historial.  Puede especificar las fechas en formato MM/DD/AAAA o hacer clic en el icono de **calendario** y elegir la fecha en un calendario.

### <a name="change-the-roles-included-in-the-history"></a>Cambio de los roles incluidos en el historial
Active o desactive la casilla **Rol** situada junto a cada rol que quiera incluir o excluir del historial.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Pasos siguientes

- [Visualización de la actividad y del historial de auditoría para los roles de recursos de Azure en PIM](azure-pim-resource-rbac.md)
