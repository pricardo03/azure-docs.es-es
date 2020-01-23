---
title: Alertas de detección de amenazas de productos de seguridad de Azure en Azure Security Center
description: En este tema se presentan los productos de seguridad de Azure de los que Azure Security Center puede mostrar alertas de amenazas
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: ad4b0373-08ee-46ca-a573-638ed93a647c
ms.service: security-center
ms.topic: conceptual
ms.date: 01/12/2020
ms.author: memildin
ms.openlocfilehash: 16cae819b1714c2b410cfa311a3602e0f4ed968a
ms.sourcegitcommit: e9776e6574c0819296f28b43c9647aa749d1f5a6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2020
ms.locfileid: "75913032"
---
# <a name="threat-detection-alerts-from-azure-waf-and-azure-ddos-protection"></a>Alertas de detección de amenazas desde Azure WAF y Azure DDoS Protection

Azure Security Center puede mostrar y recopilar alertas de detección de amenazas generadas por los siguientes productos de seguridad de Azure (es obligatoria una licencia independiente para cada producto):

* [Azure WAF](#azure-waf)
* [Azure DDoS Protection](#azure-ddos)

## Azure WAF <a name="azure-waf"></a>

Firewall de aplicaciones web (WAF) es una característica de Azure Application Gateway que proporciona a las aplicaciones web una protección centralizada contra vulnerabilidades de seguridad comunes.

Las aplicaciones web son cada vez más el objetivo de ataques malintencionados que aprovechan vulnerabilidades habitualmente conocidas. El firewall de aplicaciones web de Application Gateway se basa en el conjunto de reglas básicas 3.0 o 2.2.9 de Open Web Application Security Project. El firewall de aplicaciones web se actualiza automáticamente para incluir protección frente a nuevas vulnerabilidades sin necesidad de configuración adicional. Las alertas de WAF se transmiten a Security Center. Para más información sobre las alertas generadas por WAF, consulte [Reglas y grupos de reglas de CRS del firewall de aplicaciones web](../web-application-firewall/ag/application-gateway-crs-rulegroups-rules.md?tabs=owasp31#crs911-31).

## Azure DDoS Protection <a name="azure-ddos"></a>

Los ataques de denegación de servicio distribuido (DDoS) son conocidos por lo fáciles que son de ejecutar. Se han convertido en un problema de seguridad muy importante, especialmente si va a trasladar sus aplicaciones a la nube. 

Un ataque DDoS intenta agotar los recursos de una aplicación haciendo que esta no esté disponible para los usuarios legítimos. Los ataques DDoS pueden dirigirse a cualquier punto de conexión accesible a través de Internet.

Azure DDoS Protection, junto con los procedimientos recomendados de diseño de aplicaciones, proporciona una defensa frente a los ataques DDoS. DDoS Protection proporciona distintos niveles de servicio. Para más información, consulte [Introducción a Azure DDoS Protection](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview).

Para obtener una lista de las alertas de Azure DDoS Protection, consulte la [Tabla de referencia de alertas](alerts-reference.md#alerts-azureddos).