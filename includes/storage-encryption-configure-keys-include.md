---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/26/2019
ms.author: tamram
ms.custom: include
ms.openlocfilehash: f846d75833b3a796e24fd23c5f841ea24a8d1876
ms.sourcegitcommit: 57eb9acf6507d746289efa317a1a5210bd32ca2c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2019
ms.locfileid: "74665920"
---
Azure Storage cifra todos los datos de las cuentas de almacenamiento en reposo. De manera predeterminada, los datos se cifran con claves administradas por Microsoft. Para tener un mayor control sobre las claves de cifrado, puede proporcionar claves administradas por el cliente en el nivel de la cuenta de almacenamiento.

Las claves administradas por el cliente se deben almacenar en Azure Key Vault. Puede crear sus propias claves y almacenarlas en un almacén de claves, o puede usar las API de Azure Key Vault para generarlas. La cuenta de almacenamiento y el almacén de claves deben estar en la misma región, pero pueden estar en distintas suscripciones. Para más información sobre cifrado de Azure Storage y la administración de claves, consulte [Cifrado de Azure Storage para datos en reposo](../articles/storage/common/storage-service-encryption.md). Para obtener más información sobre Azure Key Vault, consulte [¿Qué es Azure Key Vault?](../articles/key-vault/key-vault-overview.md)
