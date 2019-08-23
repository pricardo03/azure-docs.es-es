---
title: archivo de inclusión
description: archivo de inclusión
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 08/21/2019
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 42e965b188db2b84579ab322fbe19781000dff7e
ms.sourcegitcommit: a3a40ad60b8ecd8dbaf7f756091a419b1fe3208e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69894155"
---
## <a name="storage"></a>Storage

|  |  |
|---------|---------|
| [SKU permitidas para cuentas de almacenamiento y máquinas virtuales](../articles/governance/policy/samples/allowed-skus-storage.md) | Requiere que las cuentas de almacenamiento y las máquinas virtuales usen las SKU aprobadas. Se usan directivas integradas para garantizar SKU aprobadas. Se especifica una matriz de las SKU de máquinas virtuales aprobadas y una matriz de las SKU de cuentas de almacenamiento aprobadas. |
| [SKU permitidas de cuentas de almacenamiento](../articles/governance/policy/samples/allowed-storage-account-skus.md) | Requiere que las cuentas de almacenamiento usen una SKU aprobada. Se especifica una matriz de las SKU aprobadas. |
| [Denegar el nivel de almacenamiento de acceso esporádico para las cuentas de almacenamiento](../articles/governance/policy/samples/deny-cool-access-tiering.md) | Prohíbe el uso del nivel de almacenamiento de acceso esporádico para las cuentas de Blob Storage.  |
| [Asegurar el tráfico HTTPS solo para una cuenta de almacenamiento](../articles/governance/policy/samples/ensure-https-storage-account.md) | Requiere que las cuentas de almacenamiento usen el tráfico HTTPS.  |
| [Asegurar el cifrado de archivos de almacenamiento](../articles/governance/policy/samples/ensure-storage-file-encryption.md) | Requiere que el cifrado de archivos esté habilitado para las cuentas de almacenamiento.  |