---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
author: rolyon
ms.service: active-directory
ms.topic: include
ms.date: 07/31/2019
ms.author: rolyon
ms.custom: include file
ms.openlocfilehash: 154d71c9cbc109834a5854b46c3e6584dcefa7eb
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2019
ms.locfileid: "68968938"
---
### <a name="policy-for-users-in-your-directory"></a>Directiva: para los usuarios del directorio

Siga estos pasos si quiere que la directiva sea para los usuarios que están en el directorio que pueden solicitar este paquete de acceso.  Los **usuarios que están en el directorio** hacen referencia a los usuarios internos, así como a los usuarios externos a los que se ha invitado previamente al directorio, ya sea que ellos solicitaron la administración de derechos con otro paquete de acceso o se los ha invitado con Azure AD B2B. Al definir la directiva, puede especificar usuarios individuales o, más comúnmente, grupos de usuarios. Por ejemplo, es posible que su organización ya tenga un grupo, como **Todos los empleados**.  Si ese grupo se agrega a la directiva para los usuarios que pueden solicitar acceso, cualquier miembro de ese grupo podrá solicitar acceso.

1. En la sección **Users who can request access** (Usuarios que pueden solicitar acceso), seleccione **For users in your directory** (Para los usuarios del directorio).

    Tenga en cuenta que la opción **For users in your directory** (Para los usuarios del directorio) incluye tanto los usuarios miembros como los invitados que se han agregado al directorio. Si solo quiere incluir usuarios miembros y no usuarios invitados, seleccione **For users in your directory** (Para los usuarios del directorio) y, a continuación, seleccione un grupo de usuarios miembros. Si es necesario, puede crear un grupo dinámico de usuarios miembros (user.userType-eq "Member"). Para más información, consulte [Reglas de pertenencia dinámica a grupos de Azure Active Directory](../articles/active-directory/users-groups-roles/groups-dynamic-membership.md).

1. En la sección **Seleccionar usuarios y grupos**, haga clic en **Agregar usuarios y grupos**.

1. En el panel Seleccionar usuarios y grupos, seleccione los usuarios y grupos que quiera agregar.

    ![Paquete de acceso: directiva; seleccionar usuarios y grupos](./media/active-directory-entitlement-management-policy/policy-select-users-groups.png)

1. Haga clic en **Seleccionar** para agregar los usuarios y grupos.

1. Pase a la sección [Directiva: solicitud](#policy-request).

### <a name="policy-for-users-not-in-your-directory"></a>Directiva: para los usuarios que no están en el directorio

Siga estos pasos si quiere que la directiva sea para los usuarios que no están en el directorio que pueden solicitar este paquete de acceso. Los **usuarios que no están en el directorio** hacen referencia a los usuarios que se encuentran en otro directorio de Azure AD y que posiblemente aún no se los haya invitado al directorio. Actualmente, solo puede agregar usuarios de organizaciones que tienen Azure AD. Los directorios deben configurarse para que se permitan en la configuración **Organizational relationships collaboration restrictions** (Restricciones de colaboración de relaciones de la organización).

> [!NOTE]
> Se creará una cuenta de usuario invitado externo para un usuario que aún no esté en el directorio cuya solicitud se haya aprobado o aprobado automáticamente. Se invitará al invitado, pero no recibirá un correo electrónico de invitación. En su lugar, recibirá un correo electrónico cuando se entregue su asignación de paquete de acceso. De forma predeterminada, más adelante cuando ese usuario invitado ya no tenga ninguna asignación de paquete de acceso porque su última asignación ha expirado o se ha cancelado, se bloqueará el inicio de sesión de esa cuenta de usuario invitado y se eliminará posteriormente. Si quiere que los usuarios invitados permanezcan en el directorio de manera indefinida, incluso si no tienen ninguna asignación de paquete de acceso, puede cambiar la configuración de la administración de derechos.

1. En la sección **Users who can request access** (Usuarios que pueden solicitar acceso), seleccione **Para los usuarios que no están en su directorio**.

1. En la sección **Seleccionar directorio externo de Azure AD**, haga clic en **Add directories** (Agregar directorios).

1. Escriba un nombre de dominio y busque un directorio de Azure AD con ese nombre de dominio.

1. Fíjese en el dominio inicial y el nombre del directorio proporcionado para comprobar que es el directorio correcto.

    > [!NOTE]
    > Todos los usuarios del directorio podrán solicitar este paquete de acceso. Esto incluye a los usuarios de todos los subdominios asociados con el directorio, no solo el dominio que ha usado en la búsqueda.

    ![Paquete de acceso: directiva; seleccionar directorios](./media/active-directory-entitlement-management-policy/policy-select-directories.png)

1. Haga clic en **Agregar** para agregar el directorio.

1. Repita este paso para agregar los directorios que quiera.

1. Cuando haya agregado todos los directorios que quiera incluir en la directiva, haga clic en **Seleccionar**.

1. Pase a la sección [Directiva: solicitud](#policy-request).

### <a name="policy-none-administrator-direct-assignments-only"></a>Directiva: ninguno (solo para las asignaciones directas del administrador)

Siga estos pasos si quiere que la directiva omita las solicitudes de acceso y permita a los administradores asignar directamente a usuarios específicos al paquete de acceso. Los usuarios no tendrán que solicitar el paquete de acceso. Aún podrá establecer la configuración de expiración, pero no hay ninguna configuración de solicitud.

1. En la sección **Users who can request access** (Usuarios que pueden solicitar acceso), seleccione **Ninguno (solo para las asignaciones directas del administrador)** .

    Después de crear el paquete de acceso, puede asignar directamente usuarios internos y externos específicos al paquete de acceso. Si especifica un usuario externo, se creará una cuenta de usuario invitado en su directorio.

1. Pase a la sección [Directiva: expiración](#policy-expiration).

### <a name="policy-request"></a>Directiva: Solicitud

En la sección de solicitud, especificará la configuración de aprobación cuando los usuarios solicitan el paquete de acceso.

1. Para requerir la aprobación de las solicitudes de los usuarios seleccionados, establezca el botón de alternancia **Requerir aprobación** en **Sí**. Para que las solicitudes se aprueban automáticamente, establezca el botón de alternancia en **No**.

1. Si requiere la aprobación, en la sección **Seleccionar aprobadores**, haga clic en **Agregar aprobadores**.

1. En el panel Seleccionar aprobadores, seleccione uno o varios usuarios o grupos para convertirlos en aprobadores.

    Solo uno de los aprobadores seleccionados debe aprobar una solicitud. No se requiere la aprobación de todos los aprobadores. La decisión de aprobación se basa en el aprobador que revise primero la solicitud.

    ![Paquete de acceso: directiva; seleccionar aprobadores](./media/active-directory-entitlement-management-policy/policy-select-approvers.png)

1. Haga clic en **Seleccionar** para agregar a los aprobadores.

1. Haga clic en **Mostrar configuración de solicitud avanzada** para mostrar la configuración adicional.

    ![Paquete de acceso: directiva; seleccionar directorios](./media/active-directory-entitlement-management-policy/policy-advanced-request.png)

1. Para requerir que los usuarios proporcionen una justificación para solicitar el paquete de acceso, establezca **Requerir justificación** en **Sí**.

1. Para requerir que el aprobador proporcione una justificación para aprobar una solicitud del paquete de acceso, establezca **Requerir justificación del aprobador** en **Sí**.

1. En el cuadro **Approval request timeout (days)** (Tiempo de expiración de la solicitud de aprobación (días)), especifique el período de tiempo que tienen los aprobadores para revisar una solicitud. Si ningún aprobador lo revisa en este número de días, la solicitud expira y el usuario tendrá que enviar otra solicitud para el paquete de acceso.

### <a name="policy-expiration"></a>Directiva: Expiration

En la sección de expiración, especificará cuándo expira la asignación de un usuario para el paquete de acceso.

1. En la sección **Expiración**, establezca **El paquete de acceso expira el** en **En la fecha**, **Número de días** o **Nunca**.

    Para la opción **En la fecha**, seleccione una fecha de expiración en el futuro.

    Para la opción **Número de días**, especifique un número entre 0 y 3660 días.

    En función de lo que seleccione, la asignación de un usuario para el paquete de acceso expirará en una fecha concreta, un número determinado de días después de la aprobación o nunca.

1. Haga clic en **Mostrar configuración de expiración avanzada** para mostrar la configuración adicional.

1. Para permitir que el usuario amplíe sus asignaciones, establezca **Permitir que los usuarios extiendan el acceso** en **Sí**.

    Si se permiten las extensiones en la directiva, el usuario recibirá un correo electrónico 14 días y 1 día antes de que se establezca la expiración de su asignación de paquete de acceso en el que se le solicitará que amplíe la asignación.

    ![Paquete de acceso: directiva; configuración de expiración](./media/active-directory-entitlement-management-policy/policy-expiration.png)

### <a name="policy-enable-policy"></a>Directiva: Habilitar directiva

1. Si quiere que el paquete de acceso esté disponible de inmediato para los usuarios de la directiva, haga clic en **Sí** para habilitar la directiva.

    Podrá habilitarla en todo momento cuando haya terminado de crear el paquete de acceso.

    ![Paquete de acceso: directiva; configuración de habilitación de la directiva](./media/active-directory-entitlement-management-policy/policy-enable.png)

1. Haga clic en **Siguiente** o **Crear**.
