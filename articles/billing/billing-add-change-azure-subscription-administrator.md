---
title: Adición o cambio de roles de administrador de suscripción de Azure | Microsoft Docs
description: Se describe cómo agregar o cambiar el coadministrador, el administrador de servicios y el administrador de cuenta de Azure.
services: ''
documentationcenter: ''
author: genlin
manager: adpick
editor: ''
tags: billing
ms.assetid: 13a72d76-e043-4212-bcac-a35f4a27ee26
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/19/2018
ms.author: cwatson
ms.openlocfilehash: 2380cd3712c47ca08e9b9b3597f09f4119238af3
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "52581620"
---
# <a name="add-or-change-azure-subscription-administrators"></a>Agregar o cambiar los administradores de la suscripción de Azure

Para administrar el acceso a recursos de Azure, debe tener el rol de administrador adecuado. En este artículo, se describe cómo agregar o cambiar el rol de administrador para un usuario en el nivel de suscripción.

> [!div class="nextstepaction"]
> [Ayude a mejorar la documentación sobre facturación de Azure](https://go.microsoft.com/fwlink/p/?linkid=2010091)

## <a name="what-administrator-role-do-i-use"></a>¿Qué rol de administrador debo usar?

Azure tiene diversos roles. Para administrar el acceso a los recursos, puede usar los roles de administrador de suscripciones clásicas, como administrador y coadministrador de servicios, o un sistema de autorización más reciente denominado control de acceso basado en rol (RBAC). Para asegurarse de tener un mejor control y para simplificar la administración de acceso, se recomienda utilizar RBAC para todas las necesidades de administración de acceso. Si es posible, se recomienda que vuelva a configurar las directivas de acceso existentes mediante RBAC. Para obtener más información, consulte el artículo [¿Qué es el control de acceso basado en rol (RBAC)?](../role-based-access-control/overview.md) y [Descripción de los distintos roles de Azure](../role-based-access-control/rbac-and-directory-admin-roles.md).

<a name="add-an-admin-for-a-subscription"></a>

## <a name="add-an-rbac-owner-for-a-subscription-in-azure-portal"></a>Adición de un propietario RBAC para una suscripción en Azure Portal 

Para agregar a alguien como administrador para una suscripción de Azure, asígnele el rol de [Propietario](../role-based-access-control/built-in-roles.md#owner) (un rol RBAC) en el ámbito de la suscripción. El rol de propietario puede administrar los recursos de la suscripción que ha asignado y no tiene privilegios de acceso a otras suscripciones.

1. Consulte [**Suscripciones** en Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
2. Seleccione la suscripción a la que quiere dar acceso.
3. En la lista, seleccione **Control de acceso (IAM)**.
4. Seleccione **Agregar**.
   (Si el botón Agregar no está disponible, significa que no le está permitido agregar permisos).
5. En el cuadro **Rol**, seleccione **Propietario**. 
6. En el cuadro **Asignar acceso a**, seleccione **Aplicación, grupo o usuario de Azure AD**. 
7. En el cuadro **Seleccionar**, escriba la dirección de correo electrónico del usuario al que quiera agregar como propietario. Seleccione el usuario y, después, **Guardar**.

    ![Captura de pantalla que muestra el rol de propietario seleccionado](./media/billing-add-change-azure-subscription-administrator/add-role.png)

Así dará al usuario acceso completo a todos los recursos, incluido el derecho a delegar este acceso a otros. Para proporcionar acceso en un ámbito diferente, como un grupo de recursos, visite la hoja **Control de acceso (IAM)** para ese ámbito.

## <a name="add-or-change-co-administrator"></a>Adición o cambio de coadministrador

Solo se puede agregar un [propietario](../role-based-access-control/built-in-roles.md#owner) como coadministrador. No se pueden agregar otros usuarios con roles, por ejemplo, de [colaborador](../role-based-access-control/built-in-roles.md#contributor) y [lector](../role-based-access-control/built-in-roles.md#reader), como coadministradores.

> [!TIP]
> Solo debe agregar al Propietario como un coadministrador, si el usuario necesita administrar las implementaciones clásicas de Azure. Se recomienda utilizar RBAC para cualquier otro fin.

1. Si no lo ha hecho ya, agregue a alguien como propietario siguiendo las instrucciones anteriores.
2. **Haga clic con el botón derecho** en el usuario de propietario que acaba de agregar y seleccione **Agregar como coadministrador**. Si no ve la opción **Agregar como coadministrador**, actualice la página o pruebe con otro explorador de Internet. 

    ![Captura de pantalla donde se agrega el coadministrador](./media/billing-add-change-azure-subscription-administrator/add-coadmin.png)

    Para quitar el permiso de coadministrador, **haga clic con el botón derecho** en el usuario Coadministrador y, luego, seleccione **Quitar coadministrador**.

    ![Captura de pantalla donde se quita el coadministrador](./media/billing-add-change-azure-subscription-administrator/remove-coadmin.png)

### <a name="adding-a-guest-user-as-a-co-administrator"></a>Adición de un usuario invitado como coadministrador

Es posible que los usuarios invitados a los que se haya asignado el rol de coadministrador observen algunas diferencias con respecto a los usuarios miembros con el rol de coadministrador. Considere el siguiente escenario:

- El usuario A, que tiene una cuenta profesional o educativa de Azure AD, es un administrador de servicios en una suscripción de Azure.
- El usuario B tiene una cuenta de Microsoft.
- El usuario A asigna el rol de coadministrador al usuario B.
- El usuario B puede hacer casi todo, pero no puede registrar las aplicaciones ni buscar usuarios en el directorio de Azure AD.

Sería de esperar que el usuario B pudiera administrarlo todo. Esta diferencia se debe a que la cuenta de Microsoft se agrega a la suscripción como usuario invitado y no como usuario miembro. En comparación con los usuarios miembros, los usuarios invitados tienen distintos permisos predeterminados en Azure AD. Por ejemplo, los usuarios miembros pueden leer otros usuarios en Azure AD y los usuarios invitados no. Los usuarios miembros pueden registrar a nuevas entidades de servicio en Azure AD y los usuarios invitados no. Si un usuario invitado debe ser capaz de realizar estas tareas, una posible solución consiste en asignarle los roles de administrador de Azure AD que el usuario invitado necesita. Por ejemplo, en el escenario anterior, podría asignarle el rol [Lectores de directorios](../active-directory/users-groups-roles/directory-assign-admin-roles.md#directory-readers) para que pueda leer otros usuarios y asignarle el rol [Desarrollador de aplicaciones](../active-directory/users-groups-roles/directory-assign-admin-roles.md#application-developer) para que pueda crear entidades de servicio. Para más información sobre los usuarios miembros e invitados y sus permisos, vea [¿Cuales son los permisos de usuario predeterminados en Azure Active Directory?](../active-directory/fundamentals/users-default-permissions.md)

Tenga en cuenta que los [roles integrados en los recursos de Azure](../role-based-access-control/built-in-roles.md) son diferentes de los [roles de administrador de Azure AD](../active-directory/users-groups-roles/directory-assign-admin-roles.md). Los roles integrados no conceden acceso a Azure AD. Para más información, vea [Descripción de los distintos roles](../role-based-access-control/rbac-and-directory-admin-roles.md).

<a name="change-service-administrator-for-a-subscription"></a>

## <a name="change-the-service-administrator-for-an-azure-subscription"></a>Cambio del administrador de servicios de una suscripción de Azure

Solo el administrador de cuenta puede cambiar el administrador de servicios de una suscripción. De forma predeterminada, al registrarse, el administrador de servicios es también el administrador de cuenta. Si el administrador de servicios se cambia a otro usuario, el administrador de cuenta pierde el acceso a Azure Portal. Sin embargo, el administrador de cuenta puede usar en cualquier momento el Centro de cuentas para cambiar el administrador de servicios de nuevo a sí mismo.

1. Asegúrese de que el escenario sea compatible mediante la comprobación de los [límites para cambiar los administradores de servicios](#limits).
1. Inicie sesión en el [Centro de cuentas](https://account.windowsazure.com/subscriptions) como administrador de cuenta.
1. Seleccione una suscripción.
1. En la parte derecha, seleccione **Editar detalles de suscripción**.

    ![Captura de pantalla que muestra el botón Editar suscripción en el Centro de cuentas](./media/billing-add-change-azure-subscription-administrator/editsub.png)
1. En el cuadro **ADMINISTRADOR DE SERVICIOS**, escriba la dirección de correo electrónico del nuevo administrador de servicios.

    ![Captura de pantalla que muestra el cuadro para cambiar el correo electrónico del administrador de servicios](./media/billing-add-change-azure-subscription-administrator/changeSA.png)

<a name="limits"></a>

### <a name="limitations-for-changing-service-administrators"></a>Limitaciones para cambiar los administradores de servicios

* Cada suscripción está asociada a un directorio de Azure AD. Para encontrar el directorio al que está asociada la suscripción, vaya a [**Suscripciones**](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) y, a continuación, seleccione una suscripción para ver el directorio.
* Si inició sesión con una cuenta profesional o educativa, puede agregar otras cuentas de su organización como administrador de servicios. Por ejemplo, abby@contoso.com puede agregar a bob@contoso.com como administrador de servicios, pero no puede agregar a john@notcontoso.com a menos que john@notcontoso.com exista en el directorio contoso.com. Los usuarios que iniciaron sesión con cuentas profesionales o educativas pueden continuar agregando usuarios de cuentas Microsoft como administrador de servicios.

  | Método de inicio de sesión | ¿Agregar un usuario de cuenta Microsoft como administrador de servicios? | ¿Agregar una cuenta profesional o educativa de la misma organización como administrador de servicios? | ¿Agregar una cuenta profesional o educativa de otra organización como administrador de servicios? |
  | --- | --- | --- | --- |
  |  Cuenta Microsoft |SÍ |No |Sin  |
  |  Cuenta profesional o educativa |SÍ |SÍ |Sin  |

## <a name="change-the-account-administrator-for-an-azure-subscription"></a>Cambio del administrador de cuenta de una suscripción a Azure

El administrador de cuenta es el usuario que se registró inicialmente en la suscripción a Azure y es el responsable de la suscripción como propietario de la facturación. Para cambiar el administrador de cuenta de una suscripción, consulte el artículo [Transferencia de la propiedad de una suscripción de Azure a otra cuenta](billing-subscription-transfer.md).

<a name="check-the-account-administrator-of-the-subscription"></a>

**¿No está seguro de quién es el administrador de cuenta?** Siga estos pasos:

1. Consulte [**Suscripciones** en Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Seleccione la suscripción que desee comprobar y, luego, consulte **Configuración**.
1. Seleccione **Propiedades**. El administrador de cuenta de la suscripción se muestra en el cuadro **Administrador de cuenta**.  

## <a name="learn-more-about-resource-access-control-and-active-directory"></a>Más información sobre el control de acceso a los recursos y Active Directory

* Para más información sobre RBAC, consulte el artículo [¿Qué es el control de acceso basado en rol (RBAC)?](../role-based-access-control/overview.md).
* Para obtener más información sobre todos los roles de Azure, consulte el artículo [Descripción de los distintos roles de Azure](../role-based-access-control/rbac-and-directory-admin-roles.md).
* Si desea más información sobre Azure Active Directory, consulte [Asociación de las suscripciones de Azure con Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md) y [Asignación de roles de administrador en Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md).

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).