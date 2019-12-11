---
title: Aplicación de página única (llamada a una API web) - Plataforma de identidad de Microsoft
description: Aprenda a compilar una aplicación de página única (llamada a una API web)
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/06/2019
ms.author: ryanwi
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4170a6642d35802581b5d1ff28eb802a6eb3482b
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74766164"
---
# <a name="single-page-application-call-a-web-api"></a>Aplicación de página única: Llamada a una API de web

Se recomienda que llame al método `acquireTokenSilent` para adquirir o renovar un token de acceso antes de llamar a una API web. Una vez que disponga de un token, puede llamar a una API web protegida.

## <a name="call-a-web-api"></a>Llamada a una API de web

### <a name="javascript"></a>JavaScript

Use el token de acceso adquirido como portador en una solicitud HTTP para llamar a cualquier API web, como Microsoft Graph API. Por ejemplo:

```javascript
    var headers = new Headers();
    var bearer = "Bearer " + access_token;
    headers.append("Authorization", bearer);
    var options = {
         method: "GET",
         headers: headers
    };
    var graphEndpoint = "https://graph.microsoft.com/v1.0/me";

    fetch(graphEndpoint, options)
        .then(function (response) {
             //do something with response
        }
```

### <a name="angular"></a>Angular

El contenedor MSAL Angular aprovecha las ventajas del interceptor HTTP para adquirir de forma automática y silenciosa tokens de acceso y adjuntarlos a las solicitudes HTTP de las API. Para más información, consulte [Aplicación de página única: adquirir un token para llamar a una API](scenario-spa-acquire-token.md).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Paso a producción](scenario-spa-production.md)
