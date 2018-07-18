---
title: Uso de b2clogin.com | Microsoft Docs
description: Más información sobre el uso de b2clogin.com en lugar de login.microsoftonline.com.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/29/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: c41c02acaeffa170d55f3c59f34a4b1ecae1c523
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2018
ms.locfileid: "34712460"
---
# <a name="using-b2clogincom"></a>Uso de b2clogin.com

>[!IMPORTANT]
>Esta característica está en versión preliminar pública. 
>

Ahora tiene la opción de usar el servicio Azure AD B2C con `<YourTenantName>.b2clogin.com` en lugar de usar `login.microsoftonline.com`.  Esto tiene muchas ventajas:
* Ya no compartirá el mismo límite de tamaño de encabezado de cookies con los demás productos de Microsoft.
* Puede quitar todas las referencias a Microsoft en la dirección URL (puede reemplazar `<YourTenantName>.onmicrosoft.com` por el identificador de inquilino).

 Para aprovechar las ventajas de b2clogin.com, necesita configurar algunas de las siguientes opciones:

1. En **Ejecutar punto de conexión ahora**, asegúrese de que utiliza `<YourTenantName>.b2clogin.com` en lugar de `login.microsoftonline.com`.
2. Si utiliza MSAL, debe establecer `validateauthority=false`.  Esto es porque el emisor del token se convierte en `<YourTenantName>.b2clogin.com`.