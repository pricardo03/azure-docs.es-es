---
title: archivo de inclusión
description: archivo de inclusión
services: backup
author: markgalioto
ms.service: backup
ms.topic: include
ms.date: 9/10/2018
ms.author: trinadhk;sogup
ms.custom: include file
ms.openlocfilehash: 64101ea5a3bbaac4a6b2e349a04d06ea84a87081
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2018
ms.locfileid: "44381107"
---
Los límites siguientes corresponden a Azure Backup.

| Identificador de límites | Límite predeterminado |
| --- | --- |
| Número de servidores o máquinas que se pueden registrar en cada almacén |50 para Windows Server/Client/SCDPM  <br/> 1000 para máquinas virtuales de IaaS |
| Tamaño del origen de datos para los datos almacenados en un almacén de Azure |54 400 GB como máximo<sup>1</sup> |
| Número de almacenes de copia de seguridad que se pueden crear en cada suscripción a Azure |500 almacenes de Recovery Services por región |
| Número de veces por día que se puede programar la copia de seguridad |3 por día para servidor o cliente Windows  <br/> 2 por día para SCDPM <br/> 1 vez al día para máquinas virtuales de IaaS |
| Discos de datos conectados a una máquina virtual de Azure para copia de seguridad |16 |
| Tamaño de un disco de datos individual conectado a una máquina virtual de Azure para copia de seguridad| 4095 GB|

* <sup>1</sup>El límite de 54 400 GB no se aplica a la copia de seguridad de máquinas virtuales IaaS.
 

