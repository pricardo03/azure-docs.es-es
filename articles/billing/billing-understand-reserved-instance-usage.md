---
title: Información sobre el uso de instancias reservadas de Azure para suscripciones de pago por uso | Microsoft Docs
description: Aprenda a interpretar los datos de uso para comprender como se aplican las suscripciones de pago por uso de Azure Reserved VM Instances.
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
ms.date: 05/09/2018
ms.author: manshuk
ms.openlocfilehash: 7e303f3e5ce0e618d941be4190f6fadb40f2e09d
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064489"
---
# <a name="understand-reserved-instance-usage-for-your-pay-as-you-go-subscription"></a>Información sobre el uso de instancias reservadas para suscripciones de pago por uso

Le explicamos el uso de una instancia reservada de máquina virtual de Azure con el valor de ReservationId de la [Página de reservas](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) y el archivo de uso del [Portal de cuentas de Azure](https://account.azure.com).


>[!NOTE]
>El contenido de este artículo no se aplica a los clientes de EA. Si es un cliente de EA, consulte [Información sobre el uso de instancias reservadas para suscripciones Pago por uso](billing-understand-reserved-instance-usage-ea.md). En este artículo también se da por hecho que la instancia reservada se aplica a una única suscripción. Si se aplica a más de una suscripción, es posible que las ventajas de la instancia reservada abarquen varios archivos CSV de uso. 

En la siguiente sección se da por hecho que está ejecutando una máquina virtual Windows Standard_DS1_v2 en la región Este de EE. UU. y que la información de la instancia reservada tiene un aspecto similar al de la siguiente tabla:

| Campo | Valor |
|---| :---: |
|ReservationId |8117adfb-1d94-4675-be2b-f3c1bca808b6|
|Cantidad |1|
|SKU | Standard_DS1_v2|
|Region | estado |

## <a name="reserved-instance-application"></a>Aplicación de instancia reservada

La parte de hardware de la máquina virtual está cubierta, dado que la máquina virtual implementada coincide con los atributos de la instancia reservada. Para ver qué software de Windows no está cubierto por la instancia reservada, vea los [costos de software de Windows de instancias reservadas de máquinas virtuales de Azure](billing-reserved-instance-windows-software-costs.md).

### <a name="statement-section-of-csv"></a>Sección de instrucción del archivo CSV
En esta sección del archivo CSV se muestra el uso total de su instancia reservada. Aplique el filtro del campo Meter Subcategory que contiene "Reservation-" y los datos aparecerán como en la siguiente captura de pantalla: ![Captura de pantalla de la información de uso y gastos de la instancia reservada filtrada](./media/billing-understand-reserved-instance-usage/billing-payg-reserved-instance-csv-statements.png).

La línea Instancias reservadas de VM base tiene el número total de horas cubiertas por la instancia reservada. En esta línea se muestra 0,00 $ porque los gastos están cubiertos por la instancia reservada. En la línea Reservation-Windows Svr (1 Core) se cubre el costo del software de Windows.

### <a name="daily-usage-section-of-csv"></a>Sección de uso diario del archivo CSV
Filtre por información adicional y escriba su **identificador de reserva**. En la siguiente captura de pantalla se muestran los campos relacionados con la instancia reservada. 

![Captura de pantalla con los detalles y los cargos de uso diario](./media/billing-understand-reserved-instance-usage/billing-payg-reserved-instance-csv-details.png)

1. **ReservationId** en el campo Additional Info es la instancia reservada que se usó para aplicar las ventajas a la máquina virtual.
2. ConsumptionMeter es el id. de medidor de la máquina virtual.
3. La línea Instancias reservadas de VM base de la columna Meter Sub-category representa la línea de costo de 0$ en la sección de instrucciones. El costo de ejecución de esta máquina virtual ya lo cubre la instancia reservada.
4. Este es el identificador de medidor de la instancia reservada. El costo del medidor es de 0 $. Cualquier máquina virtual apta para la instancia reservada tiene este MeterId en el CSV para cubrir el costo. 
5. Standard_DS1_v2 es una máquina virtual de vCPU y se implementa sin la Ventaja híbrida de Azure. Por lo tanto, este medidor cubre el costo extra del software de Windows. Consulte los [costos de software de Windows de las instancias reservadas de máquina virtual de Azure](billing-reserved-instance-windows-software-costs.md) para buscar el medidor correspondiente a la máquina virtual de 1 núcleo de la serie D. Si se usa la Ventaja híbrida de Azure, no se aplicará este cargo adicional. 

## <a name="next-steps"></a>Pasos siguientes
Para más información acerca de las instancias reservadas, consulte los siguientes artículos:

- [¿Qué es Azure Reserved Virtual Machine Instances?](billing-save-compute-costs-reservations.md)
- [Pago por adelantado de máquinas virtuales con Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Administración de instancias reservadas en Azure](billing-manage-reserved-vm-instance.md)
- [Información sobre cómo se aplica el descuento de instancia reservada de máquina virtual](billing-understand-vm-reservation-charges.md)
- [Interpretación del uso de instancias reservadas para la inscripción Enterprise](billing-understand-reserved-instance-usage-ea.md)
- [Costos de software de Windows no incluidos con las instancias reservadas](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si tiene más preguntas, [póngase en contacto con el soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.
