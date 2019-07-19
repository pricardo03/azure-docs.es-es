---
title: Visualización y descarga de los datos de uso y los cargos de Azure
description: En este artículo se describe cómo descargar o ver los datos de uso y los cargos diarios de Azure.
keywords: utilización de facturación, cargos de utilización, descarga de utilización, visualización de utilización, factura de Azure, utilización de Azure
author: bandersmsft
manager: jureid
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 7d2d7be562eaaa7dd21e63735f5697ffe5a62f8a
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491438"
---
# <a name="view-and-download-your-azure-usage-and-charges"></a>Visualización y descarga de los datos de uso y los cargos de Azure

Si es un cliente de EA o tiene un [contrato de cliente de Microsoft](#check-your-access-to-a-microsoft-customer-agreement), puede descargar los datos de uso y los cargos de Azure en [Azure Portal](https://portal.azure.com/). Si se trata de otras suscripciones, vaya al [Centro de cuentas de Azure](https://account.azure.com/Subscriptions) para descargar la utilización.

Solo determinados roles tienen permiso para obtener la información de uso de Azure, como el administrador de la cuenta o el administrador de Enterprise. Para obtener más información sobre cómo obtener acceso a la información de facturación, vea [Manage access to Azure billing using roles](billing-manage-access.md) (Administrar el acceso a la facturación de Azure mediante roles).

Si tiene un [contrato de cliente de Microsoft](#check-your-access-to-a-microsoft-customer-agreement), debe ser el propietario del perfil de facturación, colaborador, lector o administrador de facturación para ver los datos de uso y los cargos de Azure. Para obtener más información acerca de los roles de facturación para los contratos de cliente de Microsoft, consulte [Tareas y roles del perfil de facturación](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

## <a name="download-usage-from-the-account-center-csv"></a>Descarga del uso desde el centro de cuentas (.csv)

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

## <a name="download-usage-for-ea-customers"></a>Descarga del uso para los clientes de EA

Para ver y descargar los datos de uso como cliente de EA, debe ser administrador de Enterprise, o bien propietario de la cuenta o administrador de departamento con la directiva para ver los cargos habilitada.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Busque *Administración de costos + facturación*.

    ![Captura de pantalla que muestra la búsqueda en Azure Portal](./media/billing-download-azure-invoice-daily-usage-date/portal-cm-billing-search.png)

1. Seleccione **Uso + cargos**.
1. Seleccione **Descargar** para el mes que quiere descargar.

## <a name="download-usage-for-your-microsoft-customer-agreement"></a>Descarga del uso para el contrato de cliente de Microsoft

Si tiene un contrato de cliente de Microsoft, puede descargar los datos de uso y los cargos de Azure asociados a su perfil de facturación. Debe ser el propietario del perfil de facturación, colaborador, lector o administrador de facturación para descargar el archivo .csv con los datos de uso y los cargos de Azure.

### <a name="download-usage-for-billed-charges"></a>Descarga del uso para los cargos facturados

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Busque *Administración de costos + facturación*.
3. Seleccione un perfil de facturación. En función de su acceso, es posible que tenga que seleccionar primero una cuenta de facturación.
4. Seleccione **Facturas**.
5. En la cuadrícula de la factura, busque la fila de la factura correspondiente a los datos de uso que quiere descargar.
6. Haga clic en el botón de puntos suspensivos (`...`) al final de la fila.

    ![Captura de pantalla que muestra los puntos suspensivos al final de la fila.](./media/billing-download-azure-invoice/billingprofile-invoicegrid.png)

7. En el menú contextual de descarga, seleccione **Uso y cargos de Azure**.

     ![Captura de pantalla que muestra el uso y los cargos de Azure seleccionados](./media/billing-download-azure-usage/contextmenu-usage.png)

### <a name="download-usage-for-pending-charges"></a>Descarga del uso para cargos pendientes

También se pueden descargar los datos de uso del mes hasta la fecha para el período de facturación actual. Estos cargos de utilización que todavía no se han facturado.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Busque *Administración de costos + facturación*.
3. Seleccione un perfil de facturación. En función de su acceso, es posible que tenga que seleccionar primero una cuenta de facturación.
4. En el área **Información general**, busque los vínculos de descarga debajo de los cargos del mes hasta la fecha.
5. Seleccione **Uso y cargos de Azure**.

    ![Captura de pantalla que muestra la descarga de la hoja Información general](./media/billing-download-azure-usage/open-usage.png)

## <a name="check-your-access-to-a-microsoft-customer-agreement"></a>Comprobación del acceso a un contrato de cliente de Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de los cargos de factura y utilización, consulte:

- [Descripción de los términos de uso detallados de Microsoft Azure](billing-understand-your-usage.md)
- [Comprender la factura de Microsoft Azure](billing-understand-your-bill.md)
- [Visualización y descarga de la factura de Microsoft Azure](billing-download-azure-invoice.md)
- [Visualización y descarga de los precios de Azure de la organización](billing-ea-pricing.md)

Si tiene un contrato de cliente de Microsoft, consulte:

- [Descripción de los términos de la utilización detallada de Azure del contrato de cliente de Microsoft](billing-mca-understand-your-usage.md)
- [Descripción de los cargos de la factura del contrato de cliente de Microsoft](billing-mca-understand-your-bill.md)
- [Visualización y descarga de la factura de Microsoft Azure](billing-download-azure-invoice.md)
- [Visualización y descarga de los documentos fiscales para el contrato de cliente de Microsoft](billing-mca-download-tax-document.md)
- [Visualización y descarga de los precios de Azure de la organización](billing-ea-pricing.md)
