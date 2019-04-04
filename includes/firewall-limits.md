---
title: archivo de inclusión
description: archivo de inclusión
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 3/25/2019
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 0467359cd9d6a067e519a62532f00459bc5f68cb
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58891088"
---
| Recurso | Límite predeterminado |
| --- | --- |
| Rendimiento de los datos |30 Gbps<sup>1</sup> |
|Reglas|10.000, todos los regla combinan de tipos.|
|Tamaño mínimo de AzureFirewallSubnet |/26|
|Intervalo de puertos en reglas de red y aplicación|0-64.000. El trabajo está en curso para relajar esta limitación.|
|Tabla de rutas|De forma predeterminada, AzureFirewallSubnet tiene una ruta 0.0.0.0/0 con el valor de NextHopType establecido en **Internet**.<br><br>Si habilita la tunelización forzada a local a través de ExpressRoute o VPN Gateway, es posible que deba configurar una ruta de 0.0.0.0/0 definido por el usuario (UDR) con el conjunto de valores de NextHopType como Internet explícitamente y asócielo con su AzureFirewallSubnet. Esto invalida una puerta de enlace predeterminada posibles publicidad de BGP a su red local. Si su organización requiere la tunelización forzada para que Firewall de Azure dirigir el tráfico de puerta de enlace predeterminada a través de la red local, póngase en contacto con soporte técnico. Podemos lista aprobada se mantiene su suscripción para garantizar el firewall requiere conectividad a Internet.|

<sup>1</sup>si necesita aumentar estos límites, póngase en contacto con soporte técnico de Azure.
