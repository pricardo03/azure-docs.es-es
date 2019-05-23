---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: ae36adc78d8c87d85c64fd61cb3a50dfcae60b85
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66170539"
---
Se paga por dos cosas: los costos de proceso por horas de la puerta de enlace de red virtual y la transferencia de datos de salida de esta. Puede encontrar más información sobre los precios en la página de [precios](https://azure.microsoft.com/pricing/details/vpn-gateway) .

**Costos del proceso de puerta de enlace de red virtual**<br>Cada puerta de enlace de red virtual tiene un costo de proceso por horas. El precio se basa en la SKU de la puerta de enlace que especifique al crear una puerta de enlace de red virtual. El costo es para la puerta de enlace en sí y se agrega a la transferencia de datos que pasa a través de la puerta de enlace. El costo de una configuración activa-activa es igual que el de activa-pasiva.

**Costos de la transferencia de datos**<br>Los costos de transferencia de datos se calculan en función del tráfico de salida de la puerta de enlace de red virtual de origen.

* Si va a enviar tráfico al dispositivo VPN local, se le cobrará por la tasa de transferencia de datos de salida de Internet.
* Si va a enviar tráfico entre redes virtuales que se encuentren en diferentes regiones, el precio dependerá de la región.
* Si va a enviar tráfico solo entre redes virtuales que están en la misma región, no habrá ningún costo por datos. El tráfico entre redes virtuales de la misma región es gratuito.