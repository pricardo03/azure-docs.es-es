---
title: Descarga de los datos de uso diario y la factura de Azure | Microsoft Docs
description: En este artículo se describe cómo descargar o ver los datos de uso diario y la factura de facturación de Azure.
keywords: factura, facturación, descarga de factura, factura de Azure, uso de Azure
services: billing
documentationcenter: ''
author: genlin
manager: adpick
editor: ''
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: banders
ms.openlocfilehash: b78fb7d697f8a72b3c2f99c4509ea6ac5c5e5566
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60616458"
---
# <a name="download-or-view-your-azure-billing-invoice-and-daily-usage-data"></a>Procedimiento para descargar las datos de uso diario y de factura de Azure

En la mayoría de las suscripciones, puede descargar la factura desde [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) o recibirla por correo electrónico. Si es cliente de Azure con un Contrato Enterprise (cliente de EA), no puede descargar las facturas de su organización. Las facturas se envían a la persona que se configuró para recibir las facturas de la inscripción.

Si es usted un cliente de EA o tiene un [contrato de cliente de Microsoft](#check-access-to-a-microsoft-customer-agreement), puede descargar el uso en [Azure Portal](https://portal.azure.com/). Si se trata de otras suscripciones, vaya al [Centro de cuentas de Azure](https://account.azure.com/Subscriptions) para descargar el uso.

Solo determinados roles tienen permiso para obtener la información de facturación y uso, como el administrador de la cuenta o el administrador de Enterprise. Para obtener más información sobre cómo obtener acceso a la información de facturación, vea [Manage access to Azure billing using roles](billing-manage-access.md) (Administrar el acceso a la facturación de Azure mediante roles).

Si tiene un contrato de cliente de Microsoft, debe ser el propietario del perfil de facturación, colaborador, lector o administrador de facturación para ver información de facturación y uso. Para más información acerca de los roles de facturación para los contratos de cliente de Microsoft, consulte [Tareas y roles del perfil de facturación](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="download-your-azure-invoices-pdf"></a>Descarga de las facturas de Azure (.pdf)

En la mayoría de las suscripciones, puede descargar la factura desde Azure Portal. Si tiene un contrato de cliente de Microsoft, consulte cómo descargar facturas para un perfil de facturación.

### <a name="download-invoices-for-an-individual-subscription"></a>Descarga de facturas para una suscripción individual

1. Seleccione su suscripción en la [página Suscripciones](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) de Azure Portal como [un usuario con acceso a las facturas](billing-manage-access.md).

2. Seleccione **Facturas**.

    ![Captura de pantalla que muestra la opción Facturación y uso](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png)

3. Haga clic en **Descargar factura** para ver una copia de la factura en PDF. Si muestra **No disponible**, vea [¿Por qué no veo una factura para el último período de facturación?](#noinvoice)

    ![Captura de pantalla que muestra los períodos de facturación, la opción de descarga y los cargos totales para cada período de facturación](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

4. También puede ver el uso diario si hace clic en el período de facturación.

Para más información sobre la factura, consulte [Comprender la factura de Microsoft Azure](billing-understand-your-bill.md). Para ayudar a administrar los costos, consulte [Prevención de costos inesperados con la administración de costos y facturación de Azure](billing-getting-started.md).

### <a name="download-invoices-for-a-microsoft-customer-agreement"></a>Descarga de facturas para un contrato de cliente de Microsoft

Las facturas se generan para cada [perfil de facturación](billing-mca-overview.md#understand-billing-profiles) en el contrato de cliente de Microsoft. Debe ser el propietario del perfil de facturación, colaborador, lector o administrador de facturación para descargar facturas desde Azure Portal.

1. Busque en **Administración de costos + facturación**.
2. Seleccione un perfil de facturación.
3. Seleccione **Facturas**.
4. En la cuadrícula de la factura, busque la fila de la factura que desea descargar.
5. Haga clic en el botón de puntos suspensivos (`...`) al final de la fila.
6. En el menú contextual de descarga, seleccione **Factura**.

Si no ve una factura del último período de facturación, consulte **Información adicional**. <!-- Fix this -->
### <a name="noinvoice"></a> ¿Por qué no veo una factura para el último período de facturación?

Pueden existir varias razones por las que no ve una factura:

- Han transcurrido menos de 30 días desde el día que se suscribió a Azure.

- La factura no se ha generado aún. Espere hasta el final del período de facturación.

- No tiene permiso para ver facturas. Si tiene un contrato de cliente de Microsoft, debe ser el propietario del perfil de facturación, colaborador, lector o administrador de facturación. Para otras suscripciones, es posible que no vea las facturas antiguas si no es el administrador de cuenta. Para obtener más información sobre cómo obtener acceso a la información de facturación, vea [Manage access to Azure billing using roles](billing-manage-access.md) (Administrar el acceso a la facturación de Azure mediante roles).

- Si tiene una evaluación gratuita o una cantidad de crédito mensual con la suscripción que no ha superado, no obtendrá una factura a menos que tenga un contrato de cliente de Microsoft.

## <a name="get-your-invoice-in-email-pdf"></a>Obtención de la factura por correo electrónico (.pdf)

Puede optar a destinatarios adicionales y configurarlos para recibir la factura de Azure en un correo electrónico. Esta característica puede no estar disponible para determinadas suscripciones, como ofertas de soporte técnico, contratos Enterprise o Azure bajo licencia Open. Si tiene un contrato de cliente de Microsoft, consulte cómo obtener las facturas del perfil de facturación por correo electrónico.

### <a name="get-your-subscriptions-invoices-in-email"></a>Obtención de las facturas de su suscripción por correo electrónico

1. Seleccione su suscripción en la [página Suscripciones](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Tendrá que habilitar la opción en cada suscripción que posea. Haga clic en **Facturas** y luego en **Email my invoice** (Enviar mi factura por correo electrónico).

    ![Captura de pantalla que muestra el flujo de participación](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)

2. Haga clic en **Recibir notificaciones** y acepte los términos.

    ![Captura de pantalla que muestra el paso 2 del flujo de participación](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)

3. Una vez que ha aceptado el contrato, puede configurar destinatarios adicionales. Cuando se quita un destinatario, ya no se almacena la dirección de correo electrónico. Si cambia de opinión, deberá volver a agregarlos.

    ![Captura de pantalla que muestra el paso 3 del flujo de participación](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)

Si no recibe un correo electrónico después de seguir los pasos, asegúrese de que su dirección de correo electrónico es correcta en las [preferencias de comunicación de su perfil](https://account.windowsazure.com/profile).

### <a name="opt-out-of-getting-your-subscriptions-invoices-in-email"></a>Opción de no recibir las facturas de su suscripción por correo electrónico

Puede optar por dejar de recibir la factura por correo electrónico; para ello, siga los pasos anteriores y haga clic en **Deshabilitar el envío de facturas por correo electrónico**. Esta opción eliminará todas las direcciones de correo electrónico configuradas para recibir las facturas por correo electrónico. Si cambia de opinión, tendrá que volver a configurar los destinatarios.

 ![Captura de pantalla que muestra el flujo para deshabilitar](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep4.PNG)

### <a name="get-your-microsoft-customer-agreement-invoices-in-email"></a>Obtención de las facturas del contrato de cliente de Microsoft por correo electrónico

Si tiene un contrato de cliente de Microsoft, puede optar por recibir la factura por correo electrónico. La factura se envía por correo electrónico a los propietarios del perfil de facturación, colaboradores, lectores y administradores de facturación. Los lectores no pueden actualizar la preferencia de factura por correo electrónico.

1. Busque en **Administración de costos + facturación**.
1. Seleccione un perfil de facturación.
1. En **Configuración**, seleccione **Propiedades**.
1. En **Enviar factura por correo electrónico**, seleccione **Actualizar la preferencia de factura por correo electrónico**.
1. Seleccione **Habilitar envío**.
1. Haga clic en **Update**(Actualizar).

### <a name="opt-out-of-getting-your-billing-profile-invoices-in-email"></a>Opción de no recibir las facturas del perfil de facturación por correo electrónico

Puede optar por dejar de recibir la factura por correo electrónico; para ello, siga los pasos anteriores y haga clic en **Deshabilitar envío**. Todos los propietarios, colaboradores, lectores y administradores de facturación dejarán también de recibir la factura por correo electrónico. Si es lector, no puede cambiar la preferencia de factura por correo electrónico.

## <a name="download-usage"></a>Descarga del uso

 Para la mayoría de las suscripciones, busque el archivo de uso diario en el [Centro de cuentas de Azure](https://account.azure.com/Subscriptions). Si es usted un cliente de EA o tiene un contrato de cliente de Microsoft, puede descargar el uso en [Azure Portal](https://portal.azure.com/). <!-- TO DO: update PayG experience to Ibiza once it ships-->

### <a name="download-usage-from-the-account-center-csv"></a>Descarga del uso desde el centro de cuentas (.csv)

1. Inicie sesión en el [Centro de cuentas de Azure](https://account.windowsazure.com/subscriptions) como administrador de la cuenta.

2. Seleccione la suscripción para la que quiere la información de factura y uso.

3. Seleccione **HISTORIAL DE FACTURACIÓN**.

    ![Captura de pantalla que muestra la opción del historial de facturación](./media/billing-download-azure-invoice-daily-usage-date/Billinghisotry.png)

4. Puede ver sus extractos de los últimos seis períodos de facturación y el período actual sin facturar.

    ![Captura de pantalla que muestra los períodos de facturación, las opciones para descargar la factura y uso diario, y los cargos totales para cada período de facturación](./media/billing-download-azure-invoice-daily-usage-date/billingSum.png)

5. Seleccione **Ver extracto actual** para ver una estimación de los cargos en el momento en que se generó la estimación. Esta información solo se actualiza diariamente y es posible que no incluya todo el uso. La factura mensual puede ser distinta a esta estimación.

    ![Captura de pantalla que muestra la opción Ver extracto actual](./media/billing-download-azure-invoice-daily-usage-date/billingSum2.png)

    ![Captura de pantalla que muestra la estimación de los cargos actuales](./media/billing-download-azure-invoice-daily-usage-date/billingSum3.png)

6. Seleccione **Descargar uso** para descargar los datos de uso diario como un archivo CSV. Si ve dos versiones disponibles, descargue la versión 2.

    ![Captura de pantalla que muestra la opción Descargar uso](./media/billing-download-azure-invoice-daily-usage-date/DLusage.png)

Solo el administrador de la cuenta puede tener acceso al Centro de cuentas de Azure. Otros administradores de facturación, como un propietario, pueden obtener información de uso mediante las [API de facturación](billing-usage-rate-card-overview.md).

Para más información sobre el uso diario, consulte [Comprender la factura de Microsoft Azure](billing-understand-your-bill.md). Para ayudar a administrar los costos, consulte [Prevención de costos inesperados con la administración de costos y facturación de Azure](billing-getting-started.md).

### <a name="download-usage-for-ea-customers"></a>Descarga del uso para los clientes de EA

Para ver y descargar los datos de uso como cliente de EA, debe ser administrador de Enterprise, o bien Propietario de la cuenta o Administrador de departamento con la directiva para ver los cargos habilitada.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Busque *Administración de costos + facturación*.

    ![Captura de pantalla que muestra la búsqueda en Azure Portal](./media/billing-download-azure-invoice-daily-usage-date/portal-cm-billing-search.png)

1. Seleccione **Uso + cargos**.
1. Seleccione **Descargar** para el mes que quiere descargar.

### <a name="download-usage-for-your-microsoft-customer-agreement"></a>Descarga del uso para un contrato de cliente de Microsoft

Para ver y descargar datos de uso de un perfil de facturación, debe ser el propietario del perfil de facturación, colaborador, lector o administrador de facturación.

#### <a name="download-usage-for-billed-charges"></a>Descarga del uso para los cargos de facturación

1. Busque en **Administración de costos + facturación**.
2. Seleccione un perfil de facturación.
3. Seleccione **Facturas**.
4. En la cuadrícula de la factura, busque la fila de la factura correspondiente al uso que desea descargar.
5. Haga clic en el botón de puntos suspensivos (`...`) al final de la fila.
6. En el menú contextual de descarga, seleccione **Uso y cargos de Azure**.

#### <a name="download-usage-for-open-charges"></a>Descarga del uso para los cargos abiertos

También puede descargar el uso del mes hasta la fecha para el período de facturación actual; esto es, los cargos que aún no se han facturado.

1. Busque en **Administración de costos + facturación**.
2. Seleccione un perfil de facturación.
3. En la hoja **Información general**, haga clic en **Uso y cargos de Azure**.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Comprobación del acceso a un contrato de cliente de Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Ponerse en contacto con nosotros

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de la factura y los cargos, consulte:

- [Comprender la factura de Microsoft Azure](billing-understand-your-bill.md)
- [Descripción de los términos en su factura de Microsoft Azure](billing-understand-your-invoice.md)
- [Descripción de los términos de uso detallados de Microsoft Azure](billing-understand-your-usage.md)
- [Ver y descargar los precios de Azure de su organización](billing-ea-pricing.md)

Si tiene un contrato de cliente de Microsoft, vea:

- [Comprender los cargos de facturación de su contrato de cliente de Microsoft](billing-mca-understand-your-bill.md)
- [Descripción de los términos en la factura del contrato de cliente de Microsoft](billing-mca-understand-your-invoice.md)
- [Descripción de los términos en los CSV de cargos y el uso de Azure para un contrato de cliente de Microsoft](billing-mca-understand-your-usage.md)
- [Ver los documentos de impuestos del contrato de cliente de Microsoft](billing-mca-download-tax-document.md)
- [Ver y descargar los precios de Azure de su organización](billing-ea-pricing.md)
