---
title: archivo de inclusión
description: archivo de inclusión
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 11/26/2018
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 8b236bc1f0089b89aca90e7c69e9b445b01a374b
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "52440430"
---
| Recurso | Límite predeterminado |
| --- | --- |
| Datos procesados |1000 TB/firewall/mes <sup>1</sup> |
|Reglas|10 k: todos los tipos de reglas combinados|
|Emparejamiento global|No compatible. Debe tener al menos una implementación de firewall por región.|
|Máximo de puertos en una sola regla de red|15<br>Un intervalo de puertos (por ejemplo: 2-10) se cuenta como dos.
|Tamaño mínimo de AzureFirewallSubnet |/26


<sup>1</sup> Si necesita aumentar estos límites, póngase en contacto con el soporte técnico de Azure.
