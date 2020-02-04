---
title: Correo electrónico de saldo vencido de Azure
description: Describe cómo realizar el pago si la suscripción de Azure tiene un saldo pendiente
author: genlin
manager: dcscontentpm
tags: billing
ms.service: cost-management-billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: banders
ms.openlocfilehash: 7216af00413b1f8022957ac134f67a5c27b6cc78
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2020
ms.locfileid: "75984400"
---
# <a name="resolve-past-due-balance-for-your-azure-subscription"></a>Saldo de la suscripción de Azure vencido

Este artículo se aplica a los clientes con una cuenta de Microsoft Online Service Program.

Si no se recibe su pago o no puede procesarse, es posible que reciba un correo electrónico o vea una alerta en el Centro de cuentas de Azure o en Azure Portal.
Si es el [administrador de cuenta](billing-subscription-transfer.md#whoisaa), puede liquidar los cargos pendientes en [Azure Portal](https://portal.azure.com). Si se encuentra en un modo de factura de pago, envíe el pago a la ubicación indicada en la parte inferior de la factura.

> [!IMPORTANT]
> * Si tiene varias suscripciones que utilizan la misma tarjeta de crédito y todas están vencidas, deberá pagar todo el saldo pendiente a la vez.
> * El instrumento de pago que se utilice para liquidar los cargos pendientes se convertirá en el nuevo método de pago activo para todas las suscripciones que estaban usando el método de pago que produjo los errores.

## <a name="resolve-past-due-balance-in-the-azure-portal"></a>Resolución del saldo pendiente pasado en Azure Portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador de la cuenta.
1. Busque **Administración de costos + facturación**.
1. En la página Información general, verá una lista de las suscripciones. Si el estado de la suscripción está vencido, haga clic en el enlace **Liquidar saldo**.
    ![Captura de pantalla que muestra el enlace Liquidar saldo](./media/resolve-past-due-balance/settle-balance-entry-point.png)
1. El saldo total pendiente refleja los cargos pendientes de todos los servicios de Microsoft que utilizaban el método de pago que produjo los errores.
1. Seleccione un método de pago para pagar el saldo. Este método de pago se convertirá en el método de pago activo para todas las suscripciones que actualmente usan el método de pago con errores.
    ![Captura de pantalla que muestra el enlace Seleccionar método de pago](./media/resolve-past-due-balance/settle-balance-screen.png)
1. Si el método de pago seleccionado también tiene cargos pendientes de servicios de Microsoft, esto se reflejará en el saldo pendiente total. Deberá pagar esos cargos pendientes también.
1. Haga clic en **Pagar**.

## <a name="troubleshoot-declined-credit-card"></a>Solución del problema de tarjeta de crédito rechazada

Si su entidad financiera rechaza el cargo en la tarjeta de crédito, póngase en contacto con ella para resolver la incidencia. Cuando lo haga compruebe que:
- Las transacciones internacionales están habilitadas en la tarjeta.
- La tarjeta tiene suficiente límite de crédito o fondos para liquidar el saldo.
- Están habilitados los pagos periódicos en la tarjeta.

## <a name="not-getting-billing-email-notifications"></a>¿No se reciben notificaciones de correo electrónico de facturación?

Si es el administrador de la cuenta, [compruebe qué dirección de correo electrónico se utiliza para las notificaciones](change-azure-account-profile.md). Se recomienda que use una dirección de correo electrónico que consulte periódicamente. Si el correo electrónico es el adecuado, compruebe la carpeta de correo basura.

## <a name="if-i-forget-to-pay-what-happens"></a>¿Qué ocurre si se olvida de pagar?

Se cancelará el servicio y los recursos ya no estarán disponibles. Los datos de Azure se eliminarán a los 90 días de terminarse el servicio. Para más información, consulte el [Microsoft Trust Center: How we manage your data](https://go.microsoft.com/fwLink/p/?LinkID=822930&clcid=0x409) (Centro de confianza de Microsoft: Administración de los datos).

Si tiene la certeza de que se ha procesado el pago pero la suscripción sigue deshabilitada, póngase en contacto con el [Soporte técnico de Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).


## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).
