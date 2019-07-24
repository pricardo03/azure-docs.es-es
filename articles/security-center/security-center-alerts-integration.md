---
title: Integración de Security Center con productos de seguridad de Azure | Microsoft Docs
description: En este tema se presentan productos de seguridad de Azure que se han integrado con Azure Security Center.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: ad4b0373-08ee-46ca-a573-638ed93a647c
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: v-mohabe
ms.openlocfilehash: 64a636cc4452de1ef4a2d0e94629e7d8e3a5878d
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2019
ms.locfileid: "68295736"
---
# <a name="security-center-integration-with-azure-security-products-in-asc"></a>Integración de Security Center con productos de seguridad de Azure en ASC

Security Center ofrece a los clientes licencias adicionales de Microsoft para incorporar sus hallazgos a Security Center y verlos de forma consolidada.

* [Azure WAF](#azure-waf)
* [Azure DDoS](#azure-ddos)

## Azure WAF <a name="azure-waf"></a>

Firewall de aplicaciones web (WAF) es una característica de Azure Application Gateway que proporciona a las aplicaciones web una protección centralizada contra vulnerabilidades de seguridad comunes.

Las aplicaciones web son cada vez más el objetivo de ataques malintencionados que aprovechan vulnerabilidades habitualmente conocidas. El firewall de aplicaciones web de Application Gateway se basa en el conjunto de reglas básicas (CRS) 3.0 o 2.2.9 de OWASP (Open Web Application Security Project). El firewall de aplicaciones web se actualiza automáticamente para incluir protección frente a nuevas vulnerabilidades sin necesidad de configuración adicional. Las alertas generadas por el firewall de aplicaciones web se transmiten a Security Center. Para más información sobre las alertas generadas por el firewall de aplicaciones web, consulte este [artículo](https://docs.microsoft.com/azure/application-gateway/application-gateway-crs-rulegroups-rules?tabs=owasp3#crs911).

## Azure DDoS <a name="azure-ddos"></a>

Los ataques de denegación de servicio distribuido (DDoS) son conocidos por lo fáciles que son de ejecutar. Por lo tanto, se han convertido en un grave problema de seguridad para los clientes que mueven sus aplicaciones a la nube. 

Un ataque DDoS intenta agotar los recursos de una aplicación haciendo que esta no esté disponible para los usuarios legítimos. Los ataques DDoS pueden dirigirse a cualquier punto de conexión accesible a través de Internet.

Azure DDoS Protection, junto con los procedimientos recomendados de diseño de aplicaciones, proporciona una defensa frente a los ataques DDoS. Azure DDoS Protection proporciona distintos niveles de servicio. Para más información, consulte [Introducción a Azure DDoS Protection](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview).

El servicio Protección contra DDoS estándar puede mitigar los tipos de ataque siguientes:

> [!div class="mx-tableFixed"]

|Alerta|DESCRIPCIÓN|
|---|---|
|**Ataque volumétrico detectado**|el objetivo del ataque es desbordar la capa de la red con una gran cantidad de tráfico aparentemente legítimo. Esto incluye ataques de tipo "flood" de UDP, de amplificación y otros ataques de tipo "flood" de paquetes falsificados. El servicio Protección contra DDoS estándar mitiga estos posibles ataques de varios gigabytes, ya que los absorbe y los limpia automáticamente aprovechando la escala de red global de Azure.|
|**Ataque de protocolo detectado**:|estos ataques representan un destino inaccesible al aprovechar una vulnerabilidad en la pila de protocolo en los niveles 3 y 4. Esto incluye ataques de tipo “flood” de SYN, ataques de reflejo y otros ataques de protocolo. El servicio Protección contra DDoS estándar mitiga estos ataques al diferenciar entre el tráfico malintencionado y el legítimo. Para ello, interactúa con el cliente y bloquea el tráfico malintencionado.|
|**Ataque de nivel de recurso (aplicación) detectado**:|estos ataques van dirigidos a paquetes de aplicaciones web y su objetivo es interrumpir la transmisión de datos entre hosts. Los ataques incluyen infracciones de protocolo HTTP, inyección de código SQL, scripts de sitios y otros ataques de nivel 7. Use el firewall de aplicaciones web de Azure Application Gateway con DDoS Protection Standard para defenderse de estos ataques. También existen ofertas de firewall de aplicaciones web de terceros disponibles en Azure Marketplace.|
