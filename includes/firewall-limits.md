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
ms.openlocfilehash: f1fd32fc02bee47d229093680166e6a13a5ba721
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2018
ms.locfileid: "49458880"
---
| Recurso | Límite predeterminado |
| --- | --- |
| Datos procesados |1000 TB/firewall/mes <sup>1</sup> |
|Reglas|10 k: todos los tipos de reglas combinados|
|Emparejamiento de VNET|Para las implementaciones de tipo hub-and-spoke, máximo de 50 redes virtuales de spoke.|
|Emparejamiento global|No compatible. Debe tener al menos una implementación de firewall por región.|
|Máximo de puertos en una sola regla de red|15<br>Un intervalo de puertos (por ejemplo: 2-10) se cuenta como dos.


<sup>1</sup> Si necesita aumentar estos límites, póngase en contacto con el soporte técnico de Azure.
