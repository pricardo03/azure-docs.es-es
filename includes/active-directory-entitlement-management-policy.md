---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
author: rolyon
ms.service: active-directory
ms.topic: include
ms.date: 05/16/2019
ms.author: rolyon
ms.custom: include file
ms.openlocfilehash: 6711506c1e489dcbd50aedd36241affc3bbed80b
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66113402"
---
### <a name="policy-for-users-in-your-directory"></a>Directiva: Para los usuarios del directorio

Siga estos pasos si desea que la directiva para los usuarios y grupos en el directorio que pueden solicitar este paquete de acceso.

1. En el **a los usuarios pueden solicitar acceso** sección, seleccione **para los usuarios del directorio**.

1. En el **Seleccionar usuarios y grupos** sección, haga clic en **agregar usuarios y grupos**.

1. En el panel de grupos y seleccione los usuarios, seleccione los usuarios y grupos que desea agregar.

    ![Paquete de acceso - selección de directiva de usuarios y grupos](./media/active-directory-entitlement-management-policy/policy-select-users-groups.png)

1. Haga clic en **seleccione** para agregar los usuarios y grupos.

1. Pasar a la [directiva: Solicitar](#policy-request) sección.

### <a name="policy-for-users-not-in-your-directory"></a>Directiva: Para los usuarios no en el directorio

Siga estos pasos si desea que la directiva para los usuarios no en el directorio que pueden solicitar este paquete de acceso. Los directorios deben configurarse para que se permiten en el **restricciones de colaboración de relaciones de organización** configuración.

> [!NOTE]
> Se creará una cuenta de usuario invitado para un usuario todavía no estén en el directorio cuya solicitud está aprobada o aprobado automáticamente. El invitado recibirán una invitación, pero no recibirá un correo electrónico de invitación. En su lugar, recibirán un correo electrónico cuando se entrega su asignación de paquetes de acceso. De forma predeterminada, más adelante cuando ese usuario invitado ya no tiene las asignaciones de paquete de acceso, porque su última asignación ha expirado o se han cancelado y que se impide el inicio de sesión y se eliminará la cuenta de usuario de invitado. Si desea tener los usuarios invitados permanecen en el directorio de manera indefinida, incluso si tienen que no tiene asignaciones de paquete de acceso, puede cambiar la configuración para la configuración de administración de derechos.

1. En el **a los usuarios pueden solicitar acceso** sección, seleccione **para los usuarios no en el directorio**.

1. En el **seleccione externo de Azure AD directory** sección, haga clic en **agregar directorios**.

1. Escriba un nombre de dominio y busque externo directorio de Azure AD.

1. Compruebe que es el directorio correcto por el nombre de directorio proporcionado y el dominio inicial.

    > [!NOTE]
    > Todos los usuarios del directorio podrán solicitar este paquete de acceso. Esto incluye usuarios de todos los subdominios asociados con el directorio, no solo el dominio utilizado en la búsqueda.

    ![Paquete de acceso - directorios de la directiva de selección](./media/active-directory-entitlement-management-policy/policy-select-directories.png)

1. Haga clic en **agregar** para agregar el directorio.

1. Repita este paso para agregar cualquier más directorios.

1. Cuando haya agregado todos los directorios que le gustaría incluir en la directiva, haga clic en **seleccione**.

1. Pasar a la [directiva: Solicitar](#policy-request) sección.

### <a name="policy-none-administrator-direct-assignments-only"></a>Directiva: Ninguno (sólo en la asignaciones directas de administrador)

Siga estos pasos si desea que la directiva para omitir las solicitudes de acceso y permiten a los administradores asignar directamente a usuarios específicos para el paquete de acceso. Los usuarios no tendrán que solicitar el acceso de paquete. Todavía se pueden establecer valores de expiración, pero no hay ninguna configuración de la solicitud.

1. En el **a los usuarios pueden solicitar acceso** sección, seleccione **ninguno (sólo asignaciones directas de administrador**.

    Después de crear el paquete de acceso, puede asignar directamente usuarios internos y externos específicos para el paquete de acceso. Si especifica un usuario externo, se creará una cuenta de usuario invitado en su directorio.

1. Pasar a la [directiva: Expiración](#policy-expiration) sección.

### <a name="policy-request"></a>Directiva: Solicitar

En la sección de la solicitud, especifique la configuración de aprobación cuando los usuarios solicitan el paquete de acceso.

1. Para requerir aprobación para las solicitudes de los usuarios seleccionados, establezca el **requieren aprobación** alternar a **Sí**. Para las solicitudes que se aprueban automáticamente, establezca el botón de alternancia en **No**.

1. Si necesita aprobación, en el **seleccionar aprobadores** sección, haga clic en **agregar aprobadores**.

1. En el panel seleccionar aprobadores, seleccione uno o varios usuarios o grupos como aprobadores.

    Solo uno de los aprobadores seleccionados debe aprobar una solicitud. No se requiere la aprobación de todos los aprobadores. La decisión de aprobación se basa en cualquier aprobador revisa la solicitud en primer lugar.

    ![Paquete de acceso - directiva - seleccionar aprobadores](./media/active-directory-entitlement-management-policy/policy-select-approvers.png)

1. Haga clic en **seleccione** para agregar los aprobadores.

1. Haga clic en **Mostrar configuración de la solicitud avanzada** para mostrar la configuración adicional.

    ![Paquete de acceso - directorios de la directiva de selección](./media/active-directory-entitlement-management-policy/policy-advanced-request.png)

1. Para requerir que los usuarios proporcionen una justificación para solicitar el acceso de paquete, establezca **requerir justificación** a **Sí**.

1. Para requerir que el aprobador proporcionar una justificación para aprobar una solicitud para el paquete de acceso, establezca **requieren la justificación del aprobador** a **Sí**.

1. En el **tiempo de espera de solicitud de aprobación (días)** , especifique la cantidad de tiempo que los aprobadores deben revisar una solicitud. Si no hay aprobadores revisión en este número de días, caduca la solicitud y el usuario deberá enviar otra solicitud para el paquete de acceso.

### <a name="policy-expiration"></a>Directiva: Expiración

En la sección de expiración, especifique cuándo expira la asignación de un usuario para el paquete de acceso.

1. En el **caducidad** sección, establezca **paquete acceso caduca** a **en fecha**, **número de días**, o **nunca**.

    Para **en fecha**, seleccione una fecha de expiración en el futuro.

    Para **número de días**, especifique un número entre 0 y 3660 días.

    Según su selección, asignación de un usuario para el paquete de acceso expire en una fecha concreta, un número determinado de días después de que se aprueban, o nunca.

1. Haga clic en **mostrar avanzada de configuración de expiración** para mostrar la configuración adicional.

1. Para permitir a ampliar sus asignaciones de usuario, establezca **permiten a los usuarios ampliar el acceso** a **Sí**.

    Si se permiten las extensiones en la directiva, el usuario recibirá un correo electrónico 14 días y también 1 día antes de su asignación de paquetes de acceso está configurado para expirar solicitándoles que ampliar la asignación.

    ![Paquete de acceso - configuración de directiva de expiración](./media/active-directory-entitlement-management-policy/policy-expiration.png)

### <a name="policy-enable-policy"></a>Directiva: Habilitar directiva

1. Si desea que el paquete de acceso estén inmediatamente disponibles para los usuarios de la directiva, haga clic en **Sí** para habilitar la directiva.

    Puede habilitarla siempre en el futuro cuando haya terminado de crear el paquete de acceso.

    ![Paquete de acceso - habilitar Directiva de configuración de directiva](./media/active-directory-entitlement-management-policy/policy-enable.png)

1. Haga clic en **siguiente** o **crear**.
