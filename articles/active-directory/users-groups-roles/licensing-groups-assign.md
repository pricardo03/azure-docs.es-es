---
title: Asignación de licencias a grupos en Azure Active Directory | Microsoft Docs
description: Procedimiento de asignación de licencias a los usuarios con las licencias de grupos de Azure Active Directory
services: active-directory
keywords: Licencias de Azure AD
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.subservice: users-groups-roles
ms.date: 03/18/2019
ms.author: curtand
ms.reviewer: sumitp
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8a54d1ad3ab809f2a2f8df6ae0e30b1b061c2be1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60471338"
---
# <a name="assign-licenses-to-users-by-group-membership-in-azure-active-directory"></a>Asignación de licencias a usuarios según su pertenencia a un grupo en Azure Active Directory

Este artículo le guiará a través del proceso de asignación de licencias de producto a un grupo de usuarios en Azure Active Directory (Azure AD) y, a continuación, comprobará que las licencias se han asignado correctamente.

En este ejemplo, el inquilino contiene un grupo de seguridad llamado **HR Department**. Este grupo incluye a todos los miembros del departamento de recursos humanos (en este caso, alrededor de 1000 usuarios). Desea asignar licencias de Office 365 Enterprise E3 a todo el departamento. El servicio Yammer Enterprise que se incluye en el producto se debe deshabilitar temporalmente hasta que el departamento esté listo para empezar a usarlo. También desea implementar licencias de Enterprise Mobility + Security para el mismo grupo de usuarios.

> [!NOTE]
> Algunos servicios de Microsoft no están disponibles en todas las ubicaciones. Para poder asignar una licencia a un usuario, el administrador tiene que especificar la propiedad Ubicación de uso en el usuario.
> 
> En el caso de la asignación de licencias de grupo, cualquier usuario sin una ubicación de uso especificada heredará la ubicación del directorio. Si hay usuarios en varias ubicaciones, se recomienda establecer la ubicación de uso siempre como parte del flujo de creación de usuarios en Azure AD (por ejemplo, mediante la configuración de AAD Connect), lo que garantiza que el resultado de la asignación de licencias siempre es correcto y que los usuarios no reciben los servicios en ubicaciones que no están permitidas.

## <a name="step-1-assign-the-required-licenses"></a>Paso 1: Asignación de las licencias necesarias

1. Inicie sesión en el [**Centro de administración de Azure AD**](https://aad.portal.azure.com) con una cuenta de administrador de licencias. Para administrar las licencias, la cuenta debe ser un administrador de licencias, un administrador de usuarios o un administrador global.

2. Seleccione **Licencias** para abrir un panel donde puede ver y administrar todos los productos con licencia del inquilino.

4. En **Todos los productos**, seleccione los nombres de producto Office 365 Enterprise E5 y Enterprise Mobility + Security E3. Para iniciar la asignación, seleccione **Asignar** en la parte superior del panel.

   ![Selección de los productos para asignar licencias](./media/licensing-groups-assign/all-products-assign.png)
  
5. En el panel **Asignar licencia**, seleccione **Usuarios y grupos** para abrir una lista de usuarios y grupos.

6. Seleccione un usuario o grupo y, luego, use el botón **Seleccionar** en la parte inferior del panel para confirmar la selección.

7. En el panel **Asignar licencia**, haga clic en **Opciones de asignación**, donde se muestran todos los planes de servicio incluidos en los dos productos que se seleccionaron anteriormente. Busque **Yammer Enterprise** y establézcalo en **Desactivar** para deshabilitar el servicio de la licencia de producto. Para confirmar, haga clic en **Aceptar** en la parte inferior de **Opciones de licencia**.

   ![Selección de planes de servicio para licencias](./media/licensing-groups-assign/assignment-options.png)
  
8. Para completar la asignación, en el panel **Asignar licencia**, haga clic en **Asignar** en la parte inferior del panel.

9. Se muestra una notificación en la esquina superior derecha con el estado y el resultado del proceso. Si no se pudo completar la asignación al grupo (por ejemplo, porque ya existían licencias en el grupo), haga clic en la notificación para ver los detalles del error.

Al asignar licencias a un grupo, Azure AD procesa todos los miembros existentes de ese grupo. Este proceso puede tardar algún tiempo, según el tamaño del grupo. En el paso siguiente se describe cómo comprobar si el proceso se ha completado y si es necesario hacer algo más para resolver los problemas.

## <a name="step-2-verify-that-the-initial-assignment-has-finished"></a>Paso 2: Comprobación de que ha finalizado la asignación inicial

1. Vaya a **Azure Active Directory** > **Grupos**. Seleccione el grupo al que se asignaron las licencias.

2. En el panel de grupo, seleccione **Licencias**. Esto le permite confirmar rápidamente si las licencias se han asignado completamente a los usuarios y si hay errores que requieran atención. Está disponible la siguiente información:

   - Lista de licencias de producto que están asignadas actualmente al grupo. Seleccione una entrada para mostrar los servicios específicos que se han habilitado y para realizar cambios.

   - Estado de los últimos cambios de licencia realizados en el grupo: si los cambios se están procesando o si se ha completado el procesamiento en todos los usuarios miembros.

   - Información acerca de los usuarios que están en estado de error porque no se les pudo asignar licencias.

   ![errores de licencia y estado de licencia](./media/licensing-groups-assign/assignment-errors.png)

3. Encontrará información más detallada sobre el procesamiento de licencias en **Azure Active Directory** > **Usuarios y grupos** > *nombre de grupo* > **Registros de auditoría**. Tenga en cuenta las siguientes actividades:

   - Actividad: **Empezar a aplicar licencias basadas en grupo a los usuarios**. Se registra cuando el sistema recoge el cambio de asignación de licencias en el grupo y empieza a aplicarlo a todos los usuarios miembros. Contiene información sobre el cambio que se realizó.

   - Actividad: **Finalizar la aplicación de licencias basadas en grupo a los usuarios**. Se registra cuando el sistema termina de procesar todos los usuarios del grupo. Contiene un resumen de cuántos usuarios se han procesado correctamente y el número de usuarios a los que no se pudieron asignar las licencias de grupo.

   [Lea esta sección](licensing-group-advanced.md#use-audit-logs-to-monitor-group-based-licensing-activity) para obtener más información sobre cómo se pueden usar los registros de auditoría para analizar los cambios realizados por las licencias basadas en grupos.

## <a name="step-3-check-for-license-problems-and-resolve-them"></a>Paso 3: Comprobación y resolución de problemas de licencias

1. Vaya a **Azure Active Directory** > **Grupos** y busque el grupo al que se asignaron las licencias.
2. En el panel de grupo, seleccione **Licencias**. La notificación situada en la parte superior del panel muestra que hay 10 usuarios a los que no se pudieron asignar licencias. Ábrala para ver una lista de todos los usuarios en estado de error de licencia para este grupo.
3. La columna **Asignaciones erróneas** indica que no se pudo asignar ninguna de las licencias de producto a los usuarios. La columna **Motivo principal del error** contiene la causa del error. En este caso, es **Planes de servicio en conflicto**.

   ![licencias que no se pudieron asignar](./media/licensing-groups-assign/failed-assignments.png)

4. Seleccione un usuario para abrir el panel **Licencias**. Este panel muestra todas las licencias que están asignadas actualmente al usuario. En este ejemplo, el usuario tiene la licencia de Office 365 Enterprise E1 que heredó del grupo **Kiosk users**. Esto entra en conflicto con la licencia E3 que el sistema intentó aplicar desde el grupo **HR Department**. Como resultado, ninguna de las licencias de dicho grupo se ha asignado al usuario.

   ![Visualización de todos los conflictos de licencia para un usuario](./media/licensing-groups-assign/user-license-view.png)

5. Para resolver este problema, quite el usuario del grupo **Kiosk users**. Una vez que Azure AD procesa el cambio, las licencias de **HR Department** se asignan correctamente.

   ![Las licencias se asignan aquí correctamente](./media/licensing-groups-assign/license-correctly-assigned.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el conjunto de características de administración de licencias mediante grupos, consulte los artículos siguientes:

* [¿En qué consisten las licencias basadas en grupos de Azure Active Directory?](../fundamentals/active-directory-licensing-whatis-azure-portal.md)
* [Identificación y resolución de problemas de licencias de un grupo en Azure Active Directory](licensing-groups-resolve-problems.md)
* [Migración de usuarios individuales con licencia a licencias basadas en grupos en Azure Active Directory](licensing-groups-migrate-users.md)
* [Cómo migrar usuarios entre diferentes licencias de productos con licencias basadas en grupos de Azure Active Directory](licensing-groups-change-licenses.md)
* [Azure Active Directory group-based licensing additional scenarios](../active-directory-licensing-group-advanced.md) (Escenarios adicionales de licencias basadas en grupos de Azure Active Directory)
* [Ejemplos de PowerShell para licencias basadas en grupos de Azure AD](licensing-ps-examples.md)
