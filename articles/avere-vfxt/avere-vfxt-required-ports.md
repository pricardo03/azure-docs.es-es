---
title: Lista blanca de puertos para Avere vFXT para Azure
description: Lista de puertos que usa Avere vFXT para Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 394415ffc7593d058d8d0bf1c0040b88ec25073b
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50669932"
---
# <a name="required-ports"></a>Puertos necesarios

Los puertos enumerados en esta sección se usan para la comunicación entrante y saliente de vFXT.

Nunca exponga el clúster de vFXT o la instancia del controlador del clúster directamente a la red pública de Internet.

## <a name="api"></a>API

| Entrada: | | |
| --- | ---- | --- |
| TCP | 22  | SSH  |
| TCP | 443 | HTTPS|



| Salida: |     |       |
|----------|-----|-------|
| TCP      | 443 | HTTPS |

 
## <a name="nfs"></a>NFS

| Entrada y salida  | | |
| --- | --- | ---|
| TCP/UDP | 111  | RPCBIND  |
| TCP/UDP | 2049 | NFS      |
| TCP/UDP | 4045 | NLOCKMGR |
| TCP/UDP | 4046 | MOUNTD   |
| TCP/UDP | 4047 | STATUS   |

