---
title: Obtención de la propiedad de la facturación de las suscripciones a Azure| Microsoft Docs
description: Aprenda a solicitar la propiedad de la facturación de las suscripciones a Azure de otros usuarios.
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
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60371960"
---
# <a name="get-billing-ownership-of-azure-subscriptions-from-other-users"></a>Obtención de la propiedad de la facturación de las suscripciones de otros usuarios

Es posible que desee tomar posesión de las suscripciones a Azure si el propietario de la facturación existente abandona la organización, o que desee pagar las suscripciones mediante su cuenta de facturación.

Puede enviar una solicitud para tomar posesión de las suscripciones de Azure de propietarios existentes en otras cuentas de facturación. La toma de posesión transfiere las responsabilidades de facturación de las suscripciones a su sección de factura.

Para solicitar la propiedad de la facturación, debe ser **propietario de la sección de factura** o **colaborador de la sección de factura**. Para más información, consulte [Tareas y roles de la sección de factura](billing-understand-mca-roles.md#invoice-section-roles-and-tasks).

Este artículo se aplica a una cuenta de facturación para un contrato de cliente de Microsoft. [Compruebe si tiene acceso a un contrato de cliente de Microsoft](#check-access-to-a-microsoft-customer-agreement).

## <a name="request-billing-ownership-in-the-azure-portal"></a>Solicitud de la propiedad de la facturación en Azure Portal

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Busque en **Administración de costos + facturación**.

   ![Captura de pantalla que muestra la búsqueda en Azure Portal de Administración de costos + facturación](./media/billing-mca-request-billing-ownership/billing-search-cost-management-billing.png)

3. Vaya a la sección de facturas. Dependiendo de su acceso, es posible que tenga que seleccionar una cuenta de facturación o un perfil de facturación. En la cuenta o perfil de facturación, seleccione **Secciones de factura** y, después, una sección de factura.
   <!-- Todo - Add a screenshot -->

4. Seleccione **Solicitudes de transferencia** en el lado inferior izquierdo.

5. En la parte superior de la página, seleccione **Agregar**.

6. Escriba la dirección de correo electrónico del usuario al que solicita la propiedad de la facturación. El usuario debe ser un administrador de cuenta en una cuenta de facturación del programa de servicio Microsoft Online o un propietario de cuenta en un Contrato Enterprise. Para más información, consulte [Visualización de las cuentas de facturación en Azure Portal](billing-view-all-accounts.md).

   ![Captura de pantalla que muestra cómo agregar una nueva solicitud de transferencia](./media/billing-mca-request-billing-ownership/mca-new-transfer-request.png)

7. Seleccione **Enviar solicitud de transferencia**.

8. El usuario recibe un correo electrónico con instrucciones para revisar la solicitud de transferencia.

   ![Captura de pantalla que muestra un correo electrónico de solicitud de transferencia](./media/billing-mca-request-billing-ownership/mca-review-transfer-request-email.png)

9. Para aprobar la solicitud de transferencia, el usuario selecciona el vínculo en el correo electrónico y sigue las instrucciones.

    ![Captura de pantalla que muestra un correo electrónico de solicitud de transferencia](./media/billing-mca-request-billing-ownership/mca-review-transfer-request.png)

## <a name="check-the-status-of-your-transfer-request-in-the-azure-portal"></a>Comprobación del estado de la solicitud de transferencia en Azure Portal

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Busque en **Administración de costos + facturación**.

   ![Captura de pantalla que muestra la búsqueda en Azure Portal de Administración de costos + facturación](./media/billing-mca-request-billing-ownership/billing-search-cost-management-billing.png)

3. Vaya a la sección de facturas. Dependiendo de su acceso, es posible que tenga que seleccionar una cuenta de facturación o un perfil de facturación. En la cuenta o perfil de facturación, seleccione **Secciones de factura** y, después, una sección de factura.
   <!-- Todo - Add a screenshot -->

4. Seleccione **Solicitudes de transferencia** en el lado inferior izquierdo.

5. En la página Solicitudes de transferencia se muestra la siguiente información:

    ![Captura de pantalla que muestra una lista de solicitudes de facturación](./media/billing-mca-request-billing-ownership/mca-view-transfer-requests.png)

   |Columna|Definición|
   |---------|---------|
   |Fecha de solicitud|La fecha en la que se envió la solicitud de transferencia|
   |Recipient|La dirección de correo electrónico del usuario que envió la solicitud para transferir la propiedad de la facturación|
   |Fecha de expiración|La fecha en que expira la solicitud|
   |Status|El estado de la solicitud de transferencia|

    La solicitud de transferencia puede tener uno de los siguientes estados:

   |Status|Definición|
   |---------|---------|
   |En curso|El usuario no ha aceptado la solicitud de transferencia.|
   |Processing|El usuario aprueba la solicitud de transferencia. La facturación de las suscripciones seleccionadas por el usuario está en proceso de transferencia a la sección de factura|
   |Completed| La facturación de las suscripciones seleccionadas por el usuario se transfiere a la sección de factura.|
   |Finalizado con errores|La solicitud se ha completado pero la facturación de algunas suscripciones que el usuario ha seleccionado no se pudo transferir.|
   |Expirada|El usuario no aceptó la solicitud a tiempo y ha expirado.|
   |Canceled|Un usuario con acceso a la solicitud de transferencia ha cancelado la solicitud.|
   |Rechazado|El usuario ha rechazado la solicitud de transferencia.|

6. Seleccione una solicitud de transferencia para ver los detalles. En la página de detalles de la transferencia se muestra la siguiente información:
   <!-- Todo - Add a screenshot -->

   |Columna  |Definición|
   |---------|---------|
   |Identificador de la solicitud de transferencia|El identificador único de la solicitud de transferencia. Si envía una solicitud de soporte técnico, comparta el identificador con el Soporte técnico de Azure para agilizar la solicitud de soporte técnico.|
   |Transferencia solicitada el|La fecha en la que se envió la solicitud de transferencia.|
   |Transferencia solicitada por|La dirección de correo electrónico del usuario que envió la solicitud de transferencia.|
   |La solicitud de transferencia caduca el| La fecha en la que expira la solicitud de transferencia.|
   |Dirección de correo electrónico del destinatario|La dirección de correo electrónico del usuario que envió la solicitud para transferir la propiedad de la facturación|
   |Vínculo de transferencia enviado al destinatario|La dirección URL que se envió al usuario para revisar la solicitud de transferencia.|

## <a name="additional-information"></a>Información adicional

En la sección siguiente se proporciona información adicional acerca de las suscripciones de transferencia.

### <a name="no-service-downtime"></a>No hay ningún tiempo de inactividad del servicio

Los servicios de Azure de la suscripción se siguen ejecutando sin interrupción. Solo cambiamos la relación de facturación de las suscripciones de Azure que el usuario selecciona para transferir.

### <a name="disabled-subscriptions"></a>Suscripciones deshabilitadas

Las suscripciones deshabilitadas no se pueden transferir. Las suscripciones deben estar en estado activo para transferir su propiedad de la facturación.

### <a name="azure-resources-transfer"></a>Transferencia de recursos de Azure

Todos los recursos de las suscripciones, como máquinas virtuales, discos y sitios web se transfieren.

### <a name="azure-marketplace-products-transfer"></a>Transferencia de productos de Azure Marketplace

Los productos de Azure Marketplace se transfieren junto con sus suscripciones correspondientes.

### <a name="azure-reservations-transfer"></a>Transferencia de reservas de Azure

Las reservas de Azure no se mueven automáticamente con las suscripciones. [Póngase en contacto con soporte técnico de Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para mover las reservas.

### <a name="access-to-azure-services"></a>Acceso a servicios de Azure

El acceso a los recursos de Azure que se ha configurado con Azure RBAC (control de acceso basado en rol) no se ve afectado durante la transición.

### <a name="azure-support-plan"></a>Plan de soporte técnico de Azure

El soporte técnico de Azure no se transfiere con las suscripciones. Si el usuario transfiere todas las suscripciones de Azure, pídale que cancele su plan de soporte técnico.

### <a name="charges-for-transferred-subscription"></a>Cargos por la suscripción transferida

El propietario de la facturación original de las suscripciones es responsable de cualquier cargo que se haya informado hasta el momento en que se complete la transferencia. La sección de factura es responsable de los cargos informados desde el momento de la transferencia en adelante. Puede haber algunos cargos que se realizaran antes de la transferencia, pero que se notificaron con posterioridad. Estos cargos se muestran en la sección de factura.

### <a name="supported-offers"></a>Ofertas admitidas

Se puede transferir la suscripción de todos los tipos u ofertas, excepto las ofertas de CSP.

### <a name="cancel-a-transfer-request"></a>Cancelación de una solicitud de transferencia

Puede cancelar la solicitud de transferencia hasta que se apruebe o rechace la solicitud. Para cancelar la solicitud de transferencia, vaya a la página de detalles de la transferencia y seleccione Cancelar en la parte inferior de la página.

### <a name="software-as-a-service-saas-transfer"></a>Transferencia del Software como servicio (SaaS)

No se transfieren los productos de SaaS con las suscripciones. Pida al usuario que [se ponga en contacto con el soporte técnico de Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para transferir la propiedad de la facturación de los productos de SaaS. Junto con la propiedad de la facturación, el usuario también puede transferir la propiedad de los recursos. La propiedad de los recursos le permite realizar operaciones de administración como eliminar y consultar los detalles del producto. El usuario debe ser propietario de un recurso en el producto de SaaS para poder transferir su propiedad.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Comprobación del acceso a un contrato de cliente de Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si necesita ayuda, [póngase en contacto con soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.

## <a name="next-steps"></a>Pasos siguientes

- La propiedad de la facturación de las suscripciones a Azure se transfiere a la sección de factura. Realice un seguimiento de los cargos de estas suscripciones en [Azure Portal](https://portal.azure.com).
- Asigne permisos para consultar y administrar la facturación de estas suscripciones a otros usuarios. Para más información, consulte [Tareas y roles de la sección de factura](billing-understand-mca-roles.md#invoice-section-roles-and-tasks).
