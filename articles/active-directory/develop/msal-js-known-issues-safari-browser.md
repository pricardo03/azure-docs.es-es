---
title: Problemas conocidos en los exploradores (biblioteca de autenticación de Microsoft para JavaScript) | Azure
description: Obtenga información sobre problemas conocidos al usar la biblioteca de autenticación de Microsoft para JavaScript (MSAL.js) con el explorador Safari.
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: cb89b1ef4dbbef234fba3152d7f85bbadfbdc64a
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2019
ms.locfileid: "65873891"
---
# <a name="known-issues-on-safari-browser-with-msaljs"></a>Problemas conocidos en el explorador Safari con MSAL.js 

## <a name="silent-token-renewal-on-safari-12-and-itp-20"></a>Renovación del token silenciosa en Safari 12 y ITP 2.0

Apple iOS 12 y los sistemas operativos de MacOS 10.14 incluye una versión de la [explorador Safari 12](https://developer.apple.com/safari/whats-new/). Para fines de seguridad y privacidad, que incluye 12 Safari el [2.0 de prevención de seguimiento inteligente](https://webkit.org/blog/8311/intelligent-tracking-prevention-2-0/). Básicamente, esto hace que el explorador va a quitar las cookies de terceros que se va a establecer. ITP 2.0 trata también las cookies establecidas por los proveedores de identidades como las cookies de terceros.

### <a name="impact-on-msaljs"></a>Impacto en MSAL.js

MSAL.js usa un Iframe oculto para realizar una adquisición del token silenciosa y renovación como parte de la `acquireTokenSilent` llamadas. Las solicitudes de token silenciosas se basan en el Iframe que tengan acceso a la sesión del usuario autenticado representado por las cookies establecidas por Azure AD. Con ITP 2.0 impidiendo el acceso a estas cookies, MSAL.js no se puede adquirir de forma silenciosa y renovar los tokens y esto da como resultado `acquireTokenSilent` errores.

En este momento no hay ninguna solución para este problema y estamos evaluando opciones con la Comunidad de estándares.

### <a name="work-around"></a>Solución alternativa

De forma predeterminada la configuración de ITP está habilitada en el explorador Safari. Puede deshabilitar esta configuración, vaya a **preferencias** -> **privacidad** y desactive el **evitar sitios seguimiento** opción.

![configuración de Safari](./media/msal-js-known-issue-safari-browser/safari.png)

Tendrá que controlar la `acquireTokenSilent` errores con un interactivo adquieren token llamada, que pide al usuario que inicie sesión.
Para evitar los inicios de sesión repetidos, un enfoque que puede implementar es controlar el `acquireTokenSilent` error y proporcionar al usuario una opción para deshabilitar la configuración de ITP en Safari antes de continuar con la llamada interactiva. Una vez que la configuración está deshabilitada, las renovaciones de token silenciosas posteriores deben ser correctas.
