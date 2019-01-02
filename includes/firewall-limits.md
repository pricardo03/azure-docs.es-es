---
title: archivo de inclusión
description: archivo de inclusión
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 12/14/2018
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 7d905550114bb76a0a091146b3972bab4a652022
ms.sourcegitcommit: b254db346732b64678419db428fd9eb200f3c3c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2018
ms.locfileid: "53430043"
---
| Recurso | Límite predeterminado |
| --- | --- |
| Datos procesados |1000 TB/firewall/mes <sup>1</sup> |
|Reglas|10 k: todos los tipos de reglas combinados|
|Emparejamiento global|No compatible. Debe tener al menos una implementación de firewall por región.|
|Máximo de puertos en una sola regla de red|15<br>Un intervalo de puertos (por ejemplo: 2-10) se cuenta como dos.
|Tamaño mínimo de AzureFirewallSubnet |/26|
|Intervalo de puertos en reglas de red y aplicación|0-64.000. El trabajo está en curso para relajar esta limitación.|
|


<sup>1</sup> Si necesita aumentar estos límites, póngase en contacto con el soporte técnico de Azure.
