---
title: 'Procedimientos recomendados para usar Azure Key Vault de Key Vault: | Microsoft Docs'
description: Este documento explica algunas de las prácticas recomendadas para utilizar Key Vault
services: key-vault
documentationcenter: ''
author: yvprashanth
manager: barbkess
tags: azure-key-vault
ms.assetid: e5b4e083-4a39-4410-8e3a-2832ad6db405
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/07/2019
ms.author: prashanthyv
ms.openlocfilehash: 85bd1858bc3f2d505e1e4d0a88e8c77a46ae4447
ms.sourcegitcommit: b8f9200112cae265155b8877f7e1621c4bcc53fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/14/2019
ms.locfileid: "57887992"
---
# <a name="best-practices-to-use-key-vault"></a>Procedimientos recomendados para utilizar Key Vault

## <a name="control-access-to-your-vault"></a>Controlar el acceso a su almacén

Azure Key Vault es un servicio en la nube que protege las claves de cifrado y los secretos, como certificados, cadenas de conexión y contraseñas. Dado que estos datos son confidenciales y críticos para la empresa, debe proteger el acceso a los almacenes de claves, de modo que solo se admita el acceso de las aplicaciones y los usuarios autorizados. Esto [artículo](key-vault-secure-your-key-vault.md) proporciona información general sobre el modelo de acceso de Key Vault. Se explican la autenticación y la autorización, y se describe cómo proteger el acceso a los almacenes de claves.

Sugerencias mientras controla el acceso a su almacén son los siguientes:
1. Bloquear el acceso a su suscripción, grupo de recursos y los almacenes de clave (RBAC)
2. Crear directivas de acceso para cada almacén.
3. Utilice la entidad de acceso con privilegios mínimos para conceder acceso
4. Activar el Firewall y [puntos de conexión de servicio de red virtual](key-vault-overview-vnet-service-endpoints.md)

## <a name="use-separate-key-vault"></a>Uso independiente de Key Vault

Nuestra recomendación es usar un almacén por aplicación por entorno (desarrollo, entorno de preproducción y producción). Esto le ayuda a no comparten secretos entre entornos y también reduce la amenaza en el caso de una infracción.

## <a name="backup-your-vault"></a>El almacén de copia de seguridad

Asegúrese de volver regular tomar SAI (UPS) de su [almacén](https://blogs.technet.microsoft.com/kv/2018/07/20/announcing-backup-and-restore-of-keys-secrets-and-certificates/) en update o delete/creación de objetos dentro de un almacén.

## <a name="turn-on-logging"></a>Activar el registro

[Activar el registro](key-vault-logging.md) para su almacén. También debe configurar las alertas.

## <a name="turn-on-recovery-options"></a>Activar las opciones de recuperación

1. Activar [eliminación temporal](key-vault-ovw-soft-delete.md).
2. Activar la protección de purgas si desea protegerse contra fuerza la eliminación del secreto / almacén incluso después de la eliminación temporal está activada.
