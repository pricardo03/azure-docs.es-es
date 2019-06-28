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
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60370089"
---
# <a name="how-the-azure-reservation-discount-is-applied-to-virtual-machines"></a>Aplicación del descuento por reserva de Azure en las máquinas virtuales

Después de comprar una instancia reservada de máquina virtual de Azure, el descuento de reserva se aplica automáticamente a las máquinas virtuales que coincidan con los atributos y la cantidad de la reserva. Una reserva cubre los costos de proceso de las máquinas virtuales.

Un descuento de reserva se aplica a las máquinas virtuales base que compre en Azure Marketplace.

Para la capacidad reservada de SQL Database, consulte [Información sobre el descuento de instancias reservadas de Azure](billing-understand-reservation-charges.md).

En la tabla siguiente se muestran los costos de la máquina virtual después de comprar una instancia reservada de máquina virtual. En cualquier caso, se le cobrará por el almacenamiento y la administración de redes según las tarifas normales.

| Tipo de máquina virtual  | Cargos con instancia reservada de máquina virtual |
|-----------------------|--------------------------------------------|
|Máquinas virtuales con Linux sin software adicional | La reserva cubre los costos de infraestructura de la máquina virtual.|
|Máquinas virtuales con Linux con cargos de software (por ejemplo, Red Hat) | La reserva cubre los costos de infraestructura. Se le cobra por el software adicional.|
|Máquinas virtuales con Windows sin software adicional |La reserva cubre los costos de infraestructura. Se le cobra por el software de Windows.|
|Máquinas virtuales con Windows con software adicional (por ejemplo, SQL Server) | La reserva cubre los costos de infraestructura. Se le cobra por el software de Windows y por el software adicional.|
|Máquinas virtuales con Windows con la [Ventaja híbrida de Azure](../virtual-machines/windows/hybrid-use-benefit-licensing.md) | La reserva cubre los costos de infraestructura. Los costos de software de Windows están cubiertos por la Ventaja híbrida de Azure. El software adicional se cobra por separado.|

## <a name="how-reservation-discount-is-applied"></a>Aplicación del descuento por reserva

Un descuento de reserva significa "*usarlo o perderlo*". Por lo tanto, si no tiene recursos coincidentes para ninguna hora, perderá una cantidad de reserva para esa hora. No se pueden arrastrar las horas reservadas no utilizadas.

Al cerrar un recurso, el descuento por reserva se aplica automáticamente a otro recurso que coincida con el ámbito especificado. Si no se encuentran recursos coincidentes en el ámbito especificado, las horas reservadas se *pierden*.

## <a name="reservation-discount-for-non-windows-vms"></a>Descuento de reserva para las máquinas virtuales que no son Windows

 El descuento de reserva de Azure se aplica a las instancias de máquinas virtuales en ejecución en períodos de una hora. Las reservas que haya comprado se vinculan con el uso emitido por las máquinas virtuales en ejecución para aplicar el descuento de reserva. Para las máquinas virtuales que no se pueden ejecutar la hora completa, se rellenará la reserva desde otras máquinas virtuales que no usen una reserva, incluidas las que se ejecutan simultáneamente. Al final de la hora, se bloquea la aplicación de reserva para las máquinas virtuales en dicha hora. En el caso de que una máquina virtual no se ejecute durante una hora o que haya máquinas virtuales simultáneas dentro de la hora que no llenen la hora de reserva, la reserva estará infrautilizada durante esa hora. En el siguiente gráfico se muestra la aplicación de una reserva a un uso de máquina virtual facturable. La ilustración se basa en una compra de reserva y dos instancias de máquina virtual coincidentes.

![Captura de pantalla de una reserva aplicada y dos instancias de máquina virtual coincidentes](./media/billing-reserved-vm-instance-application/billing-reserved-vm-instance-application.png)

1. Cualquier uso por encima de la línea de reservas se cobrará según las tarifas normales de pago por uso. No se le cobrará por ningún uso por debajo de la línea de reservas, ya que el pago se ha hecho como parte de la compra de la reserva.
2. En la hora 1, la instancia 1 se ejecuta durante 0,75 horas y la instancia 2 se ejecuta durante 0,5 horas. El uso total de la hora 1 es de 1,25 horas. Se le cobrará según las tarifas de pago por uso para las 0,25 horas restantes.
3. Para las horas 2 y 3, ambas instancias se ejecutaron durante 1 hora cada una. Una instancia está cubierta por la reserva, mientras que la otra se cobra según las tarifas de pago por uso.
4. Durante la hora 4, la instancia 1 se ejecuta durante 0,5 horas y la instancia 2 se ejecuta durante 1 hora. La instancia 1 está cubierta al completo por la reserva y 0,5 horas de la instancia 2 también. Se le cobrará según la tarifa de pago por uso por las 0,5 horas restantes.

Para comprender y ver la aplicación de Azure Reservations en los informes de uso de facturación consulte [Información sobre el uso de reservas](billing-understand-reserved-instance-usage-ea.md).

## <a name="reservation-discount-for-windows-vms"></a>Descuento de reserva para máquinas virtuales Windows

Cuando ejecuta instancias de máquina virtual Windows, la reserva se aplica para cubrir los costos de infraestructuras. La aplicación de la reserva a los costos de infraestructuras de máquinas virtuales con Windows es la misma que para las máquinas virtuales que no ejecutan Windows. Se le cobra por separado por el software de Windows según el número de vCPU. Consulte los [costos de software de Windows con Reservations](billing-reserved-Instance-windows-software-costs.md). Puede cubrir los costos de licencia de Windows con la [Ventaja híbrida de Azure para Windows Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).

## <a name="discount-can-apply-to-different-sizes"></a>Se puede aplicar el descuento a los distintos tamaños

Al comprar una instancia reservada de máquina virtual, si selecciona **Optimizado para**: **flexibilidad de tamaño de instancia**, la cobertura de descuento dependerá del tamaño de máquina virtual que elija. La reserva se puede aplicar a los tamaños de máquinas virtuales (VM) en el mismo grupo de series de tamaño. Si quiere saber más, vea [Flexibilidad en el tamaño de las máquinas virtuales con Azure Reserved VM Instances](../virtual-machines/windows/reserved-vm-instance-size-flexibility.md).

## <a name="discount-applies-to-matching-servicetype-only"></a>El descuento solo se aplica al tipo de servicio coincidente

Se aplica un descuento de reserva solo para el uso de la máquina virtual donde el valor `ServiceType` de `AdditionalInfo` coincide con la reserva que se compra. La aplicación del descuento de reserva omite el medidor que se utiliza para las máquinas virtuales y que solo evalúa `ServiceType`. Sepa para qué tipo de servicio compró la máquina virtual. Puede cambiar una reserva de máquina virtual de almacenamiento no Premium por una reserva de almacenamiento Premium, o de la manera opuesta.

## <a name="classic-vms-and-cloud-services"></a>Servicios en la nube y máquinas virtuales clásicas

Las instancias reservadas de máquinas virtuales se aplican automáticamente tanto a las máquinas virtuales clásicas como a los servicios en la nube cuando la flexibilidad de tamaño de instancia está habilitada. Para Cloud Services, el descuento por la reserva se aplica solo al costo del proceso. Cuando el descuento por la reserva se aplica a Cloud Services, los cargos por uso se dividen en cargos de proceso (medidor de Linux) y cargos de Cloud Services (medidor de administración de Cloud Services). Para más información, consulte [Aplicación del descuento por la reserva a Cloud Services.](billing-reserved-instance-windows-software-costs.md#cloud-services-software-meters-not-included-in-reservation-cost)

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Ponerse en contacto con nosotros

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de Azure Reservations, consulte los siguientes artículos:

- [Qué son las reservas de Azure](billing-save-compute-costs-reservations.md)
- [Pago por adelantado de máquinas virtuales con Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Pago por adelantado de los recursos de proceso de SQL Database con capacidad reservada de Azure SQL Database](../sql-database/sql-database-reserved-capacity.md)
- [Administración de las reservas de Azure](billing-manage-reserved-vm-instance.md)
- [Información sobre el uso de reservas para suscripciones de pago por uso](billing-understand-reserved-instance-usage.md)
- [Información sobre el uso de reservas para la inscripción Enterprise](billing-understand-reserved-instance-usage-ea.md)
- [Información sobre el uso de reservas para suscripciones de CSP](/partner-center/azure-reservations)
- [Costos de software de Windows no incluidos con reservas](billing-reserved-instance-windows-software-costs.md)
