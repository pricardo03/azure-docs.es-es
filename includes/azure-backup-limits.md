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
ms.openlocfilehash: 9e9c09c1825f5c8383a708e8bd343146396f878e
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66238610"
---
Los límites siguientes corresponden a Azure Backup.

| **Límite** | **Valor predeterminado** |
| --- | --- |
| Los servidores o máquinas que se pueden registrar en un almacén. | Windows Server/Windows Client/System Center Data Protection Manager: 50. <br/><br/> Máquinas virtuales de IaaS: 1,000.  |
| Tamaño de un origen de datos en el almacenamiento del almacén. |54,400 GB como máximo. El límite no se aplica a VM de IaaS. |
| Almacenes de backup en una suscripción de Azure. |500 almacenes por región. |
| Programar copias de seguridad diarias. |Windows Server/cliente Windows: Tres al día.<br/> System Center DPM: Dos un día. <br/> Máquinas virtuales de IaaS: Una vez al día.  |
| Los discos asociados a una máquina virtual de Azure para copia de seguridad. | 16 |
| Disco de datos individual conectado a la máquina virtual de Azure para copia de seguridad.| 4095 GB|
