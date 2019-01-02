---
title: 'Error no se encontraron suscripciones: inicio de sesión de Azure Portal | Microsoft Docs'
description: Proporciona la solución para un problema en el que se produce un error del tipo No se encontraron suscripciones al iniciar sesión en Azure Portal o en el Centro de cuentas de Azure.
services: ''
documentationcenter: ''
author: genlin
manager: adpick
editor: ''
tags: billing
ms.assetid: d1545298-99db-4941-8e97-f24a06bb7cb6
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 05/11/2018
ms.author: cwatson
ms.custom: seodec18
ms.openlocfilehash: cef34d4e22732c2d718daf7ce10c23b25ff4649d
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53081310"
---
# <a name="no-subscriptions-found-sign-in-error-for-azure-portal-or-azure-account-center"></a>Error de inicio de sesión No se encontraron suscripciones en Azure Portal o en el Centro de cuentas de Azure

Puede recibir un mensaje de error "No se encontraron suscripciones" cuando intente iniciar sesión en [Azure Portal](https://portal.azure.com/) o en el [Centro de cuentas de Azure](https://account.windowsazure.com/Subscriptions). En este artículo se brinda una solución para este problema.

## <a name="symptom"></a>Síntoma

Cuando intenta iniciar sesión en [Azure Portal](https://portal.azure.com/) o en el [Centro de cuentas de Azure](https://account.windowsazure.com/Subscriptions), recibe el mensaje de error siguiente: "No se han encontrado suscripciones".

## <a name="cause"></a>Causa

Este problema se produce si seleccionó el directorio equivocado, o si la cuenta no tiene permisos suficientes. 

## <a name="solution"></a>Solución

### <a name="scenario-1-error-message-is-received-in-the-azure-portalhttpsportalazurecom"></a>Escenario 1: El mensaje de error se recibe en [Azure Portal](https://portal.azure.com)

Para corregir este problema:

* Asegúrese de que se seleccionó el directorio correcto de Azure haciendo clic en la cuenta pertinente en la esquina superior derecha.

  ![Selección del directorio en la parte superior derecha de Azure Portal](./media/billing-no-subscriptions-found/directory-switch.png)
* Si se ha seleccionado el directorio correcto de Azure pero sigue apareciendo el mensaje de error, [asigne el rol Propietario a la cuenta](../role-based-access-control/role-assignments-portal.md).

### <a name="scenario-2-error-message-is-received-in-the-azure-account-centerhttpsaccountwindowsazurecomsubscriptions"></a>Escenario 2: El mensaje de error se recibe en el [Centro de cuentas de Azure](https://account.windowsazure.com/Subscriptions)

Compruebe si la cuenta que ha usado es el administrador de cuentas. Para comprobar quién es el administrador de cuentas, siga estos pasos:

1. Inicie sesión en la [vista de suscripciones de Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Seleccione la suscripción que desee comprobar y, luego, consulte **Configuración**.
1. Seleccione **Propiedades**. El administrador de cuentas de la suscripción se muestra en el cuadro **Administrador de cuentas** .  

## <a name="need-help-contact-us"></a>¿Necesita ayuda? Póngase en contacto con nosotros.

Si tiene alguna pregunta o necesita ayuda, [cree una solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
