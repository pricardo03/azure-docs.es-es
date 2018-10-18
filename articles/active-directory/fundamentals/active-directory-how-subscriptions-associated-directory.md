---
title: Adición de una suscripción de Azure existente al inquilino de Azure Active Directory | Microsoft Docs
description: Obtenga información sobre cómo añadir una suscripción de Azure existente al inquilino de Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: dd62b22eca40a214c5b08a9bc48815e40fe90e47
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "45984093"
---
# <a name="how-to-associate-or-add-an-azure-subscription-to-azure-active-directory"></a>Asociación o adición de una suscripción de Azure a Azure Active Directory
La suscripción de Azure tiene una relación de confianza con Azure Active Directory (Azure AD), lo que significa que la suscripción confía en Azure AD para autenticar usuarios, servicios y dispositivos. Varias suscripciones pueden confiar en el mismo directorio de Azure AD, pero cada suscripción solo confiará en un único directorio.

Si su suscripción expira, se pierde el acceso a los otros recursos asociados a la suscripción. Sin embargo, el directorio de Azure AD permanece en Azure, lo que le permite asociarlo y administrarlo con otra suscripción de Azure.

Todos los usuarios tienen un único directorio "principal" para la autenticación. Sin embargo, los usuarios también pueden ser invitados en otros directorios. Puede ver los directorios principales e invitados para cada usuario en Azure AD.

>[!Important]
>Todos los usuarios del [control de acceso basado en rol (RBAC)](../../role-based-access-control/role-assignments-portal.md) con acceso asignado, junto con todos los administradores de suscripción, pierden el acceso cuando se cambia el directorio de la suscripción. Además, si cuenta con almacenes de claves, también se verán afectados por el traslado de suscripción. Para solucionarlo, debe [cambiar el id. de inquilino del almacén de claves](../../key-vault/key-vault-subscription-move-fix.md) antes de reanudar las operaciones.


## <a name="before-you-begin"></a>Antes de empezar
Para poder asociar o agregar su suscripción, debe realizar las siguientes tareas:

- Iniciar sesión con una cuenta que:
    - Tenga acceso de **propietario de RBAC** a la suscripción.

    - Exista tanto en el directorio actual que está asociado con la suscripción como en el directorio nuevo con el que quiere asociar la suscripción más adelante. Para obtener más información sobre cómo obtener acceso a otro directorio, consulte [¿Cómo agregan los administradores de Azure Active Directory a usuarios de colaboración B2B?](../b2b/add-users-administrator.md).

- Asegúrese de que no usa una suscripción de proveedores de servicios en la nube de Azure (CSP) (MS-AZR-0145P, MS-AZR-0146P, MS-AZR-159P), una suscripción interna de Microsoft (MS-AZR-0015P) o una suscripción a Microsoft Imagine (MS-AZR-0144P).
    
## <a name="to-associate-an-existing-subscription-to-your-azure-ad-directory"></a>Asociación de una suscripción existente al directorio de Azure AD
1. Inicie sesión y seleccione la suscripción que quiere usar en la [Página de suscripciones de Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

2. Seleccione **Cambiar directorio**.

    ![Página de suscripciones, con la opción Cambiar directorio resaltada](media/active-directory-how-subscriptions-associated-directory/change-directory-button.png)

3. Revise las advertencias que aparecen y, a continuación, seleccione **Cambiar**.

    ![Página de cambio de directorio que muestra el directorio al que se va a cambiar](media/active-directory-how-subscriptions-associated-directory/edit-directory-ui.png)

    Se cambia el directorio de la suscripción y se muestra un mensaje de confirmación.

    ![Mensaje de operación correcta](media/active-directory-how-subscriptions-associated-directory/edit-directory-success.png)    

4. Utilice el modificador del directorio para ir a su directorio nuevo. Puede tardar hasta 10 minutos para que todo se muestre correctamente.

    ![Página del modificador de directorio](media/active-directory-how-subscriptions-associated-directory/directory-switcher.png)

Cambiar el directorio de suscripción es una operación de nivel de servicio, por lo que no afecta a la propiedad de facturación de suscripción. El administrador de cuenta todavía puede cambiar al administrador de servicio desde el [centro de cuentas](https://account.azure.com/subscriptions). Para eliminar el directorio original, debe transferir la propiedad de facturación de suscripción a un nuevo administrador de cuenta. Para más información acerca de cómo transferir la propiedad de facturación, vea [Transferencia de la propiedad de una suscripción de Azure a otra cuenta](../../billing/billing-subscription-transfer.md). 

## <a name="next-steps"></a>Pasos siguientes

- Para crear un nuevo inquilino de Azure AD, consulte [Access Azure Active Directory to create a new tenant](active-directory-access-create-new-tenant.md) (Acceso a Azure Active Directory para crear un nuevo inquilino)

- Para más información acerca de cómo se controla el acceso a los recursos en Microsoft Azure, consulte [Descripción de acceso a los recursos de Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)

- Para obtener más información acerca de cómo asignar roles en Azure AD, vea [cómo asignar roles de directorio a los usuarios con Azure Active Directory](active-directory-users-assign-role-azure-portal.md)
