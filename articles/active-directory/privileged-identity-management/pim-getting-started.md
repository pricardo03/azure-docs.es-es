---
title: 'Primer uso de PIM: Azure Active Directory | Microsoft Docs'
description: Aprenda a habilitar y empezar a usar Azure AD Privileged Identity Management (PIM) en Azure Portal.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: pim
ms.topic: conceptual
ms.workload: identity
ms.date: 03/05/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 733743f4680f3197a1754d87b4672641b2321907
ms.sourcegitcommit: 05b36f7e0e4ba1a821bacce53a1e3df7e510c53a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78399345"
---
# <a name="start-using-privileged-identity-management"></a>Empiece a usar Privileged Identity Management

Con Privileged Identity Management (PIM), puede administrar, controlar y supervisar el acceso dentro de la organización de Azure Active Directory (Azure AD). Este ámbito incluye el acceso a los recursos de Azure, a Azure AD y a otros servicios en línea de Microsoft, como Office 365 o Microsoft Intune.

En este artículo se describe cómo habilitar y empezar a usar Privileged Identity Management.

## <a name="prerequisites"></a>Prerrequisitos

Para usar Privileged Identity Management, debe tener una de las licencias siguientes:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5

Para más información, consulte [Requisitos de licencia para usar Privileged Identity Management](subscription-requirements.md).

## <a name="first-person-to-use-pim"></a>Primera persona que usa PIM

Si es la primera persona que usa Privileged Identity Management en el directorio, se le asignarán automáticamente los roles de [Administrador de seguridad](../users-groups-roles/directory-assign-admin-roles.md#security-administrator) y [Administrador de roles con privilegios](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) del directorio. Solo los administradores de rol con privilegios pueden administrar las asignaciones de roles de los usuarios de Azure AD. Además, puede elegir ejecutar el [Asistente para seguridad](pim-security-wizard.md) que le guía por la experiencia inicial de detección y asignación.

## <a name="enable-pim"></a>Habilitación de PIM

Para empezar a usar Privileged Identity Management en el directorio, primero debe habilitarlo.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global de su directorio.

    Necesita ser un administrador global con una cuenta profesional (por ejemplo, @yourdomain.com), no una cuenta Microsoft (por ejemplo, @outlook.com), para habilitar Privileged Identity Management en un directorio.

1. Haga clic en **Todos los servicios** y busque el servicio **Azure AD Privileged Identity Management**.

    ![Azure AD Privileged Identity Management en Todos los servicios](./media/pim-getting-started/pim-all-services-find.png)

1. Haga clic para abrir el Inicio rápido de Privileged Identity Management.

1. En la lista, haga clic en **Consentimiento para PIM**.

    ![Consentimiento para que Privileged Identity Management habilite Privileged Identity Management](./media/pim-getting-started/consent-pim.png)

1. Haga clic en **Comprobar mi identidad** para comprobar su identidad con Azure MFA. Se le pedirá que elija una cuenta.

    ![Seleccione una ventana de la cuenta para comprobar su identidad.](./media/pim-getting-started/pick-account.png)

1. Si se necesita más información para la comprobación, se le guiará por el proceso. Para más información, consulte [Obtención de ayuda con la verificación en dos pasos](../user-help/multi-factor-authentication-end-user-troubleshoot.md).

    ![Ventana que indica que se necesita más información si la organización necesita más información](./media/pim-getting-started/more-information-required.png)

    Por ejemplo, puede que se le pida que proporcione comprobación telefónica.

    ![Página de comprobación de seguridad adicional que le pregunta cómo contactar con usted](./media/pim-getting-started/additional-security-verification.png)

1. Una vez que haya completado el proceso de comprobación, haga clic en el botón **Consentir**.

1. En el mensaje que aparece, haga clic en **Sí** para dar su consentimiento al servicio Privileged Identity Management.

    ![Consentimiento para el mensaje de Privileged Identity Management para completar el proceso de consentimiento](./media/pim-getting-started/consent-pim-message.png)

## <a name="sign-up-pim-for-azure-ad-roles"></a>Registro de PIM para roles de Azure AD

Una vez que haya habilitado Privileged Identity Management para su directorio, deberá registrar Privileged Identity Management para administrar los roles de Azure AD.

1. Abra **Azure AD Privileged Identity Management**.

1. Haga clic en **Roles de Azure AD**.

    ![Registro de Privileged Identity Management para roles de Azure AD](./media/pim-getting-started/sign-up-pim-azure-ad-roles.png)

1. Haga clic en **Registrarse**.

1. En el mensaje que aparece, haga clic en **Sí** para registrar Privileged Identity Management para administrar los roles de Azure AD.

    ![Registro de Privileged Identity Management para el mensaje de roles de Azure AD](./media/pim-getting-started/sign-up-pim-message.png)

    Cuando finalice el registro, se habilitarán las opciones de Azure AD. Es posible que deba actualizar el portal.

    Para información sobre cómo detectar y seleccionar los recursos de Azure que se van a proteger con Privileged Identity Management, consulte [Detección de recursos de Azure que se administran en PIM](pim-resource-roles-discover-resources.md).

## <a name="navigate-to-your-tasks"></a>Navegación a sus tareas

Una vez que se configuró Privileged Identity Management, puede iniciar las tareas de administración de identidades.

![Ventana de navegación en Privileged Identity Management que muestra las opciones Tareas y Administrar](./media/pim-getting-started/pim-quickstart-tasks.png)

| Tarea y administrar | Descripción |
| --- | --- |
| **Mis roles**  | Muestra una lista de los roles elegibles y activos que tiene asignados. Aquí es donde puede activar cualquier función elegible asignada. |
| **Mis solicitudes** | Muestra las solicitudes pendientes para activar las asignaciones de rol elegibles. |
| **Aprobar solicitudes** | Muestra una lista de las solicitudes realizadas por los usuarios de su directorio para activar roles elegibles que usted tiene designados para aprobar. |
| **Revisar acceso** | Enumera las revisiones de acceso activas que tiene asignadas para completar, tanto si revisa el acceso usted mismo como si lo hace otro usuario. |
| **Roles de Azure AD** | Muestra un panel y los valores de los administradores de rol con privilegios para administrar las asignaciones de roles de Azure AD. Este panel está deshabilitado para todos aquellos que no sean administradores de roles con privilegios. Estos usuarios tienen acceso a un panel especial denominado My view (Mi vista). El panel My view (Mi vista) solo muestra información sobre el acceso al panel del usuario, no del inquilino completo. |
| **Recursos de Azure** | Muestra un panel y los valores de los administradores de rol con privilegios para administrar las asignaciones de roles de recursos de Azure. Este panel está deshabilitado para todos aquellos que no sean administradores de roles con privilegios. Estos usuarios tienen acceso a un panel especial denominado My view (Mi vista). El panel My view (Mi vista) solo muestra información sobre el acceso al panel del usuario, no del inquilino completo. |

## <a name="add-a-pim-tile-to-the-dashboard"></a>Incorporación de un icono de PIM al panel

Para que sea más fácil abrir Privileged Identity Management, debe agregar un icono de Privileged Identity Management al panel de Azure Portal.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

1. Haga clic en **Todos los servicios** y busque el servicio **Azure AD Privileged Identity Management**.

    ![Azure AD Privileged Identity Management en Todos los servicios](./media/pim-getting-started/pim-all-services-find.png)

1. Haga clic para abrir el Inicio rápido de Privileged Identity Management.

1. Active la opción **Anclar hoja al panel** para anclar la hoja Inicio rápido de Privileged Identity Management al panel.

    ![Icono de marcador para anclar la hoja Privileged Identity Management al panel](./media/pim-getting-started/pim-quickstart-pin-to-dashboard.png)

    En el panel de Azure, verá un icono como este:

    ![Icono de Inicio rápido de Privileged Identity Management en el panel](./media/pim-getting-started/pim-quickstart-dashboard-tile.png)

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de roles de Azure AD en Privileged Identity Management](pim-how-to-add-role-to-user.md)
- [Detección de recursos de Azure que se administran en PIM](pim-resource-roles-discover-resources.md)
