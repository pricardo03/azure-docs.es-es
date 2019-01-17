---
title: archivo de inclusión
description: archivo de inclusión
services: backup
author: rayne-wiselman
ms.service: backup
ms.topic: include
ms.date: 12/07/2018
ms.author: raynew
ms.custom: include file
ms.openlocfilehash: 9ad1208283527f35e04dede2706fd0e424096dc5
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2019
ms.locfileid: "54300694"
---
Los límites siguientes corresponden a Azure Backup.

| **Límite** | **Valor predeterminado** |
| --- | --- |
| Servidores/máquinas que se pueden registrar en un almacén | Windows Server/Windows Client/System Center DPM: 50 <br/><br/> Máquinas virtuales de IaaS: 1000  |
| Tamaño de un origen de datos en almacenamiento de almacén |54 400 GB como máximo. El límite no se aplica a la copia de seguridad de una máquina virtual de IaaS |
| Almacenes de Backup en una suscripción de Azure |500 almacenes por región |
| Programación de copias de seguridad diarias |Windows Server/cliente Windows: 3 al día<br/> System Center DPM: 2 al día <br/> Máquinas virtuales de IaaS: Una vez al día  |
| Discos de datos conectados a una máquina virtual de Azure para copia de seguridad | 16 |
| Disco de datos individual conectado a la máquina virtual de Azure para copia de seguridad| 4095 GB|
