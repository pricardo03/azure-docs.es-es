---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 04/16/2019
ms.author: tamram
ms.custom: include
ms.openlocfilehash: 3076065d23d4701711c0ecb60349e77ee5c9b7c8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61593538"
---
Almacenamiento de Azure admite el cifrado en reposo con claves administradas por Microsoft o claves administradas por el cliente. Claves administradas por el cliente permiten crear, girar, deshabilitar y revocar el acceso a los controles.

Usar Azure Key Vault para administrar las claves y auditar su uso. Puede crear sus propias claves y almacenarlas en un almacén de claves, o puede usar las API de Azure Key Vault para generar las claves. La cuenta de almacenamiento y el almacén de claves deben estar en la misma región, pero pueden estar en distintas suscripciones. Para obtener más información sobre Azure Key Vault, consulte [¿Qué es Azure Key Vault?](../articles/key-vault/key-vault-overview.md)
