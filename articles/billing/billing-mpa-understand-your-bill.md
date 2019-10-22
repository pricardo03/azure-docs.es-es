---
title: 'Descripción de los cargos en la factura del contrato Microsoft Partner Agreement: Azure'
description: Aprenda a leer y comprender los cargos en la factura.
author: jureid
manager: jureid
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/10/2019
ms.author: banders
ms.openlocfilehash: 579a052f8bde780ac33de85c5a08d9b3e79d3096
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72515768"
---
# <a name="understand-charges-on-your-microsoft-partner-agreement-invoice"></a>Descripción de los cargos en la factura del contrato Microsoft Partner Agreement

 En la cuenta de facturación de un contrato Microsoft Partner Agreement, se genera una factura cada mes para cada perfil de facturación. La factura incluye todos los cargos del cliente del mes anterior. Puede comprender los cargos en la factura mediante el análisis de las transacciones individuales en Azure Portal. También puede ver las facturas en Azure Portal. Para más información, consulte [Descarga de facturas en Azure Portal](billing-download-azure-invoice.md).

Este artículo se aplica a una cuenta de facturación para un contrato Microsoft Partner Agreement. [Compruebe si tiene acceso a un contrato Microsoft Partner Agreement](#check-access-to-a-microsoft-partner-agreement).

## <a name="view-transactions-for-an-invoice-in-the-azure-portal"></a>Visualización de las transacciones de una factura en Azure Portal

1. Inicie sesión en el [Azure Portal](https://www.azure.com).

2. Busque *Administración de costos + facturación*.

   <!--  ![Screenshot that shows Azure portal search for cost management + billing](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png) -->

3. Seleccione **Todas las transacciones** en el lado izquierdo de la página. Dependiendo del acceso, es posible que tenga que seleccionar una cuenta de facturación, un perfil de facturación o un cliente y, luego, **Todas las transacciones**.

4. En la página Todas las transacciones se muestra la siguiente información:

    <!-- ![Screenshot that shows the billed transactions list](./media/billing-mpa-understand-your-bill/mpa-billing-profile-all-transactions.png) -->

    |Columna  |Definición  |
    |---------|---------|
    |Date     | La fecha de la transacción  |
    |El identificador de la factura     | El identificador de la factura en la se ha facturado la transacción. Si envía una solicitud de soporte técnico, comparta el identificador con el Soporte técnico de Azure para agilizar la solicitud de soporte técnico. |
    |Tipo de transacción     |  El tipo de transacción, como los cargos por compra, cancelación y uso  |
    |Familia de productos     | La categoría del producto, como proceso para máquinas virtuales o base de datos para la base de datos de Azure SQL|
    |SKU de producto     | Un código único que identifica la instancia del producto |
    |Importe     |  El importe de la transacción      |
    |Perfil de facturación     | La transacción se muestra en la factura de este perfil de facturación. |

5. Busque el identificador de la factura para filtrar las transacciones de la factura.

## <a name="review-pending-charges-to-estimate-your-next-invoice"></a>Revisión de los cargos pendientes para calcular la próxima factura

Los cargos se calculan y se consideran pendientes hasta que se facturan. Puede ver los cargos pendientes para el perfil de facturación del contrato Microsoft Partner Agreement en Azure Portal para calcular la factura siguiente. Los cargos pendientes son estimados y no incluyen impuestos. Los cargos reales de la próxima factura serán distintos de los cargos pendientes.

### <a name="view-pending-transactions"></a>Visualización de las transacciones pendientes

Una vez que se identifiquen los cargos pendientes, puede entender los cargos mediante el análisis de las transacciones individuales que contribuyeron a los cargos. En este momento, los cargos por uso pendientes no se muestran en la página Todas las transacciones. Puede ver los cargos por uso pendientes en la página de suscripciones a Azure. Para más información, consulte [Visualización de los cargos por uso pendientes](#view-pending-usage-charges).

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Busque *Administración de costos + facturación*.

   <!-- ![Screenshot that shows Azure portal search for cost management + billing](./media/billing-understand-your-bill-mca/billing-search-cost-management-billing.png) -->

3. Seleccione un perfil de facturación. Dependiendo del acceso, es posible que tenga que seleccionar una cuenta de facturación. En la cuenta de facturación, seleccione **Perfiles de facturación** y, después, uno de los perfiles.

4. Seleccione **Todas las transacciones** en el lado izquierdo de la página.

5. Busque *los pendientes*. Utilice el filtro **Intervalo de tiempo** para ver los cargos pendientes del mes actual o del último mes.

   <!-- ![Screenshot that shows the pending transactions list](./media/billing-mpa-understand-your-bill/mpa-billing-profile-pending-transactions.png) -->

### <a name="view-pending-charges-by-customer"></a>Visualización de cargos pendientes por cliente

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Busque *Administración de costos + facturación*.

3. Seleccione un perfil de facturación. Dependiendo del acceso, es posible que tenga que seleccionar una cuenta de facturación. En la cuenta de facturación, seleccione **Perfiles de facturación** y, después, uno de los perfiles.

4. Seleccione **Clientes** en el panel izquierdo de la página.

    <!-- ![screenshot of billing profile customers list](./media/billing-mpa-understand-your-bill/mpa-billing-profile-customers.png) -->

5. La página Clientes muestra los cargos del mes actual y del último mes para cada cliente asociado con el perfil de facturación. Los cargos del mes hasta la fecha son los cargos pendientes para el mes actual y se facturan cuando se genera la factura para el mes. Si la factura del mes pasado todavía no se ha generado, entonces los cargos del mes pasado también están pendientes.

### <a name="view-pending-usage-charges"></a>Visualización de los cargos por uso pendientes

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Busque *Administración de costos + facturación*.

3. Seleccione un perfil de facturación. Dependiendo del acceso, es posible que tenga que seleccionar una cuenta de facturación. En la cuenta de facturación, seleccione **Perfiles de facturación** y, después, uno de los perfiles.

4. Seleccione **Suscripciones de Azure** en el lado izquierdo de la página.

5. La página de suscripciones a Azure muestra los cargos del mes actual y del último mes para cada suscripción en el perfil de facturación. Los cargos del mes hasta la fecha son los cargos pendientes para el mes actual y se facturan cuando se genera la factura para el mes. Si la factura del mes pasado todavía no se ha generado, entonces los cargos del mes pasado también están pendientes.

<!--     ![Screenshot that shows the Azure subscriptions list for MPA billing profile](./media/billing-mpa-understand-your-bill/mpa-billing-profile-subscriptions-list.png) -->

## <a name="analyze-your-azure-usage-charges"></a>Análisis de los cargos por uso de Azure

Utilice el archivo .csv de cargos y de uso de Azure para analizar los cargos basados en el uso. Puede [descargar el archivo CSV de uso y cargos de Azure en Azure Portal](billing-download-azure-daily-usage.md).

Puede filtrar el archivo de uso y de cargos de Azure para conciliar los cargos de uso de cada producto que aparece en el archivo pdf de la factura. Para ver los cargos detallados de uso de un producto determinado, filtre la columna **product** del archivo CSV de uso y cargos de Azure para que incluya solo el nombre de ese producto.

También puede filtrar la columna **customerName** del archivo CSV de uso y cargos de Azure para ver los cargos de uso diario de cada uno de los clientes. Si desea ver los cargos de uso diario por suscripción de Azure, filtre la columna **subscriptionName**.


## <a name="pay-your-bill"></a>Pagar la factura

Las instrucciones para pagar la factura se muestran en la parte inferior de la factura. Puede pagar por transferencia o por cheque en un plazo de 60 días a partir de la fecha de factura.

Si ya ha pagado la factura, puede comprobar el estado del pago en la página Facturas de Azure Portal.

## <a name="check-access-to-a-microsoft-partner-agreement"></a>Comprobación del acceso a un contrato Microsoft Partner Agreement
[!INCLUDE [billing-check-mpa](../../includes/billing-check-mpa.md)]

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de la factura y del uso detallado, consulte:
