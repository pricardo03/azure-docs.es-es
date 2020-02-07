---
title: Creación de recomendaciones de reservas de Azure
description: Obtenga información sobre cómo se crean las recomendaciones de reservas de Azure para máquinas virtuales.
author: banders
ms.reviewer: yashar
ms.service: cost-management-billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/28/2020
ms.author: banders
ms.openlocfilehash: a43e0c4d86bd47c953b50ab9fb1fd8df00e7df3d
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2020
ms.locfileid: "76851358"
---
# <a name="how-reservation-recommendations-are-created"></a>Creación de recomendaciones de reservas

Las recomendaciones de compra de instancias reservadas (RI) de Azure se generan mediante la [API de recomendaciones de reservas](/rest/api/consumption/reservationrecommendations) de Azure Consumption. También [Azure Advisor](../..//advisor/advisor-cost-recommendations.md#buy-reserved-virtual-machine-instances-to-save-money-over-pay-as-you-go-costs) utiliza las recomendaciones de la API. Advisor muestra las recomendaciones en Azure Portal.

Si tiene VM que se ejecutan en Azure, puede aprovechar las ventajas del precio con descuento para las RI y pagar por adelantado las VM. La API de recomendaciones de Microsoft Consumption evalúa el uso para 7, 30 y 60 días, y recomienda configuraciones óptimas para las RI. Calcula el costo que pagaría si no tuviese RI en relación con el costo que pagaría con las RI para optimizar el ahorro.

Azure Advisor muestra las recomendaciones basadas en un período de 30 días.

Para simplificar, en el ejemplo siguiente se muestran los cálculos que se hacen para una recomendación de siete días. El mismo método se aplica al calcular recomendaciones para 30 o 60 días.

## <a name="calculation-method-example"></a>Ejemplo de método de cálculo

Supongamos que el uso por hora de una VM Windows para una SKU y región específicas varía a lo largo de siete días (168 horas). El uso mínimo es de 65 unidades, y el uso máximo es de 127 unidades durante los siete días. En este ejemplo, la hora 79 usó 80 VM, y usted adquirió 75 RI.

Si compra 75 instancias reservadas, paga los siguientes costos por la hora 79:

- 75 instancias reservadas. El costo se paga por adelantado al adquirir las RI.
- Las instancias reservadas cubren el costo de hardware de ejecución de las VM, por lo que pagará por 75 horas al precio de solo software.
- El uso de la hora 79 es de 80, por lo que pagará cinco horas de Windows más el precio de la combinación de hardware. El precio de la combinación se basa en la tarifa del Contrato Enterprise (EA) o de pago por uso.

Si compra 75 RI, puede calcular el costo total si suma los costos por hora anteriores. También puede calcular el costo actual mediante la tarifa. La diferencia entre los dos importes es el ahorro para los siete días en este ejemplo.

La API realiza cálculos para cada punto de uso en particular. A continuación, devuelve la cantidad recomendada en la que se maximizan los ahorros. En el ejemplo siguiente, el gráfico muestra el pico de ahorro se encuentra en 68. Con posterioridad, el ahorro se reduce, por lo que la API recomienda 68.

![Diagrama que muestra el ahorro máximo](./media/reserved-instance-purchase-recommendations/peak-savings.png)

## <a name="other-expected-api-behavior"></a>Otro comportamiento esperado de la API

- La API muestra los posibles ahorros con la [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/) para Windows cuando se usa la ventaja. Si no se usa la ventaja, las recomendaciones de la API se basan en el costo básico de Windows. Si está a su disposición, considere la posibilidad de usar al Ventaja híbrida de Azure para aumentar los ahorros.
- Al usar un período de búsqueda de siete días, es posible que no obtenga recomendaciones cuando las VM estén apagadas durante más de un día.

## <a name="next-steps"></a>Pasos siguientes
- Más información sobre la [Aplicación del descuento por reserva de Azure en las máquinas virtuales](../manage/understand-vm-reservation-charges.md).
