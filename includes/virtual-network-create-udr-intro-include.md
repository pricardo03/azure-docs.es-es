---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-network
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: 482241deb1081ac8a5265a076eabbdc3fb6d659e
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66170857"
---
Aunque el uso de rutas del sistema facilita el tráfico automáticamente para su implementación, hay casos en los que seguramente quiera controlar el enrutamiento de paquetes a través de una aplicación virtual. Puede hacerlo mediante la creación de rutas definidas por el usuario que especifiquen el próximo salto de los paquetes que fluyen a una subred específica para que así vayan a su dispositivo virtual y se habilite el reenvío IP de la máquina virtual que funciona como aplicación virtual.

A continuación se indican algunos de los casos donde se pueden usar aplicaciones virtuales:

* Supervisión del tráfico con un sistema de detección de intrusos (IDS)
* Control del tráfico con un firewall

Para obtener más información sobre el enrutamiento definido por el usuario y el reenvío IP, visite [Rutas definidas por el usuario y reenvío IP](../articles/virtual-network/virtual-networks-udr-overview.md).

