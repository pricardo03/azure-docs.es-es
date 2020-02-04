---
title: Cambio de la tarjeta de crédito para Azure
description: Describe cómo cambiar la tarjeta de crédito que se usa para pagar una suscripción de Azure.
author: bandersmsft
manager: jureid
tags: billing
ms.service: cost-management-billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: banders
ms.openlocfilehash: 6ae55e0075883de08af516b71089f00353975b34
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2020
ms.locfileid: "75985920"
---
# <a name="add-update-or-remove-a-credit-card-for-azure"></a>Agregar, actualizar o quitar una tarjeta de crédito para Azure

Este documento se aplica a los clientes que se han suscrito a Azure online con una tarjeta de crédito.

En Azure Portal, puede agregar una nueva tarjeta de crédito, actualizar una existente o eliminar una que no se use. Debe ser [administrador de la cuenta](billing-subscription-transfer.md#whoisaa) para realizar estos cambios.

Si tiene un [contrato de cliente de Microsoft](#check-access-to-a-microsoft-customer-agreement), los métodos de pago están asociados a perfiles de facturación. Aprenda a [cambiar el método de pago predeterminado de un perfil de facturación](#change-payment-method-for-a-billing-profile). Solo el usuario que se suscribió a Azure puede actualizar el método de pago.

**¿Desea cambiar al pago con factura (cheque o transferencia bancaria?** Consulte [Pago de suscripciones de Azure con factura](pay-by-invoice.md).

<a id="addcard"></a>

## <a name="add-a-new-credit-card-to-an-azure-subscription"></a>Adición de una nueva tarjeta de crédito a una suscripción de Azure

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador de la cuenta.
1. Busque **Administración de costos + facturación**.

    ![Captura de pantalla que muestra la búsqueda.](./media/change-credit-card/search.png)

1. Seleccione la suscripción a la que quiere agregar la tarjeta de crédito.
1. Seleccione **Métodos de pago**.

    ![Captura de pantalla que muestra la opción Administrar métodos de pago seleccionada.](./media/change-credit-card/payment-methods-blade-x.png)

1. En la esquina superior izquierda, seleccione "+" para agregar una tarjeta. Aparecerá un formulario de tarjeta de crédito a la derecha.
1. Escriba los detalles de la tarjeta de crédito.

    ![Captura de pantalla que muestra la adición de una nueva tarjeta.](./media/change-credit-card/sub-add-new-x.png)

1. Para que esta tarjeta sea su forma de pago activa, active la casilla situada junto a **Convertir en mi método de pago activo** en la parte superior del formulario. Esta tarjeta se convertirá en el instrumento de pago activo para todas las suscripciones que utilicen la misma tarjeta que la suscripción seleccionada.

1. Seleccione **Next** (Siguiente).

Si se produce un error después de agregar la tarjeta de crédito, consulte [Tarjeta de crédito rechazada al suscribirse a Azure](../../billing/billing-credit-card-fails-during-azure-sign-up.md).

## <a name="update-existing-credit-card"></a>Actualización de la tarjeta de crédito existente

Si se renueva la tarjeta de crédito y el número sigue siendo el mismo, actualice la información de la tarjeta de crédito existente, como la fecha de expiración. Si su número de tarjeta de crédito cambia porque la tarjeta se pierda, se la roben o caduque, siga los pasos de la sección [Adición de una tarjeta de crédito como método de pago](#addcard). No es necesario actualizar el CVV.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador de la cuenta.
1. Busque **Administración de costos + facturación**.

    ![Captura de pantalla que muestra la búsqueda.](./media/change-credit-card/search.png)

1. Seleccione **Métodos de pago**.

    ![Captura de pantalla que muestra la opción Administrar métodos de pago seleccionada.](./media/change-credit-card/payment-methods-blade-x.png)

1. Haga clic en la tarjeta de crédito que desea editar. Aparecerá un formulario de tarjeta de crédito a la derecha.

    ![Captura de pantalla que muestra la tarjeta de crédito seleccionada.](./media/change-credit-card/edit-card-x.png)

1. Actualice los detalles de la tarjeta de crédito.
1. Seleccione **Guardar**.

## <a name="use-a-different-credit-card"></a>Uso de una tarjeta de crédito diferente

Si se tiene el mismo método de pago activo en más de una suscripción, el cambio del método de pago activo en cualquiera de estas suscripciones también actualizará el método de pago activo en las demás.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador de la cuenta.
1. Busque **Administración de costos + facturación**.

    ![Captura de pantalla que muestra la búsqueda.](./media/change-credit-card/search.png)

1. Seleccione la suscripción a la que quiere agregar la tarjeta de crédito.
1. Seleccione **Métodos de pago**.

    ![Captura de pantalla que muestra la opción Administrar métodos de pago seleccionada.](./media/change-credit-card/payment-methods-blade-x.png)

1. Seleccione la casilla junto a la tarjeta que quiere convertir en el método de pago activo.
1. Haga clic en **Establecer como activa**.
    ![Captura de pantalla que muestra la tarjeta de crédito seleccionada y establecida como activa.](./media/change-credit-card/sub-change-active-x.png)

## <a name="remove-a-credit-card-from-the-account"></a>Eliminación de una tarjeta de crédito de la cuenta

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador de la cuenta.
1. Seleccione **Administración de costos + facturación** en el lado izquierdo de la página.

    ![Captura de pantalla que muestra la búsqueda.](./media/change-credit-card/search.png)

1. En **Facturación**, seleccione **Métodos de pago**.

    ![Captura de pantalla que muestra la opción Administrar métodos de pago seleccionada.](./media/change-credit-card/payment-methods-blade-x.png)

1. Seleccione la casilla situada junto a la tarjeta que desea quitar.
1. Haga clic en **Eliminar**.

Si la tarjeta de crédito es el método de pago activo de otras suscripciones de Microsoft, no podrá eliminarla de la cuenta de Azure. Cambie el método de pago activo para todas las suscripciones vinculadas a esta tarjeta de crédito y pruebe de nuevo.

## <a name="change-payment-method-for-a-billing-profile"></a>Cambio del método de pago de un perfil de facturación

Para cambiar el método de pago de un perfil de facturación, debe ser la persona que se suscribió a Azure.

Si desea cambiar el método de pago predeterminado a cheque o transferencia bancaria, aprenda a [cambiar un perfil de facturación a pago con cheque o transferencia bancaria](pay-by-invoice.md).

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Busque en **Administración de costos + facturación**.
1. En el menú izquierdo, haga clic en **Perfiles de facturación**.

    ![Captura de pantalla que muestra el perfil de facturación en el menú](./media/change-credit-card/billing-profile.png)

1. Seleccione un perfil de facturación.
1. En el menú de la izquierda, seleccione **Métodos de pago**.

   ![Captura de pantalla que muestra los métodos de pago del menú](./media/change-credit-card/billing-profile-payment-methods.png)

1. Sobre el método de pago predeterminado, haga clic en **Cambiar**.

    ![Captura de pantalla que muestra el botón Cambiar](./media/change-credit-card/customer-led-switch-credit-card.png)

1. Seleccione una tarjeta existente o agregue una nueva.

## <a name="troubleshooting"></a>Solución de problemas
No se admiten tarjetas virtuales ni prepago. Si aparecen errores al agregar o actualizar una tarjeta de crédito válida, pruebe a abrir el explorador en modo privado.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes
Las siguientes secciones responden a las preguntas más frecuentes sobre cómo cambiar la información de la tarjeta de crédito.

### <a name="my-subscription-is-disabled-why-cant-i-remove-my-credit-card-now"></a>Mi suscripción está deshabilitada. ¿Por qué no puedo eliminar mi tarjeta de crédito ahora?

Una vez que la suscripción está deshabilitada o se cancela, esperamos 90 días antes de eliminarla permanentemente. Mantenemos su método de pago en el archivo durante el período de retención por si desea reactivar la suscripción. Después de ese período, la suscripción se eliminará definitivamente.

Si tiene que quitar la tarjeta de crédito antes de que finalice el período de retención de 90 días, [vuelva a activar la suscripción](subscription-disabled.md). Si no puede reactivarla, [póngase en contacto con el soporte técnico de Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="why-do-i-keep-getting-your-login-session-has-expired-please-click-here-to-log-back-in"></a>¿Por qué sigo recibiendo un mensaje similar a "Su sesión de inicio de sesión ha expirado. Haga clic aquí para volver a iniciar sesión"?

Si sigue apareciendo este mensaje de error incluso si ya cerró la sesión y volvió a iniciarla, inténtelo de nuevo con una sesión de exploración privada.

### <a name="how-do-i-use-a-different-card-for-each-subscription-i-have"></a>¿Cómo se usa una tarjeta diferente para cada suscripción que tengo?

Desgraciadamente, si sus suscripciones ya están usando la misma tarjeta, no es posible separarlas para utilizar varias tarjetas. Sin embargo, cuando se registra con una nueva suscripción, puede elegir usar un nuevo método de pago para esa suscripción.

### <a name="how-do-i-make-payments"></a>¿Cómo puedo realizar los pagos?

Si ha configurado una tarjeta de crédito como método de pago, se le cobra automáticamente en su tarjeta después de finalizar cada período de facturación. No es necesario hacer nada más.

Si desea [pagar con factura](pay-by-invoice.md), envíe el pago a la ubicación indicada en la parte inferior de la factura.

### <a name="how-do-i-change-the-tax-id"></a>¿Cómo se cambia el número de identificación tributaria?

Para agregar o actualizar el número de identificación tributaria, actualice el perfil en el [Centro de cuentas de Azure](https://account.azure.com/Profile) y seleccione **Registro fiscal**. Este número de identificación tributaria se utiliza para los cálculos de exención fiscal y aparece en la factura.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Comprobación del acceso a un contrato de cliente de Microsoft
[!INCLUDE [billing-check-mca](../../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Pasos siguientes
- Obtenga información sobre [reservas de Azure](../reservations/save-compute-costs-reservations.md) para ver si pueden hacerle ahorrar dinero.
