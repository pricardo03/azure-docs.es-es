---
title: Visualización de reservas de recursos de Azure | Microsoft Docs
description: Aprenda a ver las reservas de Azure Reservations en Azure Portal.
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
ms.openlocfilehash: 872837c774368820527b12778b1a7dd4ddc5c7af
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60369227"
---
# <a name="view-azure-reservations-in-the-azure-portal"></a>Ver las reservas de Azure en Azure portal

Dependiendo del tipo de suscripción y los permisos, hay un par de formas de ver las reservas de Azure.

## <a name="view-purchased-reservations"></a>Ver las reservas adquiridas

De forma predeterminada, al comprar una reserva, usted y el administrador de cuenta pueden ver la reserva. Usted y el Administrador de cuenta obtienen automáticamente el rol de propietario en el pedido de reserva y la reserva. Para permitir que otras personas ver la reserva, debe agregarlos como un **propietario** o **lector** en el pedido de reserva o reserva.

Para más información, consulte [Agregar o cambiar los usuarios que pueden administrar una reserva](billing-manage-reserved-vm-instance.md#add-or-change-users-who-can-manage-a-reservation).

Para ver una reserva como propietario o lector,

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Busque **Reservations**.
    ![Captura de pantalla que muestra la búsqueda de Azure portal](./media/billing-view-reservation/portal-reservation-search.png)  
3. La lista muestra todas las reservas donde tiene el rol propietario o el lector. Cada reserva, muestra el último porcentaje de uso conocido.  
    ![Ejemplo que muestra una lista de reservas](./media/billing-view-reservation/view-reservations.png)
4. Seleccionar una reserva y ver la tendencia de utilización de los últimos cinco días.  
    ![Tendencias de uso de reserva de ejemplo que muestra](./media/billing-view-reservation/reservation-utilization.png)
5. También puede obtener el [utilización de la reserva](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage) mediante la API de uso de instancias reservadas y con el [paquete de contenido de Power BI de Microsoft Azure consumo Insights](/power-bi/service-connect-to-azure-consumption-insights).

Si necesita cambiar el ámbito de una reserva, dividir una reserva o cambiar quién puede administrar una reserva, consulte [Administración de reservas para los recursos de Azure](billing-manage-reserved-vm-instance.md).

## <a name="view-reservation-transactions-for-enterprise-enrollments"></a>Visualización de transacciones de reserva para las inscripciones empresariales

 Si tiene una inscripción empresarial dirigida a asociados, consulte las reservas en **Informes** en el portal de EA. Para otras inscripciones empresariales, puede ver las reservas en el portal de EA y en Azure Portal. Debe ser un administrador de EA para ver las transacciones de reserva.

Para ver las transacciones de reserva en Azure Portal,

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Busque en **Administración de costos + facturación**.

    ![Captura de pantalla que muestra la búsqueda en Azure Portal](./media/billing-view-reservation/portal-cm-billing-search.png)

1. Seleccione **Transacciones de reserva**.
1. Para filtrar los resultados, seleccione **Intervalo de tiempo**, **Tipo** o **Descripción**.
1. Seleccione **Aplicar**.

    ![Captura de pantalla que muestra los resultados de las transacciones de reserva](./media/billing-view-reservation/portal-billing-reservation-transaction-results.png)

Para obtener los datos mediante una API, consulte [Get Reserved Instance transaction charges for enterprise customers](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-charges) (Obtención de cargos por transacciones de instancias reservadas para clientes empresariales).

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de las reservas de Azure, consulte los siguientes artículos:

- [¿Cuáles son las reservas para Azure?](billing-save-compute-costs-reservations.md)
- [Administrar reservas para Azure](billing-manage-reserved-vm-instance.md)

Comprar un plan de servicio:

- [Pago por adelantado de la capacidad reservada de Azure Cosmos DB](../cosmos-db/cosmos-db-reserved-capacity.md)
- [Pago por adelantado por recursos de proceso de SQL Database con capacidad reservada de Azure SQL Database](../sql-database/sql-database-reserved-capacity.md)
- [Pago por adelantado de máquinas virtuales con Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)

Comprar un plan de software:

- [Pago por adelantado para los planes de software de Red Hat de las reservas de Azure](../virtual-machines/linux/prepay-rhel-software-charges.md)
- [Pago por adelantado para planes de software SUSE con Azure Reservations](../virtual-machines/linux/prepay-suse-software-charges.md)

Entender el uso de:

- [Información sobre el uso de reservas para suscripciones de pago por uso](billing-understand-reserved-instance-usage.md)
- [Información sobre el uso de reservas para la inscripción Enterprise](billing-understand-reserved-instance-usage-ea.md)
- [Información sobre el uso de reservas para suscripciones de CSP](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Ponerse en contacto con nosotros

Si tiene alguna pregunta o necesita ayuda, [crear una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).
