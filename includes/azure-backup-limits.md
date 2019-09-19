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
ms.sourcegitcommit: cd70273f0845cd39b435bd5978ca0df4ac4d7b2c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "67186715"
---
Los límites siguientes corresponden a Azure Backup.

| **Límite** | **Valor predeterminado** |
| --- | --- |
| Servidores o máquinas que se pueden registrar en un almacén. | Windows Server/Windows Client/System Center Data Protection Manager: 50. <br/><br/> Máquinas virtuales de IaaS: 1000.  |
| Tamaño de un origen de datos en almacenamiento de almacén. |54 400 GB como máximo. El límite no se aplica a la copia de seguridad de una máquina virtual de IaaS. |
| Almacenes de copia de seguridad en una suscripción de Azure. |500 almacenes por región. |
| Programación de copias de seguridad diarias. |Windows Server/cliente Windows: Tres veces al día.<br/> System Center DPM: Dos veces al día. <br/> Máquinas virtuales de IaaS: Una vez al día.  |
| Discos de datos conectados a una máquina virtual de Azure para copia de seguridad. | 16 |
| Disco de datos individual conectado a la máquina virtual de Azure para copia de seguridad.| 4095 GB|
