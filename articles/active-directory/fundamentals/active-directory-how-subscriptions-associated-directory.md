---
title: 'Incorporación de una suscripción de Azure existente al inquilino: Azure Active Directory | Microsoft Docs'
description: Instrucciones sobre cómo agregar una suscripción de Azure existente al inquilino de Azure Active Directory.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: a64bad11f5b83ddd7f6d7236ffed4ff4a6e39c2c
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68561870"
---
# <a name="associate-or-add-an-azure-subscription-to-your-azure-active-directory-tenant"></a>Asociación o incorporación de una suscripción de Azure al inquilino de Azure Active Directory

Una suscripción de Azure tiene una relación de confianza con Azure Active Directory (Azure AD), lo que significa que la suscripción confía en Azure AD para autenticar usuarios, servicios y dispositivos. Varias suscripciones pueden confiar en el mismo directorio de Azure AD, pero cada suscripción solo confiará en un único directorio.

Si su suscripción expira, se pierde el acceso a los otros recursos asociados a la suscripción. Sin embargo, el directorio de Azure AD permanece en Azure, lo que le permite asociarlo y administrarlo con otra suscripción de Azure.

Todos los usuarios tienen un único directorio *particular* para la autenticación. Sin embargo, los usuarios también pueden ser invitados en otros directorios. Puede ver los directorios principales e invitados para cada usuario en Azure AD.

> [!Important]
> Al asociar una suscripción a un directorio diferente, los usuarios que tengan roles asignados mediante el [control de acceso basado en rol (RBAC)](../../role-based-access-control/role-assignments-portal.md) perderán el acceso. Los administradores de suscripciones clásicas (el administrador y los coadministradores del servicio) también perderán el acceso.
> 
> Además, mover el clúster de Azure Kubernetes Service (AKS) a otra suscripción, o mover la suscripción propietaria del clúster a un nuevo inquilino, provoca que el clúster pierda funcionalidad debido a la pérdida de asignaciones de roles y derechos de entidades de servicio. Para obtener más información sobre AKS, consulte [Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/).

## <a name="before-you-begin"></a>Antes de empezar

Para poder asociar o agregar su suscripción, debe realizar las siguientes tareas:

1. Revise la siguiente lista de cambios y cómo le pueden afectar:

    - Los usuarios que tienen roles asignados mediante RBAC perderán el acceso.
    - El administrador y los coadministradores del servicio perderán el acceso.
    - Si tiene almacenes de claves, no se podrá acceder a ellos y tendrá que corregirlos después de la asociación.
    - Si tiene identidades administradas para recursos, como Virtual Machines o Logic Apps, tendrá que volver a habilitarlas o crearlas después de la asociación.
    - Si tiene una instancia de Azure Stack registrada, tendrá que volver a registrarla después de la asociación.

1. Iniciar sesión con una cuenta que:
    - Tiene una asignación de rol [Propietario](../../role-based-access-control/built-in-roles.md#owner) para la suscripción. Para obtener más información sobre cómo asignar el rol Propietario, consulte [Administración del acceso a los recursos de Azure mediante RBAC y Azure Portal](../../role-based-access-control/role-assignments-portal.md).
    - Exista tanto en el directorio actual que está asociado con la suscripción como en el directorio nuevo con el que quiere asociar la suscripción más adelante. Para obtener más información sobre cómo obtener acceso a otro directorio, consulte [¿Cómo agregan los administradores de Azure Active Directory a usuarios de colaboración B2B?](../b2b/add-users-administrator.md).

1. Asegúrese de que no usa una suscripción de proveedores de servicios en la nube de Azure (CSP) (MS-AZR-0145P, MS-AZR-0146P, MS-AZR-159P), una suscripción interna de Microsoft (MS-AZR-0015P) o una suscripción a Microsoft Imagine (MS-AZR-0144P).
    
## <a name="to-associate-an-existing-subscription-to-your-azure-ad-directory"></a>Asociación de una suscripción existente al directorio de Azure AD

1. Inicie sesión y seleccione la suscripción que quiere usar en la [Página de suscripciones de Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

2. Seleccione **Cambiar directorio**.

    ![Página de suscripciones, con la opción Cambiar directorio resaltada](media/active-directory-how-subscriptions-associated-directory/change-directory-button.png)

3. Revise las advertencias que aparecen y, a continuación, seleccione **Cambiar**.

    ![Página de cambio de directorio que muestra el directorio al que se va a cambiar](media/active-directory-how-subscriptions-associated-directory/edit-directory-ui.png)

    Se cambia el directorio de la suscripción y se muestra un mensaje de confirmación.

    ![Mensaje de operación correcta sobre el cambio de directorio](media/active-directory-how-subscriptions-associated-directory/edit-directory-success.png)
4. Use el **conmutador de directorios** para ir al nuevo directorio. Puede tardar varias horas hasta que todo se muestre correctamente. Si parece que el proceso tarda demasiado, asegúrese de comprobar que el **filtro de suscripción global** de la suscripción que ha movido no se haya ocultado.

    ![Página del conmutador de directorios con información de ejemplo](media/active-directory-how-subscriptions-associated-directory/directory-switcher.png)

Cambiar el directorio de suscripción es una operación de nivel de servicio, por lo que no afecta a la propiedad de facturación de suscripción. El administrador de cuenta todavía puede cambiar al administrador de servicio desde el [centro de cuentas](https://account.azure.com/subscriptions). Para eliminar el directorio original, debe transferir la propiedad de facturación de suscripción a un nuevo administrador de cuenta. Para más información acerca de cómo transferir la propiedad de facturación, vea [Transferencia de la propiedad de una suscripción de Azure a otra cuenta](../../billing/billing-subscription-transfer.md).

## <a name="post-association-steps"></a>Pasos posteriores a la asociación
Después de asociar una suscripción a un directorio distinto, puede que deba realizar pasos adicionales para reanudar las operaciones.

1. Si tiene almacenes de claves, debe cambiar el Id. de inquilino del almacén de claves. Para más información, consulte [Cambio del identificador de inquilino de Key Vault después de mover una suscripción](../../key-vault/key-vault-subscription-move-fix.md).

2. Si estaba usando identidades administradas asignadas por el sistema para recursos, debe volver a habilitarlas. Si estaba usando identidades administradas asignadas por el usuario, debe volver a crearlas. Después de volver a habilitar o crear las identidades administradas, debe volver a restablecer los permisos asignados a esas identidades. Para más información, consulte [¿Qué es Managed Identities for Azure Resources?](../managed-identities-azure-resources/overview.md).

3. Si ha registrado una instancia de Azure Stack que usa esta suscripción, debe volver a registrarla. Para obtener más información, consulte [Registro de Azure Stack con Azure](/azure-stack/operator/azure-stack-registration).



## <a name="next-steps"></a>Pasos siguientes

- Para crear un nuevo inquilino de Azure AD, consulte [Access Azure Active Directory to create a new tenant](active-directory-access-create-new-tenant.md) (Acceso a Azure Active Directory para crear un nuevo inquilino)

- Para más información acerca de cómo se controla el acceso a los recursos en Microsoft Azure, consulte [Descripción de acceso a los recursos de Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)

- Para obtener más información acerca de cómo asignar roles en Azure AD, vea [cómo asignar roles de directorio a los usuarios con Azure Active Directory](active-directory-users-assign-role-azure-portal.md)
