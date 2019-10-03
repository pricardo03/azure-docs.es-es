---
title: Visualización y descarga de los precios de Azure de la organización
description: Obtenga información sobre cómo ver y descargar los precios o calcule los costos con los precios de la organización.
author: bandersmsft
manager: jureid
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: banders
ms.custom: seodec18
ms.openlocfilehash: 486a6b46b6c4590b7f49cd8aba449204cd8f4fac
ms.sourcegitcommit: d4c9821b31f5a12ab4cc60036fde00e7d8dc4421
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71709697"
---
# <a name="view-and-download-your-organizations-azure-pricing"></a>Visualización y descarga de los precios de Azure de la organización

Los clientes de Azure que tienen un Contrato Enterprise (EA) de Azure o un [Contrato de cliente de Microsoft](#check-your-access-to-a-microsoft-customer-agreement) pueden ver y descargar los precios en Azure Portal.

## <a name="ea-pricing"></a>Precios de EA

Según las directivas que haya establecido el administrador de empresa para la organización, solo determinados roles administrativos proporcionan acceso a la información de precios de EA de la organización. Para más información, consulte [Roles administrativos del Contrato Enterprise de Azure en Azure](billing-understand-ea-roles.md).

1. Como administrador de Enterprise, inicie sesión en [Azure Portal](https://portal.azure.com/).
1. Busque *Administración de costos + facturación*.

   ![Captura de pantalla que muestra la búsqueda en Azure Portal](./media/billing-ea-pricing/portal-cm-billing-search.png)

1. En la cuenta de facturación, seleccione **Uso y cargos**.

   ![Captura de pantalla que muestra el uso y los cargos en Facturación](./media/billing-ea-pricing/ea-pricing-usage-charges-nav.png)

1. Seleccione ![captura de pantalla que muestra la búsqueda de Azure Portal](./media/billing-ea-pricing/download-icon.png) **Descargar** para el mes.

1. En **Hoja de precios**, seleccione **Descargar CSV**.

   ![Captura de pantalla que muestra el botón Descargar CSV de la hoja de precios](./media/billing-ea-pricing/download-ea-price-sheet.png)

## <a name="microsoft-customer-agreement-pricing"></a>Precios de Contrato de cliente de Microsoft

Debe ser el propietario del perfil de facturación, colaborador, lector o administrador de facturación para ver y descargar los precios. Para obtener más información acerca de los roles de facturación para los contratos de cliente de Microsoft, consulte [Tareas y roles del perfil de facturación](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

### <a name="download-price-sheets-for-the-current-billing-period"></a>Descarga de las hojas de precios para el período de facturación actual

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com).
1. Busque *Administración de costos + facturación*.
1. Seleccione un perfil de facturación. En función de su acceso, es posible que tenga que seleccionar primero una cuenta de facturación.
1. En el área **Información general**, busque los vínculos de descarga debajo de los cargos del mes hasta la fecha.
1. Seleccione **Hoja de precios de Azure**.
![Captura de pantalla que muestra la descarga desde Información general](./media/billing-ea-pricing/open-pricing.png).

### <a name="download-price-sheets-for-billed-charges"></a>Descarga de las hojas de precios para los cargos facturados

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com).
1. Busque *Administración de costos + facturación*.
1. Seleccione un perfil de facturación. En función de su acceso, es posible que tenga que seleccionar primero una cuenta de facturación.
1. Seleccione **Facturas**.
1. En la cuadrícula de la factura, busque la fila de la factura correspondiente a la hoja de precios que quiere descargar.
1. Haga clic en el botón de puntos suspensivos (`...`) al final de la fila.
![Captura de pantalla que muestra los puntos suspensivos seleccionados](./media/billing-ea-pricing/billingprofile-invoicegrid.png).

1. Si quiere ver los precios de los servicios en la factura seleccionada, seleccione **Hoja de precios de la factura**.
1. Si quiere ver los precios de todos los servicios de Azure para el período de facturación determinado, seleccione **Hoja de precios de Azure**.

![Captura de pantalla que muestra el menú contextual con las hojas de precios.](./media/billing-ea-pricing/contextmenu-pricesheet.png)

## <a name="estimate-costs-with-the-azure-pricing-calculator"></a>Estimación de costos con la calculadora de precios de Azure

También puede usar los precios de la organización para estimar los costos con la calculadora de precios de Azure.

1. Vaya a la [calculadora de precios de Azure](https://azure.microsoft.com/pricing/calculator).
1. En la esquina superior derecha, seleccione **Iniciar sesión**.
1. En **Programs and Offer** > **Licensing Program** (Programas y oferta > Programa de licencias), seleccione **Enterprise Agreement (EA)** (Contrato Enterprise [EA]).
1. En **Programs and Offer** > **Selected agreement**, (Programas y oferta > Seleccionar contrato), seleccione **None selected** (Ninguno seleccionado).

    ![Captura de pantalla que muestra el botón Descargar CSV de la hoja de precios](./media/billing-ea-pricing/ea-pricing-calculator-estimate.png)

1. Elija la organización.
1. Seleccione **Aplicar**.
1. Busque y, a continuación, agregue productos a su estimación.
1. Los precios estimados mostrados se basan en los precios de la organización que ha seleccionado.

## <a name="check-your-access-to-a-microsoft-customer-agreement"></a>Comprobación del acceso a un contrato de cliente de Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="next-steps"></a>Pasos siguientes

Si es un cliente EA, consulte:

- [Administración del acceso a la información de facturación de EA de Azure](billing-manage-access.md)
- [Visualización y descarga de la factura de Microsoft Azure](billing-download-azure-invoice.md)
- [Visualización y descarga de la utilización y los cargos de Microsoft Azure](billing-download-azure-daily-usage.md)
- [Descripción de la factura de los clientes con un Contrato Enterprise](billing-understand-your-bill-ea.md)

Si tiene un contrato de cliente de Microsoft, consulte:

- [Descripción de los términos de la hoja de precios para un contrato de cliente de Microsoft](billing-mca-understand-pricesheet.md)
- [Visualización y descarga de la factura de Microsoft Azure](billing-download-azure-invoice.md)
- [Visualización y descarga de la utilización y los cargos de Microsoft Azure](billing-download-azure-daily-usage.md)
- [Visualización y descarga de los documentos de impuestos del perfil de facturación](billing-mca-download-tax-document.md)
- [Descripción de los cargos en la factura del perfil de facturación](billing-mca-understand-your-bill.md)
