---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 01/22/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 3d100ec08b7b6d70366e605daf9c67edc6ab3bf3
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/16/2019
ms.locfileid: "56331075"
---
Las cuentas de Premium Storage tienen los siguientes objetivos de escalabilidad:

| Capacidad total de la cuenta | Ancho de banda total de una cuenta de almacenamiento con redundancia local |
| --- | --- | 
| Capacidad de disco: 35 TB <br>Capacidad de instantánea: 10 TB | Hasta 50 gigabits por segundo de entrada<sup>1</sup> y salida<sup>2</sup> |

<sup>1</sup> Todos los datos (solicitudes) que se envían a una cuenta de almacenamiento

<sup>1</sup> Todos los datos (respuestas) que se reciben desde una cuenta de almacenamiento

Si usa cuentas de Premium Storage para los discos no administrados y la aplicación supera los objetivos de escalabilidad de una cuenta de almacenamiento individual, es posible que quiera migrar a Managed Disks. Si no desea migrar a Managed Disks, compile la aplicación para que use varias cuentas de almacenamiento. A continuación, divida los datos en esas cuentas de almacenamiento. Por ejemplo, si desea asociar discos de 51 TB entre varias VM, distribúyalos entre dos cuentas de almacenamiento. 35 TB es el límite de una cuenta de Premium Storage única. Asegúrese de que una sola cuenta de Premium Storage nunca tenga más de 35 TB de discos aprovisionados.