---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/25/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 3af8077627d56ce44c5e2959e2c79b967b09d9e5
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2019
ms.locfileid: "69011939"
---
Una firma de acceso compartido (SAS) le permite conceder acceso limitado a los contenedores y blobs de la cuenta de almacenamiento. Cuando crea una SAS, especifica sus restricciones, incluidos los recursos de Azure Storage a los que puede acceder un cliente, los permisos que tiene en esos recursos y el tiempo durante el cual la SAS es válida.

Cada SAS se firma con una clave. Puede firmar una SAS de dos maneras:

- Con una clave creada con las credenciales de Azure Active Directory (Azure AD). Una SAS firmada con credenciales de Azure AD es una SAS de *delegación de usuario*.
- Con la clave de la cuenta de almacenamiento. Tanto una *SAS de servicio* como una *SAS de cuenta* se firman con la clave de la cuenta de almacenamiento.
