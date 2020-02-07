---
title: Aplicación de un descuento por reserva a Azure Disk Storage
description: Obtenga información sobre cómo se aplica el descuento de discos reservados de Azure a los discos administrados SSD Premium.
author: roygara
ms.service: cost-management-billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2020
ms.author: rogarana
ms.openlocfilehash: 7f23aaebc20b562768fb04ae988e4aff1b62fb21
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2020
ms.locfileid: "76902134"
---
# <a name="understand-how-your-reservation-discount-is-applied-to-azure-disk-storage"></a>Aplicación del descuento por reserva a Azure Disk Storage

Después de comprar capacidad reservada de Azure Storage, el descuento por reserva se aplica automáticamente a los recursos de disco que coinciden con los términos de la reserva. El descuento por reserva se aplica solo a la capacidad de disco, las instantáneas de disco se cobran según las tarifas de pago por uso.

Para obtener más información sobre la reserva de discos de Azure, consulte [Reducción de costos con la reserva de discos de Azure](../../virtual-machines/linux/disks-reserved-capacity.md).
Para obtener más información sobre los precios de la reserva de discos de Azure, consulte los [precios de los discos de Azure](https://azure.microsoft.com/pricing/details/managed-disks/).

## <a name="how-the-reservation-discount-is-applied"></a>Aplicación del descuento por reserva

El descuento por reserva de discos de Azure es un descuento "usarlo o perderlo" y se aplica a los recursos de discos administrados cada hora. Si no tiene ningún recurso de discos administrados que cumpla los términos de la reserva para una hora dada, perderá una cantidad de reserva para esa hora; las horas sin usar no se acumulan.

Al eliminar un recurso, el descuento por reserva se aplica automáticamente a otro que coincida con el ámbito especificado. Si no se encuentran recursos coincidentes en el ámbito especificado, las horas reservadas se pierden.

## <a name="discount-examples"></a>Ejemplos de descuento

En los ejemplos siguientes se muestra cómo se aplica el descuento por reserva de discos de Azure en función de las implementaciones:

Supongamos que compró 100 discos P30 (aprox. 1 TiB por disco) de capacidad reservada en la región Oeste de EE. UU. 2 para un plazo de 1 año. Suponiendo que el costo de esta reserva de ejemplo es de 140 100 $, puede optar por pagar la cantidad completa por adelantado o pagar cuotas mensuales fijas de 11 675 $ durante los próximos 12 meses.
En estos ejemplos, se supone que se ha registrado para un plan de pago de reserva mensual. En los siguientes escenarios se describe lo que sucede si el uso excede la capacidad reservada, no llega a esta o la usa por niveles.

### <a name="underusing-your-capacity"></a>Uso por debajo de la capacidad

Supongamos que en una hora dada dentro del período de reserva ha implementado solo 99 SSD Premium P30 de la reserva de 100 discos P30. El disco P30 restante no se usa durante esa hora y no se acumula para uso posterior.

### <a name="overusing-your-capacity"></a>Uso por encima de la capacidad

Supongamos que en una hora determinada dentro del período de reserva, usó 101 SSD Premium P30. El descuento por reserva se aplica a 100 discos P30, y el disco P30 restante se cobra según las tarifas de pago por uso durante esa hora. Par la hora siguiente, si el uso disminuye a 100 discos P30, todo el uso queda cubierto por la reserva.

### <a name="tiering-your-capacity"></a>Uso de la capacidad por niveles

Supongamos que en una hora dada dentro del período de reserva, desea usar un total de 200 SSD Premium P30. Durante los primeros 30 minutos, solo se usa 100. Durante este período, el uso está totalmente cubierto, ya que hizo una reserva para 100 discos P30. Si después deja de usar los primeros 100 (de modo que usa cero) y, a continuación, comienza a usar los otros 100 durante los 30 minutos restantes, ese uso también está cubierto por la reserva.

![Representación de los ejemplos de subutilización, sobreutilización y utilización por niveles de la capacidad](media/understand-disk-reservations/reserved-disks-example-scenarios.png)

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Ponerse en contacto con nosotros

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes

- [Reducción de costos con la reserva de discos de Azure (Linux)](../../virtual-machines/linux/disks-reserved-capacity.md)
- [Reducción de costos con la reserva de discos de Azure (Windows)](../../virtual-machines/windows/disks-reserved-capacity.md)
- [¿Qué es Azure Reservations?](save-compute-costs-reservations.md)