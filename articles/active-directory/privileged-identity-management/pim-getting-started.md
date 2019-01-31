---
title: Primer uso de PIM - Azure | Microsoft Docs
description: Aprenda a habilitar y empezar a usar Azure AD Privileged Identity Management (PIM) en Azure Portal.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: pim
ms.topic: conceptual
ms.workload: identity
ms.date: 11/09/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: a670b6f1e8c2298214e28de6183064d6db424b33
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55191608"
---
# <a name="start-using-pim"></a>Primer uso de PIM

Con Azure Active Directory (Azure AD) Privileged Identity Management (PIM), puede administrar, controlar y supervisar el acceso dentro de la organización. Este ámbito incluye el acceso a los recursos de Azure, a Azure AD y a otros servicios en línea de Microsoft, como Office 365 o Microsoft Intune.

En este artículo se describe cómo habilitar y empezar a usar PIM.

## <a name="prerequisites"></a>Requisitos previos

Para usar PIM, debe tener una de las siguientes licencias:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5

Para más información, consulte los [Requisitos de licencia para usar PIM](subscription-requirements.md).

## <a name="first-person-to-use-pim"></a>Primera persona que usa PIM

Si es la primera persona que usa PIM en el directorio, se le asignarán automáticamente los roles de [Administrador de seguridad](../users-groups-roles/directory-assign-admin-roles.md#security-administrator) y [Administrador de roles con privilegios](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) en el directorio. Solo los administradores de rol con privilegios pueden administrar las asignaciones de roles de directorio de los usuarios de Azure AD. Además, puede elegir ejecutar el [Asistente para seguridad](pim-security-wizard.md) que le guía por la experiencia inicial de detección y asignación.

## <a name="enable-pim"></a>Habilitación de PIM

Para empezar a usar PIM en el directorio, primero debe habilitarlo.

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global de su directorio.

    Necesita ser un administrador global con una cuenta profesional (por ejemplo, @yourdomain.com), no una cuenta Microsoft (por ejemplo, @outlook.com), para habilitar PIM en un directorio.

1. Haga clic en **Todos los servicios** y busque el servicio **Azure AD Privileged Identity Management**.

    ![Azure AD Privileged Identity Management en Todos los servicios](./media/pim-getting-started/pim-all-services-find.png)

1. Haga clic para abrir el Inicio rápido de PIM.

1. En la lista, haga clic en **Consentimiento para PIM**.

    ![Consentimiento para PIM](./media/pim-getting-started/consent-pim.png)

1. Haga clic en **Comprobar mi identidad** para comprobar su identidad con Azure MFA. Se le pedirá que elija una cuenta.

    ![Selección de la cuenta](./media/pim-getting-started/pick-account.png)

1. Si se necesita más información para la comprobación, se le guiará por el proceso. Para más información, consulte [Obtención de ayuda con la verificación en dos pasos](https://go.microsoft.com/fwlink/p/?LinkId=708614).

    ![Se necesita más información](./media/pim-getting-started/more-information-required.png)

    Por ejemplo, puede que se le pida que proporcione comprobación telefónica.

    ![Comprobación de seguridad adicional](./media/pim-getting-started/additional-security-verification.png)

1. Una vez que haya completado el proceso de comprobación, haga clic en el botón **Consentir**.

1. En el mensaje que aparece, haga clic en **Sí** para dar su consentimiento al servicio PIM.

    ![Mensaje de consentimiento a PIM](./media/pim-getting-started/consent-pim-message.png)

## <a name="sign-up-pim-for-azure-ad-roles"></a>Registro de PIM para roles de Azure AD

Una vez que ha habilitado PIM para su directorio, deberá registrarlo para administrar roles de Azure AD.

1. Abra **Azure AD Privileged Identity Management**.

1. Haga clic en **Roles de Azure AD**.

    ![Registro de PIM para roles de Azure AD](./media/pim-getting-started/sign-up-pim-azure-ad-roles.png)

1. Haga clic en **Registrarse**.

1. En el mensaje que aparece, haga clic en **Sí** para registrar PIM para administrar roles de Azure AD.

    ![Mensaje de registro de PIM para roles de Azure AD](./media/pim-getting-started/sign-up-pim-message.png)

    Cuando finalice el registro, se habilitarán las opciones de Azure AD. Es posible que deba actualizar el portal.

    Para información sobre cómo detectar y seleccionar los recursos de Azure para proteger con PIM, consulte [Detección de recursos de Azure que se administran en PIM](pim-resource-roles-discover-resources.md).

## <a name="navigate-to-your-tasks"></a>Navegación a sus tareas

Una vez que se configura PIM, puede realizar las tareas de administración de identidades.

![Tareas de nivel superior para PIM (captura de pantalla)](./media/pim-getting-started/pim-quickstart-tasks.png)

| Tarea y administrar | DESCRIPCIÓN |
| --- | --- |
| **Mis roles**  | Muestra una lista de los roles elegibles y activos que tiene asignados. Aquí es donde puede activar cualquier función elegible asignada. |
| **Mis solicitudes** | Muestra las solicitudes pendientes para activar las asignaciones de rol elegibles. |
| **Acceso a las aplicaciones** | Le permite reducir los posibles retrasos y usar un rol inmediatamente después de la activación. |
| **Aprobar solicitudes** | Muestra una lista de las solicitudes realizadas por los usuarios de su directorio para activar roles elegibles que usted tiene designados para aprobar. |
| **Revisar acceso** | Enumera las revisiones de acceso activas que tiene asignadas para completar, tanto si revisa el acceso usted mismo como si lo hace otro usuario. |
| **Roles de Azure AD** | Muestra un panel y los valores de los administradores de rol con privilegios para administrar las asignaciones de roles de directorio de Azure AD. Este panel está deshabilitado para todos aquellos que no sean administradores de roles con privilegios. Estos usuarios tienen acceso a un panel especial denominado My view (Mi vista). El panel My view (Mi vista) solo muestra información sobre el acceso al panel del usuario, no del inquilino completo. |
| **Recursos de Azure** | Muestra un panel y los valores de los administradores de rol con privilegios para administrar las asignaciones de roles de recursos de Azure. Este panel está deshabilitado para todos aquellos que no sean administradores de roles con privilegios. Estos usuarios tienen acceso a un panel especial denominado My view (Mi vista). El panel My view (Mi vista) solo muestra información sobre el acceso al panel del usuario, no del inquilino completo. |

## <a name="add-a-pim-tile-to-the-dashboard"></a>Incorporación de un icono de PIM al panel

Para que abrir PIM sea más fácil, debe agregar un icono de PIM al panel de Azure Portal.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

1. Haga clic en **Todos los servicios** y busque el servicio **Azure AD Privileged Identity Management**.

    ![Azure AD Privileged Identity Management en Todos los servicios](./media/pim-getting-started/pim-all-services-find.png)

1. Haga clic para abrir el Inicio rápido de PIM.

1. Marque **Anclar hoja al panel** para anclar la hoja Inicio rápido de PIM al panel.

    ![Anclar hoja al panel](./media/pim-getting-started/pim-quickstart-pin-to-dashboard.png)

    En el panel de Azure, verá un icono como este:

    ![Icono de inicio rápido de PIM](./media/pim-getting-started/pim-quickstart-dashboard-tile.png)

## <a name="next-steps"></a>Pasos siguientes

- [Asignación de roles de directorio de Azure AD en PIM](pim-how-to-add-role-to-user.md)
- [Detección de recursos de Azure que se administran en PIM](pim-resource-roles-discover-resources.md)
