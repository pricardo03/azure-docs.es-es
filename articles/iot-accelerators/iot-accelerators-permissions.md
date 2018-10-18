---
title: Aceleradores de soluciones de Azure IoT y Azure Active Directory | Microsoft Docs
description: Describe la forma en que los aceleradores de soluciones de Azure IoT usan Azure Active Directory para administrar permisos.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 11/10/2017
ms.author: dobett
ms.openlocfilehash: e45954389c8dd1b484a7009460c541bf35266973
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2018
ms.locfileid: "44713857"
---
# <a name="permissions-on-the-azureiotsolutionscom-site"></a>Permisos en el sitio azureiotsolutions.com

## <a name="what-happens-when-you-sign-in"></a>Qué ocurre al iniciar sesión

La primera vez que inicie sesión en [azureiotsuite.com][lnk-azureiotsolutions], el sitio determinará los niveles de permiso que tiene según el inquilino de Azure Active Directory (AAD) seleccionado actualmente y la suscripción de Azure.

1. En primer lugar, para rellenar la lista de los inquilinos que aparece junto al nombre de usuario, el sitio descubre en Azure a qué inquilinos de AAD pertenece el usuario. Actualmente, el sitio no puede obtener tokens de usuario para más de un solo inquilino a la vez. Por tanto, al cambiar inquilinos mediante la lista desplegable de la esquina superior derecha, el sitio inicia si sesión en dicho inquilino para obtener los tokens de este.

2. A continuación, el sitio descubre en Azure qué suscripciones ha asociado al inquilino seleccionado. Las suscripciones disponibles se ven al crear un nuevo acelerador de soluciones.

3. Por último, el sitio recupera todos los recursos de las suscripciones y los grupos de recursos etiquetados como aceleradores de soluciones y rellena los iconos de la página principal.

En las secciones siguientes se describen los roles que controlan el acceso a los aceleradores de soluciones.

## <a name="aad-roles"></a>Roles de AAD

Los roles de AAD controlan la capacidad de aprovisionar aceleradores de soluciones, para administrar usuarios y varios servicios de Azure en un acelerador de soluciones.

Puede encontrar más información sobre los roles del administrador en AAD en [Asignación de roles de administrador en Azure Active Directory][lnk-aad-admin]. El artículo actual se centra en los roles de **administrador global** y de directorio de **usuario** que utilizan los aceleradores de soluciones.

### <a name="global-administrator"></a>Administrador global

Puede haber muchos administradores globales por inquilino de AAD:

* Al crear un inquilino de AAD, quien lo crea es de forma predeterminada el administrador global del mismo.
* El administrador global puede aprovisionar un acelerador de soluciones básico y estándar (una implementación básica usa una única máquina virtual Azure).

### <a name="domain-user"></a>Usuario de dominio

Puede haber muchos usuarios de dominio por inquilino de AAD:

* Un usuario de dominio puede aprovisionar un acelerador de soluciones básico mediante el sitio [azureiotsolutions.com][lnk-azureiotsolutions].
* Un usuario de dominio puede crear un acelerador de soluciones básico mediante la CLI.

### <a name="guest-user"></a>Usuario invitado

Puede haber muchos usuarios invitados por inquilino de AAD. Los usuarios invitados tienen un conjunto de derechos limitado en el inquilino de AAD. Como consecuencia, los usuarios invitados no pueden aprovisionar un acelerador de soluciones en el inquilino de AAD.

Para más información acerca de los usuarios y roles de AAD, consulte estos recursos:

* [Creación de usuarios en Azure AD][lnk-create-edit-users]
* [Asignación de usuarios a aplicaciones][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Roles de administrador de suscripciones de Azure

Los roles de administrador de Azure controlan la capacidad para asignar una suscripción de Azure a un inquilino de AAD.

Encontrará más información sobre los roles de administrador de Azure en el artículo [Agregar o cambiar los administradores de la suscripción de Azure][lnk-admin-roles].

## <a name="faq"></a>Preguntas más frecuentes

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-complete-this-task"></a>Soy administrador de servicios y deseo cambiar la asignación de directorio entre mi suscripción y un inquilino de AAD específico. ¿Cómo completo esta tarea?

Vea [Adición de una suscripción existente al directorio de Azure AD](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md#to-associate-an-existing-subscription-to-your-azure-ad-directory).

### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organizational-account"></a>Quiero cambiar un administrador de servicios o coadministrador cuando haya iniciado sesión con una cuenta de la organización.

Consulte el artículo de asistencia [Changing Service Administrator and Co-Administrator when logged in with an organizational account][lnk-service-admins] (Cambiar el administrador de servicios y el coadministrador cuando ha iniciado sesión con una cuenta organizativa).

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>¿Por qué veo este error? "Su cuenta no tiene los permisos adecuados para crear una solución. Póngase en contacto con el administrador de cuentas o inténtelo con otra cuenta."

Observe el diagrama siguiente para obtener instrucciones:

![][img-flowchart]

> [!NOTE]
> Si sigue viendo el error después de validar que usted es el administrador global del inquilino de AAD y el coadministrador de la suscripción, haga que el administrador de cuenta quite el usuario y vuelva a asignar los permisos necesarios en este orden. En primer lugar, agregue al usuario como administrador global y luego agregue a un usuario como coadministrador de la suscripción de Azure. Si los problemas persisten, póngase en contacto con [Ayuda y soporte técnico][lnk-help-support].

### <a name="why-am-i-seeing-this-error-when-i-have-an-azure-subscription-an-azure-subscription-is-required-to-create-pre-configured-solutions-you-can-create-a-free-trial-account-in-just-a-couple-of-minutes"></a>¿Por qué veo este error cuando tengo una suscripción de Azure? "Para crear soluciones preconfiguradas se requiere una suscripción de Azure. Puede crear una cuenta de evaluación gratuita en pocos minutos".

Si está seguro de que tiene una suscripción de Azure, valide la asignación del inquilino de la suscripción y asegúrese de que se ha seleccionado el inquilino correcto en la lista desplegable. Si ha validado que el inquilino deseado es el correcto, siga el diagrama anterior y valide la asignación de la suscripción y de este inquilino de AAD.

## <a name="next-steps"></a>Pasos siguientes
Para seguir obteniendo más información sobre los aceleradores de soluciones de IoT, va cómo puede [personalizar un acelerador de soluciones][lnk-customize].

[img-flowchart]: media/iot-accelerators-permissions/flowchart.png

[lnk-azureiotsolutions]: https://www.azureiotsolutions.com
[lnk-rm-github-repo]: https://github.com/Azure/remote-monitoring-services-dotnet
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-cf-github-repo]: https://github.com/Azure/azure-iot-connected-factory
[lnk-aad-admin]:../active-directory/users-groups-roles/directory-assign-admin-roles.md
[lnk-portal]: https://portal.azure.com
[lnk-create-edit-users]:../active-directory/fundamentals/active-directory-users-profile-azure-portal.md
[lnk-assign-app-roles]:../active-directory/manage-apps/assign-user-or-group-access-portal.md
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin
[lnk-admin-roles]: ../billing/billing-add-change-azure-subscription-administrator.md
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-accelerators-remote-monitoring-customize.md
