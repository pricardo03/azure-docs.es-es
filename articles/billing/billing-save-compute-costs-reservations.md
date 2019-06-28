---
title: ¿Qué es Azure Reservations? | Microsoft Docs
description: Obtenga información sobre Azure Reservations y precios para ahorrar en máquinas virtuales, SQL Database, Azure Cosmos DB y otros costos de recursos.
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 04/13/2019
ms.author: banders
ms.openlocfilehash: b20983c45cd62b9812cdb52de32a6e29da459efe
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60370805"
---
# <a name="what-are-azure-reservations"></a>¿Qué es Azure Reservations?

Con Azure Reservations ahorrará gastos al pagar por adelantado por un año o tres años de máquinas virtuales, capacidad de proceso de SQL Database, rendimiento de Azure Cosmos DB u otros recursos de Azure. El pago adelantado le permite obtener un descuento en los recursos que utiliza. Reservations puede reducir considerablemente los costos en máquinas virtuales, capacidad de proceso de SQL Database, Azure Cosmos DB u otros recursos en hasta un 72 % en precios de pago por uso. Reservations ofrece un descuento en la facturación y no afecta al estado del entorno de ejecución de los recursos.

Puede comprar una reserva en [Azure Portal](https://aka.ms/reservations).

## <a name="why-buy-a-reservation"></a>¿Por qué comprar una reserva?

Si tiene máquinas virtuales, Azure Cosmos DB o SQL Database que se ejecuten durante largos períodos, la compra de una reserva le proporcionará la opción más rentable. Por ejemplo, si ejecuta continuamente cuatro instancias de un servicio sin una reserva, se le cobra según las tarifas de pago por uso. Si compra una reserva para esos recursos, obtendrá inmediatamente el descuento por reserva. Ya no se aplicarán a los recursos las tarifas de pago por uso.

## <a name="charges-covered-by-reservation"></a>Cargos cubiertos por la reserva

Planes de servicio:

- Instancia reservada de máquina virtual: las reservas solamente cubren los costos de proceso de las máquinas virtuales. No cubre cargos por software adicional, administración de redes ni almacenamiento.
- Capacidad reservada de Azure Cosmos DB: Una reserva abarca el rendimiento aprovisionado para sus recursos. No cubre los cargos de almacenamiento y redes.
- Núcleo virtual reservado de SQL Database: los únicos costos que se incluyen con las reservas son los costos de proceso. La licencia se factura por separado.

Para las máquinas virtuales de Windows y SQL Database, puede cubrir los costos de licencia de Windows con la [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/).

## <a name="whos-eligible-to-purchase-a-reservation"></a>¿Quién puede comprar una reserva?

Para comprar un plan, debe tener un rol de propietario de la suscripción en una suscripción Enterprise (MS-AZR-0017P o MS-AZR-0148P) o Pago por uso (MS-AZR-003P o MS-AZR-0023P). Los proveedores de soluciones en la nube pueden usar Azure Portal o el  [Centro de partners](/partner-center/azure-reservations) para comprar reservas de Azure.

Los clientes de Contrato Enterprise (EA) pueden limitar sus compras a los administradores de EA mediante la desactivación de la opción **Add Reserved Instances** (Agregar instancias reservadas) en EA Portal. Para poder comprar una reserva, los administradores de Contrato Enterprise deben ser propietarios de la suscripción para al menos una suscripción de Contrato Enterprise. La opción es útil para empresas que requieren un equipo centralizado para comprar reservas para distintos centros de coste. Después de la compra, los equipos centralizados pueden agregar propietarios de centros de coste a las reservas. Los propietarios pueden entonces ampliar la reserva a sus suscripciones. El equipo central no necesita tener acceso al propietario de la suscripción donde se compra la reserva.

Un descuento de reserva solo se aplica a los recursos asociados con los tipos de suscripción Enterprise, Pago por uso o CSP.

## <a name="reservation-scope"></a>Ámbito de la reserva

Un ámbito de reserva determina los recursos a los que se aplica el descuento por reserva. Un ámbito de reserva puede tener los siguientes valores:

**Ámbito compartido**: el descuento por reserva se aplica a los recursos coincidentes en suscripciones aptas dentro del contexto de facturación.

- Para los clientes con Contrato Enterprise, el contexto de facturación es la inscripción.
 Para los clientes de Pago por uso, el ámbito de facturación incluye todas las suscripciones aptas creadas por el administrador de la cuenta.

**Suscripción única**: el descuento por reserva se aplica a los recursos coincidentes de la suscripción seleccionada.

También puede [actualizar el ámbito después de comprar una reserva](billing-manage-reserved-vm-instance.md#change-the-reservation-scope).

## <a name="discounted-subscription-and-offer-types"></a>Tipos de ofertas y suscripciones con descuento

Los descuentos por reserva se aplican a las siguientes suscripciones aptas y tipos de ofertas.

- Contrato Enterprise (números de oferta: MS-AZR-0017P o MS-AZR-0148P)
- Pago por uso (números de oferta: MS-AZR-0003P o MS-AZR-0023P)
- Suscripciones de CSP

Los recursos que se ejecutan en una suscripción con otros tipos de oferta no recibirán el descuento por la reserva.

## <a name="how-is-a-reservation-billed"></a>¿Cómo se factura una reserva?

La reserva se cobra en el método de pago vinculado a la suscripción. Si tiene una suscripción Enterprise, el costo de la reserva se deducirá del saldo del compromiso monetario. Si este no es suficiente para cubrir el costo de la reserva, se le cobrará el monto restante. Si tiene una suscripción de pago por uso, el cargo se aplicará inmediatamente en la tarjeta de crédito vinculada a su cuenta. Si se le factura por factura, verá los cargos la próxima vez que reciba una.

## <a name="how-reservation-discount-is-applied"></a>Aplicación del descuento por reserva

El descuento por reserva se aplica a la utilización de recursos que coincide con los atributos seleccionados al comprar la reserva. Estos atributos incluyen el ámbito en el que se ejecutan las máquinas virtuales, SQL Database, Azure Cosmos DB u otros recursos coincidentes. Por ejemplo, si quiere un descuento por reserva para las cuatro máquinas virtuales Estándar D2 en la región Oeste de EE. UU., seleccione la suscripción en la que se ejecutan las máquinas virtuales.

Un descuento de reserva significa "*usarlo o perderlo*". Si no tiene recursos coincidentes para ninguna hora, perderá una cantidad de reserva para esa hora. No se pueden arrastrar las horas reservadas no utilizadas.

Al cerrar un recurso, el descuento por reserva se aplica automáticamente a otro recurso que coincida con el ámbito especificado. Si no se encuentran recursos coincidentes en el ámbito especificado, entonces las horas reservadas se *pierden*.

Por ejemplo, más adelante podría crear un recurso y tener una reserva que coincida con la que está infrautilizada. En este ejemplo, el descuento por reserva se aplica automáticamente al nuevo recurso coincidente.

Si, por el contrario, sus máquinas virtuales se ejecutan en diferentes suscripciones dentro de su cuenta o inscripción, seleccione el ámbito compartido. El ámbito compartido le permite aplicar el descuento de la reserva en varias suscripciones. También puede cambiar el ámbito después de la compra de la reserva. Para más información, consulte [Administración de Azure Reservations](billing-manage-reserved-vm-instance.md).

Un descuento de reserva solo se aplica a los recursos asociados con los tipos de suscripción Enterprise, Pago por uso o CSP. Los recursos que se ejecutan en una suscripción con otros tipos de oferta no recibirán el descuento por la reserva.

## <a name="when-the-reservation-term-expires"></a>Cuando expira el plazo de reserva

Al final del plazo de reserva, el descuento en la facturación expira y la máquina virtual, SQL Database, Azure Cosmos DB u otros recursos se facturan según los precios de pago por uso. Azure Reservations no se renueva automáticamente. Para seguir disfrutando del descuento de facturación, debe comprar una reserva nueva para los servicios y el software aplicables.

## <a name="discount-applies-to-different-sizes"></a>El descuento se aplica a los diferentes tamaños

Al comprar una reserva, puede aplicar el descuento a otras instancias con atributos que están dentro del mismo grupo de tamaño. Esta característica se conoce como flexibilidad de tamaño de instancia. La flexibilidad de la cobertura del descuento depende del tipo de reserva y los atributos que elija al comprar la reserva.

Planes de servicio:

- Instancias reservadas de máquina virtual: al comprar la reserva y seleccionar **Optimizado para**: **flexibilidad de tamaño de instancia**, la cobertura de descuento dependerá del tamaño de máquina virtual que elija. La reserva se puede aplicar a los tamaños de máquinas virtuales (VM) en el mismo grupo de series de tamaño. Si quiere saber más, vea [Flexibilidad en el tamaño de las máquinas virtuales con Azure Reserved VM Instances](../virtual-machines/windows/reserved-vm-instance-size-flexibility.md).
- Capacidad reservada de SQL Database: la cobertura del descuento depende del nivel de rendimiento elegido. Para más información, vea [Aprenda cómo se aplica un descuento en la reserva de Azure a las instancias de SQL Database](billing-understand-reservation-charges.md).
- Capacidad reservada de Azure Cosmos DB: la cobertura del descuento depende del rendimiento aprovisionado. Para más información, vea [Aplicación del descuento por reserva a Azure Cosmos DB](billing-understand-cosmosdb-reservation-charges.md).

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

- Para más información acerca de Azure Reservations, consulte los siguientes artículos:
    - [Administración de Azure Reservations](billing-manage-reserved-vm-instance.md)
    - [Información sobre el uso de reservas para suscripciones de pago por uso](billing-understand-reserved-instance-usage.md)
    - [Información sobre el uso de reservas para la inscripción Enterprise](billing-understand-reserved-instance-usage-ea.md)
    - [Costos de software de Windows no incluidos con reservas](billing-reserved-instance-windows-software-costs.md)
    - [Azure Reservations en el programa del Proveedor de soluciones en la nube (CSP) del Centro de partners](/partner-center/azure-reservations)

- Más información sobre las reservas para los planes de servicio:
    - [Máquinas virtuales con Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
    - [Recursos de Azure Cosmos DB con capacidad reservada de Azure Cosmos DB](../cosmos-db/cosmos-db-reserved-capacity.md)
    - [Recursos de proceso de SQL Database con capacidad reservada de Azure SQL Database](../sql-database/sql-database-reserved-capacity.md) Más información sobre las reservas para planes de software:
    - [Planes de software de Red Hat con reservas de Azure](../virtual-machines/linux/prepay-rhel-software-charges.md)
    - [Planes de software SUSE con reservas de Azure](../virtual-machines/linux/prepay-suse-software-charges.md)
