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
ms.openlocfilehash: 3009948c72b0cad6ba603a3ccbe35db8d93fe2c7
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34664734"
---
## <a name="storage"></a>Storage

|  |  |
|---------|---------|
| [SKU permitidas para cuentas de almacenamiento y máquinas virtuales](../articles/azure-policy/scripts/allowed-skus-storage.md) | Requiere que las cuentas de almacenamiento y las máquinas virtuales usen las SKU aprobadas. Se usan directivas integradas para garantizar SKU aprobadas. Se especifica una matriz de las SKU de máquinas virtuales aprobadas y una matriz de las SKU de cuentas de almacenamiento aprobadas. |
| [SKU permitidas de cuentas de almacenamiento](../articles/azure-policy/scripts/allowed-stor-acct-skus.md) | Requiere que las cuentas de almacenamiento usen una SKU aprobada. Se especifica una matriz de las SKU aprobadas. |
| [Denegar el nivel de almacenamiento de acceso esporádico para las cuentas de almacenamiento](../articles/azure-policy/scripts/deny-cool-access-tiering.md) | Prohíbe el uso del nivel de almacenamiento de acceso esporádico para las cuentas de Blob Storage.  |
| [Asegurar el tráfico HTTPS solo para una cuenta de almacenamiento](../articles/azure-policy/scripts/ensure-https-stor-acct.md) | Requiere que las cuentas de almacenamiento usen el tráfico HTTPS.  |
| [Asegurar el cifrado de archivos de almacenamiento](../articles/azure-policy/scripts/ensure-store-file-enc.md) | Requiere que el cifrado de archivos esté habilitado para las cuentas de almacenamiento.  |
| [Requerir el cifrado de la cuenta de almacenamiento](../articles/azure-policy/scripts/req-store-acct-enc.md) | Requiere que la cuenta de almacenamiento use el cifrado de blobs.  |