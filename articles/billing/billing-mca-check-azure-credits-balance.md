---
title: Seguimiento del saldo del crédito de Azure para el contrato de cliente de Microsoft | Microsoft Docs
description: Aprenda a comprobar el saldo del crédito de Azure para el contrato de cliente de Microsoft.
services: ''
documentationcenter: ''
author: amberbhargava
manager: amberb
editor: banders
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/28/2019
ms.author: amberb
ms.openlocfilehash: 1e8c3e6863b9cd8f2f5ced18a57918c32c865e75
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60372259"
---
# <a name="track-azure-credit-balance-for-microsoft-customer-agreement"></a>Seguimiento del saldo del crédito para el contrato de cliente de Microsoft

Puede consultar el saldo del crédito de Azure para el contrato de cliente de Microsoft en Azure Portal. Los créditos se utilizan para pagar productos que están cubiertos por el importe de los créditos.

Se le cobrará cuando utilice productos que no estén cubiertos por los créditos o cuando su uso exceda el saldo del crédito. Para más información, consulte [Productos que no están cubiertos por los créditos de Azure.](#products-that-arent-covered-by-azure-credits)

Este artículo se aplica a una cuenta de facturación para un contrato de cliente de Microsoft. [Compruebe si tiene acceso a un contrato de cliente de Microsoft](#check-access-to-a-microsoft-customer-agreement).

## <a name="check-credit-balance-in-the-azure-portal"></a>Comprobación del saldo del crédito en Azure Portal

1. Inicie sesión en el [Azure Portal]( https://portal.azure.com).

2. Busque en **Administración de costos + facturación**.

   ![Captura de pantalla que muestra la búsqueda en el portal para la administración de cosos y facturación](./media/billing-mca-check-azure-credits-balance/billing-search-cost-management-billing.png)

3. Vaya al perfil de facturación. Dependiendo de su acceso, es posible que tenga que seleccionar una cuenta de facturación. En la cuenta de facturación, seleccione **Perfiles de facturación** y, después, un perfil de facturación.

4. Seleccione **Créditos de Azure**.

5. En la página de créditos de Azure se muestra la siguiente información:

   ![Captura de pantalla con el saldo del crédito y las transacciones para un perfil de facturación](./media/billing-mca-check-azure-credits-balance/billing-mca-credits-overview.png)

   | Término               | Definición                           |
   |--------------------|--------------------------------------------------------|
   | Saldo estimado  | Es la cantidad estimada de crédito del que dispone teniendo en cuenta todas las transacciones pendientes y no liquidadas. |
   | Saldo actual    | Es la cantidad de créditos desde la última factura. No incluye las transacciones pendientes. |
   | Transacciones       | Todas las transacciones de facturación que han afectado al saldo del crédito de Azure |

   Cuando su saldo estimado desciende a 0, se le cobrará por todo su uso, incluidos los productos que están cubiertos por créditos.

6. Seleccione **Lista de crédito** para ver la lista de créditos para el perfil de facturación. La lista de créditos proporciona la siguiente información:

   ![Captura de pantalla de las listas de créditos para un perfil de facturación](./media/billing-mca-check-azure-credits-balance/billing-mca-credits-list.png)

   | Término                 | Definición                           |
   |----------------------|--------------------------------------------------------|
   | Origen               | El origen de la adquisición del crédito |
   | Fecha de inicio           | La fecha en la que se adquirió el crédito |
   | Fecha de expiración      | Cuando expira el crédito |
   | Saldo              | El saldo desde la última factura |
   | Importe original      | El importe original del crédito |
   | Status               | El estado actual del crédito. El estado puede ser activo, usado, expirado o en proceso de expiración |

## <a name="how-credits-are-used-in-microsoft-customer-agreement"></a>Uso de los créditos en el contrato de cliente de Microsoft

En una cuenta de facturación para un contrato de cliente de Microsoft, utilizará perfiles de facturación que le permiten administrar las facturas y los métodos de pago. Se crea una factura mensual para cada perfil de facturación y utilizará los métodos de pago para pagar la factura.

Los créditos de Azure son uno de los métodos de pago. Obtiene crédito de Microsoft como crédito promocional y crédito de nivel de servicio. Estos créditos se asignan a un perfil de facturación. Cuando se genera una factura para el perfil de facturación, los créditos se aplican automáticamente a la cantidad total facturada para calcular la cantidad que tiene que pagar. Paga el importe restante con otro método de pago, como cheque o transferencia bancaria.

## <a name="products-that-arent-covered-by-azure-credits"></a>Productos que no están cubiertos por los créditos de Azure

 Los siguientes productos no están cubiertos por los créditos de Azure. Se le cobrará por usar estos productos, con independencia del saldo del crédito:

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
- Productos de Azure Marketplace
- Planes de soporte técnico de Azure

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Comprobación del acceso a un contrato de cliente de Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si necesita ayuda, [póngase en contacto con soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.

## <a name="next-steps"></a>Pasos siguientes

- [Descripción de la cuenta de facturación para el contrato de cliente de Microsoft](billing-mca-overview.md)
- [Descripción de los términos en la factura del contrato de cliente de Microsoft](billing-mca-understand-your-invoice.md)