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
ms.openlocfilehash: 52e227b933533bf60a474d0417c57e4b75c7978f
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58224641"
---
# <a name="download-or-view-your-azure-billing-invoice-and-daily-usage-data"></a>Procedimiento para descargar las datos de uso diario y de factura de Azure

En la mayoría de las suscripciones, puede descargar la factura desde [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) o recibirla por correo electrónico. Si es cliente de Azure con un Contrato Enterprise (cliente de EA), no puede descargar las facturas de su organización. Las facturas se envían a la persona que se configuró para recibir las facturas de la inscripción.

Si un cliente de EA o tienen un [contrato del cliente de Microsoft](#check-access-to-a-microsoft-customer-agreement), puede descargar uso en el [portal Azure](https://portal.azure.com/). Para otras suscripciones, vaya a la [centro de cuentas de Azure](https://account.azure.com/Subscriptions) descargar uso.

Solo determinados roles tienen permiso para obtener información de factura y uso, como el Administrador de cuenta o administrador de empresa de facturación. Para obtener más información sobre cómo obtener acceso a la información de facturación, vea [Manage access to Azure billing using roles](billing-manage-access.md) (Administrar el acceso a la facturación de Azure mediante roles).

Si tiene un contrato de cliente de Microsoft, debe ser un perfil de facturación propietario, Colaborador, lector, o el administrador para ver información de facturación y uso de la factura. Para más información acerca de las funciones de facturación para los contratos de cliente de Microsoft, consulte [perfil roles y tareas de facturación](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="download-your-azure-invoices-pdf"></a>Descargar las facturas de Azure (.pdf)

Para la mayoría de las suscripciones, puede descargar la factura desde Azure portal. Si tiene un contrato de cliente de Microsoft, consulte la descarga de facturas para un perfil de facturación.

### <a name="download-invoices-for-an-individual-subscription"></a>Descargar facturas para una suscripción individual

1. Seleccione su suscripción en la [página suscripciones](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) en el portal de Azure como [un usuario con acceso a las facturas](billing-manage-access.md).

2. Seleccione **Facturas**.

    ![Captura de pantalla que muestra la opción Facturación y uso](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png)

3. Haga clic en **Descargar factura** para ver una copia de la factura en PDF. Si muestra **No disponible**, vea [¿Por qué no veo una factura para el último período de facturación?](#noinvoice)

    ![Captura de pantalla que muestra los períodos de facturación, la opción de descarga y los cargos totales para cada período de facturación](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

4. También puede ver el uso diario si hace clic en el período de facturación.

Para más información sobre la factura, consulte [Comprender la factura de Microsoft Azure](billing-understand-your-bill.md). Para ayudar a administrar los costos, consulte [Prevención de costos inesperados con la administración de costos y facturación de Azure](billing-getting-started.md).

### <a name="download-invoices-for-a-microsoft-customer-agreement"></a>Descargar facturas para un contrato de cliente de Microsoft

Las facturas se generan para cada [el perfil de facturación](billing-mca-overview.md#understand-billing-profiles) en el contrato del cliente de Microsoft. Debe ser un perfil de facturación propietario, Colaborador, lector, o para descargar facturas desde el portal de Azure de factura.

1. Busque en **Administración de costos + facturación**.
2. Seleccione un perfil de facturación.
3. Seleccione **Facturas**.
4. En la cuadrícula de la factura, busque la fila de la factura que desea descargar.
5. Haga clic en el botón de puntos suspensivos (`...`) al final de la fila.
6. En el menú contextual de descarga, seleccione **factura**.

Si no ve una factura del último período de facturación, consulte **información adicional**. <!-- Fix this -->
### <a name="noinvoice"></a> ¿Por qué no veo una factura para el último período de facturación?

Pueden existir varias razones por las que no ve una factura:

- Han transcurrido menos de 30 días desde el día que se suscribió a Azure.

- La factura no se ha generado aún. Espere hasta el final del período de facturación.

- No tiene permiso para ver las facturas. Si tiene un contrato de cliente de Microsoft, debe ser el perfil de facturación propietario, Colaborador, lector, o el Administrador de facturas. Para otras suscripciones, es posible que no verá las facturas antiguas si no es el Administrador de cuenta. Para obtener más información sobre cómo obtener acceso a la información de facturación, vea [Manage access to Azure billing using roles](billing-manage-access.md) (Administrar el acceso a la facturación de Azure mediante roles).

- Si tiene una evaluación gratuita o una cantidad de crédito mensual con la suscripción que no ha superado, no obtendrá una factura a menos que tenga un contrato de cliente de Microsoft.

## <a name="get-your-invoice-in-email-pdf"></a>Obtención de la factura por correo electrónico (.pdf)

Puede optar a destinatarios adicionales y configurarlos para recibir la factura de Azure en un correo electrónico. Esta característica puede no estar disponible para determinadas suscripciones, como ofertas de soporte técnico, contratos Enterprise o Azure bajo licencia Open. Si tiene un contrato de Microsoft Customer, consulte obtener su perfil de facturación de facturas por correo electrónico.

### <a name="get-your-subscriptions-invoices-in-email"></a>Obtener facturas de su suscripción de correo electrónico

1. Seleccione su suscripción en la [página Suscripciones](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Tendrá que habilitar la opción en cada suscripción que posea. Haga clic en **Facturas** y luego en **Email my invoice** (Enviar mi factura por correo electrónico).

    ![Captura de pantalla que muestra el flujo de participación](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)

2. Haga clic en **Recibir notificaciones** y acepte los términos.

    ![Captura de pantalla que muestra el paso 2 del flujo de participación](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)

3. Una vez que ha aceptado el contrato, puede configurar destinatarios adicionales. Cuando se quita un destinatario, ya no se almacena la dirección de correo electrónico. Si cambia de opinión, deberá volver a agregarlos.

    ![Captura de pantalla que muestra el paso 3 del flujo de participación](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)

Si no recibe un correo electrónico después de seguir los pasos, asegúrese de que su dirección de correo electrónico es correcta en las [preferencias de comunicación de su perfil](https://account.windowsazure.com/profile).

### <a name="opt-out-of-getting-your-subscriptions-invoices-in-email"></a>Dejar de participar en obtener facturas de su suscripción de correo electrónico

Puede optar al obtener la factura por correo electrónico siguiendo los pasos anteriores y haga clic en **rechazar facturas por correo electrónico**. Esta opción eliminará todas las direcciones de correo electrónico configuradas para recibir las facturas por correo electrónico. Si participa en se pueden volver a configurar a los destinatarios.

 ![Captura de pantalla que muestra el flujo para deshabilitar](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep4.PNG)

### <a name="get-your-microsoft-customer-agreement-invoices-in-email"></a>Obtener las facturas de contrato de cliente de Microsoft por correo electrónico

Si tiene un contrato de cliente de Microsoft, puede participar en la obtención de la factura en un correo electrónico. Todos los facturación perfil propietarios, colaboradores, lectores y factura administradores obtendrá la factura por correo electrónico. Los lectores no pueden actualizar la preferencia de factura por correo electrónico.

1. Busque en **Administración de costos + facturación**.
1. Seleccione un perfil de facturación.
1. En **Configuración**, seleccione **Propiedades**.
1. En **factura por correo electrónico**, seleccione **preferencia de factura de correo electrónico de actualización**.
1. Seleccione **participar**.
1. Haga clic en **Update**(Actualizar).

### <a name="opt-out-of-getting-your-billing-profile-invoices-in-email"></a>Dejar de participar en obtener las facturas de perfil de facturación por correo electrónico

Puede optar al obtener la factura por correo electrónico siguiendo los pasos anteriores y haga clic en **participar**. Todos los propietarios, colaboradores, lectores y factura administradores se abandona se empieza a la factura por correo electrónico, ser demasiado. Si es un lector, no se puede cambiar la preferencia de factura por correo electrónico.

## <a name="download-usage"></a>Descarga del uso

 Para la mayoría de las suscripciones, busque el archivo de uso diario en el [Centro de cuentas de Azure](https://account.azure.com/Subscriptions). Si el cliente de EA o tienen un contrato de cliente de Microsoft, puede descargar uso en el [portal Azure](https://portal.azure.com/). <!-- TO DO: update PayG experience to Ibiza once it ships-->

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

Para ver y descargar datos de uso como un cliente EA, debe ser un administrador de empresa, el propietario de la cuenta, o con la vista del Administrador de departamento cobra directiva habilitada.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Busque *costo Management + facturación*.

    ![Captura de pantalla que muestra la búsqueda en Azure Portal](./media/billing-download-azure-invoice-daily-usage-date/portal-cm-billing-search.png)

1. Seleccione **Uso + cargos**.
1. Seleccione **Descargar** para el mes que quiere descargar.

### <a name="download-usage-for-your-microsoft-customer-agreement"></a>Descargar el uso de su contrato de cliente de Microsoft

Para ver y descargar datos de uso de un perfil de facturación, debe ser un perfil de facturación propietario, Colaborador, lector, o el Administrador de facturas.

#### <a name="download-usage-for-billed-charges"></a>Descargar uso para los cargos de facturación

1. Busque en **Administración de costos + facturación**.
2. Seleccione un perfil de facturación.
3. Seleccione **Facturas**.
4. En la cuadrícula de la factura, busque la fila de la factura correspondiente al uso que desea descargar.
5. Haga clic en el botón de puntos suspensivos (`...`) al final de la fila.
6. En el menú contextual de descarga, seleccione **cargos y el uso de Azure**.

#### <a name="download-usage-for-open-charges"></a>Descargar uso para abrir cargos

También puede descargar uso del mes hasta la fecha para el período de facturación actual, lo que significa que disponen de los cargos no se ha facturado todavía.

1. Busque en **Administración de costos + facturación**.
2. Seleccione un perfil de facturación.
3. En el **Introducción** hoja, haga clic en **cargos y el uso de Azure descargar**.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Comprobar el acceso a un contrato de cliente de Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Ponerse en contacto con nosotros

Si tiene alguna pregunta o necesita ayuda, [crear una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de la factura y los cargos, consulte:

- [Comprender la factura de Microsoft Azure](billing-understand-your-bill.md)
- [Descripción de los términos en la factura de Azure](billing-understand-your-invoice.md)
- [Descripción de los términos de uso detallados de Microsoft Azure](billing-understand-your-usage.md)
- [Ver los precios de Azure de su organización](billing-ea-pricing.md)

Si tiene un contrato de cliente de Microsoft, consulte:

- [Comprender los cargos en la factura para el perfil de facturación ](billing-mca-understand-your-bill.md)
- [Descripción de los términos de la factura para el perfil de facturación](billing-mca-understand-your-invoice.md)
- [Comprender el archivo de uso y los cargos de Azure para el perfil de facturación](billing-mca-understand-your-usage.md)
- [Ver y descargar documentos de impuestos para el perfil de facturación](billing-mca-download-tax-document.md)
- [Ver los precios de Azure de su organización](billing-ea-pricing.md)
