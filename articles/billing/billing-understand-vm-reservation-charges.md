---
title: Información sobre el descuento de Azure Reserved VM Instances | Microsoft Docs
description: Obtenga información sobre cómo se aplica el descuento de Azure Reserved VM Instance a las máquinas virtuales en ejecución.
documentationcenter: ''
author: yashesvi
manager: yashar
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/13/2019
ms.author: banders
ms.openlocfilehash: b112dd881d4b2e87e617111d00bc82c6151d7750
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60370089"
---
# <a name="how-the-azure-reservation-discount-is-applied-to-virtual-machines"></a>Cómo se aplica el descuento de reserva de Azure para máquinas virtuales

Después de comprar una instancia reservada de máquina virtual de Azure, el descuento de reserva se aplica automáticamente a las máquinas virtuales que coincidan con los atributos y la cantidad de la reserva. Una reserva cubre los costos de proceso de las máquinas virtuales.

Un descuento de reserva se aplica a las máquinas virtuales bases que compre en Azure Marketplace.

Para la capacidad reservada de SQL Database, consulte [Información sobre el descuento de instancias reservadas de Azure](billing-understand-reservation-charges.md).

En la tabla siguiente se muestran los costos de la máquina virtual después de comprar una instancia reservada de máquina virtual. En cualquier caso, se le cobrará por el almacenamiento y la administración de redes según las tarifas normales.

| Tipo de máquina virtual  | Cargos con instancia reservada de máquina virtual |
|-----------------------|--------------------------------------------|
|Máquinas virtuales con Linux sin software adicional | La reserva cubre los costos de infraestructura de la máquina virtual.|
|Máquinas virtuales con Linux con cargos de software (por ejemplo, Red Hat) | La reserva cubre los costos de infraestructura. Se le cobra por el software adicional.|
|Máquinas virtuales con Windows sin software adicional |La reserva cubre los costos de infraestructura. Se le cobra por el software de Windows.|
|Máquinas virtuales con Windows con software adicional (por ejemplo, SQL Server) | La reserva cubre los costos de infraestructura. Se le cobra por el software de Windows y por el software adicional.|
|Máquinas virtuales con Windows con la [Ventaja híbrida de Azure](../virtual-machines/windows/hybrid-use-benefit-licensing.md) | La reserva cubre los costos de infraestructura. Los costos de software de Windows están cubiertos por la Ventaja híbrida de Azure. El software adicional se cobra por separado.|

## <a name="how-reservation-discount-is-applied"></a>Cómo se aplica el descuento de reserva

Un descuento de reserva es "*-it-o-perder-usarla*". Por lo tanto, si no tiene recursos coincidentes para cualquier hora, perder una cantidad de reserva para esa hora. No se puede llevar a cabo reenviar horas reservadas no utilizadas.

Cuando se apaga un recurso, el descuento de reserva se aplica automáticamente a otro recurso coincidente en el ámbito especificado. Si se encuentra ningún recurso coincidente en el ámbito especificado, que son las horas reservadas *pierde*.

## <a name="reservation-discount-for-non-windows-vms"></a>Descuento de reserva para las máquinas virtuales que no son Windows

 El descuento de reserva de Azure se aplica a las instancias de máquinas virtuales en ejecución en períodos de una hora. Las reservas que haya comprado se vinculan con el uso emitido por las máquinas virtuales en ejecución para aplicar el descuento de reserva. Para las máquinas virtuales que no se pueden ejecutar la hora completa, se rellenará la reserva desde otras máquinas virtuales que no usen una reserva, incluidas las que se ejecutan simultáneamente. Al final de la hora, se bloquea la aplicación de reserva para las máquinas virtuales en dicha hora. En el caso de que una máquina virtual no se ejecute durante una hora o que haya máquinas virtuales simultáneas dentro de la hora que no llenen la hora de reserva, la reserva estará infrautilizada durante esa hora. En el siguiente gráfico se muestra la aplicación de una reserva a un uso de máquina virtual facturable. La ilustración se basa en una compra de reserva y dos instancias de máquina virtual coincidentes.

![Captura de pantalla de una reserva aplicada y dos instancias de máquina virtual coincidentes](./media/billing-reserved-vm-instance-application/billing-reserved-vm-instance-application.png)

1. Cualquier uso por encima de la línea de reserva se cobrará a las tarifas de pago por uso. No se le cobrará por ningún uso por debajo de la línea de reservas, ya que el pago se ha hecho como parte de la compra de la reserva.
2. En la hora 1, la instancia 1 se ejecuta durante 0,75 horas y la instancia 2 se ejecuta durante 0,5 horas. El uso total de la hora 1 es de 1,25 horas. Se le cobrará según las tarifas de pago por uso para las 0,25 horas restantes.
3. Para las horas 2 y 3, ambas instancias se ejecutaron durante 1 hora cada una. Una instancia está cubierta por la reserva, mientras que la otra se cobra según las tarifas de pago por uso.
4. Durante la hora 4, la instancia 1 se ejecuta durante 0,5 horas y la instancia 2 se ejecuta durante 1 hora. La instancia 1 está cubierta al completo por la reserva y 0,5 horas de la instancia 2 también. Se le cobrará según la tarifa de pago por uso por las 0,5 horas restantes.

Para comprender y ver la aplicación de Azure Reservations en los informes de uso de facturación consulte [Información sobre el uso de reservas](billing-understand-reserved-instance-usage-ea.md).

## <a name="reservation-discount-for-windows-vms"></a>Descuento de reserva para las máquinas virtuales de Windows

Cuando ejecuta instancias de máquina virtual Windows, la reserva se aplica para cubrir los costos de infraestructuras. La aplicación de la reserva a los costos de infraestructuras de máquinas virtuales con Windows es la misma que para las máquinas virtuales que no ejecutan Windows. Se le cobra por separado por el software de Windows según el número de vCPU. Consulte los [costos de software de Windows con Reservations](billing-reserved-Instance-windows-software-costs.md). Puede cubrir los costos de licencia de Windows con la [Ventaja híbrida de Azure para Windows Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).

## <a name="discount-can-apply-to-different-sizes"></a>Puede aplicar el descuento para distintos tamaños

Al comprar una instancia reservada de máquina virtual, si selecciona **Optimizado para**: **flexibilidad de tamaño de instancia**, la cobertura de descuento dependerá del tamaño de máquina virtual que elija. La reserva se puede aplicar a los tamaños de máquinas virtuales (VM) en el mismo grupo de series de tamaño. Si quiere saber más, vea [Flexibilidad en el tamaño de las máquinas virtuales con Azure Reserved VM Instances](../virtual-machines/windows/reserved-vm-instance-size-flexibility.md).

## <a name="discount-applies-to-matching-servicetype-only"></a>Descuento se aplica a la coincidencia solo ServiceType

Se aplica un descuento de reserva sólo para el uso de la máquina virtual donde la `ServiceType` valor en `AdditionalInfo` coincide con la reserva que se compra. Aplicación del descuento de reserva omite el medidor usa para las máquinas virtuales y solo se evalúa como `ServiceType`. Saber qué servicio escriba que adquirió de la máquina virtual. Puede intercambiar una reserva de la máquina virtual para una reserva de almacenamiento premium o de forma opuesta de sin premium storage.

## <a name="classic-vms-and-cloud-services"></a>Las máquinas virtuales clásicas y servicios en la nube

Instancias reservada de máquina virtual automáticamente se aplican a ambas máquinas virtuales clásicas y servicios en la nube cuando se habilita la flexibilidad de tamaño de instancia. Para Cloud Services, el descuento por la reserva se aplica solo al costo del proceso. Cuando el descuento de reserva se aplica a servicios en la nube, los cargos de uso se dividen en los gastos de proceso (medidor de Linux) y servicios en la nube cargos (medidor de administración de servicios en la nube). Para obtener más información, consulte [cómo se aplica el descuento de reserva a los servicios de nube](billing-reserved-instance-windows-software-costs.md#cloud-services-software-meters-not-included-in-reservation-cost).

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Ponerse en contacto con nosotros

Si tiene alguna pregunta o necesita ayuda, [crear una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de Azure Reservations, consulte los siguientes artículos:

- [¿Cuáles son las reservas para Azure?](billing-save-compute-costs-reservations.md)
- [Pago por adelantado de máquinas virtuales con Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Pago por adelantado por recursos de proceso de SQL Database con capacidad reservada de Azure SQL Database](../sql-database/sql-database-reserved-capacity.md)
- [Administrar reservas para Azure](billing-manage-reserved-vm-instance.md)
- [Información sobre el uso de reservas para suscripciones de pago por uso](billing-understand-reserved-instance-usage.md)
- [Información sobre el uso de reservas para la inscripción Enterprise](billing-understand-reserved-instance-usage-ea.md)
- [Información sobre el uso de reservas para suscripciones de CSP](/partner-center/azure-reservations)
- [Costos de software de Windows no incluidos con reservas](billing-reserved-instance-windows-software-costs.md)
