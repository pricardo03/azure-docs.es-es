---
title: Uso de ver y descargar Azure y los cargos | Microsoft Docs
description: Describe cómo descargar o ver los cargos y el uso diario de Azure.
keywords: uso de facturación, los cargos de uso, uso descargar, ver el uso, el uso de la factura de azure, azure
services: billing
documentationcenter: ''
author: genlin
manager: jureid
editor: ''
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: banders
ms.openlocfilehash: 20404cea2aca984ef35472fa94d37c04eb8080e4
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57872661"
---
# <a name="view-and-download-your-azure-usage-and-charges"></a>Ver y descargar los cargos y el uso de Azure

Si un cliente de EA o tienen un [contrato del cliente de Microsoft](#check-your-access-to-a-microsoft-customer-agreement), puede descargar el uso de Azure y los cargos en el [portal Azure](https://portal.azure.com/). Para otras suscripciones, vaya a la [centro de cuentas de Azure](https://account.azure.com/Subscriptions) descargar uso.

Solo determinados roles tienen permiso para obtener información de uso de Azure, como el Administrador de cuenta o administrador de empresa. Para obtener más información sobre cómo obtener acceso a la información de facturación, vea [Manage access to Azure billing using roles](billing-manage-access.md) (Administrar el acceso a la facturación de Azure mediante roles).

Si tiene un [contrato del cliente de Microsoft](#Check-your-access-to-a-Microsoft-Customer-Agreement), debe ser un perfil de facturación propietario, Colaborador, lector, o el administrador para ver los cargos y el uso de Azure de factura. Para más información acerca de las funciones de facturación para los contratos de cliente de Microsoft, consulte [perfil roles y tareas de facturación](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

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

Para ver y descargar datos de uso como un cliente EA, debe ser un administrador de empresa, el propietario de la cuenta, o con la vista del Administrador de departamento cobra directiva habilitada.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Busque *costo Management + facturación*.

    ![Captura de pantalla que muestra la búsqueda en Azure Portal](./media/billing-download-azure-invoice-daily-usage-date/portal-cm-billing-search.png)

1. Seleccione **Uso + cargos**.
1. Seleccione **Descargar** para el mes que quiere descargar.

## <a name="download-usage-for-your-microsoft-customer-agreement"></a>Descargar el uso de su contrato de cliente de Microsoft

Si tiene un contrato de cliente de Microsoft, puede descargar los cargos y el uso de Azure para el perfil de facturación. Debe ser un perfil de facturación propietario, Colaborador, lector, o para descargar el uso de Azure y los cargos de CSV de factura.

### <a name="download-usage-for-billed-charges"></a>Descargar uso para los cargos de facturación

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Busque en **Administración de costos + facturación**.
3. Seleccione un perfil de facturación. Según el acceso, debe seleccionar primero una cuenta de facturación.
4. Seleccione **Facturas**.
5. En la cuadrícula de la factura, busque la fila de la factura correspondiente al uso que desea descargar.
6. Haga clic en el botón de puntos suspensivos (`...`) al final de la fila.

    ![Captura de pantalla que muestra los puntos suspensivos al final de la fila](./media/billing-download-azure-invoice/billingprofile-invoicegrid.png)

7. En el menú contextual de descarga, seleccione **cargos y el uso de Azure**.

     ![Captura de pantalla que muestra el uso de Azure y los cargos seleccionados](./media/billing-download-azure-usage/contextmenu-usage.png)

### <a name="download-usage-for-pending-charges"></a>Descargar de pendientes los cargos de uso

También puede descargar el uso del mes hasta la fecha para el período de facturación actual. Estos cargos de uso que no se ha facturado.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Busque en **Administración de costos + facturación**.
3. Seleccione un perfil de facturación. Según el acceso, debe seleccionar primero una cuenta de facturación.
4. En el **Introducción** hoja, busque los vínculos de descarga por debajo de los cargos del mes hasta la fecha.
5. Seleccione **cargos y el uso de Azure**.

    ![Captura de pantalla que muestra la descarga de información general](./media/billing-download-azure-usage/open-usage.png)

## <a name="check-your-access-to-a-microsoft-customer-agreement"></a>Comprobar el acceso a un contrato de cliente de Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Ponerse en contacto con nosotros

Si tiene alguna pregunta o necesita ayuda, [crear una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los cargos de uso y la factura, consulte:

- [Descripción de los términos de uso detallados de Microsoft Azure](billing-understand-your-usage.md)
- [Comprender la factura de Microsoft Azure](billing-understand-your-bill.md)
- [Ver y descargar la factura de Microsoft Azure](billing-download-azure-invoice.md)
- [Ver y descargar los precios de Azure de su organización](billing-ea-pricing.md)

Si tiene un contrato de cliente de Microsoft, consulte:

- [Descripción de los términos en su contrato de cliente de Microsoft Azure detallados de uso](billing-mca-understand-your-usage.md)
- [Comprender los cargos en la factura del contrato de cliente de Microsoft](billing-mca-understand-your-bill.md)
- [Ver y descargar la factura de Microsoft Azure](billing-download-azure-invoice.md)
- [Ver y descargar documentos de impuestos de su contrato de cliente de Microsoft](billing-mca-download-tax-document.md)
- [Ver y descargar los precios de Azure de su organización](billing-ea-pricing.md)
