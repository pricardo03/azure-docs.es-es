---
title: Interpretación del uso de instancias reservadas de Azure para la inscripción Enterprise | Microsoft Docs
description: Aprenda a interpretar los datos de uso para comprender como se aplican las inscripciones Enterprise de Azure Reserved VM Instances.
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
ms.openlocfilehash: d6e8b2544f919abeb7fde0e37fc12bd29f0171ef
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064701"
---
# <a name="understand-azure-reserved-instance-usage-for-your-enterprise-enrollment"></a>Interpretación del uso de instancias reservadas de Azure para la inscripción Enterprise
Le explicamos el uso de instancias reservadas con el valor de **ReservationId** de la [Página de reservas](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) y el archivo de uso del [portal de EA](https://ea.azure.com). También puede ver el uso de la instancia reservada en la sección de resumen de uso del [portal de EA](https://ea.azure.com).

>[!NOTE]
>Si compró la instancia reservada en un contexto de facturación de pago por uso, consulte [Información sobre el uso de instancias reservadas para suscripciones de pago por uso.](billing-understand-reserved-instance-usage.md)

En la siguiente sección se da por hecho que está ejecutando una máquina virtual Windows Standard_D1_v2 en la región Este de EE. UU. y que la información de la instancia reservada tiene un aspecto similar al de la siguiente tabla:

| Campo | Valor |
|---| --- |
|ReservationId |8f82d880-d33e-4e0d-bcb5-6bcb5de0c719|
|Cantidad |1|
|SKU | Standard_D1|
|Region | estado |

## <a name="reserved-instance-application"></a>Aplicación de instancia reservada

La parte de hardware de la máquina virtual está cubierta, dado que la máquina virtual implementada coincide con los atributos de la instancia reservada. Para ver qué software de Windows no está cubierto por la instancia reservada, vaya a los costos de software de Azure Reserved VM Instances y vea los [costos de software de Windows para Azure Reserved VM Instances](billing-reserved-instance-windows-software-costs.md).


### <a name="reserved-instance-usage-in-csv"></a>Uso de la instancia reservada en csv
Puede descargar el archivo CSV de uso de EA desde el portal de EA. En el archivo CSV descargado, filtre por la columna Additional Info (Información adicional) y escriba su **identificador de reserva**. En la siguiente captura de pantalla se muestran los campos relacionados con la instancia reservada:

![Archivo csv del Contrato Enterprise (EA) para instancias reservadas de Azure](./media/billing-understand-reserved-instance-usage-ea/billing-ea-reserved-instance-csv.png)

1. **ReservationId** en el campo Additional Info representa a la instancia reservada que se usó para aplicar las ventajas a la máquina virtual.
2. ConsumptionMeter es el valor de MeterId de la máquina virtual.
3. Se trata de ReservationMeter sin ningún costo, ya que el costo de la máquina virtual en ejecución ya se ha pagado con la instancia reservada. 
4. Standard_D1 es una máquina virtual de vCPU y se implementa sin la Ventaja híbrida de Azure. Por lo tanto, este medidor cubre el costo extra del software de Windows. Consulte los [costos de software de Windows de las instancias reservadas de máquina virtual de Azure](billing-reserved-instance-windows-software-costs.md) para buscar el medidor correspondiente a la máquina virtual de un núcleo de la serie D. Si se usa la Ventaja híbrida de Azure, no se aplicará este cargo adicional.

### <a name="reserved-instance-usage-in-usage-summary-page-in-ea-portal"></a>Uso de la instancia reservada en la página de resumen de uso del portal de EA

El uso de instancias reservadas también aparece en la sección de resumen de uso del portal de EA: ![Resumen de uso del Contrato Enterprise (EA)](./media/billing-understand-reserved-instance-usage-ea/billing-ea-reserved-instance-usagesummary.png)

1. No se le cobra por el componente de hardware de la máquina virtual, ya que está cubierto por la instancia reservada. 
2. Se le cobra por el software de Windows, ya que la Ventaja híbrida de Azure no se usa. 

## <a name="next-steps"></a>Pasos siguientes
Para más información acerca de las instancias reservadas de Azure, consulte los siguientes artículos:

- [¿Qué es Azure Reserved Virtual Machine Instances?](billing-save-compute-costs-reservations.md)
- [Pago por adelantado de máquinas virtuales con Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Administración de instancias reservadas en Azure](billing-manage-reserved-vm-instance.md)
- [Información sobre cómo se aplica el descuento de instancia reservada de máquina virtual](billing-understand-vm-reservation-charges.md)
- [Información sobre el uso de instancias reservadas para suscripciones de pago por uso](billing-understand-reserved-instance-usage.md)
- [Costos de software de Windows no incluidos con las instancias reservadas](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si tiene más preguntas, [póngase en contacto con el soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.