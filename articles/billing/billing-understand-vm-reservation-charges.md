---
title: Información sobre el descuento de Azure Reserved VM Instances | Microsoft Docs
description: Obtenga información sobre cómo se aplica el descuento de Azure Reserved VM Instance a las máquinas virtuales en ejecución.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashar
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2018
ms.author: yashar
ms.openlocfilehash: bce4e05cbc4ecc8a44f2929eecbeee4093d39652
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2018
ms.locfileid: "39628365"
---
# <a name="understand-how-the-azure-reservation-discount-is-applied-to-virtual-machines"></a>Aprenda cómo se aplica un descuento en la reserva de Azure a las máquinas virtuales

Después de comprar una instancia reservada de máquina virtual de Azure, el descuento de reserva se aplica automáticamente a las máquinas virtuales que coincidan con los atributos y la cantidad de la reserva. Una reserva cubre los costos de proceso de las máquinas virtuales.

Para la capacidad reservada de SQL Database, consulte [Información sobre el descuento de instancias reservadas de Azure](billing-understand-reservation-charges.md).

En la tabla siguiente se muestran los costos de la máquina virtual después de comprar una instancia reservada de máquina virtual. En cualquier caso, se le cobrará por el almacenamiento y la administración de redes según las tarifas normales.

| Tipo de máquina virtual  | Cargos con instancia reservada de máquina virtual |
|-----------------------|--------------------------------------------|
|Máquinas virtuales con Linux sin software adicional | La reserva cubre los costos de infraestructura de la máquina virtual.|
|Máquinas virtuales con Linux con cargos de software (por ejemplo, Red Hat) | La reserva cubre los costos de infraestructura. Se le cobra por el software adicional.|
|Máquinas virtuales con Windows sin software adicional |La reserva cubre los costos de infraestructura. Se le cobra por el software de Windows.|
|Máquinas virtuales con Windows con software adicional (por ejemplo, SQL Server) | La reserva cubre los costos de infraestructura. Se le cobra por el software de Windows y por el software adicional.|
|Máquinas virtuales con Windows con la [Ventaja híbrida de Azure](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) | La reserva cubre los costos de infraestructura. Los costos de software de Windows están cubiertos por la Ventaja híbrida de Azure. El software adicional se cobra por separado.| 

## <a name="application-of-reservation-discount-to-non-windows-vms"></a>Aplicación del descuento de reserva en máquinas virtuales que no ejecutan Windows

 El descuento de reserva de Azure se aplica a las instancias de máquinas virtuales en ejecución en períodos de una hora. Las reservas que haya comprado se vinculan con el uso emitido por las máquinas virtuales en ejecución para aplicar el descuento de reserva. Para las máquinas virtuales que no se pueden ejecutar la hora completa, se rellenará la reserva desde otras máquinas virtuales que no usen una reserva, incluidas las que se ejecutan simultáneamente. Al final de la hora, se bloquea la aplicación de reserva para las máquinas virtuales en dicha hora. En el caso de que una máquina virtual no se ejecute durante una hora o que haya máquinas virtuales simultáneas dentro de la hora que no llenen la hora de reserva, la reserva estará infrautilizada durante esa hora. En el siguiente gráfico se muestra la aplicación de una reserva a un uso de máquina virtual facturable. La ilustración se basa en una compra de reserva y dos instancias de máquina virtual coincidentes.

![Captura de pantalla de una reserva aplicada y dos instancias de máquina virtual coincidentes](media/billing-reserved-vm-instance-application/billing-reserved-vm-instance-application.png)

1. Cualquier uso por encima de la línea de reservas se cobrará según las tarifas normales de pago por uso. No se le cobrará por ningún uso por debajo de la línea de reservas, ya que el pago se ha hecho como parte de la compra de la reserva.
2. En la hora 1, la instancia 1 se ejecuta durante 0,75 horas y la instancia 2 se ejecuta durante 0,5 horas. El uso total de la hora 1 es de 1,25 horas. Se le cobrará según las tarifas de pago por uso para las 0,25 horas restantes.
3. Para las horas 2 y 3, ambas instancias se ejecutaron durante 1 hora cada una. Una instancia está cubierta por la reserva, mientras que la otra se cobra según las tarifas de pago por uso.
4. Durante la hora 4, la instancia 1 se ejecuta durante 0,5 horas y la instancia 2 se ejecuta durante 1 hora. La instancia 1 está cubierta al completo por la reserva y 0,5 horas de la instancia 2 también. Se le cobrará según la tarifa de pago por uso por las 0,5 horas restantes.

Para información sobre la aplicación de las reservas en informes de uso de facturación de Azure y para ver cómo hacerla, consulte [Información sobre el uso de reservas](https://go.microsoft.com/fwlink/?linkid=862757).

## <a name="application-of-reservation-discount-to-windows-vms"></a>Aplicación del descuento de reserva a máquinas virtuales con Windows

Cuando ejecuta instancias de máquina virtual Windows, la reserva se aplica para cubrir los costos de infraestructuras. La aplicación de la reserva a los costos de infraestructuras de máquinas virtuales con Windows es la misma que para las máquinas virtuales que no ejecutan Windows. Se le cobra por separado por el software de Windows según el número de vCPU. Consulte los [costos de software de Windows con reservas](https://go.microsoft.com/fwlink/?linkid=862756). Puede cubrir los costos de licencia de Windows con la [Ventaja híbrida de Azure para Windows Server] (https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing)).

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de las reservas, consulte los siguientes artículos:

- [¿Qué son las reservas de Azure?](billing-save-compute-costs-reservations.md)
- [Pago por adelantado de máquinas virtuales con Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Pago por adelantado por recursos de proceso de SQL Database con capacidad reservada de Azure SQL Database](../sql-database/sql-database-reserved-capacity.md)
- [Administración de reservas de Azure](billing-manage-reserved-vm-instance.md)
- [Información sobre el uso de reservas para suscripciones de pago por uso](billing-understand-reserved-instance-usage.md)
- [Información sobre el uso de reservas para la inscripción Enterprise](billing-understand-reserved-instance-usage-ea.md)
- [Información sobre el uso de reservas para suscripciones de CSP](https://docs.microsoft.com/partner-center/azure-reservations)
- [Costos de software de Windows no incluidos con reservas](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si tiene más preguntas, [póngase en contacto con el soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.
