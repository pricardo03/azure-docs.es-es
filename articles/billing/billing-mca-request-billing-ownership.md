---
title: Obtener la propiedad de las suscripciones de Azure de facturación | Microsoft Docs
description: Obtenga información sobre cómo solicitar la propiedad de facturación de suscripciones de Azure a otros usuarios.
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
ms.openlocfilehash: be8c7fcebca224196d9eac7d22387989b1bdfd46
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60371960"
---
# <a name="get-billing-ownership-of-azure-subscriptions-from-other-users"></a>Obtener la propiedad de las suscripciones de Azure a otros usuarios de facturación

Es posible que desee tomar posesión de las suscripciones de Azure si el propietario de la facturación existente está abandonando la organización, o desea pagar por las suscripciones a través de su cuenta de facturación.

Puede enviar una solicitud para tomar posesión de las suscripciones de Azure de los propietarios existentes en otras cuentas de facturación. Toma de posesión transfiere las responsabilidades de facturación de suscripciones a la sección de factura.

Para solicitar la propiedad de facturación, debe ser un **propietario de la sección de factura** o **colaborador de la sección de factura**. Para obtener más información, consulte [tareas de funciones de la sección de factura](billing-understand-mca-roles.md#invoice-section-roles-and-tasks).

En este artículo se aplica a una cuenta de facturación para un contrato de cliente de Microsoft. [Compruebe si tiene acceso a un contrato de cliente de Microsoft](#check-access-to-a-microsoft-customer-agreement).

## <a name="request-billing-ownership-in-the-azure-portal"></a>Solicitar la propiedad de facturación en Azure portal

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Busque en **Administración de costos + facturación**.

   ![Captura de pantalla que muestra la búsqueda de Azure portal de administración de costos + facturación](./media/billing-mca-request-billing-ownership/billing-search-cost-management-billing.png)

3. Vaya a la sección de facturas. Según el acceso, debe seleccionar una cuenta de facturación o el perfil de facturación. En la cuenta o perfil de facturación, seleccione **factura secciones** y, a continuación, una sección de factura.
   <!-- Todo - Add a screenshot -->

4. Seleccione **solicitudes de transferencia** desde el lado inferior izquierdo.

5. En la parte superior de la página, seleccione **Agregar**.

6. Escriba la dirección de correo electrónico del usuario que está solicitando la propiedad de facturación de. El usuario debe ser un administrador de cuenta en una cuenta de facturación del programa de servicio de Microsoft Online o el propietario de la cuenta en un contrato Enterprise. Para obtener más información, consulte [ver sus cuentas de facturación en Azure portal](billing-view-all-accounts.md).

   ![Captura de pantalla que muestra cómo agregar una nueva solicitud de transferencia](./media/billing-mca-request-billing-ownership/mca-new-transfer-request.png)

7. Seleccione **enviar solicitud de transferencia**.

8. El usuario recibe un correo electrónico con instrucciones para revisar la solicitud de transferencia.

   ![Captura de pantalla de ese correo electrónico de solicitud de muestra revisión transferencia](./media/billing-mca-request-billing-ownership/mca-review-transfer-request-email.png)

9. Para aprobar la solicitud de transferencia, el usuario selecciona el vínculo del correo electrónico y sigue las instrucciones.

    ![Captura de pantalla de ese correo electrónico de solicitud de muestra revisión transferencia](./media/billing-mca-request-billing-ownership/mca-review-transfer-request.png)

## <a name="check-the-status-of-your-transfer-request-in-the-azure-portal"></a>Comprobar el estado de la solicitud de transferencia en el portal de Azure

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Busque en **Administración de costos + facturación**.

   ![Captura de pantalla que muestra la búsqueda de Azure portal de administración de costos + facturación](./media/billing-mca-request-billing-ownership/billing-search-cost-management-billing.png)

3. Vaya a la sección de facturas. Según el acceso, debe seleccionar una cuenta de facturación o el perfil de facturación. En la cuenta o perfil de facturación, seleccione **factura secciones** y, a continuación, una sección de factura.
   <!-- Todo - Add a screenshot -->

4. Seleccione **solicitudes de transferencia** desde el lado inferior izquierdo.

5. La página de solicitudes de transferencia muestra la siguiente información:

    ![Captura de pantalla que muestra la lista de solicitudes de transferencia](./media/billing-mca-request-billing-ownership/mca-view-transfer-requests.png)

   |Columna|Definición|
   |---------|---------|
   |Fecha de solicitud|La fecha en que se envió la solicitud de transferencia|
   |Recipient|La dirección de correo electrónico del usuario que envió la solicitud para transferir la propiedad de facturación|
   |Fecha de expiración|La fecha de caducidad de la solicitud|
   |Status|El estado de solicitud de transferencia|

    La solicitud de transferencia puede tener uno de los siguientes estados:

   |Status|Definición|
   |---------|---------|
   |En curso|El usuario no ha aceptado la solicitud de transferencia|
   |Processing|El usuario aprueba la solicitud de transferencia. La facturación de suscripciones que ha seleccionado el usuario se transfieren a la sección de factura|
   |Completed| La facturación de suscripciones que ha seleccionado el usuario se transfiere a la sección de factura|
   |Finalizado con errores|No se pudo transferir el se completó la solicitud, pero la facturación de algunas suscripciones que el usuario seleccionado|
   |Expirada|El usuario no aceptó la solicitud en el tiempo y ha expirado|
   |Canceled|Un usuario con acceso a la solicitud de transferencia cancelada la solicitud|
   |Rechazado|El usuario rechazó la solicitud de transferencia|

6. Seleccione una solicitud de transferencia para ver los detalles. La página de detalles de la transferencia muestra la siguiente información:
   <!-- Todo - Add a screenshot -->

   |Columna  |Definición|
   |---------|---------|
   |Id. de solicitud de transferencia|El identificador único para la solicitud de transferencia. Si envía una solicitud de soporte técnico, comparten el identificador con el soporte técnico de Azure para acelerar su solicitud de soporte técnico|
   |Transferencia solicitada|La fecha en que se envió la solicitud de transferencia|
   |Transferencia solicitada por|La dirección de correo electrónico del usuario que envió la solicitud de transferencia|
   |Solicitud de transferencia caduca en| La fecha de caducidad de la solicitud de transferencia|
   |Dirección de correo electrónico del destinatario|La dirección de correo electrónico del usuario que envió la solicitud para transferir la propiedad de facturación|
   |Vínculo de transferencia que se envían al destinatario|La dirección url que se envió al usuario para revisar la solicitud de transferencia|

## <a name="additional-information"></a>Información adicional

La siguiente sección proporciona información adicional acerca de cómo transferir las suscripciones.

### <a name="no-service-downtime"></a>No hay ningún tiempo de inactividad del servicio

Servicios de Azure en la suscripción ejecutándose sin ninguna interrupción. Solo podemos cambiar la relación de facturación para las suscripciones de Azure que el usuario selecciona para transferir.

### <a name="disabled-subscriptions"></a>Suscripciones deshabilitadas

No se puede transferir las suscripciones deshabilitadas. Suscripciones deben estar en estado activo para transferir su propiedad de facturación.

### <a name="azure-resources-transfer"></a>Transferencia de recursos de Azure

Todos los recursos de las suscripciones, como máquinas virtuales, discos y transferencia de los sitios Web.

### <a name="azure-marketplace-products-transfer"></a>Transferencia de productos de Azure Marketplace

Transferencia de productos de Azure Marketplace, junto con sus suscripciones correspondientes.

### <a name="azure-reservations-transfer"></a>Transferencia de las reservas de Azure

Las reservas de Azure no se mueven automáticamente con las suscripciones. [Póngase en contacto con soporte técnico de Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para mover las reservas de direcciones.

### <a name="access-to-azure-services"></a>Acceso a servicios de Azure

Acceso a recursos de Azure que se ha configurado con Azure RBAC (control de acceso basado en roles) no se ve afectada durante la transición.

### <a name="azure-support-plan"></a>Plan de soporte técnico de Azure

Soporte técnico de Azure no transfiere con las suscripciones. Si el usuario transfiere todas las suscripciones de Azure, pídale que cancelar su plan de soporte técnico.

### <a name="charges-for-transferred-subscription"></a>Cargos de suscripción transferido

El propietario original de facturación de las suscripciones es responsable de los cargos que informó hasta el momento en que se ha completado la transferencia. La sección de la factura es responsable de cargos notificados desde el momento de la transferencia y versiones posteriores. Puede haber algunos cargos que tuvieron lugar antes de la transferencia, pero se notifican con posterioridad. Estos cargos se muestran en la sección de factura.

### <a name="supported-offers"></a>Ofertas admitidas

Se puede transferir la suscripción de todos los tipos o las ofertas, excepto la CSP ofrece.

### <a name="cancel-a-transfer-request"></a>Cancelar una solicitud de transferencia

Puede cancelar la solicitud de transferencia hasta que se aprueba o rechaza la solicitud. Para cancelar la solicitud de transferencia, vaya a la página de detalles de la transferencia y seleccione Cancelar en la parte inferior de la página.

### <a name="software-as-a-service-saas-transfer"></a>Software como una transferencia de servicio (SaaS)

No se transfieren con las suscripciones a productos SaaS. Pida al usuario que [soporte técnico de Azure de contacto](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para transferir la propiedad de facturación de los productos de SaaS. Junto con la propiedad de facturación, el usuario también puede transferir la propiedad de recurso. Propiedad de recursos le permite realizar operaciones de administración, como eliminar y ver los detalles del producto. Usuario debe ser propietario de un recurso en el producto de SaaS para transferir la propiedad de recurso.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Comprobar el acceso a un contrato de cliente de Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si necesita ayuda, [póngase en contacto con soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.

## <a name="next-steps"></a>Pasos siguientes

- La propiedad de facturación de las suscripciones de Azure se transfiere a la sección de factura. Realizar un seguimiento de los cargos de estas suscripciones en el [portal Azure](https://portal.azure.com).
- Asignar permisos para ver y administrar la facturación de estas suscripciones a otros usuarios. Para obtener más información, consulte [sección roles y tareas de factura](billing-understand-mca-roles.md#invoice-section-roles-and-tasks).
