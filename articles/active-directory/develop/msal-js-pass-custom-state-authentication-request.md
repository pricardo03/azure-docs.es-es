---
title: Paso del estado personalizado en las solicitudes de autenticación (Biblioteca de autenticación de Microsoft para JavaScript) | Azure
description: Aprenda a pasar un valor de estado personalizado en la solicitud de autenticación con la Biblioteca de autenticación de Microsoft para JavaScript (MSAL.js).
services: active-directory
documentationcenter: dev-center-name
author: TylerMSFT
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/29/2019
ms.author: twhitney
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: d2ae12624b3d897f05437f7795d1a1eee32ca37a
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2019
ms.locfileid: "69532754"
---
# <a name="pass-custom-state-in-authentication-requests-using-msaljs"></a>Paso del estado personalizado en solicitudes de autenticación con MSAL.js
El parámetro *state*, tal como se define en OAuth 2.0, se incluye en una solicitud de autenticación y también se devuelve en la respuesta del token para evitar ataques de falsificación de la solicitud entre sitios. De forma predeterminada, la Biblioteca de autenticación de Microsoft para JavaScript (MSAL.js) pasa un valor único del parámetro *state* generado al azar en las solicitudes de autenticación.

El parámetro state también se puede usar para codificar la información de estado de la aplicación antes de la redirección. Puede pasar el estado del usuario en la aplicación, por ejemplo, la página o vista en el que estuviera, como entrada para este parámetro. La biblioteca MSAL.js le permite pasar el estado personalizado como parámetro de estado en el objeto `Request`:

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

El estado pasado se anexa al GUID único establecido por MSAL.js al enviar la solicitud. Cuando se devuelve la respuesta, MSAL.js busca una coincidencia de estado y, a continuación, devuelve el estado personalizado pasado en el objeto `Response` como `accountState`.

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

Para más información, lea sobre cómo [crear una aplicación de página única (SPA)](scenario-spa-overview.md) mediante MSAL.js.