---
title: Configuración de su cuenta de facturación para un contrato de cliente de Microsoft - Azure | Microsoft Docs
description: Aprenda a configurar su cuenta de facturación para un contrato de cliente de Microsoft.
services: ''
documentationcenter: ''
author: amberbhargava
manager: amberb
editor: banders
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/28/2019
ms.author: banders
ms.openlocfilehash: 90f832319fa2343003af58bd99eb64c0cbd94dd8
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57894553"
---
# <a name="set-up-your-billing-account-for-a-microsoft-customer-agreement"></a>Configuración de su cuenta de facturación para un contrato de cliente de Microsoft

Si ha caducado su inscripción al contrato Enterprise o a punto de expirar, puede firmar un acuerdo de Microsoft al cliente renovar su inscripción. La renovación incluye los siguientes pasos:

1. Acepte el contrato de cliente nuevo de Microsoft. Trabaje con su representante de campo de Microsoft para entender los detalles y acepte el contrato de nuevo.
2. Configure la nueva cuenta de facturación que se crea para el nuevo contrato de cliente de Microsoft.

En este artículo se describe los cambios en la facturación después de la instalación existente y le guiará a través de la configuración de la nueva cuenta de facturación.

Para configurar la cuenta de facturación, debe pasar la facturación de suscripciones de Azure de su inscripción al contrato Enterprise a la nueva cuenta. El programa de instalación no afecta a los servicios de Azure que se ejecutan en sus suscripciones. Sin embargo, cambia la manera en que va a administrar la facturación para las suscripciones.

- En lugar de la [portal EA](https://ea.azure.com), podrá administrar los servicios de Azure y facturación, en el [portal Azure](https://portal.azure.com).
- Recibirá una factura mensual y digital para los cargos. Puede ver y analizar la factura en la página de facturación de Azure Cost Management +.
- En lugar de los departamentos de TI y la cuenta en su inscripción al contrato Enterprise, usará la estructura y los ámbitos de la nueva cuenta de facturación para administrar y organizar la facturación.

Antes de iniciar el programa de instalación, se recomienda que realice lo siguiente:

- **Comprender la nueva cuenta de facturación**
  - La nueva cuenta simplifica la facturación de su organización. [Obtener una introducción rápida de la nueva cuenta de facturación](billing-mca-overview.md)
- **Comprobar el acceso para completar la instalación**
  - Solo los usuarios con determinados permisos administrativos pueden completar la instalación. Compruebe si tiene la [acceso requerido para completar la instalación](#access-required-to-complete-the-setup).
- **Comprender los cambios realizados en la jerarquía de facturación**
  - Que la nueva cuenta de facturación se organiza de forma diferente que su inscripción al contrato Enterprise. [Comprender los cambios realizados en la jerarquía en la nueva cuenta de facturación](#understand-changes-to-your-billing-hierarchy).
- **Entender los cambios para acceder a de los administradores facturación**
  - Los administradores de su inscripción al contrato Enterprise obtienen acceso a los ámbitos en la nueva cuenta de facturación. [Entender los cambios a su acceso](#understand-changes-to-your-billing-administrators-access).
- **Características de contrato Enterprise de la vista que se reemplazan por la nueva cuenta**
  - Características de vista de la inscripción del contrato Enterprise que se reemplazan por las funciones de la nueva cuenta.
- **Ver las respuestas a preguntas más frecuentes**
  - Vista [información adicional](#additional-information) para obtener más información sobre el programa de instalación.

## <a name="access-required-to-complete-the-setup"></a>Acceso requerido para completar la instalación

Para completar la instalación, necesita el siguiente acceso:

- Propietario del perfil de facturación que se creó cuando se ha firmado el contrato del cliente de Microsoft. Para más información sobre los perfiles de facturación, consulte [perfiles facturación](billing-mca-overview.md#understand-billing-profiles).

- Administrador de empresa en el que se renueva la inscripción.

### <a name="if-youre-not-an-enterprise-administrator-on-the-enrollment"></a>Si no es un administrador de empresa en la inscripción

Puede solicitar la inscripción de los administradores de empresa para completar la configuración de su cuenta de facturación.

1. Inicie sesión en el portal de Azure mediante el vínculo de correo electrónico que se enviará cuando firmado el contrato del cliente de Microsoft.

2. Si otra persona de su organización había firmado el contrato o no tiene el correo electrónico, inicie sesión con el siguiente vínculo. Reemplace **enrollmentNumber** con el número de inscripción de su contrato enterprise que se ha renovado.

   `https://portal.azure.com/#blade/Microsoft_Azure_Billing/EATransitionToMCA/enrollmentId/enrollmentNumber`

3. Seleccione los administradores de empresa que desea enviar la solicitud.

   ![Captura de pantalla que muestra invitar a los administradores de empresa](./media/billing-mca-setup-account/ea-mca-invite-admins.png)

4. Seleccione **solicitud de envío**.

   Los administradores recibirán un correo electrónico con instrucciones para completar la instalación.

### <a name="if-youre-not-an-owner-of-the-billing-profile"></a>Si no es un propietario del perfil de facturación

El usuario de su organización, que firmó el contrato del cliente de Microsoft se agrega como propietario en el perfil de facturación. Solicitar al usuario que le agregue como propietario para que pueda completar la instalación.  <!-- Todo Are there any next steps -->

## <a name="understand-changes-to-your-billing-hierarchy"></a>Información sobre los cambios en la jerarquía de facturación

La nueva cuenta de facturación simplifica la facturación de su organización al proporcionarle mejorada de facturación y las capacidades de administración de costos. El diagrama siguiente explica cómo se organiza la facturación en la nueva cuenta de facturación.

![Imagen de ea mca post transición jerarquía](./media/billing-mca-setup-account/mca-post-transition-hierarchy.png)

1. Use la cuenta de facturación para administrar la facturación de su contrato de cliente de Microsoft. Para obtener más información acerca de la cuenta de facturación, vea [comprender la cuenta de facturación](billing-mca-overview.md#understand-billing-account).
2. Use el perfil de facturación para administrar la facturación para su organización, similar a la inscripción al contrato Enterprise. Los administradores de empresa se conviertan en propietarios del perfil de facturación. Para más información sobre los perfiles de facturación, consulte [perfiles facturación](billing-mca-overview.md#understand-billing-profiles).
3. Usar una sección de la factura para organizar los costos según sus necesidades, similares a los departamentos de su inscripción al contrato Enterprise. Departamento se convierte en las secciones de la factura y administradores de departamento se conviertan en propietarios de las secciones de la factura correspondiente. Para más información acerca de las secciones de la factura, consulte [comprender la factura secciones](billing-mca-overview.md#understand-invoice-sections).
4. No se admiten las cuentas que se crearon en su contrato Enterprise en la nueva cuenta de facturación. Las suscripciones de la cuenta pertenecen a la sección de factura correspondientes para su departamento. Los propietarios de cuentas pueden crear y administrar suscripciones para sus secciones de la factura.

## <a name="understand-changes-to-your-billing-administrators-access"></a>Entender los cambios para acceder a de los administradores facturación

Dependiendo de su acceso, los administradores de facturación en su inscripción al contrato Enterprise obtienen acceso a los ámbitos en la nueva cuenta de facturación. En la tabla siguiente se explica el cambio en el acceso durante la instalación:

| Rol existente | Rol de la transición de POST |
| --- | --- |
| **Administrador de empresa (lectura sólo = n)** | **: El propietario del perfil de facturación** </br> Administrar todo el contenido en el perfil de facturación </br> - **Propietario de la sección de factura en todas las secciones de la factura** </br> Administrar todo el contenido de las secciones de factura |
| **Administrador de empresa (lectura sólo = Yes)** | **: Lector de perfil de facturación** </br> Vista de solo lectura de todo el contenido en la cuenta de facturación</br>**-Lector de factura sección en la sección de todas las facturas**</br> Vista de solo lectura de todo el contenido en la sección de factura|
| **Administrador de departamento (lectura sólo = n)** |**-Propietario de la sección de factura en la sección de factura que creó para sus respectivos departamentos** </br>Administrar todo el contenido de la sección de factura|
| **Administrador de departamento (lectura sólo = Yes)**|**-Lector de la sección de factura en la sección de factura que creó para sus respectivos departamentos**</br> Vista de solo lectura de todo el contenido en la sección de factura|
| **Propietario de la cuenta** | **-Creador de suscripción azure en la sección de factura que creó para sus respectivos departamentos** </br>  Crear suscripciones de Azure para su sección de factura|

Un inquilino de Azure Active Directory está seleccionado para la nueva cuenta de facturación al firmar el acuerdo de cliente de Microsoft. Si no existe un inquilino para su organización, se crea un nuevo inquilino. El inquilino representa a su organización en Azure Active Directory. Los administradores de inquilinos global de su organización use el inquilino para administrar el acceso de aplicaciones y datos de su organización.

La nueva cuenta de solo es compatible con los usuarios del inquilino al que se ha seleccionado al firmar el acuerdo de cliente de Microsoft. Si los usuarios con permisos administrativos en su contrato Enterprise forman parte del inquilino, obtendrá acceso a la nueva cuenta de facturación durante la instalación. Si no son parte del inquilino, no podrá tener acceso a la nueva cuenta de facturación, a menos que se invitar a ellos.

Cuando se invita a los usuarios, que se agregan al inquilino como usuarios invitados y obtención acceso a la cuenta de facturación. Para invitar a los usuarios, el acceso de invitado debe estar activado para el inquilino. Para obtener más información, consulte [controlar el acceso de invitado en Azure Active Directory](https://docs.microsoft.com/en-us/microsoftteams/teams-dependencies#control-guest-access-in-azure-active-directory). Si el acceso de invitado está desactivado, póngase en contacto con los administradores globales del inquilino para activarla. <!-- Todo - How can they find their global administrator -->

## <a name="view-features-replaced-by-the-new-billing-account"></a>Ver las características que se sustituye por la nueva cuenta de facturación

Características del contrato Enterprise siguientes se reemplazan con nuevas características de la cuenta de facturación para un contrato de cliente de Microsoft.

### <a name="enterprise-agreement-accounts"></a>Cuentas de contrato Enterprise

No se admiten las cuentas que se crearon en su inscripción al contrato Enterprise en la nueva cuenta de facturación. Las suscripciones de la cuenta pertenecen a la sección de factura que creó para sus respectivos departamentos. Los propietarios de cuentas se convierten en los creadores de suscripción de Azure y pueden crear y administrar suscripciones para sus secciones de la factura.

### <a name="notification-contacts"></a>Contactos de notificación

Contactos de notificación se envían comunicaciones por correo electrónico sobre el contrato Enterprise de Azure. No se admiten en la nueva cuenta de facturación. Correos electrónicos sobre los créditos de Azure y las facturas se envían a los usuarios que tienen acceso a los perfiles de facturación en su cuenta de facturación.

### <a name="spending-quotas"></a>Las cuotas de gasto

Las cuotas de gastos que se establecieron para departamentos de su inscripción al contrato Enterprise se reemplazan por los presupuestos en la nueva cuenta de facturación. Se crea un presupuesto para cada cuota de gasto establecido en los departamentos de TI en su inscripción. Para obtener más información sobre los presupuestos, consulte [crear y administrar presupuestos Azure](../cost-management/manage-budgets.md).

### <a name="cost-centers"></a>Centros de costo

Centro de costo que se establecieron en las suscripciones de Azure en su inscripción al contrato Enterprise se realiza a través de la nueva cuenta de facturación. Sin embargo, no se admiten los centros de costo para los departamentos de TI y las cuentas de contrato Enterprise.

## <a name="additional-information"></a>Información adicional

Las secciones siguientes proporcionan información adicional acerca de cómo configurar su cuenta de facturación.

### <a name="no-service-downtime"></a>No hay ningún tiempo de inactividad del servicio

Los servicios de Azure de su suscripción se siguen ejecutando sin ninguna interrupción. Solo hacemos la transición de la relación de facturación para sus suscripciones de Azure. Los recursos existentes, los grupos de recursos o los grupos de administración no se verán afectados.

### <a name="user-access-to-azure-resources"></a>Acceso de usuario a los recursos de Azure

Acceso a recursos de Azure que se ha configurado con Azure RBAC (control de acceso basado en roles) no se ve afectada durante la transición.

### <a name="azure-reservations"></a>Reservas de Azure

Las reservas de Azure en su inscripción al contrato Enterprise se mueve a la nueva cuenta de facturación. Durante la transición, no habrá ningún cambio en los descuentos de reserva que se aplican a sus suscripciones.

### <a name="azure-marketplace-products"></a>Productos de Azure Marketplace

Los productos de Azure Marketplace en su inscripción al contrato Enterprise se mueven junto con las suscripciones. No habrá ningún cambio en el acceso al servicio de los productos de Marketplace durante la transición.

### <a name="support-plan"></a>Plan de soporte técnico

Beneficios de soporte técnico no se transfieren como parte de la transición. Comprar un nuevo plan de soporte técnico para obtener los beneficios para suscripciones de Azure en la nueva cuenta de facturación.

### <a name="past-charges-and-balance"></a>Más allá de los cargos y saldo

Saldo de cargos y créditos antes de la transición se puede ver en su inscripción al contrato Enterprise a través del portal de Azure. <!--Todo - Add a link for this-->

### <a name="when-should-the-setup-be-completed"></a>¿Cuándo se debe completar la instalación?

Completar la configuración de su cuenta de facturación antes de que expire su inscripción al contrato Enterprise. Si expira la inscripción, servicios en las suscripciones de Azure se sigue seguir funcionando sin interrupciones. Sin embargo, se le cobrarán las tarifas para minoristas para los servicios.

### <a name="changes-to-the-enterprise-agreement-enrollment-after-the-setup"></a>Cambios realizados en la inscripción al contrato Enterprise después de la instalación

Suscripciones de Azure que se crean para la inscripción del contrato Enterprise, una vez que se puede mover manualmente la transición a la nueva cuenta de facturación. Para obtener más información, consulte [obtener propiedad de las suscripciones de Azure a otros usuarios de facturación](billing-mca-request-billing-ownership.md). Para mover las reservas de Azure que se compraron después de la transición, [póngase en contacto con soporte técnico de Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). También puede proporcionar a los usuarios acceso a la cuenta de facturación después de la transición. Para obtener más información, consulte [administrar roles de facturación en el portal de Azure](billing-understand-mca-roles.md#manage-billing-roles-in-the-azure-portal)

### <a name="revert-the-transition"></a>Revertir la transición

No se puede revertir la transición. Una vez que la facturación de las suscripciones de Azure es la transición a la nueva cuenta de facturación, no puede revertirla a su inscripción al contrato Enterprise.

### <a name="closing-your-browser-during-setup"></a>Cierre el explorador durante la instalación

Antes de hacer clic en **iniciar transición**, puede cerrar el explorador. Puede volver a la instalación mediante el vínculo que obtuvo en el correo electrónico y comenzar la transición. Si cierra el explorador, una vez iniciada la transición, la transición seguirá ejecutándose en. Volver a la página de estado de transición para supervisar el estado más reciente de la transición. Recibirá un correo electrónico cuando se complete la transición.

## <a name="complete-the-setup-in-the-azure-portal"></a>Completar la instalación en el portal de Azure

Para completar la instalación, necesita acceso a la nueva cuenta de facturación y la inscripción del contrato Enterprise. Para obtener más información, consulte [acceso necesario para completar la configuración de su cuenta de facturación](#access-required-to-complete-the-setup).

1. Inicie sesión en el portal de Azure mediante el vínculo de correo electrónico que se enviará cuando firmado el contrato del cliente de Microsoft.

2. Si otra persona de su organización había firmado el contrato o no tiene el correo electrónico, inicie sesión con el siguiente vínculo. Reemplace **enrollmentNumber** con el número de inscripción de su contrato Enterprise que se ha renovado.

   `https://portal.azure.com/#blade/Microsoft_Azure_Billing/EATransitionToMCA/enrollmentId/enrollmentNumber`

3. Seleccione **iniciar transición** en el último paso del programa de instalación. Una vez que seleccione la transición de inicio:

    ![Captura de pantalla que muestra al Asistente para instalación](./media/billing-mca-setup-account/ea-mca-set-up-wizard.png)

    - Se crea una jerarquía de facturación correspondiente a la jerarquía del contrato Enterprise en la nueva cuenta de facturación. Para obtener más información, consulte [entender los cambios a la jerarquía de facturación](#understand-changes-to-your-billing-hierarchy).
    - Los administradores de su inscripción al contrato Enterprise tienen acceso a la nueva cuenta de facturación para que sigan administrar la facturación de su organización.
    - La facturación de las suscripciones de Azure se envía a la nueva cuenta. **No habrá ningún impacto en los servicios de Azure durante esta transición. Se le mantienen en ejecución sin interrupciones**.
    - Si dispone de las reservas de Azure, se mueven a la nueva cuenta de facturación con el mismo descuento y término. Se seguirá el descuento de reserva que se aplicarán durante la transición.

4. Puede supervisar el estado de la transición en el **la transición de estado** página.

   ![Captura de pantalla que muestra el estado de transición](./media/billing-mca-setup-account/ea-mca-set-up-status.png)

## <a name="validate-the-billing-account-is-set-up-properly"></a>Validar la facturación de la cuenta está configurada correctamente

 Validar lo siguiente para asegurarse de que la nueva cuenta de facturación está configurada correctamente:

### <a name="azure-subscriptions"></a>Suscripciones de Azure

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Busque en **Administración de costos + facturación**.

   ![Captura de pantalla que muestra la búsqueda en Azure Portal](./media/billing-mca-setup-account/billing-search-cost-management-billing.png)

3. Seleccione el perfil de facturación. Según el acceso, debe seleccionar una cuenta de facturación. En la cuenta de facturación, seleccione **facturación perfiles** y, a continuación, el perfil de facturación.

4. Seleccione **suscripciones de Azure** del lado izquierdo.

   ![Captura de pantalla que muestra la lista de suscripciones](./media/billing-mca-setup-account/billing-mca-subscriptions-post-transition.png)

Las suscripciones de Azure que se realiza la transición desde su inscripción al contrato Enterprise a la nueva cuenta de facturación se muestran en la página suscripciones de Azure. Si cree que falta alguna suscripción, realizar la transición de la facturación de la suscripción manualmente en el portal de Azure. Para obtener más información, consulte [obtener propiedad de las suscripciones de Azure a otros usuarios de facturación](billing-mca-request-billing-ownership.md)

### <a name="azure-reservations"></a>Reservas de Azure

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Busque en **Administración de costos + facturación**.

   ![Captura de pantalla que muestra la búsqueda en Azure Portal](./media/billing-mca-setup-account/billing-search-cost-management-billing.png)

3. Seleccione una sección de la factura. Según el acceso, debe seleccionar una cuenta de facturación o un perfil de facturación.  En la cuenta de facturación o el perfil de facturación, seleccione **factura secciones** y, a continuación, una sección de factura.

    ![Captura de pantalla que muestra la lista de transición de entrada de sección de factura](./media/billing-mca-setup-account/billing-mca-invoice-sections-post-transition.png)

4. Seleccione **todos los productos** del lado izquierdo.

5. Buscar en **reservada**.

    ![Captura de pantalla que muestra la lista de suscripciones post transición](./media/billing-mca-setup-account/billing-mca-azure-reservations-post-transition.png)

Las reservas de Azure que se han movido desde su inscripción al contrato Enterprise a la nueva cuenta de facturación se muestran en la página de todos los productos. Repita los pasos para todas las secciones de la factura comprobar que todas las reservas de Azure se mueven desde su inscripción al contrato Enterprise. Si cree que falta cualquier reserva Azure [póngase en contacto con soporte técnico de Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para mover la reserva a la nueva cuenta de facturación.

### <a name="access-of-enterprise-administrators-on-the-billing-profile"></a>Acceso de los administradores de empresa en el perfil de facturación

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Busque en **Administración de costos + facturación**.

   ![Captura de pantalla que muestra la búsqueda en Azure Portal](./media/billing-mca-setup-account/billing-search-cost-management-billing.png)

3. Seleccione el perfil de facturación que creó para su inscripción. Según el acceso, debe seleccionar una cuenta de facturación.  En la cuenta de facturación, seleccione **facturación perfiles** y, a continuación, el perfil de facturación.

4. Seleccione **control de acceso (IAM)** del lado izquierdo.

   ![Captura de pantalla que muestra el acceso de la transición de post los administradores de empresa](./media/billing-mca-setup-account/billing-mca-ea-admins-access-post-transition.png)

Los administradores de organización se muestran como propietarios de perfiles mientras la empresa se enumeran los administradores con permisos de solo lectura de facturación como lectores de perfil de facturación. Si cree que falta el acceso para los administradores de empresa, puede concederles acceso en el portal de Azure. Para obtener más información, consulte [administrar roles de facturación en Azure portal](billing-understand-mca-roles.md#manage-billing-roles-in-the-azure-portal).

### <a name="access-of-enterprise-administrators-department-administrators-and-account-owners-on-invoice-sections"></a>Acceso de los administradores de empresas, administradores de departamento y los propietarios de cuentas en las secciones de la factura

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Busque en **Administración de costos + facturación**.

   ![Captura de pantalla que muestra la búsqueda en Azure Portal](./media/billing-mca-setup-account/billing-search-cost-management-billing.png).

3. Seleccione una sección de la factura. Las secciones de la factura tienen el mismo nombre que sus respectivos departamentos en inscripciones al contrato Enterprise. Según el acceso, debe seleccionar un perfil de facturación o una cuenta de facturación. En el perfil de facturación o la cuenta de facturación, seleccione **factura secciones** y, a continuación, seleccione una sección de la factura.

   ![Captura de pantalla que muestra la lista de transición de entrada de sección de factura](./media/billing-mca-setup-account/billing-mca-invoice-sections-post-transition.png)

4. Seleccione **control de acceso (IAM)** del lado izquierdo.

    ![Captura de pantalla que muestra el acceso de departamento y cuenta de administradores tener acceso a la transición de post](./media/billing-mca-setup-account/billing-mca-department-account-admins-access-post-transition.png)

Los administradores de empresas y administradores de departamento se muestran como propietarios de la sección de factura o lectores de la sección de factura mientras que los propietarios de cuentas en el departamento aparecen como creadores de suscripción de Azure. Repita el paso para todas las secciones de la factura comprobar el acceso para todos los departamentos de su inscripción al contrato Enterprise. Los propietarios de cuentas que no eran parte de cualquier departamento obtendrá permiso en una sección de factura denominada **sección predeterminada de la factura**. Si cree que falta el acceso para los administradores, puede concederles acceso en el portal de Azure. Para obtener más información, consulte [administrar roles de facturación en Azure portal](billing-understand-mca-roles.md#manage-billing-roles-in-the-azure-portal).

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si necesita ayuda, [póngase en contacto con soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.

## <a name="next-steps"></a>Pasos siguientes

- [Introducción a la nueva cuenta de facturación](billing-mca-overview.md)

- [Completar tareas de contrato Enterprise en su cuenta de facturación para un contrato de cliente de Microsoft](billing-mca-enterprise-operations.md)

- [Administrar el acceso a su cuenta de facturación](billing-understand-mca-roles.md)
