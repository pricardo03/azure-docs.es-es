---
title: Visualización y descarga de los datos de uso y los cargos de Azure
description: En este artículo se describe cómo descargar o ver los datos de uso y los cargos diarios de Azure.
keywords: billing usage, usage charges, usage download, view usage, azure invoice, azure usage
author: bandersmsft
manager: jureid
tags: billing
ms.service: cost-management-billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: banders
ms.openlocfilehash: dcf4e3b9d85909c8f1d149c9d1940a6755b431a1
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74224013"
---
# <a name="view-and-download-your-azure-usage-and-charges"></a>Visualización y descarga de los datos de uso y los cargos de Azure

Puede descargar un desglose diario del uso y los cargos de Azure en Azure Portal. Solo determinados roles tienen permiso para obtener la información de uso de Azure, como el administrador de la cuenta o el administrador de Enterprise. Para obtener más información sobre cómo obtener acceso a la información de facturación, vea [Manage access to Azure billing using roles](billing-manage-access.md) (Administrar el acceso a la facturación de Azure mediante roles).

Si tiene un contrato de cliente de Microsoft (MCA), debe ser el propietario del perfil de facturación, colaborador, lector o administrador de facturación para ver los datos de uso y los cargos de Azure.  Si tiene un contrato Microsoft Partner Agreement (MPA), solo el administrador global o el agente de administrador de la organización asociada pueden ver y descargar el uso y los cargos de Azure. [Compruebe el tipo de la cuenta de facturación en Azure Portal](#check-your-billing-account-type).

## <a name="download-usage-from-the-azure-portal-csv"></a>Descarga de uso en Azure Portal (.csv)

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Busque *Administración de costos + facturación*.

    ![Captura de pantalla que muestra la búsqueda en Azure Portal](./media/billing-download-azure-usage/portal-cm-billing-search.png)

1. Dependiendo de su acceso, es posible que tenga que seleccionar una cuenta de facturación o un perfil de facturación.
1. En el menú de la izquierda, seleccione **Facturas** en **Facturación**.
1. En la cuadrícula de la factura, busque la fila del período de facturación correspondiente al uso que quiere descargar.
1. Haga clic en el icono de descargas o los puntos suspensivos (`...`) de la derecha.
1. Seleccione **Descargar uso y cargos de Azure** en el menú de descargas.

## <a name="download-usage-for-ea-customers"></a>Descarga del uso para los clientes de EA

Para ver y descargar los datos de uso como cliente de EA, debe ser administrador de Enterprise, o bien propietario de la cuenta o administrador de departamento con la directiva para ver los cargos habilitada.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Busque *Administración de costos + facturación*.

    ![Captura de pantalla que muestra la búsqueda en Azure Portal](./media/billing-download-azure-usage/portal-cm-billing-search.png)

1. Seleccione **Uso + cargos**.
1. Seleccione **Descargar** para el mes que quiere descargar.

## <a name="download-usage-for-pending-charges"></a>Descarga del uso para cargos pendientes

Si tiene un contrato de cliente de Microsoft, puede descargar el uso mensual hasta la fecha para el período de facturación actual. Estos cargos de utilización que todavía no se han facturado.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Busque *Administración de costos + facturación*.
3. Seleccione un perfil de facturación. En función de su acceso, es posible que tenga que seleccionar primero una cuenta de facturación.
4. En el área **Información general**, busque los vínculos de descarga debajo de los cargos del mes hasta la fecha.
5. Seleccione **Uso y cargos de Azure**.

    ![Captura de pantalla que muestra la descarga de la hoja Información general](./media/billing-download-azure-usage/open-usage.png)

## <a name="check-your-billing-account-type"></a>Comprobación del tipo de cuenta de facturación
[!INCLUDE [billing-check-account-type](../../includes/billing-check-account-type.md)]

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
