---
title: 'Incorporación de una suscripción de Azure existente al inquilino: Azure Active Directory | Microsoft Docs'
description: Instrucciones sobre cómo agregar una suscripción de Azure existente al inquilino de Azure Active Directory.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3322e49c6fdc590b785806f67b5081700bf8b37b
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59788640"
---
# <a name="associate-or-add-an-azure-subscription-to-your-azure-active-directory-tenant"></a>Asociación o incorporación de una suscripción de Azure al inquilino de Azure Active Directory

Una suscripción de Azure tiene una relación de confianza con Azure Active Directory (Azure AD), lo que significa que confía en la suscripción de Azure AD para autenticar usuarios, servicios y dispositivos. Varias suscripciones pueden confiar en el mismo directorio de Azure AD, pero cada suscripción solo confiará en un único directorio.

Si su suscripción expira, se pierde el acceso a los otros recursos asociados a la suscripción. Sin embargo, el directorio de Azure AD permanece en Azure, lo que le permite asociarlo y administrarlo con otra suscripción de Azure.

Todos los usuarios tienen una sola *principal* directorio para la autenticación. Sin embargo, los usuarios también pueden ser invitados en otros directorios. Puede ver los directorios principales e invitados para cada usuario en Azure AD.

> [!Important]
> Al asociar una suscripción a un directorio diferente, los usuarios que tienen roles asignados mediante [control de acceso basado en roles (RBAC)](../../role-based-access-control/role-assignments-portal.md) perderán el acceso. Los administradores de suscripciones clásico (Administrador de servicios y los coadministradores) también perderá el acceso.
> 
> Además, mover el clúster de Azure Kubernetes Service (AKS) a una suscripción diferente o mover la suscripción propietaria de clúster a un nuevo inquilino, hace que el clúster pierda funcionalidad debido a las asignaciones de roles perdido y derechos de entidades de seguridad de servicio. Para obtener más información acerca de AKS, consulte [Azure Kubernetes Service (AKS)](https://docs.microsoft.com/en-us/azure/aks/).

## <a name="before-you-begin"></a>Antes de empezar

Para poder asociar o agregar su suscripción, debe realizar las siguientes tareas:

1. Revise la siguiente lista de cambios y cómo pueden verse afectados:

    - Los usuarios que se han asignado los roles mediante RBAC perderán el acceso
    - Administrador de servicios y los coadministradores perderán el acceso
    - Si dispone de los almacenes de claves, que sean inaccesibles y tendrá que corregirlos después de la asociación
    - Si tiene un registrado Azure Stack, tendrá que volver a registrarla después de la asociación

1. Iniciar sesión con una cuenta que:
    - Tiene un [propietario](../../role-based-access-control/built-in-roles.md#owner) asignación de roles para la suscripción. Para obtener información sobre cómo asignar el rol de propietario, consulte [administrar el acceso a recursos de Azure mediante RBAC y Azure portal](../../role-based-access-control/role-assignments-portal.md).
    - Exista tanto en el directorio actual que está asociado con la suscripción como en el directorio nuevo con el que quiere asociar la suscripción más adelante. Para obtener más información sobre cómo obtener acceso a otro directorio, consulte [¿Cómo agregan los administradores de Azure Active Directory a usuarios de colaboración B2B?](../b2b/add-users-administrator.md).

1. Asegúrese de que no usa una suscripción de proveedores de servicios en la nube de Azure (CSP) (MS-AZR-0145P, MS-AZR-0146P, MS-AZR-159P), una suscripción interna de Microsoft (MS-AZR-0015P) o una suscripción a Microsoft Imagine (MS-AZR-0144P).
    
## <a name="to-associate-an-existing-subscription-to-your-azure-ad-directory"></a>Asociación de una suscripción existente al directorio de Azure AD

1. Inicie sesión y seleccione la suscripción que quiere usar en la [Página de suscripciones de Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

2. Seleccione **Cambiar directorio**.

    ![Página de suscripciones, con la opción Cambiar directorio resaltada](media/active-directory-how-subscriptions-associated-directory/change-directory-button.png)

3. Revise las advertencias que aparecen y, a continuación, seleccione **Cambiar**.

    ![Página de cambio de directorio que muestra el directorio al que se va a cambiar](media/active-directory-how-subscriptions-associated-directory/edit-directory-ui.png)

    Se cambia el directorio de la suscripción y se muestra un mensaje de confirmación.

    ![Mensaje de confirmación sobre el cambio de directorio](media/active-directory-how-subscriptions-associated-directory/edit-directory-success.png)    
4. Use la **modificador del directorio** para ir a su nuevo directorio. Puede tardar hasta 10 minutos para que todo se muestre correctamente.

    ![Página de modificador Directory, con información de ejemplo](media/active-directory-how-subscriptions-associated-directory/directory-switcher.png)

Cambiar el directorio de suscripción es una operación de nivel de servicio, por lo que no afecta a la propiedad de facturación de suscripción. El administrador de cuenta todavía puede cambiar al administrador de servicio desde el [centro de cuentas](https://account.azure.com/subscriptions). Para eliminar el directorio original, debe transferir la propiedad de facturación de suscripción a un nuevo administrador de cuenta. Para más información acerca de cómo transferir la propiedad de facturación, vea [Transferencia de la propiedad de una suscripción de Azure a otra cuenta](../../billing/billing-subscription-transfer.md).

## <a name="post-association-steps"></a>Pasos posteriores a la asociación
Después de asociar una suscripción a un directorio diferente, puede haber pasos adicionales que se deben realizar para reanudar las operaciones.

1. Si dispone de los almacenes de claves, debe cambiar el identificador del inquilino de almacén de claves. Para obtener más información, consulte [cambiar un identificador de inquilino de key vault después de mover una suscripción](../../key-vault/key-vault-subscription-move-fix.md).

2. Si ha registrado un usando esta suscripción de Azure Stack, debe volver a registrar. Para obtener más información, consulte [registrar Azure Stack con Azure](../../azure-stack/azure-stack-registration.md).



## <a name="next-steps"></a>Pasos siguientes

- Para crear un nuevo inquilino de Azure AD, consulte [Access Azure Active Directory to create a new tenant](active-directory-access-create-new-tenant.md) (Acceso a Azure Active Directory para crear un nuevo inquilino)

- Para más información acerca de cómo se controla el acceso a los recursos en Microsoft Azure, consulte [Descripción de acceso a los recursos de Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)

- Para obtener más información acerca de cómo asignar roles en Azure AD, vea [cómo asignar roles de directorio a los usuarios con Azure Active Directory](active-directory-users-assign-role-azure-portal.md)
