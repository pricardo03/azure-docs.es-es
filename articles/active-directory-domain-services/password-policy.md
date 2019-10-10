---
title: Creación y uso de directivas de contraseña en Azure AD Domain Services | Microsoft Docs
description: Aprenda cómo y por qué usar directivas de contraseña específicas para proteger y controlar las contraseñas de cuenta en un dominio administrado de Azure AD DS.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 1a14637e-b3d0-4fd9-ba7a-576b8df62ff2
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: article
ms.date: 08/08/2019
ms.author: iainfou
ms.openlocfilehash: 19a618bd576687fcb0d92f8e35613e4cdc749e70
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71320454"
---
# <a name="password-and-account-lockout-policies-on-managed-domains"></a>Directivas de bloqueo de cuenta y contraseña en dominios administrados

Para administrar la seguridad de la cuenta en Azure Active Directory Domain Services (Azure AD DS), puede definir directivas de contraseña específicas que controlen la configuración como, por ejemplo, la longitud mínima, la hora de expiración o la complejidad de la contraseña. Se aplica una directiva de contraseñas predeterminada a todos los usuarios de un dominio administrado de Azure AD DS. Para proporcionar un control pormenorizado y satisfacer las necesidades específicas de la empresa o las de cumplimiento, se pueden crear y aplicar directivas adicionales a grupos de usuarios específicos.

En este artículo se muestra cómo crear y configurar una directiva de contraseñas específica mediante el Centro de administración de Active Directory.

## <a name="before-you-begin"></a>Antes de empezar

Para completar este artículo, necesitará los siguientes recursos y privilegios:

* Una suscripción de Azure activa.
  * Si no tiene una suscripción a Azure, [cree una cuenta](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Un inquilino de Azure Active Directory asociado a su suscripción, ya sea sincronizado con un directorio en el entorno local o con un directorio solo en la nube.
  * Si es necesario, [cree un inquilino de Azure Active Directory][create-azure-ad-tenant] o [asocie una suscripción a Azure con su cuenta][associate-azure-ad-tenant].
* Un dominio administrado de Azure Active Directory Domain Services habilitado y configurado en su inquilino de Azure AD.
  * Si es necesario, complete el tutorial para [crear y configurar una instancia de Azure Active Directory Domain Services][create-azure-ad-ds-instance].
* Una máquina virtual de administración de Windows Server que esté unida al dominio administrado de Azure AD DS.
  * Si es necesario, complete el tutorial para [crear una máquina virtual de administración][tutorial-create-management-vm].
* Una cuenta de usuario que sea miembro del grupo de *administradores de Azure AD DC* en el inquilino de Azure AD.

## <a name="fine-grained-password-policies-fgpp-overview"></a>Introducción a las directivas de contraseñas específicas (FGPP)

Las directivas de contraseñas específicas (FGPP) le permiten aplicar restricciones específicas de directivas de bloqueo de cuentas y contraseñas a diferentes usuarios de un dominio. Por ejemplo, para proteger las cuentas con privilegios, puede aplicar una configuración de contraseña más estricta que para las cuentas normales sin privilegios. Puede crear varias FGPP para especificar directivas de contraseñas en un dominio administrado de Azure AD DS.

Con FGPP puede configurar las siguientes opciones de contraseña:

* Longitud mínima de contraseña
* Historial de contraseñas
* Las contraseñas deben satisfacer los requisitos de complejidad
* Antigüedad mínima de contraseña
* Antigüedad máxima de contraseña
* Directiva de bloqueo de cuentas
  * Duración de bloqueo de cuenta
  * Número de intentos de inicio de sesión con error permitidos
  * Restablecer el número de intentos de inicio de sesión con error después de

FGPP solo afecta a los usuarios creados en Azure AD DS. Los usuarios de la nube y los usuarios del dominio sincronizados en el dominio administrado de Azure AD DS desde Azure AD no se ven afectados por las directivas de contraseñas.

Las directivas se distribuyen, mediante la asociación de grupos, en el dominio administrado de Azure AD DS y los cambios que realice se aplicarán en el siguiente inicio de sesión de usuario. Al cambiar la directiva, no se desbloquea una cuenta de usuario que ya esté bloqueada.

## <a name="default-fine-grained-password-policy-settings"></a>Configuración de la directiva de contraseñas específica predeterminada

En un dominio administrado de Azure AD DS, las siguientes directivas de contraseñas están configuradas de forma predeterminada y se aplican a todos los usuarios:

* **Longitud mínima de contraseña (caracteres):** 7
* **Antigüedad máxima de contraseña (duración):** 90 días
* **Las contraseñas deben satisfacer los requisitos de complejidad**

Las siguientes directivas de bloqueo de cuenta están configuradas de forma predeterminada:

* **Duración del bloqueo de cuenta:** 30
* **Número de intentos de inicio de sesión con error permitidos:** 5
* **Restablecer el número de intentos de inicio de sesión con error después de:** 30 minutos

Con esta configuración predeterminada, las cuentas de usuario se bloquean durante 30 minutos si se usan cinco contraseñas no válidas en dos minutos. Las cuentas se desbloquean automáticamente pasados 30 minutos.

No se puede modificar ni eliminar la directiva de contraseñas específica integrada predeterminada. Los miembros del grupo *Administradores de controladores de dominio de AAD* pueden crear una FGPP personalizada y configurarla para invalidar (tener preferencia sobre) la FGPP integrada predeterminada, como se indica en la siguiente sección.

## <a name="create-a-custom-fine-grained-password-policy"></a>Creación de una directiva de contraseñas específica personalizada

Cuando compile aplicaciones en Azure, puede que desee configurar una FGPP personalizada. Entre los ejemplos de la necesidad de crear una FGPP personalizada se incluye establecer una directiva de bloqueo de cuenta diferente o configurar un valor de duración de contraseña predeterminado para el dominio administrado.

Puede crear una FGPP personalizada y aplicarla a grupos específicos del dominio administrado de Azure AD DS. Esta configuración invalida eficazmente la FGPP predeterminada. También puede crear directivas de contraseñas específicas personalizadas y aplicarlas a cualquier unidad organizativa personalizada que cree en el dominio administrado de Azure AD DS.

Para crear una directiva de contraseñas específica personalizada, use las herramientas administrativas de Active Directory desde una máquina virtual unida a un dominio. El Centro de administración de Active Directory le permite ver, editar y crear recursos en un dominio administrado de Azure AD DS, incluidas las unidades organizativas.

> [!NOTE]
> Para crear una directiva de contraseñas específica personalizada en un dominio administrado de Azure AD DS, debe haber iniciado sesión en una cuenta de usuario que sea miembro del grupo *Administradores del controlador de dominio de AAD*.

1. En la pantalla Inicio, seleccione **Herramientas administrativas**. Se muestra una lista de las herramientas de administración disponibles que se instalaron en el tutorial para [crear una máquina virtual de administración][tutorial-create-management-vm].
1. Para crear y administrar unidades organizativas, seleccione **Centro de administración de Active Directory** de la lista de herramientas administrativas.
1. En el panel izquierdo, elija el dominio administrado de Azure AD DS como, por ejemplo, *contoso.com*.
1. Abra el contenedor **Sistema** y el contenedor **Configuración de contraseña**.

    Aparecerá una directiva FGPP integrada para el dominio administrado de Azure AD DS. No se puede modificar esta directiva FGPP integrada. Sin embargo, puede crear una nueva directiva FGPP personalizada que invalide la directiva predeterminada.
1. En el panel **Tareas** de la derecha, seleccione **Nueva > Configuración de contraseña**.
1. En el cuadro de diálogo **Crear configuración de contraseña**, escriba un nombre para la directiva como *MyCustomFGPP*. Establezca la prioridad de forma adecuada para invalidar la directiva FGPP predeterminada (que es *200*). Establézcala, por ejemplo, en *1*.

    Edite otras opciones de la directiva de contraseñas como desee, como la opción **Exigir historial de contraseñas** para solicitar al usuario que cree una contraseña que sea diferente de las *24* contraseñas anteriores.

    ![Creación de una directiva de contraseñas específica personalizada](./media/how-to/custom-fgpp.png)

1. Desactive **Proteger contra eliminación accidental**. Si esta opción está seleccionada, la directiva FGPP no se podrá guardar.
1. En la sección **Se aplica directamente a**, seleccione el botón **Agregar**. En el cuadro de diálogo **Select Users or Groups** (Seleccionar usuarios y grupos) haga clic en el botón **Locations** (Ubicaciones).

    ![Seleccionar los usuarios y grupos a los que se va a aplicar la directiva de contraseñas](./media/how-to/fgpp-applies-to.png)

1. Las directivas de contraseñas específicas solo se pueden aplicar a grupos. En el cuadro de diálogo **Ubicaciones**, expanda el nombre de dominio, por ejemplo, *contoso.com* y, a continuación, seleccione una unidad organizativa como **Usuarios del controlador de dominio de AAD**. Si tiene una unidad organizativa personalizada que contiene un grupo de usuarios al que desea aplicar la directiva, seleccione esa unidad organizativa.

    ![Seleccionar la unidad organizativa a la que pertenece ese grupo](./media/how-to/fgpp-container.png)

1. Escriba el nombre del grupo al que desea aplicar la directiva y, después, seleccione **Comprobar nombres** para validar que el grupo existe.

    ![Buscar y seleccionar el grupo al que aplicar la FGPP](./media/how-to/fgpp-apply-group.png)

1. Cuando ya aparezca el nombre del grupo que seleccionó en la sección **Se aplica directamente a**, seleccione **Aceptar** para guardar la directiva de contraseñas personalizada.

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de las directivas de contraseñas específicas y el uso del centro de administración de Active Directory, consulte los siguientes artículos:

* [Más información sobre las directivas de contraseñas específicas](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770394(v=ws.10))
* [Configuración de directivas de contraseñas específicas mediante el Centro de administración de AD](/windows-server/identity/ad-ds/get-started/adac/introduction-to-active-directory-administrative-center-enhancements--level-100-#fine_grained_pswd_policy_mgmt)

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[tutorial-create-management-vm]: tutorial-create-management-vm.md
