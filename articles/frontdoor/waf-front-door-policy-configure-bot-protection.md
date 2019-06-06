---
title: Configurar la protección de bot de firewall de aplicaciones web con Azure puerta de entrada (versión preliminar)
description: Obtenga información sobre firewall de aplicaciones web (WAF).
services: frontdoor
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/31/2019
ms.author: tyao;kumud
ms.openlocfilehash: af92f3b9049862fc19c69965f979b7dfe8c62526
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481630"
---
# <a name="configure-bot-protection-for-web-application-firewall-preview"></a>Configurar la protección de bot de firewall de aplicaciones web (versión preliminar)
Este artículo muestra cómo configurar regla de protección de bot en firewall de aplicaciones web de Azure (WAF) para la puerta de entrada mediante el uso de la plantilla de Azure Resource Manager, Azure PowerShell o CLI de Azure.

Un conjunto de reglas de protección de Bot administrado se puede habilitar para que el WAF realizar acciones personalizadas en las solicitudes de direcciones IP malintencionadas conocidas. Se obtienen las direcciones IP de la fuente de Microsoft Threat Intelligence. [Intelligent Security Graph](https://www.microsoft.com/security/operations/intelligence) alimenta la inteligencia de amenazas de Microsoft y se usa en varios servicios, incluidos Azure Security Center.

> [!IMPORTANT]
> Conjunto de reglas de protección de bot está actualmente en versión preliminar pública y se proporciona con un acuerdo de nivel de servicio de vista previa. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.  Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Requisitos previos

Crear una directiva básica de WAF para puerta delantera siguiendo las instrucciones descritas en [crear una directiva de WAF para Azure puerta de entrada mediante el portal de Azure](waf-front-door-create-portal.md).

## <a name="enable-bot-protection-rule-set"></a>Habilitar conjunto de reglas de protección de bot

1. En la página de directivas básicas que creó en la sección anterior, en **configuración**, haga clic en **reglas**.
2. En la página de detalles, en la **administrar reglas** sección, en el menú desplegable, seleccione la casilla situada delante de la regla **BotProtection-preview-0.1**y, a continuación, seleccione **guardar**anteriormente.
    
   ![Regla de protección de bot](./media/waf-front-door-configure-bot-protection/botprotect2.png)

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [supervisa el WAF](waf-front-door-monitor.md).
