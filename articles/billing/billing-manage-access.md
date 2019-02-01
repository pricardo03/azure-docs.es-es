---
title: Administración del acceso a la facturación de Azure | Microsoft Docs
description: Obtenga información acerca de cómo acceder a la información de facturación de Azure para los miembros del equipo.
services: ''
documentationcenter: ''
author: vikramdesai01
manager: amberb
editor: ''
tags: billing
ms.assetid: e4c4d136-2826-4938-868f-a7e67ff6b025
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/02/2018
ms.author: banders
ms.custom: seodec18
ms.openlocfilehash: 516d8e9116bbdcd421897b5a5c2d9755e7e636c8
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2019
ms.locfileid: "54903560"
---
# <a name="manage-access-to-billing-information-for-azure"></a>Administración del acceso a la información de facturación de Azure

Para la mayoría de las suscripciones, puede brindar acceso a la información de facturación a los miembros de su equipo en **Suscripciones** de Azure Portal. Si es cliente de Azure con un Contrato Enterprise (cliente de EA) y es el administrador de Enterprise, puede otorgar permisos a los administradores de departamentos y a los propietarios de la cuenta en Enterprise Portal.

## <a name="give-access-to-billing"></a>Concesión de acceso a la facturación

Todos, excepto los clientes de EA, pueden conceder acceso a la información de facturación de Azure mediante la asignación de uno de los roles de usuario siguientes a miembros de su equipo:

- Administrador de cuenta
- Administrador de servicios
- Coadministrador
- Propietario
- Colaborador
- Lector
- Lector de facturación

Para asignar los roles, consulte [Administración del acceso mediante RBAC y Azure Portal](../role-based-access-control/role-assignments-portal.md).

Esos roles tienen acceso a la información de facturación en [Azure Portal](https://portal.azure.com/). Los usuarios que tienen asignados esos roles también pueden usar las [API de facturación](billing-usage-rate-card-overview.md) para obtener detalles de uso y facturas mediante programación. Para más información, consulte el artículo sobre los [roles para RBAC de Azure](../role-based-access-control/built-in-roles.md).

### <a name="opt-in"></a> Permitir que los usuarios descarguen facturas

Después de asignar los roles adecuados a los miembros del equipo, el administrador de cuenta debe activar el acceso para descargar facturas en Azure Portal. Las facturas anteriores a diciembre de 2016 solo están disponibles para el administrador de cuenta.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).
1. Como administrador de la cuenta, seleccione la suscripción desde la [hoja Suscripciones](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) en Azure Portal.

1. Seleccione **Facturas** y, después, **Acceso a las facturas**.

    ![Captura de pantalla muestra cómo delegar acceso a las facturas](./media/billing-manage-access/AA-optin.png)

1. Seleccione **On** (Activar) y guarde.

    ![Captura de pantalla muestra cómo activar o desactivar la delegación de acceso a la factura](./media/billing-manage-access/AA-optinAllow.png)

El administrador de la cuenta también puede configurar que las facturas se envíen por correo electrónico. Para obtener más información, vea [Obtención de la factura por correo electrónico](billing-download-azure-invoice-daily-usage-date.md).

## <a name="give-read-only-access-to-billing"></a>Concesión de acceso de solo lectura a la facturación

Asigne el rol Lector de facturación a otro usuario que necesite tener acceso de solo lectura a la información de facturación de suscripción, pero no la capacidad de administrar ni crear los servicios de Azure. Este rol es adecuado para los usuarios de una organización que son responsables de la administración financiera y de costos para las suscripciones de Azure.

Si es un cliente de EA, un propietario de la cuenta o un administrador de departamento puede asignar el rol Lector de facturación a los miembros del equipo. Pero para que ese Lector de facturación vea la información de facturación correspondiente al departamento o la cuenta, el administrador de Enterprise debe habilitar las directivas **PC: ver los cargos** o **AD: ver los cargos** en Enterprise Portal.

La característica Lector de facturación está en versión preliminar y aún no admite nubes no globales.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).
1. Seleccione su suscripción en la [hoja de suscripciones](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) de Azure Portal.

1. Seleccione **Access Control (IAM)**.
1. Seleccione **Asignaciones de roles** para ver todas las asignaciones de roles de esta suscripción.
1. Seleccione **Agregar asignación de roles**.
1. En la lista desplegable **Rol**, elija **Lector de facturación**.
1. En el cuadro de texto **Seleccionar**, escriba el nombre o el correo electrónico del usuario que quiere agregar.
1. Seleccione el usuario.
1. Seleccione **Guardar**.
1. Transcurridos unos instantes, al usuario se le asigna el rol Lector de facturación en el ámbito de la suscripción.
1. El Lector de facturación recibe un correo electrónico con un vínculo para iniciar sesión.

    ![Captura de pantalla que muestra lo que puede ver el Lector de facturación en Azure Portal](./media/billing-manage-access/billing-reader-view.png)

## <a name="allow-department-administrator-or-account-owner-billing-access"></a>Permiso para acceder a la facturación para administradores de departamentos o propietarios de la cuenta

El administrador de Enterprise puede permitir que los administradores de departamentos y los propietarios de la cuentan vean los detalles del uso y los costos asociados con los departamentos y las cuentas que administran.

1. Como administrador de Enterprise, inicie sesión en el [portal de EA](https://ea.azure.com/).
1. Seleccione **Administrar**.
1. En **Inscripción**, cambie la directiva **AD: ver los cargos** a **Habilitada** para que el administrador de departamento vea el uso y los costos.
1. Cambie la directiva **PC: ver los cargos** a **Habilitada** para que el propietario de la cuenta vea el uso y los costos.


Para más información, consulte [Understand Azure Enterprise Agreement administrative roles in Azure](billing-understand-ea-roles.md) (Roles administrativos del Contrato Enterprise de Azure en Azure).

## <a name="only-account-admins-can-access-account-center"></a>Solo los administradores de la cuenta pueden acceder al Centro de cuentas

El administrador de la cuenta es el propietario legal de la suscripción. De forma predeterminada, la persona que se registró o que compró la suscripción de Azure es el administrador de la cuenta, a menos que la [propiedad de la suscripción se transfiriera](billing-subscription-transfer.md) a otra persona. El Administrador de la cuenta puede crear y cancelar suscripciones, cambiar la dirección de facturación de una suscripción y administrar directivas de acceso para la suscripción desde el [Centro de cuentas](https://account.azure.com/Subscriptions).

## <a name="next-steps"></a>Pasos siguientes

- Los usuarios de otros roles, como Propietario o Colaborador, pueden tener acceso no solo a la información de facturación, sino también a los servicios de Azure. Para administrar estos roles, vea [Administración del acceso mediante RBAC y Azure Portal](../role-based-access-control/role-assignments-portal.md).
- Para más información sobre los roles, consulte [Roles integrados en los recursos de Azure](../role-based-access-control/built-in-roles.md).

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
