---
title: Información sobre el uso de reservas de Azure para la inscripción Enterprise | Microsoft Docs
description: Aprenda a interpretar los datos de uso para comprender cómo se aplican las reservas de Azure a las inscripciones Enterprise.
services: billing
documentationcenter: ''
author: manish-shukla01
manager: manshuk
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2018
ms.author: manshuk
ms.openlocfilehash: 5ce0103315f297996ed3f3bd88b5e53558e22e14
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2018
ms.locfileid: "39628239"
---
# <a name="understand-azure-reservation-usage-for-your-enterprise-enrollment"></a>Información sobre el uso de reservas de Azure para la inscripción Enterprise

Use el **ReservationId** (Id. de reserva) de la [página Reservas](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) y el archivo de uso del [portal de EA](https://ea.azure.com) para evaluar el uso de las reservas. También puede ver el uso de las reservas en la sección de resumen de uso del [portal de EA](https://ea.azure.com).

Si compró la reserva en un contexto de facturación de pago por uso, consulte [Información sobre el uso de reservas para suscripciones de pago por uso](billing-understand-reserved-instance-usage.md).

## <a name="usage-for-reserved-virtual-machines-instances"></a>Uso de instancias reservadas de máquinas virtuales

En las secciones siguientes se dará por hecho que está ejecutando una máquina virtual Windows Standard_D1_v2 en la región Este de EE. UU. y que la información de la reserva tiene una apariencia similar a la de la siguiente tabla:

| Campo | Valor |
|---| --- |
|ReservationId |8f82d880-d33e-4e0d-bcb5-6bcb5de0c719|
|Cantidad |1|
|SKU | Standard_D1|
|Region | estado |

La parte de hardware de la máquina virtual está cubierta, dado que la máquina virtual implementada coincide con los atributos de la reserva. Para ver qué software de Windows no está cubierto por la reserva de máquina virtual, consulte [Costos del software de Windows de las instancias de máquina virtual de reserva de Azure](billing-reserved-instance-windows-software-costs.md).

### <a name="usage-in-csv-file-for-reserved-vm-instances"></a>Uso en un archivo CSV de instancias reservadas de máquina virtual

Puede descargar el archivo CSV de uso del Contrato Enterprise desde el portal de Enterprise. En el archivo CSV, filtre por la columna **Información adicional** y escriba su **ReservationID** (Identificador de reserva). En la siguiente captura de pantalla se muestran los campos relacionados con la reserva:

![Archivo CSV del Contrato Enterprise (EA) para reservas de Azure](./media/billing-understand-reserved-instance-usage-ea/billing-ea-reserved-instance-csv.png)

1. **ReservationId** (Identificador de reserva) en el campo **Información adicional** representa la reserva que se aplicó a la máquina virtual.
2. **ConsumptionMeter** es el identificador del medidor de la máquina virtual.
3. **Id. de medidor** es el medidor de la reserva con un costo de 0 $. El costo de la máquina virtual en ejecución se paga por la instancia reservada de máquina virtual.
4. Standard_D1 es una máquina virtual vCPU y se implementa sin la Ventaja híbrida de Azure. Por lo tanto, este medidor cubre el costo extra del software Windows. Para buscar el medidor correspondiente a la máquina virtual de 1 núcleo de serie D, consulte [Costos del software de Windows de las instancias de máquina virtual de reserva de Azure](billing-reserved-instance-windows-software-costs.md).  Si tiene la Ventaja híbrida de Azure, no se aplicará este cargo adicional.

## <a name="usage-for-sql-database-reserved-capacity-reservations"></a>Uso de reservas de capacidad reservada de SQL Database

En las siguientes secciones, se dará por hecho que está ejecutando SQL Database Gen 4 en la región Este de EE. UU. y que la información de la reserva tiene una apariencia similar a la de la siguiente tabla:

| Campo | Valor |
|---| --- |
|ReservationId |8244e673-83e9-45ad-b54b-3f5295d37cae|
|Cantidad |2|
|Producto| SQL Database Gen 4 (2 núcleos)|
|Region | estado |

### <a name="usage-in-csv-file-for-sql-database-reserved-capacity"></a>Uso en el archivo CSV de la capacidad reservada de SQL Database

Filtre por **Información adicional** y escriba su **Identificador de reserva**. En la siguiente captura de pantalla se muestran los campos relacionados con la reserva.

![Archivo CSV del Contrato Enterprise (EA) para la capacidad reservada de SQL Database](./media/billing-understand-reserved-instance-usage-ea/billing-ea-sql-db-reserved-capacity-csv.png)

1. **ReservationId** (Id. de reserva) en el campo **Información adicional** es la reserva que se aplicó al origen de SQL Database.
2. **ConsumptionMeter** es el identificador del medidor del recurso de SQL Database.
3. **Id. de medidor** es el medidor de la reserva con un costo de 0 $. Los recursos de SQL Database que cumplen los requisitos para la reserva muestran este identificador de medidor en el archivo CSV.

## <a name="usage-summary-page-in-enterprise-portal"></a>Página de resumen de uso del portal de Enterprise

El uso de reservas de Azure también aparece en la sección de resumen de uso del portal de Enterprise: ![Resumen de uso del Contrato Enterprise (EA)](./media/billing-understand-reserved-instance-usage-ea/billing-ea-reserved-instance-usagesummary.png)

1. No se le cobra por el componente de hardware de la máquina virtual, ya que está cubierto por la reserva. Para una reserva de SQL Database, verá una línea con **Nombre de servicio** como el uso de la capacidad reservada de Azure SQL Database.
2. En este ejemplo, no tiene la Ventaja híbrida de Azure, por lo que se le cobrará por el software de Windows utilizado con la máquina virtual.

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de las reservas de Azure, consulte los siguientes artículos:

- [¿Qué son las reservas de Azure?](billing-save-compute-costs-reservations.md)
- [Pago por adelantado de máquinas virtuales con Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Pago por adelantado por recursos de proceso de SQL Database con capacidad reservada de Azure SQL Database](../sql-database/sql-database-reserved-capacity.md) 
- [Administración de reservas de Azure](billing-manage-reserved-vm-instance.md)
- [Información sobre cómo se aplica el descuento por la reserva](billing-understand-vm-reservation-charges.md)
- [Información sobre el uso de reservas para suscripciones de pago por uso](billing-understand-reserved-instance-usage.md)
- [Costos de software de Windows no incluidos con reservas](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si tiene más preguntas, [póngase en contacto con el soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.