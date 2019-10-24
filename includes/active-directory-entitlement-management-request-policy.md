---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
author: msaburnley
ms.service: active-directory
ms.topic: include
ms.date: 10/15/2019
ms.author: ajburnle
ms.custom: include file
ms.openlocfilehash: aba3dd34383edbfb555adcc2063e4a2b76af1959
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72389298"
---
## <a name="for-users-in-your-directory"></a>para los usuarios del directorio

Siga estos pasos si quiere que los usuarios que están en el directorio puedan solicitar este paquete de acceso. Al definir la directiva de solicitud, puede especificar usuarios individuales o, más comúnmente, grupos de usuarios. Por ejemplo, es posible que su organización ya tenga un grupo, como **Todos los empleados**.  Si ese grupo se agrega a la directiva para los usuarios que pueden solicitar acceso, cualquier miembro de ese grupo podrá solicitar acceso.

1. En la sección **Usuarios que pueden solicitar acceso**, seleccione **Para los usuarios que están en su directorio**.

    Cuando se selecciona esta opción, aparecen nuevas opciones para restringir aún más quién en el directorio puede solicitar este paquete de acceso.

    ![Paquete de acceso - Solicitudes - Para los usuarios que están en el directorio](./media/active-directory-entitlement-management-request-policy/for-users-in-your-directory.png)

1. Seleccione una de las siguientes opciones:

    |  |  |
    | --- | --- |
    | **Usuarios y grupos específicos** | Elija esta opción si desea que solo los usuarios y grupos del directorio que especifique puedan solicitar este paquete de acceso. |
    | **Todos los miembros (excepto invitados)** | Elija esta opción si desea que todos los usuarios miembros del directorio puedan solicitar este paquete de acceso. Esta opción no incluye ningún usuario invitado al que pueda haber invitado en su directorio. |
    | **Todos los usuarios (incluidos invitados)** | Elija esta opción si desea que todos los usuarios miembros y los usuarios invitados del directorio puedan solicitar este paquete de acceso. |

    Los usuarios invitados hacen referencia a usuarios externos que han sido invitados al directorio con [Azure AD B2B](../articles/active-directory/b2b/what-is-b2b.md). Para más información sobre las diferencias entre los usuarios miembros y los usuarios invitados, consulte [¿Cuales son los permisos de usuario predeterminados en Azure Active Directory?](../articles/active-directory/fundamentals/users-default-permissions.md).

1. Si seleccionó **Usuarios y grupos específicos**, haga clic en **Agregar usuarios y grupos**.

1. En el panel Seleccionar usuarios y grupos, seleccione los usuarios y grupos que quiera agregar.

    ![Paquete de acceso - Solicitudes - Seleccionar usuarios y grupos](./media/active-directory-entitlement-management-request-policy/select-users-groups.png)

1. Haga clic en **Seleccionar** para agregar los usuarios y grupos.

1. Vaya a la sección [Aprobación](#approval).

## <a name="for-users-not-in-your-directory"></a>para los usuarios que no están en el directorio

Siga estos pasos si quiere que los usuarios que no están en el directorio puedan solicitar este paquete de acceso. Los **usuarios que no están en el directorio** hacen referencia a los usuarios que se encuentran en otro directorio de Azure AD y que posiblemente aún no han sido invitados al directorio. Los directorios de Azure AD deben configurarse para permitir invitaciones en **Restricciones de colaboración**. Para más información, consulte [Habilitación de la colaboración externa B2B y administración de quién puede invitar a otros usuarios](../articles/active-directory/b2b/delegate-invitations.md).

> [!NOTE]
> Se creará una cuenta de usuario invitado para un usuario que aún no esté en el directorio cuya solicitud se haya aprobado o aprobado automáticamente. Se invitará al invitado, pero no recibirá un correo electrónico de invitación. En su lugar, recibirá un correo electrónico cuando se entregue su asignación de paquete de acceso. De forma predeterminada, más adelante cuando ese usuario invitado ya no tenga ninguna asignación de paquete de acceso porque su última asignación ha expirado o se ha cancelado, se bloqueará el inicio de sesión de esa cuenta de usuario invitado y se eliminará posteriormente. Si quiere que los usuarios invitados permanezcan en el directorio de manera indefinida, incluso si no tienen ninguna asignación de paquete de acceso, puede cambiar la configuración de la administración de derechos. Para más información acerca del objeto de usuario de invitado, consulte [Propiedades de un usuario de colaboración B2B de Azure Active Directory](../articles/active-directory/b2b/user-properties.md).

1. En la sección **Usuarios que pueden solicitar acceso**, seleccione **Para los usuarios que están en su directorio**.

    Cuando se selecciona esta opción, aparecen nuevas opciones.

    ![Paquete de acceso - Solicitudes - Para los usuarios que no están en el directorio](./media/active-directory-entitlement-management-request-policy/for-users-not-in-your-directory.png)

1. Seleccione una de las siguientes opciones:

    |  |  |
    | --- | --- |
    | **Organizaciones conectadas específicas** | Elija esta opción si desea seleccionar de una lista de organizaciones que el administrador haya agregado anteriormente. Todos los usuarios de las organizaciones seleccionadas podrán solicitar este paquete de acceso. |
    | **Todas las organizaciones conectadas** | Elija esta opción si desea que todos los usuarios de las organizaciones conectadas puedan solicitar este paquete de acceso. |

    Una organización conectada es un dominio o directorio externo de Azure AD con el que colabore con frecuencia.

1. Si seleccionó **Organizaciones conectadas específicas**, haga clic en **Agregar directorios** para seleccionar en una lista de las organizaciones conectadas que el administrador haya agregado anteriormente.

1. Escriba un nombre de dominio para buscar una organización conectada con ese nombre de dominio.

    ![Paquete de acceso - Solicitudes - Seleccionar directorios](./media/active-directory-entitlement-management-request-policy/select-directories.png)

    Si la organización con la que desea colaborar no está en la lista, puede pedir al administrador que la agregue como una organización conectada. 

1. Una vez que haya seleccionado todas las organizaciones conectadas, haga clic en **Seleccionar**.

    > [!NOTE]
    > Todos los usuarios de las organizaciones conectadas seleccionadas podrán solicitar este paquete de acceso. Esto incluye a los usuarios de todos los subdominios asociados a las organizaciones conectadas.

1. Vaya a la sección [Aprobación](#approval).

## <a name="none-administrator-direct-assignments-only"></a>ninguno (solo para las asignaciones directas del administrador)

Siga estos pasos si quiere omitir las solicitudes de acceso y permitir a los administradores asignar directamente a usuarios específicos al paquete de acceso. Los usuarios no tendrán que solicitar el paquete de acceso. Aún podrá establecer la configuración del ciclo de vida, pero no hay ninguna configuración de solicitud.

1. En la sección **Usuarios que pueden solicitar acceso**, seleccione **Ninguno (solo para las asignaciones directas del administrador)** .

    ![Paquete de acceso - Solicitudes - Ninguno (solo para las asignaciones directas del administrador)](./media/active-directory-entitlement-management-request-policy/none-admin-direct-assignments-only.png)

    Después de crear el paquete de acceso, puede asignar directamente usuarios internos y externos específicos al paquete de acceso. Si especifica un usuario externo, se creará una cuenta de usuario invitado en su directorio. Para información sobre la asignación directa de un usuario, vea [View y cambiar las asignaciones de un paquete de acceso ](../articles/active-directory/governance/entitlement-management-access-package-assignments.md).

1. Vaya a la sección [Habilitación de solicitudes](#enable-requests).

## <a name="approval"></a>Aprobación

En la sección Aprobación, especifique si se requiere una aprobación cuando los usuarios soliciten este paquete de acceso. La configuración de aprobación funciona de la siguiente manera:

- Una solicitud solo debe aprobarla uno de los aprobadores seleccionados o un aprobador de reserva. No se requiere la aprobación de todos los aprobadores.
- La decisión de aprobación se basa en el aprobador que revise primero la solicitud.

1. Para requerir la aprobación de las solicitudes de los usuarios seleccionados, establezca el botón de alternancia **Requerir aprobación** en **Sí**. Para que las solicitudes se aprueban automáticamente, establezca el botón de alternancia en **No**.

    ![Acceder a paquete - Solicitudes - Configuración de aprobación](./media/active-directory-entitlement-management-request-policy/approval.png)

1. Para requerir que los usuarios proporcionen una justificación para solicitar el paquete de acceso, establezca la opción **R** en **Sí**.

1. Determine si la solicitud necesitará una aprobación única o en varias fases. Establezca la opción **	Número de fases** en **1**  para una sola fase.

1. En el caso de los aprobadores, seleccione **Administrador como aprobador** o **Elegir aprobadores específicos**.

    El administrador viene determinado por el atributo **Administrador** en el perfil del usuario de Azure AD. Para más información, consulte [Incorporación o actualización de la información de perfil de un usuario mediante Azure Active Directory](../articles/active-directory/fundamentals/active-directory-users-profile-azure-portal.md).

    ![Perfil de usuario de Azure Active Directory: atributo Administrador](./media/active-directory-entitlement-management-request-policy/profile-manager.png)

1. Si seleccionó Administrador como aprobador, haga clic en **Agregar reserva** para seleccionar uno o varios usuarios o grupos del directorio para que sean un aprobador de reserva en caso de que la administración de derechos no pueda encontrar el administrador.

1. Si seleccionó Elegir aprobadores específicos, haga clic en **Agregar aprobadores** para seleccionar uno o varios usuarios o grupos del directorio para que sean aprobadores.

1. En **¿Dentro de cuántos días es necesario tomar una decisión?** , especifique el número de días que un aprobador tiene que revisar una solicitud para este paquete de acceso.

    Si no se aprueba una solicitud durante este período de tiempo, se rechazará automáticamente. El usuario tendrá que enviar otra solicitud para el paquete de acceso.

1. Para requerir que los usuarios proporcionen una justificación para solicitar el paquete de acceso, establezca **Requerir justificación** en **Sí**.

    Una justificación es visible para otros aprobadores y el solicitante.

## <a name="enable-requests"></a>Habilitación de solicitudes

1. Si quiere que el paquete de acceso esté disponible de inmediato para los usuarios de la directiva de solicitud para que puedan solicitarlo, haga clic en **Sí** para habilitarla.

    Podrá habilitarla en todo momento cuando haya terminado de crear el paquete de acceso.

    Si seleccionó **None (solo asignaciones directas de administrador)**  y establece la habilitación en **No** , los administradores no podrán asignar directamente este paquete de acceso.

    ![Paquete de acceso: directiva; configuración de habilitación de la directiva](./media/active-directory-entitlement-management-request-policy/enable-requests.png)

1. Haga clic en **Next**.
