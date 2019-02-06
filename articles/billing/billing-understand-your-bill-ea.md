---
title: Descripción de la factura de Azure Enterprise | Microsoft Docs
description: Aprenda a leer y a comprender el uso y facturación para los clientes de Azure con un Contrato Enterprise
services: ''
documentationcenter: ''
author: adpick
manager: dougeby
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2018
ms.author: banders
ms.openlocfilehash: 36ce4d96e02bac1eae1791acf811da468726b4a6
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2019
ms.locfileid: "54902710"
---
# <a name="understand-your-bill-for-azure-customers-with-an-enterprise-agreement"></a>Descripción de la factura de los clientes de Azure con un Contrato Enterprise

Los clientes de Azure con un Contrato Enterprise reciben una factura cuando superan el crédito de la organización o usan servicios que no están incluidos en ese crédito. 

El crédito de su organización incluye su compromiso monetario. El compromiso monetario es la cantidad que su organización paga por adelantado por el uso de los servicios de Azure. Para agregar fondos al compromiso monetario de su Contrato Enterprise, póngase en contacto con el administrador de la cuenta Microsoft o el distribuidor.  

## <a name="when-credit-exceeded-or-doesnt-apply"></a>Si el crédito se supera o no es aplicable

Recibirá una o varias facturas si sucede lo siguiente:

- **Uso por encima del límite del servicio**: los gastos por uso de la organización superan el saldo de crédito.
- **Gastos facturados por separado**: los servicios que ha usado la organización no están cubiertos por el crédito. Se le facturará por los siguientes servicios independientemente del saldo de crédito:
    - Canonical
    - Citrix XenApp Essentials
    - Citrix XenDesktop 
    - Usuario registrado
    - Openlogic
    - Usuario registrado de Remote Access Rights XenApp Essentials
    - Ubuntu Advantage
    - Visual Studio Enterprise (mensual)
    - Visual Studio Enterprise (anual)
    - Visual Studio Professional (mensual)
    - Visual Studio Professional (anual)
- **Cargos de Marketplace**: las compras y el uso de Azure Marketplace no están incluidos en el crédito de la organización y se facturan por separado. El administrador de la empresa tiene la capacidad de habilitar o deshabilitar las compras de Marketplace para su organización en Enterprise Portal. 

Si tiene gastos debido a un uso por encima del límite del servicio y gastos facturados por separado durante el período de facturación, recibirá una factura que incluya ambos tipos de gastos. Los gastos de Marketplace siempre se facturan de forma independiente.

## <a name="review-charges"></a>Revisión de los gastos

Para revisar y comprobar los gastos en su factura, debe ser un administrador de empresa. Para más información, consulte [Roles administrativos del Contrato Enterprise de Azure en Azure](billing-understand-ea-roles.md). Si no sabe quién es el administrador de la empresa para su organización, [póngase en contacto con el equipo de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="review-service-overage-invoice"></a>Revisión de una factura con un uso por encima del límite del servicio

Compare el importe total por uso en Enterprise Portal en **Informes** > **Resumen de uso** con su factura por uso por encima del límite del servicio. La factura por uso por encima del límite del servicio incluye el uso que ha superado el crédito de la organización y los servicios que no están incluidos en ese crédito. Los importes que aparecen en **Resumen de uso** no incluyen impuestos. 

1. Inicie sesión en [Enterprise Portal](https://ea.azure.com).
1. Seleccione **Informes**.
1. En la esquina superior derecha de la pestaña, cambie la vista de **M** a **C** y haga que coincida con el período de tiempo de la factura.
 
   ![Captura de pantalla que muestra la opción M + C en Resumen de uso.](./media/billing-understand-your-bill-ea/ea-portal-usage-sumary-cm-option.png)

1. El importe de **Uso total** debe coincidir con el **Importe total extendido** en la factura por un uso por encima del límite del servicio. La tabla siguiente enumera los términos y las descripciones que se muestran en la factura y en el **Resumen de uso** de Enterprise Portal:

   |Término de facturación|Término de Resumen de uso|DESCRIPCIÓN|
   |---|---|---|
   |Importe total extendido|Uso total|El gasto por uso total antes de impuestos durante el período específico anterior a la aplicación del crédito.|
   |Uso de compromiso|Uso de compromiso|El crédito que se aplica durante ese período específico.|
   |Total venta|Uso por encima del límite total|El gasto por uso por encima del límite total que supera el importe del crédito. Este importe no incluye impuestos.|
   |Importe de impuestos|No aplicable|Impuestos que se aplican al importe total de venta durante el período específico.|
   |Importe total|No aplicable|El importe a pagar de la factura una vez aplicado el crédito y agregado los impuestos.|
1. Para más información sobre los gastos, vaya a **Descargar uso** > **Descargar informe avanzado**. Este informe no incluye los impuestos ni los gastos por reservas o de Marketplace.

      ![Captura de pantalla que muestra Descargar informe avanzado en la pestaña Descargar uso.](./media/billing-understand-your-bill-ea/ea-portal-download-usage-advanced.png)

### <a name="review-marketplace-invoice"></a>Revisión de la factura de Marketplace

Compare el total de Azure Marketplace en **Informes** > **Resumen de uso** en Enterprise Portal con la factura de Marketplace. La factura de Marketplace es solo para el uso y las compras de Azure Marketplace. Los importes que aparecen en **Resumen de uso** no incluyen impuestos. 

1. Inicie sesión en [Enterprise Portal](https://ea.azure.com).
1. Seleccione **Informes**.
1. En la esquina superior derecha de la pestaña, cambie la vista de **M** a **C** y haga que coincida con el período de tiempo de la factura.

     ![Captura de pantalla que muestra la opción M + C en Resumen de uso.](./media/billing-understand-your-bill-ea/ea-portal-usage-sumary-cm-option.png)

1. El importe total de **Azure Marketplace** debe coincidir con el importe **Total venta** en su factura de Marketplace.
1. Para más información sobre los gastos basados en el uso, vaya a **Descargar uso**. En **Gastos de Marketplace**, seleccione **Descargar**. Este informe no incluye los impuestos ni las compras únicas.

     ![Captura de pantalla que muestra la opción Descargar en Gastos de Marketplace.](./media/billing-understand-your-bill-ea/ea-portal-download-usage-marketplace.png)

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
