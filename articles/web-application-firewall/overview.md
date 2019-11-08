---
title: Introducción al firewall de aplicaciones web de Azure
description: En este artículo se proporciona una introducción al firewall de aplicaciones web (WAF) de Azure.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.date: 10/04/2019
ms.author: victorh
ms.topic: overview
ms.openlocfilehash: 7b43a6bdac254493da8693b55158e15746e76dc3
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73488291"
---
# <a name="what-is-azure-web-application-firewall"></a>¿Qué es el firewall de aplicaciones web de Azure?

El firewall de aplicaciones web (WAF) ofrece una protección centralizada de las aplicaciones web contra las vulnerabilidades de seguridad más habituales. Las aplicaciones web son cada vez más el objetivo de ataques malintencionados que aprovechan vulnerabilidades habitualmente conocidas. Los scripts entre sitios y las inyecciones de código SQL están dentro de los ataques más comunes.

![Introducción a WAF](media/overview/wafoverview.png)

Evitar dichos ataques en el código de la aplicación es todo un desafío. Puede requerir un mantenimiento riguroso, revisión y supervisión en varias capas de la topología de la aplicación. Un firewall de aplicaciones web centralizado ayuda a simplificar muchísimo la administración de la seguridad. Un WAF también proporciona a los administradores de la aplicación a un mejor control de la protección contra amenazas e intrusiones.

Las soluciones de WAF pueden reaccionar más rápido ante una amenaza de la seguridad mediante la aplicación centralizada de revisiones que aborden una vulnerabilidad conocida, en lugar de proteger de manera individual cada aplicación web.

WAF se puede implementar con Azure Application Gateway y Azure Front Door Service. Actualmente, WAF tiene características personalizadas para cada servicio específico. Para más información sobre las características de WAF para cada servicio, consulte la introducción a cada servicio.

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre el firewall de aplicaciones web en Application Gateway, consulte [Firewall de aplicaciones web en Azure Application Gateway](./ag/ag-overview.md).
- Para más información sobre el firewall de aplicaciones web en Azure Front Door Service, consulte [Firewall de aplicaciones web en Azure Front Door Service](./afds/afds-overview.md).
