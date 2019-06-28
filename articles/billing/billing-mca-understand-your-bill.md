---
title: Descripción de los cargos de Azure en la factura del contrato de cliente de Microsoft | Microsoft Docs
description: Aprenda a leer y comprender los cargos en la factura.
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
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60372178"
---
# <a name="understand-the-charges-on-your-microsoft-customer-agreements-invoice"></a>Comprenda los cargos de la factura del contrato de cliente de Microsoft.

Puede comprender los cargos en la factura mediante el análisis de las transacciones individuales.

En la cuenta de facturación de un contrato de cliente de Microsoft, se genera una factura cada mes para cada perfil de facturación. La factura incluye todos los cargos del mes anterior. También puede ver las facturas en Azure Portal. Para más información, consulte [Descarga de facturas para un contrato de cliente de Microsoft](billing-download-azure-invoice-daily-usage-date.md#download-invoices-for-a-microsoft-customer-agreement).

Este artículo se aplica a una cuenta de facturación para un contrato de cliente de Microsoft. [Compruebe si tiene acceso a un contrato de cliente de Microsoft](#check-access-to-a-microsoft-customer-agreement).

## <a name="view-transactions-for-an-invoice-in-the-azure-portal"></a>Visualización de las transacciones de una factura en Azure Portal

1. Inicie sesión en el [Azure Portal](https://www.azure.com).

2. Busque en **Administración de costos + facturación**.

    ![Captura de pantalla que muestra la búsqueda en Azure Portal de Administración de costos + facturación](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png)

3. Seleccione **Todas las transacciones** en el lado izquierdo de la pantalla. Dependiendo de su acceso, es posible que tenga que seleccionar una cuenta de facturación o un perfil de facturación y, después, **Todas las transacciones**.

4. En la página Todas las transacciones se muestra la siguiente información:

    ![Captura de pantalla que muestra la lista de transacciones facturadas](./media/billing-understand-your-bill-mca/mca-billed-transactions-list.png)

    |Columna  |Definición  |
    |---------|---------|
    |Date     | La fecha de la transacción  |
    |El identificador de la factura     | El identificador de la factura en la se ha facturado la transacción. Si envía una solicitud de soporte técnico, comparta el identificador con el Soporte técnico de Azure para agilizar la solicitud de soporte técnico. |
    |Tipo de transacción     |  El tipo de transacción, como los cargos por compra, cancelación y uso  |
    |Familia de productos     | La categoría del producto, como proceso para máquinas virtuales o base de datos para la base de datos de Azure SQL|
    |SKU de producto     | Un código único que identifica la instancia del producto |
    |Importe     |  El importe de la transacción      |
    |Sección de factura     | La transacción se muestra en esta sección de la factura del perfil de facturación. |
    |Perfil de facturación     | La transacción se muestra en la factura de este perfil de facturación. |

5. Busque el identificador de la factura para filtrar las transacciones de la factura.

### <a name="view-transactions-by-invoice-sections"></a>Visualización de las transacciones por secciones de factura

Las secciones de factura permiten organizar los costos en una factura del perfil de facturación. Para más información, consulte [Descripción de las secciones de factura](billing-mca-overview.md#understand-invoice-sections). Cuando se genera una factura, los cargos de todas las secciones del perfil de facturación se reflejan en la factura.

En la imagen siguiente se muestran los cargos por la sección de factura del departamento de contabilidad en una factura de ejemplo.

![Imagen de ejemplo que muestra los detalles de la información de la sección de factura](./media/billing-understand-your-bill-mca/invoicesection-details.png)

Cuando haya identificado los cargos por una sección de factura, puede ver las transacciones en Azure Portal para entender los cargos.

1. Vaya a la página Todas las transacciones en Azure Portal para ver las transacciones de una factura. Para más información, consulte [Visualización de las transacciones de una factura en Azure Portal](#view-transactions-for-an-invoice-in-the-azure-portal).

2. Filtre por nombre de la sección de factura para ver las transacciones de la sección de factura.

## <a name="understand-pending-charges-to-estimate-your-next-invoice"></a>Descripción de los cargos pendientes para calcular la próxima factura

En la cuenta de facturación de un contrato de cliente de Microsoft, hasta que se facturan los cargos, se calculan y se consideran pendientes. Puede ver cargos pendientes en Azure Portal para calcular la próxima factura. Los cargos pendientes se calculan y no incluyen impuestos, por lo que los cargos reales en su próxima factura variarán de los cargos pendientes.

### <a name="view-summary-of-pending-charges"></a>Visualización del resumen de los cargos pendientes

1. Inicie sesión en el [Azure Portal](https://www.azure.com).

2. Busque en **Administración de costos + facturación**.

   ![Captura de pantalla que muestra la búsqueda en Azure Portal de Administración de costos + facturación](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png)

3. Seleccione un perfil de facturación. Dependiendo del acceso, es posible que tenga que seleccionar una cuenta de facturación. En la cuenta de facturación, seleccione **Perfiles de facturación** y, después, uno de los perfiles.

4. Seleccione la pestaña **Resumen** en la parte superior de la pantalla.

5. La sección de cargos muestra los cargos del mes hasta la fecha y del mes pasado.

   ![Captura de pantalla que muestra la búsqueda en Azure Portal de Administración de costos + facturación](./media/billing-understand-your-bill-mca/mca-billing-profile-summary.png)

Los cargos del mes hasta la fecha son los cargos pendientes para el mes actual y se facturan cuando se genera la factura para el mes. Si la factura del mes pasado todavía no se ha generado, entonces los cargos del mes pasado también están pendientes y se reflejarán en la próxima factura.

### <a name="view-pending-transactions"></a>Visualización de las transacciones pendientes

Una vez que se identifiquen los cargos pendientes, puede entender los cargos mediante el análisis de las transacciones individuales que contribuyeron a los cargos. En este momento, los cargos por uso pendientes no se muestran en la página Todas las transacciones. Puede ver los cargos por uso pendientes en la página de suscripciones a Azure. Para más información, consulte [Visualización de los cargos por uso pendientes](#view-pending-usage-charges).

1. Inicie sesión en el [Azure Portal](https://www.azure.com).

2. Busque en **Administración de costos + facturación**.

   ![Captura de pantalla que muestra la búsqueda en Azure Portal de Administración de costos + facturación](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png)

3. Seleccione un perfil de facturación. Dependiendo del acceso, es posible que tenga que seleccionar una cuenta de facturación. En la cuenta de facturación, seleccione **Perfiles de facturación** y, después, uno de los perfiles.

4. Seleccione **Todas las transacciones** en el lado izquierdo de la pantalla.

5. Busque **los pendientes**. Utilice el filtro **Intervalo de tiempo** para ver los cargos pendientes del mes actual o del último mes.

   ![Captura de pantalla que muestra la lista de transacciones pendientes](./media/billing-understand-your-bill-mca/mca-pending-transactions-list.png)

### <a name="view-pending-usage-charges"></a>Visualización de los cargos por uso pendientes

1. Inicie sesión en el [Azure Portal](https://www.azure.com).

2. Busque en **Administración de costos + facturación**.

   ![Captura de pantalla que muestra la búsqueda en Azure Portal de Administración de costos + facturación](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png)

3. Seleccione un perfil de facturación. Dependiendo del acceso, es posible que tenga que seleccionar una cuenta de facturación. En la cuenta de facturación, seleccione **Perfiles de facturación** y, después, uno de los perfiles.

4. Seleccione **Todas las suscripciones** en el lado izquierdo de la pantalla.

5. La página de suscripciones a Azure muestra los cargos del mes actual y del último mes para cada suscripción en el perfil de facturación. Los cargos del mes hasta la fecha son los cargos pendientes para el mes actual y se facturan cuando se genera la factura para el mes. Si la factura del mes pasado todavía no se ha generado, entonces los cargos del mes pasado también están pendientes.

    ![Captura de pantalla que muestra la lista de suscripciones a Azure para el perfil de facturación](./media/billing-understand-your-bill-mca/mca-billing-profile-subscriptions-list.png)

## <a name="analyze-your-azure-usage-charges"></a>Análisis de los cargos por uso de Azure

Utilice el archivo csv de cargos y de uso de Azure para analizar los cargos basados en el uso. Puede descargar el archivo para una factura o para cargos pendientes. Para más información, consulte [Obtención de los datos de uso diario y de factura de facturación de Azure](billing-download-azure-invoice-daily-usage-date.md).

### <a name="view-detailed-usage-by-invoice-section"></a>Visualización del uso detallado por sección de factura

Puede filtrar el archivo de cargos y de uso de Azure para conciliar los cargos de uso para las secciones de factura.

Los pasos siguientes le guiarán por la conciliación de los cargos de proceso para la sección de factura del departamento de contabilidad:

![Imagen de ejemplo que muestra los detalles de la información de la sección de factura](./media/billing-understand-your-bill-mca/invoicesection-details.png)

 | Factura (PDF) | CSV de cargos y de uso de Azure |
 | --- | --- |
 |Departamento de contabilidad |invoiceSectionName |
 |Cargos por uso: plan de Microsoft Azure |productOrderName |
 |Proceso |serviceFamily |

1. Filtre la columna **invoiceSectionName** del archivo CSV  por **Departamento de contabilidad**.
2. Filtre la columna **productOrderName** del archivo CSV por **Plan de Microsoft Azure**.
3. Filtre la columna **serviceFamily** del archivo CSV por **Microsoft.Compute**.

![Captura de pantalla que muestra el archivo de cargos y de uso filtrado por la sección de factura](./media/billing-understand-your-bill-mca/billing-usage-file-filtered-by-invoice-section.png)

<!--Todo Add screenshot of usage file -->

### <a name="view-detailed-usage-by-subscription"></a>Visualización del uso detallado por suscripción

Puede filtrar el archivo de cargos y de uso de Azure para conciliar los cargos de uso para las suscripciones. Para ver todas las suscripciones en un perfil de facturación, consulte [Visualización de los cargos por uso pendientes](#view-pending-usage-charges).

Una vez identificados los cargos de una suscripción, utilice el archivo csv de cargos y de uso de Azure para analizar los cargos.

La captura de pantalla siguiente muestra la lista de suscripciones en Azure Portal.

![Captura de pantalla que muestra la lista de suscripciones a Azure para el perfil de facturación](./media/billing-understand-your-bill-mca/mca-billing-profile-subscriptions-list-highlighted.png)

Filtre la columna **subscriptionName** en el archivo CSV de cargos y de uso de Azure para **WA_Subscription** con el fin de ver los cargos de uso detallados de WA_Subscription.

![Captura de pantalla que muestra el archivo de cargos y de uso filtrado por suscripción](./media/billing-understand-your-bill-mca/billing-usage-file-filtered-by-subscription.png)

## <a name="pay-your-bill"></a>Pagar la factura

Las instrucciones para pagar la factura se muestran en la parte inferior de la factura. [Aprenda cómo pagar](billing-mca-understand-your-invoice.md#how-to-pay).

Si ya ha pagado la factura, puede comprobar el estado del pago en la página Facturas de Azure Portal.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Comprobación del acceso a un contrato de cliente de Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Ponerse en contacto con nosotros

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de la factura y del uso detallado, consulte:

- [Obtención de los datos de uso diario y la factura de Azure](billing-download-azure-invoice-daily-usage-date.md)
- [Descripción de los términos en la factura del contrato de cliente de Microsoft](billing-mca-understand-your-invoice.md)
- [Descripción de los términos del CSV de uso del contrato de cliente de Microsoft](billing-mca-understand-your-usage.md)
