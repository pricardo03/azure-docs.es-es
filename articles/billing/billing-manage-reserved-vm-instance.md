---
title: Administración de reservas de Azure | Microsoft Docs
description: Aprenda a cambiar el ámbito de la suscripción y a administrar el acceso para reservas de Azure.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashesvi
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2018
ms.author: yashesvi
ms.openlocfilehash: d47c85d4197f45db50f1974b6faea270e6761237
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2018
ms.locfileid: "39628579"
---
# <a name="manage-reservations-for-resources-in-azure"></a>Administración de reservas para los recursos de Azure

Tras adquirir una reserva de Azure, puede ser conveniente aplicarla a otra suscripción distinta de la que se especificó durante la compra. O bien, si las máquinas virtuales, las bases de datos SQL u otros recursos correspondientes se ejecutan en varias suscripciones, quizá le interese cambiar el ámbito de reserva para que sea compartido. Para obtener el máximo descuento en la reserva, asegúrese de que el número de instancias compradas coincida con los atributos y el número de recursos que se estén ejecutando. Para más información, consulte [Reservas de Azure](https://go.microsoft.com/fwlink/?linkid=862121).

## <a name="change-the-scope-for-a-reservation"></a>Cambio del ámbito de una reserva

 El descuento de la reserva se aplica a las máquinas virtuales, las bases de datos SQL u otros recursos que se correspondan con la reserva y que se ejecuten dentro del ámbito de esta. El ámbito de una reserva puede ser una suscripción única o todas las suscripciones del contexto de facturación. Si establece que el ámbito es una suscripción única, la reserva se corresponderá con los recursos que se ejecuten en la suscripción seleccionada. Si establece que el ámbito es compartido, Azure asociará la reserva con los recursos que se ejecuten en todas las suscripciones dentro del contexto de facturación. El contexto de facturación depende de la suscripción que se usó para comprar la reserva.

Para cambiar el ámbito de una reserva:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los servicios** > **Reservations**.
3. Seleccione la reserva.
4. Seleccione **Configuración** > **Configuración**.
5. Cambie el ámbito. Si cambia el ámbito para que pase de ser compartido a único, solo podrá seleccionar suscripciones de las que sea el propietario. Únicamente se pueden seleccionar las suscripciones que pertenezcan al mismo contexto de facturación que la reserva. El contexto de facturación viene determinado por la suscripción que se seleccionó al adquirir la reserva. El ámbito solo se aplica a las suscripciones de las ofertas Pago por uso MS-AZR-0003P y Enterprise MS-AZR-0017P. Las suscripciones de desarrollo y pruebas de contratos Enterprise no son aptas para obtener el descuento de reserva.

## <a name="add-or-change-users-who-can-manage-a-reservation"></a>Agregar o cambiar los usuarios que pueden administrar una reserva

Para delegar la administración de una reserva, agregue usuarios a roles en dicha reserva. De forma predeterminada, la persona que compró la reserva y el administrador de cuenta tienen el rol de propietario en la reserva.

Puede administrar el acceso a las reservas de forma independiente de las suscripciones que obtienen el descuento de reserva. El hecho de que a un usuario se le concedan permisos para administrar una reserva no implica que también se le otorguen derechos para administrar la suscripción. Y si a un usuario se le conceden permisos para administrar una suscripción dentro del ámbito de la reserva, no significa que se le otorguen derechos para administrar la reserva.

Para delegar la administración de acceso en una reserva:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los servicios** > **Reservations** para enumerar las reservas a las que tiene acceso.
3. Seleccione la reserva cuyo acceso quiere delegar a otros usuarios.
4. En el menú, seleccione **Control de acceso (IAM)**.
5. Seleccione **Agregar** > **Rol** > **Propietario** (u otro rol si quiere conceder acceso limitado).
6. Escriba la dirección de correo electrónico del usuario al que quiera agregar como propietario. 
7. Seleccione el usuario y, después, **Guardar**.

## <a name="optimize-reserved-vm-instance-for-vm-size-flexibility-or-capacity-priority"></a>Optimización de la instancia reservada de máquina virtual para la prioridad de la capacidad o de la flexibilidad del tamaño de máquina virtual

 La flexibilidad de la instancia de máquina virtual aplica el descuento por reserva a otras máquinas virtuales del mismo [grupo de tamaño de máquina virtual](https://aka.ms/RIVMGroups). De forma predeterminada, cuando se comparte el ámbito de la reserva, se aplica la flexibilidad del tamaño de instancia y la capacidad del centro de datos no tiene prioridad para las implementaciones de máquina virtual. En las reservas donde el ámbito es único, puede optimizar la reserva para la prioridad de capacidad en lugar de la flexibilidad del tamaño de instancia de máquina virtual. La prioridad de capacidad reserva la capacidad del centro de datos para las implementaciones y ofrece mayor confianza en la capacidad de iniciar las instancias de máquina virtual cuando las necesita.

Para cambiar el ámbito de una reserva:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los servicios** > **Reservations**.
3. Seleccione la reserva.
4. Seleccione **Configuración** > **Configuración**.
5. Cambie la configuración de la optimización.

## <a name="split-a-single-reservation-into-two-reservations"></a>División de una reserva única en dos

 Si se compran varias instancias, se pueden asignar algunas instancias de una reserva a distintas suscripciones. De forma predeterminada, todas las instancias (la cantidad especificada durante la compra) tienen un ámbito, que puede ser de suscripción única o compartida. Pongamos por ejemplo que se han comprado diez máquinas virtuales D2 estándar y como ámbito se ha especificado la suscripción A. En caso de que interese, se puede establecer el ámbito de siete reservas en la suscripción A y el de las tres restantes en la suscripción B. Dividir una reserva permite distribuir las instancias para lograr una administración más precisa del ámbito. Si quiere simplificar la asignación a las suscripciones, elija el ámbito compartido. Sin embargo, para la administración de costos o fines presupuestarios, puede asignar las cantidades a suscripciones concretas.

 Una reserva se puede dividir en dos mediante PowerShell, CLI o la API.

### <a name="split-a-reservation-by-using-powershell"></a>División de una reserva con PowerShell

1. Ejecute el comando siguiente para obtener el identificador de pedido de reserva:

    ```powershell
    # Get the reservation orders you have access to
    Get-AzureRmReservationOrder
    ```

2. Obtenga los detalles de una reserva:

    ```powershell
    Get-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a
    ```

3. Divida la reserva en dos y distribuya las instancias:

    ```powershell
    # Split the reservation. The sum of the reservations, the quantity, must equal the total number of instances in the reservation that you're splitting.
    Split-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a -Quantity 3,2
    ```
4. Si quiere actualizar el ámbito, ejecute el siguiente comando:
    ```powershell
    Update-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId 5257501b-d3e8-449d-a1ab-4879b1863aca -AppliedScopeType Single -AppliedScope /subscriptions/15bb3be0-76d5-491c-8078-61fe3468d414
    ```

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de las reservas de Azure, consulte los siguientes artículos:

- [¿Qué son las reservas de Azure?](billing-save-compute-costs-reservations.md)
- [Pago por adelantado de máquinas virtuales con Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Pago por adelantado por recursos de proceso de SQL Database con capacidad reservada de Azure SQL Database](../sql-database/sql-database-reserved-capacity.md)
- [Información sobre cómo se aplica el descuento por la reserva](billing-understand-vm-reservation-charges.md)
- [Información sobre el uso de reservas para suscripciones de pago por uso](billing-understand-reserved-instance-usage.md)
- [Información sobre el uso de reservas para la inscripción Enterprise](billing-understand-reserved-instance-usage-ea.md)
- [Costos de software de Windows no incluidos con reservas](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si tiene más preguntas, [póngase en contacto con el soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.
