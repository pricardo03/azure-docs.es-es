---
title: archivo de inclusión
description: archivo de inclusión
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 7/30/2018
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 0a4a66543b323b1dbe56158851dafcb1dfe695f0
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47185389"
---
| Recurso | Límite predeterminado |
| --- | --- |
| Datos procesados |1000 TB/firewall/mes <sup>1</sup> |
|Reglas|10 000 reglas de aplicación, 10 000 reglas de red|
|Emparejamiento de VNET|Para las implementaciones de tipo hub-and-spoke, máximo de 50 redes virtuales de spoke.|
|Emparejamiento global|No compatible. Debe tener al menos una implementación de firewall por región.|
|Máximo de puertos en una sola regla de red|15<br>Un intervalo de puertos (por ejemplo: 2-10) se cuenta como dos.


<sup>1</sup> Si necesita aumentar estos límites, póngase en contacto con el soporte técnico de Azure.
