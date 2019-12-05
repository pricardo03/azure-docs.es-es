---
title: Configuración de la protección contra bots para WAF con Azure Front Door (versión preliminar)
description: Obtenga información sobre el firewall de aplicaciones web (WAF).
author: vhorne
ms.service: web-application-firewall
ms.topic: article
services: web-application-firewall
ms.date: 08/21/2019
ms.author: victorh
ms.openlocfilehash: 95660f764d28172ecb55a4952b785fea5f2aa4bb
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2019
ms.locfileid: "74186709"
---
# <a name="configure-bot-protection-for-web-application-firewall-preview"></a>Configuración de la protección contra bots para el firewall de aplicaciones web (versión preliminar)
En este artículo se muestra cómo configurar la regla de protección contra bots en un firewall de aplicaciones web (WAF) de Azure para Front Door mediante la CLI de Azure, Azure PowerShell o una plantilla de Azure Resource Manager.

Se puede habilitar un conjunto de reglas de protección contra bots administrado para que WAF realice acciones personalizadas en las solicitudes de direcciones IP malintencionadas conocidas. Las direcciones IP proceden de la fuente Inteligencia sobre amenazas de Microsoft. [Intelligent Security Graph](https://www.microsoft.com/security/operations/intelligence) impulsa la inteligencia sobre amenazas de Microsoft y lo utilizan numerosos servicios, incluido Azure Security Center.

> [!IMPORTANT]
> Actualmente, el conjunto de reglas de protección contra bots se encuentra en versión preliminar pública y se proporciona con un Acuerdo de nivel de servicio de versión preliminar. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.  Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Requisitos previos

Crear una directiva básica de WAF para Front Door siguiendo las instrucciones descritas en [Creación de una directiva de WAF para Azure Front Door mediante Azure Portal](waf-front-door-create-portal.md).

## <a name="enable-bot-protection-rule-set"></a>Habilitación del conjunto de reglas de protección contra bots

1. En la página de directivas básicas que creó en la sección anterior, en **Configuración**, haga clic en **Reglas**.
2. En la página de detalles, en la sección **Administrar reglas**, en el menú desplegable seleccione la casilla situada delante de la regla **BotProtection-preview-0.1** y **Guardar**.
    
   ![Regla de protección contra bots](.././media/waf-front-door-configure-bot-protection/botprotect2.png)

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [supervisar WAF](waf-front-door-monitor.md).
