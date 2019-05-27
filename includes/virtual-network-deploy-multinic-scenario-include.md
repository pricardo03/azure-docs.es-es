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
ms.openlocfilehash: d20ef44fd5c117e4e3a568542bb022c451ac23fc
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66170850"
---
## <a name="scenario"></a>Escenario
Este documento le guiará a través de una implementación que usa varias NIC en máquinas virtuales en un escenario concreto. En este escenario, tiene una carga de trabajo de IaaS de dos niveles hospedada en Azure. Cada nivel se implementa en su propia subred de una red virtual (VNet). El nivel de front-end se compone de varios servidores web, agrupados en un equilibrador de carga de alta disponibilidad. El nivel de back-end se compone de varios servidores de bases de datos. Estos servidores de bases de datos se implementarán con dos NIC cada uno: una para el acceso a la base de datos y la otra para la administración. El escenario también incluye los grupos de seguridad de red (NSGs) para controlar qué tráfico se permite en cada subred y la NIC de la implementación. La siguiente ilustración muestra la arquitectura básica de este escenario:

![Escenario de MultiNIC](./media/virtual-network-deploy-multinic-scenario-include/Figure1.png)

