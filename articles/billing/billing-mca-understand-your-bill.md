---
title: Comprender los cargos de facturación del su contrato cliente de Microsoft - Azure | Microsoft Docs
description: Obtenga información sobre cómo leer y comprender los cargos en su factura
services: ''
documentationcenter: ''
author: jureid
manager: jureid
editor: ''
tags: billing
ms.assetid: 32eea268-161c-4b93-8774-bc435d78a8c9
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/19/2019
ms.author: banders
ms.openlocfilehash: f93152ae3db926fb989c219d1e515abaf0281bf4
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57837888"
---
# <a name="understand-the-charges-on-your-microsoft-customer-agreements-invoice"></a>Comprender los cargos de facturación del su contrato cliente de Microsoft

Puede comprender los cargos en su factura mediante el análisis de las transacciones individuales.

En la cuenta de facturación para un contrato de cliente de Microsoft, se genera una factura cada mes para cada perfil de facturación. La factura incluye todos los cargos del mes anterior. Puede ver las facturas en el portal de Azure. Para obtener más información, consulte [descargar facturas para un contrato de cliente de Microsoft](billing-download-azure-invoice-daily-usage-date.md#download-invoices-for-a-microsoft-customer-agreement).

En este artículo se aplica a una cuenta de facturación para un contrato de cliente de Microsoft. [Compruebe si tiene acceso a un contrato de cliente de Microsoft](#check-access-to-a-microsoft-customer-agreement).

## <a name="view-transactions-for-an-invoice-in-the-azure-portal"></a>Ver las transacciones de una factura en el portal de Azure

1. Inicie sesión en el [Azure Portal](https://www.azure.com).

2. Buscar en **costo Management + facturación**.

    ![Captura de pantalla que muestra la búsqueda de Azure portal de administración de costos + facturación](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png)

3. Seleccione **todas las transacciones** del lado izquierdo de la pantalla. Según el acceso puede que tenga que seleccionar una cuenta de facturación o un perfil de facturación, a continuación, seleccione **todas las transacciones**.

4. La página de todas las transacciones muestra la siguiente información:

    ![Captura de pantalla que muestra la lista de transacciones de facturación](./media/billing-understand-your-bill-mca/mca-billed-transactions-list.png)

    |Columna  |Definición  |
    |---------|---------|
    |Date     | La fecha de transacción  |
    |Id. de factura     | El identificador de la factura en el que se obtuvo facturar la transacción. Si envía una solicitud de soporte técnico, comparten el identificador con el soporte técnico de Azure para acelerar su solicitud de soporte técnico |
    |Tipo de transacción     |  El tipo de transacción, como los cargos de compra, cancel y uso  |
    |Familia de productos     | La categoría de producto, como el proceso para máquinas virtuales o de base de datos de SQL Azure|
    |Sku del producto     | Un código único que identifica la instancia del producto |
    |Importe     |  La cantidad de transacciones      |
    |Secciones de factura     | La transacción se muestra en esta sección de factura del perfil de facturación |
    |Perfil de facturación     | La transacción se muestra en la factura de este perfil de facturación |

5. Buscar en el identificador de la factura para filtrar las transacciones de la factura.

### <a name="view-transactions-by-invoice-sections"></a>Ver las transacciones en las secciones de la factura

Factura secciones permiten organizar los costos en la factura de un perfil de facturación. Para obtener más información, consulte [comprender la factura sección](billing-mca-overview.md#understand-invoice-sections). Cuando se genera una factura, reflejan los cargos para todas las secciones en el perfil de facturación en la factura.

La siguiente imagen muestra los cargos de la sección de factura del departamento de contabilidad en una factura de ejemplo.

![Imagen de ejemplo que muestra los detalles de la información de la sección de factura](./media/billing-understand-your-bill-mca/invoicesection-details.png)

Una vez que haya identificado los cargos por una sección de la factura, puede ver las transacciones en el portal de Azure para comprender los cargos.

1. Vaya a la página de todas las transacciones en Azure portal para ver las transacciones de una factura. Para obtener más información, consulte [ver las transacciones de una factura en el portal de Azure](#view-transactions-for-an-invoice-in-the-azure-portal).

2. Filtrar por nombre de sección de factura para ver las transacciones de la sección de factura.

## <a name="understand-pending-charges-to-estimate-your-next-invoice"></a>Comprender los cargos pendientes de para estimar la próxima factura

En la cuenta de facturación para un contrato de cliente de Microsoft, hasta que se facturan los cargos, son estimaciones y considera pendientes. Puede ver pendientes cargos en el portal de Azure para estimar la próxima factura. Los cargos pendientes son estimaciones y no incluyen impuestos, por lo que los cargos reales en la próxima factura varían en función de los cargos pendientes.

### <a name="view-summary-of-pending-charges"></a>Ver el resumen de los cargos pendientes de

1. Inicie sesión en el [Azure Portal](https://www.azure.com).

2. Buscar en **costo Management + facturación**.

   ![Captura de pantalla que muestra la búsqueda de Azure portal de administración de costos + facturación](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png)

3. Seleccione un perfil de facturación. Según el acceso, debe seleccionar una cuenta de facturación. En la cuenta de facturación, seleccione **facturación perfiles** , a continuación, seleccione un perfil de facturación.

4. Seleccione **resumen** ficha desde la parte superior de la pantalla.

5. La sección cargos muestra el mes hasta la fecha y los cargos del mes pasado.

   ![Captura de pantalla que muestra la búsqueda de Azure portal de administración de costos + facturación](./media/billing-understand-your-bill-mca/mca-billing-profile-summary.png)

Los cargos del mes hasta la fecha son los cargos pendientes para el mes actual y se le cobrará cuando se genera la factura del mes. Si no todavía se genera la factura para el mes pasado, los cargos del mes pasado también están pendientes y se reflejarán en la próxima factura.

### <a name="view-pending-transactions"></a>Ver las transacciones pendientes

Una vez identificados los cargos pendientes de, puede comprender los cargos mediante el análisis de las transacciones individuales que han contribuido a los cargos. En este momento, pendientes uso cargos no se muestran en la página de transacción todas. Puede ver los cargos de uso pendientes en la página suscripciones de Azure. Para obtener más información, consulte [Ver pendientes los cargos de uso](#view-pending-usage-charges)

1. Inicie sesión en el [Azure Portal](https://www.azure.com).

2. Buscar en **costo Management + facturación**.

   ![Captura de pantalla que muestra la búsqueda de Azure portal de administración de costos + facturación](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png)

3. Seleccione un perfil de facturación. Según el acceso, debe seleccionar una cuenta de facturación. En la cuenta de facturación, seleccione **facturación perfiles** , a continuación, seleccione un perfil de facturación.

4. Seleccione **todas las transacciones** del lado izquierdo de la pantalla.

5. Busque **pendiente**. Use la **Timespan** filtro para ver pendientes cargos actual o último mes.

   ![Captura de pantalla que muestra la lista de transacciones pendientes](./media/billing-understand-your-bill-mca/mca-pending-transactions-list.png)

### <a name="view-pending-usage-charges"></a>Ver los cargos de uso pendientes

1. Inicie sesión en el [Azure Portal](https://www.azure.com).

2. Buscar en **costo Management + facturación**.

   ![Captura de pantalla que muestra la búsqueda de Azure portal de administración de costos + facturación](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png)

3. Seleccione un perfil de facturación. Según el acceso, debe seleccionar una cuenta de facturación. En la cuenta de facturación, seleccione **facturación perfiles** , a continuación, seleccione un perfil de facturación.

4. Seleccione **todas las suscripciones** del lado izquierdo de la pantalla.

5. La página de suscripciones de Azure muestra actuales y gastos del último mes para cada suscripción en el perfil de facturación. Los cargos del mes hasta la fecha son los cargos pendientes para el mes actual y se le cobrará cuando se genera la factura del mes. Si no todavía se genera la factura para el mes pasado, los cargos del mes pasado también están pendientes.

    ![Captura de pantalla que muestra la lista de suscripciones de Azure para el perfil de facturación](./media/billing-understand-your-bill-mca/mca-billing-profile-subscriptions-list.png)

## <a name="analyze-your-azure-usage-charges"></a>Analizar los cargos de uso de Azure

Use el archivo csv de uso y los cargos de Azure para analizar los cargos de uso. Puede descargar el archivo para una factura o pendiente de cargos. Para obtener más información, consulte [obtener los datos de uso diario y la factura de facturación de Azure](billing-download-azure-invoice-daily-usage-date.md).

### <a name="view-detailed-usage-by-invoice-section"></a>Ver el uso detallado mediante la sección de factura

Puede filtrar el archivo de uso y los cargos de Azure para conciliar los cargos de uso para las secciones de la factura.

Los siguientes pasos le guiarán a través de conciliación de los gastos de proceso para la sección de factura del departamento de contabilidad:

![Imagen de ejemplo que muestra los detalles de la información de la sección de factura](./media/billing-understand-your-bill-mca/invoicesection-details.png)

 | Factura PDF | Uso de Azure y los cargos de CSV |
 | --- | --- |
 |Departamento de contabilidad |invoiceSectionName |
 |Cargos de uso - Plan de Microsoft Azure |productOrderName |
 |Proceso |serviceFamily |

1. Filtro del **invoiceSectionName** columna en el archivo CSV **departamento de contabilidad**.
2. Filtro del **productOrderName** columna en el archivo CSV **Plan de Microsoft Azure**.
3. Filtro del **serviceFamily** columna en el archivo CSV **Microsoft.Compute**.

![Captura de pantalla que muestra el uso y cobra archivo filtrado por la sección de factura](./media/billing-understand-your-bill-mca/billing-usage-file-filtered-by-invoice-section.png)

<!--Todo Add screenshot of usage file -->

### <a name="view-detailed-usage-by-subscription"></a>Ver el uso detallado por suscripción

Puede filtrar el archivo de csv de uso y los cargos Azure para conciliar los cargos de uso para las suscripciones. Para ver todas las suscripciones en un perfil de facturación, consulte [Ver pendientes los cargos de uso](#view-pending-usage-charges).

Una vez identificados los cargos para una suscripción, use el archivo csv de uso y los cargos de Azure para analizar los cargos.

Captura de pantalla siguiente muestra la lista de suscripciones en el portal de Azure.

![Captura de pantalla que muestra la lista de suscripciones de Azure para el perfil de facturación](./media/billing-understand-your-bill-mca/mca-billing-profile-subscriptions-list-highlighted.png)

Filtro del **subscriptionName** columna en el archivo CSV de uso y los cargos de Azure para **WA_Subscription** para ver los cargos de uso detallado de WA_Subscription.

![Captura de pantalla que muestra el uso y cobra archivo filtrado por suscripción](./media/billing-understand-your-bill-mca/billing-usage-file-filtered-by-subscription.png)

## <a name="pay-your-bill"></a>Pagar la factura

Instrucciones para pagar la factura se muestran en la parte inferior de la factura. [Aprenda cómo pagar](billing-mca-understand-your-invoice.md#how-to-pay).

Si ya ha pagado la factura, puede comprobar el estado del pago en la página de facturas en el portal de Azure.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Comprobar el acceso a un contrato de cliente de Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Ponerse en contacto con nosotros

Si tiene alguna pregunta o necesita ayuda, [crear una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de la factura y uso detallado, vea:

- [Obtención de los datos de uso diario y la factura de Azure](billing-download-azure-invoice-daily-usage-date.md)
- [Descripción de los términos en la factura del contrato de cliente de Microsoft](billing-mca-understand-your-invoice.md)
- [Descripción de los términos en el archivo CSV de uso de contrato de cliente de Microsoft](billing-mca-understand-your-usage.md)
