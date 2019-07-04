---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 09/12/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 66e21aefa5648262ca2fcc61501905f725ac0e4b
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186219"
---
Antes de comenzar con la configuración, compruebe que se cumplen los criterios siguientes:

* Si ya tiene una red virtual a la que desea conectarse, verifique que ninguna de las subredes de la red local se superpone a las redes virtuales con las que desee conectarse. La red virtual no requiere una subred de puerta de enlace y no puede tener ninguna puerta de enlace de red virtual. Si no tiene una red virtual, puede crear una siguiendo los pasos de este artículo.
* Obtenga un intervalo de direcciones IP para la región del concentrador. El concentrador es una red virtual y el intervalo de direcciones que especifique para la región del concentrador no se puede superponer a ninguna de las redes virtuales existentes a las que ya esté conectado. Igualmente no se puede superponer a los intervalos de direcciones con las que esté conectadas en el entorno local. Si no está familiarizado con los intervalos de direcciones IP ubicados en la red local, necesita trabajar con alguien que pueda proporcionarle estos detalles.
* Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.