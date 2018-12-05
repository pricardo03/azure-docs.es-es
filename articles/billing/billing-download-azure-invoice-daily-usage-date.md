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
ms.date: 11/16/2018
ms.author: cwatson
ms.openlocfilehash: 80721fc82a54c62c982298cb8eabb999caaf1dfb
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "52583115"
---
# <a name="download-or-view-your-azure-billing-invoice-and-daily-usage-data"></a>Procedimiento para descargar las datos de uso diario y de factura de Azure

En la mayoría de las suscripciones, puede descargar la factura desde [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) o recibirla por correo electrónico. Si es cliente de Azure con un Contrato Enterprise (cliente de EA), no puede descargar las facturas de su organización. Las facturas se envían a la persona que se configuró para recibir las facturas de la inscripción.

Si quiere descargar el uso como cliente de EA, este se encuentra disponible en [Azure Portal](https://portal.azure.com/) > **Administración de costos + Facturación** > **Uso + cargos**. Si se trata de otras suscripciones, vaya al [Centro de cuentas de Azure](https://account.azure.com/Subscriptions).

Solo determinados roles tienen permiso para obtener la información de facturación y uso, como el administrador de la cuenta o el administrador de Enterprise. Para obtener más información sobre cómo obtener acceso a la información de facturación, vea [Manage access to Azure billing using roles](billing-manage-access.md) (Administrar el acceso a la facturación de Azure mediante roles).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="download-or-view-your-invoice"></a>Descarga o visualización de la factura

 Si es cliente de EA, no puede descargar las facturas de su organización. Las facturas se envían a la persona que se configuró para recibir las facturas de la inscripción. Si se trata de otras suscripciones, puede recibir la factura por correo electrónico o descargarla desde Azure Portal.

### <a name="get-your-invoice-in-email-pdf"></a>Obtención de la factura por correo electrónico (.pdf)
Puede optar a destinatarios adicionales y configurarlos para recibir la factura de Azure en un correo electrónico. Esta característica puede no estar disponible para determinadas suscripciones, como ofertas de soporte técnico, contratos Enterprise o Azure bajo licencia Open.

1. Seleccione su suscripción en la [página Suscripciones](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Tendrá que habilitar la opción en cada suscripción que posea. Haga clic en **Facturas** y luego en **Email my invoice** (Enviar mi factura por correo electrónico). 

    ![Captura de pantalla que muestra el flujo de participación](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)
    
2. Haga clic en **Recibir notificaciones** y acepte los términos.

    ![Captura de pantalla que muestra el paso 2 del flujo de participación](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)
 
3. Una vez que ha aceptado el contrato, puede configurar destinatarios adicionales. Cuando se quita un destinatario, ya no se almacena la dirección de correo electrónico. Si cambia de opinión, deberá volver a agregarlos.

    ![Captura de pantalla que muestra el paso 3 del flujo de participación](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)
    
Si no recibe un correo electrónico después de seguir los pasos, asegúrese de que su dirección de correo electrónico es correcta en las [preferencias de comunicación de su perfil](https://account.windowsazure.com/profile).

### <a name="opt-out-from-getting-your-invoice-in-email"></a>No participar en la obtención de factura por correo electrónico
Si no desea obtener su factura por correo electrónico, haga clic en **Deshabilitar el envío de facturas por correo electrónico**. Esta opción eliminará todas las direcciones de correo electrónico configuradas para recibir las facturas por correo electrónico. Si cambia de opinión, tendrá que volver a configurar los destinatarios.

 ![Captura de pantalla que muestra el flujo para deshabilitar](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep4.PNG)

### <a name="download-invoice-from-azure-portal-pdf"></a>Descarga de la factura desde Azure Portal (.pdf)

1. Seleccione su suscripción en la [página Suscripciones](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) de Azure Portal como [un usuario con acceso a las facturas](billing-manage-access.md).

2. Seleccione **Facturas**. 

    ![Captura de pantalla que muestra la opción Facturación y uso](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png) 

3. Haga clic en **Descargar factura** para ver una copia de la factura en PDF. Si muestra **No disponible**, vea [¿Por qué no veo una factura para el último período de facturación?](#noinvoice)

    ![Captura de pantalla que muestra los períodos de facturación, la opción de descarga y los cargos totales para cada período de facturación](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

4. También puede ver el uso diario si hace clic en el período de facturación. 

Para más información sobre la factura, consulte [Comprender la factura de Microsoft Azure](billing-understand-your-bill.md). Para ayudar a administrar los costos, consulte [Prevención de costos inesperados con la administración de costos y facturación de Azure](billing-getting-started.md).

### <a name="noinvoice"></a> ¿Por qué no veo una factura para el último período de facturación?

Pueden existir varias razones por las que no ve una factura:

- Tiene un importe de crédito mensual con la suscripción que no ha superado o tiene una evaluación gratuita. Solo se genera una factura cuando debe dinero.

- Han transcurrido menos de 30 días desde el día que se suscribió a Azure.

- La factura no se ha generado aún. Espere hasta el final del período de facturación.

- Si no es el administrador de la cuenta, es posible que las facturas anteriores no estén disponibles para usted.

## <a name="download-usage"></a>Descarga del uso

 Para la mayoría de las suscripciones, busque el archivo de uso diario en el [Centro de cuentas de Azure](https://account.azure.com/Subscriptions). Si quiere descargar el uso como cliente de EA, este se encuentra disponible en [Azure Portal](https://portal.azure.com/) > **Administración de costos + Facturación** > **Uso + cargos**. 

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

1. Inicie sesión en el [Azure Portal]( http://portal.azure.com).
1. Busque en **Administración de costos + facturación**.

    ![Captura de pantalla que muestra la búsqueda en Azure Portal](./media/billing-download-azure-invoice-daily-usage-date/portal-cm-billing-search.png)

1. Seleccione **Uso + cargos**.
1. Seleccione **Descargar** para el mes que quiere descargar.

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
