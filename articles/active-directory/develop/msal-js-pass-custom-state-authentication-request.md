---
title: Pasar información de estado personalizado en las solicitudes de autenticación (biblioteca de autenticación de Microsoft para JavaScript) | Azure
description: Obtenga información sobre cómo pasar un valor de parámetro de estado personalizado en la solicitud de autenticación mediante la biblioteca de autenticación de Microsoft para JavaScript (MSAL.js).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/29/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f29d84838ddb11ac359d7a04dbce8e39dd05ac01
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66420504"
---
# <a name="pass-custom-state-in-authentication-requests-using-msaljs"></a>Pasar información de estado personalizado en las solicitudes de autenticación mediante MSAL.js
El *estado* parámetro, tal como se define por OAuth 2.0, se incluye en una solicitud de autenticación y también se devuelve en la respuesta del token para evitar ataques de falsificación de solicitud entre sitios. De forma predeterminada, la biblioteca de autenticación de Microsoft para JavaScript (MSAL.js) pasa generado al azar único *estado* valor del parámetro en las solicitudes de autenticación.

El parámetro de estado también se puede usar para codificar la información de estado de la aplicación antes de redirección. Puede pasar el estado del usuario en la aplicación, por ejemplo, la página o vista que estaban, como entrada para este parámetro. La biblioteca MSAL.js le permite pasar la información de estado personalizada como parámetro de estado en el `Request` objeto:

```javascript
// Request type
export type AuthenticationParameters = {
    scopes?: Array<string>;
    extraScopesToConsent?: Array<string>;
    prompt?: string;
    extraQueryParameters?: QPDict;
    claimsRequest?: string;
    authority?: string;
    state?: string;
    correlationId?: string;
    account?: Account;
    sid?: string;
    loginHint?: string;
};
```

Por ejemplo:

```javascript
let loginRequest = {
    scopes: ["user.read", "user.write"],
    state: “page_url”
}

myMSALObj.loginPopup(loginRequest);
```

El estado pasado se anexa al GUID único establecido por MSAL.js al enviar la solicitud. Cuando se devuelve la respuesta, MSAL.js busca una coincidencia de estado y, a continuación, devuelve personalizado pasado en el estado en el `Response` objeto como `accountState`.

```javascript
export type AuthResponse = {
    uniqueId: string;
    tenantId: string;
    tokenType: string;
    idToken: IdToken;
    accessToken: string;
    scopes: Array<string>;
    expiresOn: Date;
    account: Account;
    accountState: string;
};
```

Para obtener más información, lea sobre [compilar una aplicación de página única (SPA)](scenario-spa-overview.md) mediante MSAL.js.