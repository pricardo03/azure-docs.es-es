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
ms.openlocfilehash: 2b1f9990985951a4e6ef260954968c0e1466c298
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60597052"
---
## <a name="scenario"></a>Escenario

Para ilustrar cómo se crean redes virtuales y subredes, en este documento se usa el siguiente escenario:

![Escenario de red virtual](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

En este escenario se crea una red virtual denominada **TestVNet** con un bloque CIDR reservado de **192.168.0.0./16**. La red virtual contiene las subredes siguientes: 

* **FrontEnd**, con **192.168.1.0/24** como su bloque CIDR.
* **BackEnd**, con **192.168.2.0/24** como su bloque CIDR.