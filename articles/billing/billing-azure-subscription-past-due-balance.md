---
title: Correo electrónico de saldo vencido de Azure | Microsoft Docs
description: Describe cómo realizar el pago si la suscripción de Azure tiene un saldo pendiente
services: ''
documentationcenter: ''
author: genlin
manager: jureid
editor: ''
tags: billing
ms.assetid: d0b88c92-fb9d-4d12-931b-c26104ad63e9
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: banders
ms.openlocfilehash: 45b5d2c94a72cb23ef2b4a8db100623b0c58b0a9
ms.sourcegitcommit: db3fe303b251c92e94072b160e546cec15361c2c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2019
ms.locfileid: "66023395"
---
# <a name="resolve-past-due-balance-for-your-azure-subscription"></a>Saldo de la suscripción de Azure vencido

Si no se recibe su pago o si no podemos procesar su pago, es posible que reciba un correo electrónico o vea una alerta en el portal de Azure o el centro de cuentas.
Si es el [cuenta Administrador](billing-subscription-transfer.md#whoisaa), puede liquidar los cargos pendientes el [portal de Azure](https://portal.azure.com). Si se encuentra en un modo de factura de pago, envíe el pago a la ubicación indicada en la parte inferior de la factura.

> [!IMPORTANT]
> * Si tiene varias suscripciones que utilizan la misma tarjeta de crédito y son todas vencido, debe pagar el saldo pendiente todo a la vez.
> * El instrumento de pago que se utiliza para liquidar los cargos pendientes se convertirá en el nuevo método de pago activo para todas las suscripciones que estaba usando el método de pago con errores.

## <a name="resolve-past-due-balance-in-the-azure-portal"></a>Vencido equilibrio en el portal de Azure

1. Inicie sesión en el [portal Azure](https://portal.azure.com) como el Administrador de cuenta.
1. Busque en **Administración de costos + facturación**.
1. En la página información general, verá una lista de las suscripciones. Si el estado de la suscripción está vencido, haga clic en el **liquidar Saldo** vínculo.
    ![Captura de pantalla que muestra liquidar vínculo saldo](./media/billing-azure-subscription-past-due-balance/settle-balance-entry-point.png)
1. El saldo total pendiente refleja los cargos pendientes en todos los servicios de Microsoft mediante el método de pago con errores.
1. Seleccione un método de pago para pagar el saldo. Este método de pago se convertirá en el método de pago activo para todas las suscripciones que actualmente usa el método de pago con errores.
    ![Captura de pantalla que muestra el vínculo de método de pago de select](./media/billing-azure-subscription-past-due-balance/settle-balance-screen.png)
1. Si el método de pago seleccionados también tiene los cargos pendientes para los servicios de Microsoft, esto se reflejará en el saldo pendiente total. Deberá pagar los cargos pendientes, demasiado.
1. Haga clic en **pagar**.

## <a name="not-getting-billing-email-notifications"></a>¿No se reciben notificaciones de correo electrónico de facturación?

Si es el administrador de la cuenta, [compruebe qué dirección de correo electrónico se utiliza para las notificaciones](billing-how-to-change-azure-account-profile.md). Se recomienda que use una dirección de correo electrónico que consulte periódicamente. Si el correo electrónico es el adecuado, compruebe la carpeta de correo basura.

## <a name="if-i-forget-to-pay-what-happens"></a>¿Qué ocurre si se olvida de pagar?

Se cancelará el servicio y los recursos ya no estarán disponibles. Los datos de Azure se eliminarán a los 90 días de terminarse el servicio. Para más información, consulte el [Microsoft Trust Center: How we manage your data](https://go.microsoft.com/fwLink/p/?LinkID=822930&clcid=0x409) (Centro de confianza de Microsoft: Administración de los datos).

Si tiene la certeza de que se ha procesado el pago pero la suscripción sigue deshabilitada, póngase en contacto con el [Soporte técnico de Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="need-help-contact-us"></a>¿Ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [crear una solicitud de soporte técnico](https://go.microsoft.com/fwlink/?linkid=2083458).
