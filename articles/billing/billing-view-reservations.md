---
title: Visualización de Azure Reservations | Microsoft Docs
description: Aprenda a ver las reservas de Azure Reservations en Azure Portal.
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
ms.date: 10/03/2018
ms.author: cwatson
ms.openlocfilehash: 4506d65d173ea483558a772d89fe7cbc8cd84944
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "52584594"
---
# <a name="view-azure-reservations-in-the-azure-portal"></a>Visualización de las reservas de Azure Reservations en Azure Portal

Dependiendo del tipo de suscripción y los permisos, hay un par de maneras de considerar las reservas de Azure Reservations.

## <a name="view-reservations-as-owner-or-reader"></a>Visualización de las reservas como propietario o lector

De forma predeterminada, al comprar una reserva, usted y el administrador de cuenta pueden ver la reserva. Usted y el administrador de cuenta obtienen automáticamente el rol de propietario en la reserva. Para permitir que otras personas vean la reserva, debe agregarlos a ella como **propietario** o **lector**. Para más información, consulte [Agregar o cambiar los usuarios que pueden administrar una reserva](billing-manage-reserved-vm-instance.md#add-or-change-users-who-can-manage-a-reservation).
 
Para ver una reserva como propietario o lector,

1. Inicie sesión en el [Azure Portal]( http://portal.azure.com).
1. Busque **Reservations**.

    ![Captura de pantalla que muestra la búsqueda en Azure Portal](./media/billing-view-reservation/portal-reservation-search.png)

1. Verá una lista de las reservas donde tenga el rol de propietario o lector.

Si necesita cambiar el ámbito de una reserva, dividir una reserva o cambiar quién puede administrar una reserva, consulte [Administración de reservas para los recursos de Azure](billing-manage-reserved-vm-instance.md).

## <a name="view-reservation-transactions-for-enterprise-enrollments"></a>Visualización de transacciones de reserva para las inscripciones empresariales

 Si tiene una inscripción empresarial dirigida a asociados, consulte las reservas en **Informes** en el portal de EA. Para otras inscripciones empresariales, puede ver las reservas en el portal de EA y en Azure Portal. Debe ser un administrador de EA para ver las transacciones de reserva.

Para ver las transacciones de reserva en Azure Portal,

1. Inicie sesión en el [Azure Portal]( http://portal.azure.com).
1. Busque en **Administración de costos + facturación**.

    ![Captura de pantalla que muestra la búsqueda en Azure Portal](./media/billing-view-reservation/portal-cm-billing-search.png)

1. Seleccione **Transacciones de reserva**.
1. Para filtrar los resultados, seleccione **Intervalo de tiempo**, **Tipo** o **Descripción**.
1. Seleccione **Aplicar**.

    ![Captura de pantalla que muestra los resultados de las transacciones de reserva](./media/billing-view-reservation/portal-billing-reservation-transaction-results.png)

Para obtener los datos mediante una API, consulte [Get Reserved Instance transaction charges for enterprise customers](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-charges) (Obtención de cargos por transacciones de instancias reservadas para clientes empresariales).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de Azure Reservations, consulte los siguientes artículos:

- [¿Qué es Azure Reservations?](billing-save-compute-costs-reservations.md)
- [Pago por adelantado de la capacidad reservada de Azure Cosmos DB](../cosmos-db/cosmos-db-reserved-capacity.md)
- [Pago por adelantado por recursos de proceso de SQL Database con capacidad reservada de Azure SQL Database](../sql-database/sql-database-reserved-capacity.md)
- [Pago por adelantado de máquinas virtuales con Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Administración de Azure Reservations](billing-manage-reserved-vm-instance.md)
- [Información sobre el uso de reservas para suscripciones de pago por uso](billing-understand-reserved-instance-usage.md)
- [Información sobre el uso de reservas para la inscripción Enterprise](billing-understand-reserved-instance-usage-ea.md)
- [Información sobre el uso de reservas para suscripciones de CSP](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
