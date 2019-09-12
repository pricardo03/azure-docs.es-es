---
title: Cómo se aplica un descuento de compra anticipada de Azure Databricks
description: Obtenga información sobre cómo se aplica a su uso un descuento de compra anticipada de Azure Databricks.
services: billing
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: banders
ms.openlocfilehash: 7c1855b587ab1d603e9c6ac24a67b0f50065361f
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "67827636"
---
# <a name="how-azure-databricks-pre-purchase-discount-is-applied"></a>Cómo se aplica un descuento de compra anticipada de Azure Databricks

Puede usar las unidades de confirmación de Azure Databricks (DBCU) que haya comprado de manera anticipada en cualquier momento durante el plazo de compra. Cualquier uso de Azure Databricks se deduce automáticamente de las DBCU compradas de manera anticipada.

A diferencia de las máquinas virtuales, las unidades compradas de manera anticipada no expiran cada hora y se pueden usar en cualquier momento durante el plazo de compra. Para obtener los descuentos de compra anticipada, no es necesario volver a implementar o asignar un plan adquirido de forma anticipada a las áreas de trabajo de Azure Databricks para el uso.

El descuento de compra anticipada solo se aplica al uso de unidades de Azure Databricks (DBU). Otros gastos tales como proceso, almacenamiento y redes se cobran por separado.

## <a name="pre-purchase-discount-application"></a>Aplicación del descuento de compra anticipada

La compra anticipada de Databricks se aplica a todos los niveles y cargas de trabajo de Databricks. Puede considerar la compra anticipada como un grupo de unidades de confirmación de Databricks pagadas previamente. El uso se deduce del grupo, independientemente de la carga de trabajo o el nivel. El uso se deduce según la proporción siguiente:

| **Carga de trabajo** | **Proporción de aplicación de DBU: nivel Estándar** | **Proporción de aplicación de DBU: nivel Premium** |
| --- | --- | --- |
| Análisis de datos | 0,4 | 0,55 |
| Ingeniería de datos | 0,15 | 0,30 |
| Ingeniería de datos ligera | 0,07 | 0,22 |

Por ejemplo, cuando se consume una cantidad de análisis de datos de nivel Estándar, se deducen 0,4 unidades de las unidades de confirmación de Databricks compradas de manera anticipada. Cuando se usa una cantidad de ingeniería de datos ligera de nivel Estándar, se deducen 0,07 unidades de la unidad de confirmación de Databricks comprada de manera anticipada.

## <a name="determine-plan-use"></a>Determinación del uso del plan

Para determinar el uso del plan de DBCU, vaya a Azure Portal > **Reservas** y haga clic en el plan de Databricks que ha comprado. Aparecerá la utilización hasta la fecha con todas las unidades restantes. Para obtener más información sobre cómo determinar el uso de reserva, consulte el artículo [Ver el uso de las reservas](billing-reservation-apis.md#see-reservation-usage).

## <a name="how-discount-application-shows-in-usage-data"></a>Cómo se muestra la aplicación del descuento en los datos de uso

Cuando el descuento de compra anticipada se aplica al uso de Databricks, los cargos a petición aparecen como cero en los datos de uso. Para obtener más información sobre los costos de reserva y el uso, vea [Obtención del uso y los costos de reservas de Contrato Enterprise](billing-understand-reserved-instance-usage-ea.md).

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre cómo administrar una reserva, consulte [Administración de Azure Reservations](billing-manage-reserved-vm-instance.md).
- Para obtener más información sobre la compra anticipada de Azure Databricks para ahorrar dinero, consulte [Optimización de los costes de Azure Databricks con una compra anticipada](billing-prepay-databricks-reserved-capacity.md).
- Para obtener más información acerca de Azure Reservations, consulte los siguientes artículos:
  - [¿Qué es Azure Reservations?](billing-save-compute-costs-reservations.md)
  - [Administración de reservas en Azure](billing-manage-reserved-vm-instance.md)
  - [Información sobre el uso de reservas de Azure para suscripciones de pago por uso](billing-understand-reserved-instance-usage.md)
  - [Información sobre el uso de reservas para la inscripción Enterprise](billing-understand-reserved-instance-usage-ea.md)
