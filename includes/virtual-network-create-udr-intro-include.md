---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-network
author: rockboyfor
ms.service: virtual-network
ms.topic: include
origin.date: 04/13/2018
ms.date: 06/11/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 482241deb1081ac8a5265a076eabbdc3fb6d659e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60743399"
---
Aunque el uso de rutas del sistema facilita el tráfico automáticamente para su implementación, hay casos en los que seguramente quiera controlar el enrutamiento de paquetes a través de una aplicación virtual. Puede hacerlo mediante la creación de rutas definidas por el usuario que especifiquen el próximo salto de los paquetes que fluyen a una subred específica para que así vayan a su dispositivo virtual y se habilite el reenvío IP de la máquina virtual que funciona como aplicación virtual.

A continuación se indican algunos de los casos donde se pueden usar aplicaciones virtuales:

* Supervisión del tráfico con un sistema de detección de intrusos (IDS)
* Control del tráfico con un firewall

Para obtener más información sobre el enrutamiento definido por el usuario y el reenvío IP, visite [Rutas definidas por el usuario y reenvío IP](../articles/virtual-network/virtual-networks-udr-overview.md).