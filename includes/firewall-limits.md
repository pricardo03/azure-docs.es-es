---
title: archivo de inclusión
description: archivo de inclusión
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 5/3/2019
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 8709d4d903bd31ff94d04ec61e226857e4190407
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65150344"
---
| Recurso | Límite predeterminado |
| --- | --- |
| Rendimiento de los datos |30 Gbps<sup>1</sup> |
|Reglas|10.000, todos los regla combinan de tipos.|
|Tamaño mínimo de AzureFirewallSubnet |/26|
|Intervalo de puertos en reglas de red y aplicación|0-64.000. El trabajo está en curso para relajar esta limitación.|
|Tabla de rutas|De forma predeterminada, AzureFirewallSubnet tiene una ruta 0.0.0.0/0 con el valor de NextHopType establecido en **Internet**.<br><br>Firewall de Azure debe tener conectividad directa a Internet. Si su AzureFirewallSubnet aprende una ruta predeterminada a la red local a través de BGP, debe reemplazar con una UDR 0.0.0.0/0 con el **NextHopType** establece valor como **Internet** mantener directa Conectividad a Internet. De forma predeterminada, Firewall de Azure no admite la tunelización forzada a una red local.<br><br>Sin embargo, si la configuración requiere la tunelización forzada a una red local, Microsoft proporcionará soporte en el caso por caso. Póngase en contacto con soporte técnico para que podamos revisar su caso. Si acepta, se obtendrá la lista blanca su suscripción y asegúrese de que se mantiene la conectividad de Internet de firewall necesarias.|

<sup>1</sup>si necesita aumentar estos límites, póngase en contacto con soporte técnico de Azure.
