---
title: archivo de inclusión
description: archivo de inclusión
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 12/20/2018
ms.author: adgera
ms.custom: include file
ms.openlocfilehash: 40ab53c941a7ac619ebb09d381a4ae0450f26e8b
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/27/2018
ms.locfileid: "53795904"
---
`objectIdType` (o **tipo de identificador de objeto**) hace referencia al tipo de identidad que se asigna a un rol. Además de los tipos `DeviceId` y `UserDefinedFunctionId`, los tipos de identificador de objeto se corresponden con las propiedades de los objetos de Azure Active Directory.

En la tabla siguiente se incluyen los tipos de identificador de objeto admitidos en Azure Digital Twins:

| Escriba | DESCRIPCIÓN |
| --- | --- |
| UserId | Asigna un rol a un usuario. |
| deviceId | Asigna un rol a un dispositivo. |
| DomainName | Asigna un rol a un nombre de dominio. Cada usuario con el nombre de dominio especificado tiene los derechos de acceso del rol correspondiente. |
| TenantId | Asigna un rol a un inquilino. Cada usuario al que pertenezca el identificador de inquilino de Azure AD especificado tiene los derechos de acceso del rol correspondiente. |
| ServicePrincipalId | Asigna un rol a un identificador de objeto de entidad de servicio. |
| UserDefinedFunctionId | Asigna un rol a una función definida por el usuario (UDF). |