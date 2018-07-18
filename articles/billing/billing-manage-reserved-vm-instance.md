---
title: Administración de Azure Reserved Virtual Machine Instances | Microsoft Docs
description: Obtenga información sobre cómo cambiar el ámbito de la suscripción y administrar el acceso para Azure Reserved VM Instances.
services: billing
documentationcenter: ''
author: vikramdesai01
manager: vikramdesai01
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2018
ms.author: vikdesai
ms.openlocfilehash: ddb9d46dc2689b0dbcd8734e276916f7cd9d2728
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064700"
---
# <a name="manage-reserved-instances-in-azure"></a>Administración de instancias reservadas en Azure

Tras adquirir una instancia de Azure Reserved VM Instances, puede que quiera aplicar la instancia reservada a otra suscripción distinta de la que se especificó durante la compra. O bien, si las máquinas virtuales correspondientes se ejecutan en varias suscripciones, quizá le interese cambiar el ámbito de la instancia reservada para que sea compartido. Para obtener el máximo descuento en la instancia reservada, asegúrese de que el número de instancias compradas coincida con los atributos y el número de máquinas virtuales que se estén ejecutando. Para más información sobre las instancias reservadas de Azure, consulte [Ahorrar mediante el pago por adelantado de máquinas virtuales de Azure](https://go.microsoft.com/fwlink/?linkid=862121).

## <a name="change-the-scope-for-a-reserved-instance"></a>Cambio del ámbito de una instancia reservada
 El descuento de la instancia reservada se aplica a las máquinas virtuales que se corresponden con la instancia reservada y que se ejecutan dentro del ámbito de esta. El ámbito de una instancia reservada puede ser una suscripción única o todas las suscripciones del contexto de facturación. Si establece que el ámbito es una suscripción única, la instancia reservada se corresponderá con las máquinas virtuales que se ejecuten en la suscripción seleccionada. Si establece que ámbito es compartido, Azure asociará la instancia reservada con las máquinas virtuales que se ejecuten en todas las suscripciones del contexto de facturación. El contexto de facturación depende de la suscripción que se usó para comprar la instancia reservada. Para más información, consulte [Pagar por adelantado máquinas virtuales con instancias reservadas de máquina virtual](https://go.microsoft.com/fwlink/?linkid=861721).

Para cambiar el ámbito de una instancia reservada: 
1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los servicios** > **Reservations**.
3. Seleccione la instancia reservada.
4. Seleccione **Configuración** > **Configuración**.
5. Cambie el ámbito. Si cambia el ámbito para que pase de ser compartido a único, solo podrá seleccionar suscripciones de las que sea el propietario. Únicamente se pueden seleccionar las suscripciones que pertenezcan al mismo contexto de facturación que la instancia reservada. El contexto de facturación viene determinado por la suscripción que se seleccionó al adquirir la instancia reservada. El ámbito solo se aplica a las suscripciones de las ofertas Pago por uso MS-AZR-0003P y Enterprise MS-AZR-0017P. Las suscripciones de desarrollo y pruebas de contratos Enterprise no son aptas para obtener el descuento de instancia reservada.

## <a name="split-a-single-reserved-instance-into-two-reserved-instances"></a>División de una única instancia reservada en dos instancias reservadas
 Si se compran varias instancias, se pueden asignar algunas instancias de una instancia reservada a distintas suscripciones. De forma predeterminada, todas las instancias (la cantidad especificada durante la compra) tienen un ámbito, que puede ser de suscripción única o compartida. Pongamos por ejemplo que se han comprado 10 máquinas virtuales D2 estándar y como ámbito se ha especificado la suscripción A. En caso de que interese, se puede establecer el ámbito de tres instancias reservadas de máquina virtual en la suscripción A y el de las tres restantes en la suscripción B. Dividir una instancia reservada permite distribuir las instancias para lograr una administración más precisa del ámbito. Si quiere simplificar la asignación a las suscripciones, elija el ámbito compartido. Sin embargo, para la administración de costos o fines presupuestarios, puede asignar las cantidades a suscripciones concretas.

 Una instancia reservada se puede dividir en dos mediante PowerShell, CLI o la API.

### <a name="split-a-reserved-instance-by-using-powershell"></a>División de una instancia reservada con PowerShell
1. Ejecute el comando siguiente para obtener el identificador de pedido de la instancia reservada:

    ```powershell
    # Get the reserved instance orders you have access to
    Get-AzureRmReservationOrder
    ```
2. Obtenga los detalles de una instancia reservada:

    ```powershell
    Get-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a
    ```
3. Divida la instancia reservada en dos y distribuya las instancias:

    ```powershell
    # Split the reserved instance. The sum of the reserved instances, the quantity, must equal the total number of instances in the reserved instance that you're splitting.
    Split-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a -Quantity 3,2
    ```
1. Si quiere actualizar el ámbito, ejecute el siguiente comando:

    ```powershell
    Update-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId 5257501b-d3e8-449d-a1ab-4879b1863aca -AppliedScopeType Single -AppliedScope /subscriptions/15bb3be0-76d5-491c-8078-61fe3468d414
    ```

## <a name="add-or-change-users-who-can-manage-a-reserved-instance"></a>Adición o cambio de usuarios que pueden administrar una instancia reservada
Para delegar la administración de una instancia reservada, agregue usuarios a roles en dicha instancia reservada. De forma predeterminada, la persona que compró la instancia reservada y el administrador de cuenta tienen el rol de propietario en la instancia reservada. 

Puede administrar el acceso a las instancias reservadas de forma independiente de las suscripciones que obtienen el descuento de instancia reservada. El hecho de que a un usuario se le concedan permisos para administrar una instancia reservada no implica que también se le otorguen derechos para administrar la suscripción. Y si a un usuario se le conceden permisos para administrar una suscripción dentro del ámbito de la instancia reservada, no significa que se le otorguen derechos para administrar la instancia reservada.
 
Para delegar la administración de acceso en una instancia reservada: 
1.  Inicie sesión en [Azure Portal](https://portal.azure.com).
2.  Seleccione **Todos los servicios** > **Reserva** para enumerar las instancias reservadas a las que tiene acceso.
3.  Seleccione la instancia reservada cuyo acceso quiere delegar a otros usuarios.
4.  En el menú, seleccione **Control de acceso (IAM)**.
5.  Seleccione **Agregar** > **Rol** > **Propietario** (u otro rol si quiere conceder acceso limitado). 
6. Escriba la dirección de correo electrónico del usuario al que quiera agregar como propietario. 
7. Seleccione el usuario y, después, **Guardar**.

## <a name="next-steps"></a>Pasos siguientes
Para más información acerca de las instancias reservadas de Azure, consulte los siguientes artículos:

- [¿Qué es Azure Reserved VM Instances?](billing-save-compute-costs-reservations.md)
- [Pago por adelantado de máquinas virtuales con Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Información sobre cómo se aplica el descuento de instancia reservada de máquina virtual](billing-understand-vm-reservation-charges.md)
- [Información sobre el uso de instancias reservadas para suscripciones de pago por uso](billing-understand-reserved-instance-usage.md)
- [Interpretación del uso de instancias reservadas para la inscripción Enterprise](billing-understand-reserved-instance-usage-ea.md)
- [Costos de software de Windows no incluidos con las instancias reservadas](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si tiene más preguntas, [póngase en contacto con el soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.
