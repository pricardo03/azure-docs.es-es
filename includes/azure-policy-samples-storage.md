---
title: archivo de inclusión
description: archivo de inclusión
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/17/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: b967558828c42331b3702fe90ce842fd1c0032bd
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47003956"
---
## <a name="storage"></a>Storage

|  |  |
|---------|---------|
| [SKU permitidas para cuentas de almacenamiento y máquinas virtuales](../articles/governance/policy/samples/allowed-skus-storage.md) | Requiere que las cuentas de almacenamiento y las máquinas virtuales usen las SKU aprobadas. Se usan directivas integradas para garantizar SKU aprobadas. Se especifica una matriz de las SKU de máquinas virtuales aprobadas y una matriz de las SKU de cuentas de almacenamiento aprobadas. |
| [SKU permitidas de cuentas de almacenamiento](../articles/governance/policy/samples/allowed-stor-acct-skus.md) | Requiere que las cuentas de almacenamiento usen una SKU aprobada. Se especifica una matriz de las SKU aprobadas. |
| [Denegar el nivel de almacenamiento de acceso esporádico para las cuentas de almacenamiento](../articles/governance/policy/samples/deny-cool-access-tiering.md) | Prohíbe el uso del nivel de almacenamiento de acceso esporádico para las cuentas de Blob Storage.  |
| [Asegurar el tráfico HTTPS solo para una cuenta de almacenamiento](../articles/governance/policy/samples/ensure-https-stor-acct.md) | Requiere que las cuentas de almacenamiento usen el tráfico HTTPS.  |
| [Asegurar el cifrado de archivos de almacenamiento](../articles/governance/policy/samples/ensure-store-file-enc.md) | Requiere que el cifrado de archivos esté habilitado para las cuentas de almacenamiento.  |
| [Requerir el cifrado de la cuenta de almacenamiento](../articles/governance/policy/samples/req-store-acct-enc.md) | Requiere que la cuenta de almacenamiento use el cifrado de blobs.  |