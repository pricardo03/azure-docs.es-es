---
title: Seguimiento del saldo del crédito de Azure para el contrato de cliente de Microsoft
description: Aprenda a comprobar el saldo del crédito de Azure para un contrato de cliente de Microsoft.
author: bandersmsft
manager: amberb
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 352737b3ea61a51a39e066d4211c8f4ceae74184
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2019
ms.locfileid: "67490962"
---
# <a name="track-microsoft-customer-agreement-azure-credit-balance"></a>Seguimiento del saldo del crédito de Azure para el contrato de cliente de Microsoft

Puede consultar el saldo del crédito de Azure para el contrato de cliente de Microsoft en Azure Portal. Los créditos se utilizan para pagar cargos que están cubiertos por el importe de los créditos.

Se le cobrará cuando utilice productos que no estén cubiertos por los créditos o cuando su uso exceda el saldo del crédito. Para más información, consulte [Productos que no están cubiertos por los créditos de Azure.(#products-that-arent-covered-by-azure-credits).

Este artículo se aplica a una cuenta de facturación para un contrato de cliente de Microsoft. [Compruebe si tiene acceso a un contrato de cliente de Microsoft](#check-access-to-a-microsoft-customer-agreement).

## <a name="check-your-credit-balance"></a>Comprobación del saldo del crédito

1. Inicie sesión en el [Azure Portal]( https://portal.azure.com).

2. Busque **Administración de costos + facturación**.

    ![Captura de pantalla que muestra la búsqueda en el portal para la administración de cosos y facturación](./media/billing-mca-check-azure-credits-balance/billing-search-cost-management-billing.png)

3.  Seleccione **Créditos de Azure** en el lado izquierdo. En función del acceso que tenga, es posible que deba seleccionar una cuenta o un perfil de facturación y, luego, seleccionar **Créditos de Azure**.

4. En la página de créditos de Azure se muestra la siguiente información:

   ![Captura de pantalla con el saldo del crédito y las transacciones para un perfil de facturación](./media/billing-mca-check-azure-credits-balance/billing-mca-credits-overview.png)

   | Término               | Definición                           |
   |--------------------|--------------------------------------------------------|
   | Saldo estimado  | Es la cantidad estimada de crédito del que dispone teniendo en cuenta todas las transacciones pendientes y no liquidadas. |
   | Saldo actual    | Es la cantidad de créditos desde la última factura. No incluye las transacciones pendientes. |
   | Transacciones       | Todas las transacciones de facturación que han afectado al saldo del crédito de Azure |

   Cuando su saldo estimado desciende a 0, se le cobrará por todo su uso, incluidos los productos que están cubiertos por créditos.

6. Seleccione **Lista de crédito** para ver la lista de créditos para el perfil de facturación. La lista de créditos proporciona la siguiente información:

   ![Captura de pantalla de las listas de créditos para un perfil de facturación](./media/billing-mca-check-azure-credits-balance/billing-mca-credits-list.png)

   | Término | Definición |
   |---|---|
   | Saldo estimado | Importe del crédito de Azure que queda después de restarle al saldo actual los cargos elegibles de crédito no facturados|
   | Saldo actual | Importe del crédito de Azure que tiene antes de considerar los cargos elegibles de crédito no facturados. Se calcula mediante la suma de los créditos de Azure nuevos que recibió y el saldo del crédito en el momento de la última factura|
   | Origen | El origen de la adquisición del crédito |
   | Fecha de inicio | La fecha en la que se adquirió el crédito |
   | Fecha de expiración | Cuando expira el crédito |
   | Saldo | El saldo desde la última factura |
   | Importe original | El importe original del crédito |
   | Status | El estado actual del crédito. El estado puede ser activo, usado, expirado o en proceso de expiración |

## <a name="how-credits-are-used"></a>Uso de los créditos

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

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Póngase en contacto con el servicio de soporte técnico.

Si necesita ayuda, [póngase en contacto con soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.

## <a name="next-steps"></a>Pasos siguientes

- [Descripción de la cuenta de facturación para el contrato de cliente de Microsoft](billing-mca-overview.md)
- [Descripción de los términos en la factura del contrato de cliente de Microsoft](billing-mca-understand-your-invoice.md)
