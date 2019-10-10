---
title: archivo de inclusión
description: archivo de inclusión
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
ms.topic: include
ms.date: 09/30/2019
ms.custom: include file
ms.openlocfilehash: 12d4278171d43fdaf8613a1c91bdbffc269adc56
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2019
ms.locfileid: "71949036"
---
`objectIdType` (o **tipo de identificador de objeto**) hace referencia al tipo de identidad que se asigna a un rol. Además de los tipos `DeviceId` y `UserDefinedFunctionId`, los tipos de identificador de objeto se corresponden con las propiedades de los objetos de Azure Active Directory.

En la tabla siguiente se incluyen los tipos de identificador de objeto admitidos en Azure Digital Twins:

| type | DESCRIPCIÓN |
| --- | --- |
| UserId | Asigna un rol a un usuario. |
| deviceId | Asigna un rol a un dispositivo. |
| DomainName | Asigna un rol a un nombre de dominio. Cada usuario con el nombre de dominio especificado tiene los derechos de acceso del rol correspondiente. |
| TenantId | Asigna un rol a un inquilino. Cada usuario al que pertenezca el identificador de inquilino de Azure AD especificado tiene los derechos de acceso del rol correspondiente. |
| ServicePrincipalId | Asigna un rol a un identificador de objeto de entidad de servicio. |
| UserDefinedFunctionId | Asigna un rol a una función definida por el usuario (UDF). |