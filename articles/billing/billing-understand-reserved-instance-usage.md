---
title: Información sobre el uso de Azure Reservations para suscripciones de pago por uso | Microsoft Docs
description: Aprenda a interpretar los datos de uso para comprender como se aplican las suscripciones de pago por uso de reservas de Azure.
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
ms.date: 09/28/2018
ms.author: cwatson
ms.openlocfilehash: 81ee681fac4496ad2d5360f01926634373c23075
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "52584492"
---
# <a name="understand-azure-reservation-usage-for-your-pay-as-you-go-subscription"></a>Información sobre el uso de reservas de Azure para suscripciones de pago por uso

Use ReservationId en la [Página de reservas](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) y el archivo de uso del [Portal de cuentas de Azure](https://account.azure.com) para evaluar la página de reservas.

Si es un cliente con un contrato Enterprise, consulte [Uso de las reservas para la inscripción Enterprise.](billing-understand-reserved-instance-usage-ea.md).

En este artículo se da por hecho que la reserva se aplica a una única suscripción. Si se aplica a más de una, es posible que las ventajas de la reserva abarquen varios archivos CSV de uso.

## <a name="usage-for-reserved-virtual-machine-instances"></a>Uso de instancias reservadas de máquina virtual

En las siguientes secciones se da por hecho que está ejecutando una máquina virtual Windows Standard_DS1_v2 en la región Este de EE. UU. y que la información de la instancia reservada de la máquina virtual tiene una apariencia similar a la de la siguiente tabla:

| Campo | Valor |
|---| :---: |
|ReservationId |8117adfb-1d94-4675-be2b-f3c1bca808b6|
|Cantidad |1|
|SKU | Standard_DS1_v2|
|Region | estado |

La parte de hardware de la máquina virtual está cubierta, dado que la máquina virtual implementada coincide con los atributos de la reserva. Para ver qué software de Windows no está cubierto por la instancia reservada de máquina virtual, consulte [Costos de software de Windows de instancias reservadas de máquinas virtuales de Azure](billing-reserved-instance-windows-software-costs.md).

### <a name="statement-section-of-csv-file-for-vms"></a>Sección de instrucción del archivo CSV para máquinas virtuales

En esta sección del archivo CSV se muestra el uso total de su reserva. Aplique el filtro en el campo **Meter Subcategory** (Subcategoría de medidor) que contenga **"Reservation-"** (Reserva). Verá algo parecido a la siguiente captura de pantalla:

![Captura de pantalla de detalles de uso de reservas y los cargos filtrados](./media/billing-understand-reserved-instance-usage/billing-payg-reserved-instance-csv-statements.png)

La línea **Instancias reservadas de VM base** tiene el número total de horas cubiertas por la reserva. En esta línea se muestra 0,00 $ porque la reserva la cubre. En la línea **Reservation-Windows Svr (1 Core)** se cubre el costo del software de Windows.

### <a name="daily-usage-section-of-csv-file"></a>Sección de uso diario del archivo CSV

Filtre por **Información adicional** y escriba su **Identificador de reserva**. En la siguiente captura de pantalla se muestran los campos relacionados con la reserva.

![Captura de pantalla con los detalles y los cargos de uso diario](./media/billing-understand-reserved-instance-usage/billing-payg-reserved-instance-csv-details.png)

1. **ReservationId** en el campo **Información adicional** es la reserva que se aplicó a la máquina virtual.
2. **ConsumptionMeter** es el identificador del medidor de la máquina virtual.
3. En **Instancias reservadas de VM base**, la línea **Meter Subcategory** (Subcategoría de medidor) representa el costo de 0 $ en la sección de instrucciones. El costo de la ejecución de esta máquina virtual ya lo cubre la reserva.
4. **Id. de medidor** es el identificador del medidor de la reserva. El costo del medidor es de 0 $. Este identificador de medidor aparece para cualquier máquina virtual apta para el descuento de reserva.
5. Standard_DS1_v2 es una máquina virtual de vCPU y se implementa sin la Ventaja híbrida de Azure. Por lo tanto, este medidor cubre el costo extra del software de Windows. Para buscar el medidor correspondiente a la máquina virtual de 1 núcleo de serie D, consulte [Costos del software de Windows de las instancias de máquina virtual de reserva de Azure](billing-reserved-instance-windows-software-costs.md). Si tiene la Ventaja híbrida de Azure, no se aplicará este cargo adicional.

## <a name="usage-for-sql-database--cosmos-db-reserved-capacity-reservations"></a>Uso de reservas de capacidad reservada para SQL Database y Cosmos DB

En las siguientes secciones se usa Azure SQL Database como ejemplo para describir el informe de uso. Puede seguir los mismos pasos para obtener también el uso para Azure Cosmos DB. 

Imagine que ejecuta una instancia de SQL Database Gen 4 en la región Este de EE. UU. y que la información de la reserva tiene una apariencia similar a la de esta tabla:

| Campo | Valor |
|---| --- |
|ReservationId |446ec809-423d-467c-8c5c-bbd5d22906b1|
|Cantidad |2|
|Producto| SQL Database Gen 4 (2 núcleos)|
|Region | estado |

### <a name="statement-section-of-csv-file"></a>Sección de instrucción del archivo CSV

Filtre por el nombre del medidor **Uso de instancias reservadas** y elija la **categoría de medición** necesaria:-Azure SQL Database o Azure Cosmos DB. Verá algo parecido a la siguiente captura de pantalla:

![Archivo CSV para la capacidad reservada de SQL Database](./media/billing-understand-reserved-instance-usage/billing-payg-sql-db-reserved-capacity-csv-statements.png)

La línea **Uso de instancias reservadas** tiene el número total de horas básicas cubiertas por la reserva. La tasa es 0 $ para esta línea, ya que la reserva cubría el costo.

### <a name="detail-section-of-csv-file"></a>Sección de detalles del archivo CSV

Filtre por **Información adicional** y escriba su **Identificador de reserva**. En la siguiente captura de pantalla se muestran los campos relacionados con la reserva de capacidad reservada de SQL Database.

![Archivo CSV para la capacidad reservada de SQL Database](./media/billing-understand-reserved-instance-usage/billing-payg-sql-db-reserved-capacity-csv-details.png)

1. El campo **ReservationId** de **Información adicional** es la reserva de capacidad reservada de SQL Database que se aplica a los recursos de bases de datos SQL.
2. **ConsumptionMeter** es el identificador del medidor del recurso de SQL Database.
3. **Id. de medidor** es el medidor de la reserva. El costo del medidor es de 0 $. Los recursos de SQL Database que cumplen los requisitos para el descuento de reserva muestran este identificador de medidor en el archivo CSV.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de Azure Reservations, consulte los siguientes artículos:

- [¿Qué es Azure Reservations?](billing-save-compute-costs-reservations.md)
- [Pago por adelantado de máquinas virtuales con Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Pago por adelantado por recursos de proceso de SQL Database con capacidad reservada de Azure SQL Database](../sql-database/sql-database-reserved-capacity.md)
- [Administración de Azure Reservations](billing-manage-reserved-vm-instance.md)
- [Información sobre cómo se aplica el descuento por la reserva](billing-understand-vm-reservation-charges.md)
- [Información sobre el uso de reservas para la inscripción Enterprise](billing-understand-reserved-instance-usage-ea.md)
- [Costos de software de Windows no incluidos con Reservations](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).


