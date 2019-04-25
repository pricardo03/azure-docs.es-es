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
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60370805"
---
# <a name="what-are-azure-reservations"></a>¿Qué es Azure Reservations?

Con Azure Reservations ahorrará gastos al pagar por adelantado por un año o tres años de máquinas virtuales, capacidad de proceso de SQL Database, rendimiento de Azure Cosmos DB u otros recursos de Azure. El pago adelantado le permite obtener un descuento en los recursos que utiliza. Reservations puede reducir considerablemente los costos en máquinas virtuales, capacidad de proceso de SQL Database, Azure Cosmos DB u otros recursos en hasta un 72 % en precios de pago por uso. Reservations ofrece un descuento en la facturación y no afecta al estado del entorno de ejecución de los recursos.

Puede comprar una reserva en [Azure Portal](https://aka.ms/reservations).

## <a name="why-buy-a-reservation"></a>¿Por qué comprar una reserva?

Si tiene máquinas virtuales, Azure Cosmos DB o bases de datos SQL que se ejecutan durante largos períodos de tiempo, comprar una reserva le ofrece la opción más rentable. Por ejemplo, cuando se ejecutan continuamente cuatro instancias de un servicio sin una reserva, se le cobrará tarifas de pago por uso. Si compra una reserva para esos recursos, obtendrá inmediatamente el descuento de reserva. Ya no se aplicarán a los recursos las tarifas de pago por uso.

## <a name="charges-covered-by-reservation"></a>Cargos de reserva

Planes de servicio:

- Instancia reservada de máquina virtual: las reservas solamente cubren los costos de proceso de las máquinas virtuales. No cubre cargos por software adicional, administración de redes ni almacenamiento.
- Capacidad reservada de Azure Cosmos DB: Una reserva cubre el rendimiento aprovisionado para sus recursos. No cubre los cargos de almacenamiento y redes.
- Núcleo virtual reservado de SQL Database: los únicos costos que se incluyen con las reservas son los costos de proceso. La licencia se factura por separado.

Para las máquinas virtuales de Windows y SQL Database, puede cubrir los costos de licencia de Windows con la [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/).

## <a name="whos-eligible-to-purchase-a-reservation"></a>¿Quién puede comprar una reserva?

Para comprar un plan, debe tener un rol de propietario de la suscripción en una empresa (MS-AZR - 0017p o MS-AZR - 0148p) o una suscripción de pago por uso (MS-AZR - 003p o MS-AZR - 0023P). Proveedores de soluciones en la nube pueden usar el portal de Azure o [centro de partners](/partner-center/azure-reservations) para comprar las reservas de Azure.

Los clientes de EA pueden limitar las compras a los administradores EA deshabilitando el **agregar instancias reservadas** opción en el Portal de EA. Los administradores EA deben ser propietario de la suscripción para al menos una suscripción de EA para comprar una reserva. La opción es útil para empresas que quieren un equipo centralizado para comprar reservas para los centros de costo diferentes. Después de la compra, centralizado de los equipos pueden agregar los propietarios del centro de costo para las reservas de direcciones. Los propietarios, a continuación, pueden definir el ámbito de la reserva a sus suscripciones. El equipo central no necesita tener acceso de propietario de la suscripción donde se adquiere la reserva.

Un descuento de reserva solo se aplica a los recursos asociados con los tipos de suscripción Enterprise, Pago por uso o CSP.

## <a name="reservation-scope"></a>Ámbito de reserva

Un ámbito de reserva determina los recursos al que se aplica el descuento de reserva. Un ámbito de reserva puede tener los siguientes valores:

**Ámbito compartido** -el descuento de reserva se aplica a los recursos de coincidencias de suscripciones válidas del contexto de facturación.

- Para clientes con contrato Enterprise, el contexto de facturación es la inscripción.
 Para los clientes de pago por uso, el ámbito de facturación es todas las suscripciones elegibles creadas por el Administrador de cuenta.

**Suscripción única** -el descuento de reserva se aplica a los recursos coincidentes en la suscripción seleccionada.

También puede [actualizar el ámbito después de comprar una reserva](billing-manage-reserved-vm-instance.md#change-the-reservation-scope).

## <a name="discounted-subscription-and-offer-types"></a>Tipos de suscripción y la oferta con descuento

Descuentos de reserva se aplican a las siguientes suscripciones elegibles y tipos de ofertas.

- Contrato Enterprise (números de la oferta: MS-AZR-0017P o MS-AZR - 0148p)
- Pago por uso (números de oferta: MS-AZR-0003P o MS-AZR - 0023 P)
- Suscripciones de CSP

Los recursos que se ejecutan en una suscripción con otros tipos de oferta no recibirán el descuento por la reserva.

## <a name="how-is-a-reservation-billed"></a>¿Cómo se factura una reserva?

La reserva se cobra en el método de pago vinculado a la suscripción. Si tiene una suscripción Enterprise, el costo de la reserva se deducirá del saldo del compromiso monetario. Si este no es suficiente para cubrir el costo de la reserva, se le cobrará el monto restante. Si tiene una suscripción de pago por uso, el cargo se aplicará inmediatamente en la tarjeta de crédito vinculada a su cuenta. Si se le factura por factura, verá los cargos la próxima vez que reciba una.

## <a name="how-reservation-discount-is-applied"></a>Cómo se aplica el descuento de reserva

El descuento de reserva se aplica a la coincidencia de los atributos que selecciona al comprar la reserva el uso de recursos. Estos atributos incluyen el ámbito en el que se ejecutan las máquinas virtuales, SQL Database, Azure Cosmos DB u otros recursos coincidentes. Por ejemplo, si desea un descuento de reserva para las cuatro máquinas virtuales de estándar D2 en la región Oeste de Estados Unidos, a continuación, seleccione la suscripción donde se ejecutan las máquinas virtuales.

Un descuento de reserva es "*-it-o-perder-usarla*". Si no tiene coincidencia de los recursos para cualquier hora, perder una cantidad de reserva para esa hora. No se puede llevar a cabo reenviar horas reservadas no utilizadas.

Cuando se apaga un recurso, el descuento de reserva se aplica automáticamente a otro recurso coincidente en el ámbito especificado. Si se encuentra ningún recurso coincidente en el ámbito especificado, que son las horas reservadas *pierde*.

Por ejemplo, más adelante podría crear un recurso y tiene una reserva de búsqueda de coincidencias que está infrautilizada. En este ejemplo, el descuento de reserva se aplica automáticamente al nuevo recurso coincidente.

Si, por el contrario, sus máquinas virtuales se ejecutan en diferentes suscripciones dentro de su cuenta o inscripción, seleccione el ámbito compartido. El ámbito compartido le permite aplicar el descuento de la reserva en varias suscripciones. También puede cambiar el ámbito después de la compra de la reserva. Para más información, consulte [Administración de Azure Reservations](billing-manage-reserved-vm-instance.md).

Un descuento de reserva solo se aplica a los recursos asociados con los tipos de suscripción Enterprise, Pago por uso o CSP. Los recursos que se ejecutan en una suscripción con otros tipos de oferta no recibirán el descuento por la reserva.

## <a name="when-the-reservation-term-expires"></a>Cuando expira el plazo de reserva

Al final del plazo de reserva, el descuento en la facturación expira y la máquina virtual, SQL Database, Azure Cosmos DB u otros recursos se facturan según los precios de pago por uso. Azure Reservations no se renueva automáticamente. Para seguir disfrutando del descuento de facturación, debe comprar una reserva nueva para los servicios y el software aplicables.

## <a name="discount-applies-to-different-sizes"></a>Descuento se aplica a los diferentes tamaños

Al comprar una reserva, puede aplicar el descuento a otras instancias con atributos que están dentro del mismo grupo de tamaño. Esta característica se conoce como la flexibilidad de tamaño de instancia. La flexibilidad de la cobertura del descuento depende del tipo de reserva y los atributos que elija al comprar la reserva.

Planes de servicio:

- Instancias reservadas de máquina virtual: Al comprar la reserva y seleccionar **optimizado para**: **flexibilidad de tamaño de instancia**, el tamaño de máquina virtual que seleccione depende de la cobertura de descuento. La reserva se puede aplicar a los tamaños de máquinas virtuales (VM) en el mismo grupo de series de tamaño. Si quiere saber más, vea [Flexibilidad en el tamaño de las máquinas virtuales con Azure Reserved VM Instances](../virtual-machines/windows/reserved-vm-instance-size-flexibility.md).
- Capacidad reservada de SQL Database: la cobertura del descuento depende del nivel de rendimiento elegido. Para más información, vea [Aprenda cómo se aplica un descuento en la reserva de Azure a las instancias de SQL Database](billing-understand-reservation-charges.md).
- Capacidad reservada de Azure Cosmos DB: la cobertura del descuento depende del rendimiento aprovisionado. Para más información, vea [Aplicación del descuento por reserva a Azure Cosmos DB](billing-understand-cosmosdb-reservation-charges.md).

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [crear una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

- Para más información acerca de Azure Reservations, consulte los siguientes artículos:
    - [Administración de Azure Reservations](billing-manage-reserved-vm-instance.md)
    - [Información sobre el uso de reservas para suscripciones de pago por uso](billing-understand-reserved-instance-usage.md)
    - [Información sobre el uso de reservas para la inscripción Enterprise](billing-understand-reserved-instance-usage-ea.md)
    - [Costos de software de Windows no incluidos con reservas](billing-reserved-instance-windows-software-costs.md)
    - [Azure Reservations en el programa del Proveedor de soluciones en la nube (CSP) del Centro de partners](/partner-center/azure-reservations)

- Más información sobre las reservas para los planes de servicio:
    - [Máquinas virtuales con instancias reservadas de VM de Azure](../virtual-machines/windows/prepay-reserved-vm-instances.md)
    - [Recursos de Azure Cosmos DB con Azure Cosmos DB capacidad reservada](../cosmos-db/cosmos-db-reserved-capacity.md)
    - [Recursos de proceso de SQL Database con Azure SQL Database la capacidad reservan](../sql-database/sql-database-reserved-capacity.md) obtener más información sobre las reservas para los planes de software:
    - [Planes de software de Red Hat de las reservas de Azure](../virtual-machines/linux/prepay-rhel-software-charges.md)
    - [Planes de software SUSE desde las reservas de Azure](../virtual-machines/linux/prepay-suse-software-charges.md)
