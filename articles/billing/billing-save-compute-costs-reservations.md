---
title: ¿Qué es Azure Reservations? | Microsoft Docs
description: Obtenga información sobre Azure Reservations y precios para ahorrar en máquinas virtuales, bases de datos SQL y otros costos de recursos.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashesvi
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2018
ms.author: yashar
ms.openlocfilehash: 82b23f46acc94fefccc871583657200b90a33f05
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2018
ms.locfileid: "43303569"
---
# <a name="what-are-azure-reservations"></a>¿Qué es Azure Reservations?

Azure Reservations le ayuda a ahorrar dinero mediante el pago por adelantado por un año o tres años de máquina virtual, capacidad de proceso de SQL Database u otros recursos de Azure. El pago adelantado le permite obtener un descuento en los recursos que utiliza. Reservations puede reducir significativamente los costos de máquinas virtuales, procesos de SQL Database u otros recursos hasta un 72 % en precios de pago por uso. Reservations ofrece un descuento en la facturación y no afecta al estado del entorno de ejecución de los recursos.

Puede comprar una reserva en [Azure Portal](https://aka.ms/reservations). Para obtener más información, consulte los temas siguientes:

- [Pago por adelantado de máquinas virtuales con Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Pago por adelantado por recursos de proceso de SQL Database con capacidad reservada de Azure SQL Database](../sql-database/sql-database-reserved-capacity.md)

## <a name="why-should-i-buy-a-reservation"></a>¿Por qué debería comprar una reserva?

Si tiene máquinas virtuales o bases de datos SQL que se ejecuten durante largos períodos, la compra de una reserva le proporcionará la opción más rentable. Por ejemplo, si ejecuta continuamente cuatro instancias de un servicio sin una reserva, se le cobra según las tarifas de pago por uso. Si compra una reserva para esos recursos, obtendrá inmediatamente el descuento por reserva. Ya no se aplicarán a los recursos las tarifas de pago por uso.

## <a name="what-charges-does-a-reservation-cover"></a>¿Qué cargos cubre una reserva?

- Instancia reservada de máquina virtual: una reserva solo cubre el costo de proceso de máquina virtual. No cubre cargos por software adicional, administración de redes ni almacenamiento.
- Núcleo virtual reservado de SQL Database: solo se incluyen con una reserva los costos de proceso. La licencia se factura por separado.

Para las máquinas virtuales de Windows y SQL Database, puede cubrir los costos de licencia de Windows con la [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/).

## <a name="whos-eligible-to-purchase-a-reservation"></a>¿Quién puede comprar una reserva?

Los clientes de Azure que tengan estos tipos de suscripción pueden comprar una reserva:

- Tipo de oferta de suscripción Contrato Enterprise (MS-AZR-0017P).
- Tipo de oferta de suscripción [Pago por uso](https://azure.microsoft.com/offers/ms-azr-0003p/) (MS-AZR-003P). Debe tener el rol de "propietario" en la suscripción para comprar una reserva.
- Los asociados de los proveedores de soluciones en la nube (CSP) pueden usar Azure Portal o el [Centro de partners](https://docs.microsoft.com/partner-center/azure-reservations) para comprar Azure Reservations.

Un descuento de reserva solo se aplica a los recursos asociados con los tipos de suscripción Enterprise, Pago por uso o CSP.

## <a name="how-is-a-reservation-billed"></a>¿Cómo se factura una reserva?

La reserva se cobra en el método de pago vinculado a la suscripción. Si tiene una suscripción Enterprise, el costo de la reserva se deducirá del saldo del compromiso monetario. Si este no es suficiente para cubrir el costo de la reserva, se le cobrará el monto restante. Si tiene una suscripción de pago por uso, el cargo se aplicará inmediatamente en la tarjeta de crédito vinculada a su cuenta. Si se le factura por factura, verá los cargos la próxima vez que reciba una.

## <a name="how-is-the-reservation-discount-applied"></a>¿Cómo se aplica el descuento por la reserva?

El descuento por reserva se aplica a la utilización de recursos que coincide con los atributos seleccionados al comprar la reserva. Estos atributos incluyen el ámbito en el que se ejecutan las máquinas virtuales, las bases de datos SQL u otros recursos coincidentes. Por ejemplo, si quiere un descuento por la reserva para sus máquinas virtuales Estándar D2 en la región Oeste de EE. UU., seleccione la suscripción en la que se ejecutan las máquinas virtuales. Si, por el contrario, sus máquinas virtuales se ejecutan en diferentes suscripciones dentro de su cuenta o inscripción, seleccione el ámbito compartido. El ámbito compartido le permite aplicar el descuento de la reserva en varias suscripciones. También puede cambiar el ámbito después de la compra de la reserva. Para más información, consulte [Administración de Azure Reservations](billing-manage-reserved-vm-instance.md).

Un descuento de reserva solo se aplica a los recursos asociados con los tipos de suscripción Enterprise, Pago por uso o CSP. Los recursos que se ejecutan en una suscripción con otros tipos de oferta no recibirán el descuento por la reserva. Para las inscripciones Enterprise, las suscripciones de Desarrollo/pruebas de Enterprise no tienen disponibles las ventajas de las reservas.

Para entender mejor cómo afecta Reservations a la facturación, vea los temas siguientes:

-  [Información sobre el descuento de Azure Reserved VM Instances](billing-understand-vm-reservation-charges.md)
- [Información sobre el descuento de Azure Reservations](billing-understand-vm-reservation-charges.md)
- Información sobre el descuento de Azure Reservations y el uso para SUSE

## <a name="what-happens-when-the-reservation-term-expires"></a>¿Qué ocurre cuando expira el plazo de reserva?

Al final del plazo de reserva, el descuento en la facturación expira y la máquina virtual, la base de datos SQL u otros recursos se facturan según los precios de pago por uso. Azure Reservations no se renueva automáticamente. Para seguir disfrutando del descuento de facturación, debe comprar una reserva nueva para los servicios y el software aplicables.

## <a name="next-steps"></a>Pasos siguientes

Empiece a ahorrar en sus máquinas virtuales con la compra de una [instancia reservada de máquina virtual](../virtual-machines/windows/prepay-reserved-vm-instances.md) o de [capacidad reservada de SQL Database](../sql-database/sql-database-reserved-capacity.md).

Para obtener más información acerca de Azure Reservations, consulte los siguientes artículos:

- [Administración de Azure Reservations](billing-manage-reserved-vm-instance.md)
- [Información sobre el uso de reservas para suscripciones de pago por uso](billing-understand-reserved-instance-usage.md)
- [Información sobre el uso de reservas para la inscripción Enterprise](billing-understand-reserved-instance-usage-ea.md)
- [Costos de software de Windows no incluidos con reservas](billing-reserved-instance-windows-software-costs.md)
- [Azure Reservations en el programa del Proveedor de soluciones en la nube (CSP) del Centro de partners](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si tiene más preguntas, [póngase en contacto con el soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.
