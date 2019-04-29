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
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60534150"
---
`objectIdType` (o **tipo de identificador de objeto**) hace referencia al tipo de identidad que se asigna a un rol. Además de los tipos `DeviceId` y `UserDefinedFunctionId`, los tipos de identificador de objeto se corresponden con las propiedades de los objetos de Azure Active Directory.

En la tabla siguiente se incluyen los tipos de identificador de objeto admitidos en Azure Digital Twins:

| Type | DESCRIPCIÓN |
| --- | --- |
| UserId | Asigna un rol a un usuario. |
| deviceId | Asigna un rol a un dispositivo. |
| DomainName | Asigna un rol a un nombre de dominio. Cada usuario con el nombre de dominio especificado tiene los derechos de acceso del rol correspondiente. |
| TenantId | Asigna un rol a un inquilino. Cada usuario al que pertenezca el identificador de inquilino de Azure AD especificado tiene los derechos de acceso del rol correspondiente. |
| ServicePrincipalId | Asigna un rol a un identificador de objeto de entidad de servicio. |
| UserDefinedFunctionId | Asigna un rol a una función definida por el usuario (UDF). |