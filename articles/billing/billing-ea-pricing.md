---
title: Ver y descargar los precios de Azure de su organización | Microsoft Docs
description: Obtenga información sobre cómo ver y descargar los precios o calcular los costos con los precios de su organización.
services: ''
documentationcenter: ''
author: adpick
manager: jureid
editor: ''
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/22/2019
ms.author: banders
ms.custom: seodec18
ms.openlocfilehash: d4c64433089f14d845cea8be9adf0ef13675cfd1
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57892479"
---
# <a name="view-and-download-your-organizations-azure-pricing"></a>Ver y descargar los precios de Azure de su organización

Los clientes de Azure con un contrato Enterprise (EA) o [contrato del cliente de Microsoft](#check-your-access-to-a-microsoft-customer-agreement) puede ver y descargar los precios de Azure portal. Si tiene un contrato de cliente de Microsoft, consulte la vista y descarga los precios de su contrato de cliente de Microsoft.

## <a name="view-and-download-ea-pricing"></a>Visualización y descarga de los precios de EA

Según las directivas que haya establecido el administrador de empresa para la organización, solo determinados roles administrativos proporcionan acceso a la información de precios de EA de la organización. Para más información, consulte [Roles administrativos del Contrato Enterprise de Azure en Azure](billing-understand-ea-roles.md).

1. Como administrador de empresa, inicie sesión en [Azure Portal](https://portal.azure.com/).
1. Busque en **Administración de costos + facturación**.

   ![Captura de pantalla que muestra la búsqueda en Azure Portal](./media/billing-ea-pricing/portal-cm-billing-search.png)

1. En la cuenta de facturación, seleccione **Uso y cargos**.

   ![Captura de pantalla que muestra el uso y los cargos en Facturación](./media/billing-ea-pricing/ea-pricing-usage-charges-nav.png)

1. Seleccione ![captura de pantalla que muestra la búsqueda de Azure Portal](./media/billing-ea-pricing/download-icon.png) **Descargar** para el mes.
1. En **Hoja de precios**, seleccione **Descargar CSV**.

   ![Captura de pantalla que muestra el botón Descargar CSV de la hoja de precios](./media/billing-ea-pricing/download-ea-price-sheet.png)

## <a name="view-and-download-pricing-for-your-microsoft-customer-agreement"></a>Ver y descargar los precios de su contrato de cliente de Microsoft

Debe ser el propietario del perfil de facturación, Colaborador, lector o administrador de facturas para ver y descargar los precios. Para más información acerca de las funciones de facturación para los contratos de cliente de Microsoft, consulte [perfil roles y tareas de facturación](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

### <a name="download-price-sheets-for-the-current-billing-period"></a>Descargue las hojas de precios para el período de facturación actual

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Busque en **Administración de costos + facturación**.
1. Seleccione un perfil de facturación. Según el acceso, debe seleccionar primero una cuenta de facturación.
1. En el **Introducción** hoja, busque los vínculos de descarga por debajo de los cargos del mes hasta la fecha.
1. Seleccione **hoja de precios de Azure**.
![Captura de pantalla que muestra la descarga de información general](./media/billing-ea-pricing/open-pricing.png)

### <a name="download-price-sheets-for-billed-charges"></a>Descargue las hojas de precios para los cargos de facturación

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Busque en **Administración de costos + facturación**.
1. Seleccione un perfil de facturación. Según el acceso, debe seleccionar primero una cuenta de facturación.
1. Seleccione **Facturas**.
1. En la cuadrícula de la factura, busque la fila de la factura correspondiente a la hoja de precios que desea descargar.
1. Haga clic en el botón de puntos suspensivos (`...`) al final de la fila.
![Captura de pantalla que muestra los puntos suspensivos seleccionado](./media/billing-ea-pricing/billingprofile-invoicegrid.png)

1. Si desea ver los precios de los servicios en la factura seleccionada, seleccione **hoja de precios de la factura**.
1. Si desea ver los precios de todos los servicios de Azure para el período de facturación determinado, seleccione **hoja de precios de Azure**.

![Captura de pantalla que muestra el menú contextual con las hojas de precios](./media/billing-ea-pricing/contextmenu-pricesheet.png)

## <a name="estimate-costs-with-the-azure-pricing-calculator"></a>Calcular los costos con la calculadora de precios de Azure

También puede usar los precios de su organización para estimar los costos con la calculadora de precios de Azure.

1. Vaya a la [calculadora de precios de Azure](https://azure.microsoft.com/pricing/calculator).
1. En la esquina superior derecha, seleccione **Iniciar sesión**.
1. En **Programs and Offer** > **Licensing Program** (Programas y oferta > Programa de licencias), seleccione **Enterprise Agreement (EA)** (Contrato Enterprise [EA]).
1. En **Programs and Offer** > **Selected agreement**, (Programas y oferta > Seleccionar contrato), seleccione **None selected** (Ninguno seleccionado).

    ![Captura de pantalla que muestra el botón Descargar CSV de la hoja de precios](./media/billing-ea-pricing/ea-pricing-calculator-estimate.png)

1. Elija la organización.
1. Seleccione **Aplicar**.
1. Busque y agregue productos a su estimación.
1. Los precios estimados mostrados se basan en los precios de la organización que ha seleccionado.

## <a name="check-your-access-to-a-microsoft-customer-agreement"></a>Comprobar el acceso a un contrato de cliente de Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="next-steps"></a>Pasos siguientes

Si es un cliente EA, consulte:

- [Administrar el acceso a la información de facturación de EA de Azure](billing-manage-access.md)
- [Ver y descargar la factura de Microsoft Azure](billing-download-azure-invoice.md)
- [Ver y descargar los cargos y el uso de Microsoft Azure](billing-download-azure-daily-usage.md)
- [Comprender la factura de los clientes con contrato Enterprise](billing-understand-your-bill-ea.md)

Si tiene un contrato de cliente de Microsoft, consulte:

- [Comprender los términos en la hoja de precios para un contrato de cliente de Microsoft](billing-mca-understand-pricesheet.md)
- [Ver y descargar la factura de Microsoft Azure](billing-download-azure-invoice.md)
- [Ver y descargar los cargos y el uso de Microsoft Azure](billing-download-azure-daily-usage.md)
- [Ver y descargar documentos de impuestos para el perfil de facturación](billing-mca-download-tax-document.md)
- [Comprender los cargos de facturación de su perfil de facturación](billing-mca-understand-your-bill.md)
