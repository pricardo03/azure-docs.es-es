---
title: Utilice Internet Explorer (biblioteca de autenticación de Microsoft para JavaScript) | Azure
description: Obtenga información sobre el uso de la biblioteca de autenticación de Microsoft para JavaScript (MSAL.js) con el Explorador de Internet Explorer.
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8cf8c84120f4c90d3943cfc31ffbf9aafcec0ba3
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2019
ms.locfileid: "65873921"
---
# <a name="known-issues-on-internet-explorer-and-microsoft-edge-browsers-with-msaljs"></a>Problemas conocidos en los exploradores de Internet Explorer y Microsoft Edge con MSAL.js

Biblioteca de autenticación de Microsoft para JavaScript (MSAL.js) se genera para [JavaScript ES5](https://fr.wikipedia.org/wiki/ECMAScript#ECMAScript_Edition_5_.28ES5.29) para que se pueda ejecutar en Internet Explorer. Sin embargo, hay algunos aspectos que debe conocer.

## <a name="run-an-app-in-internet-explorer"></a>Ejecutar una aplicación en Internet Explorer
Si piensa usar MSAL.js en aplicaciones que se pueden ejecutar en Internet Explorer, deberá agregar una referencia a un polyfill promesa antes de hacer referencia a la secuencia de comandos MSAL.js.

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js" class="pre"></script>
```

Esto es porque Internet Explorer no admite las promesas de JavaScript de forma nativa.

## <a name="debugging-an-application-running-in-internet-explorer"></a>Depurar una aplicación que se ejecutan en Internet Explorer

### <a name="running-in-production"></a>Que se ejecutan en producción
Implementar la aplicación en producción (por ejemplo en Azure Web apps) normalmente funciona bien, proporciona que el usuario final ha aceptado los elementos emergentes. Hemos probado con Internet Explorer 11.

### <a name="running-locally"></a>Se ejecuta localmente
Si desea ejecutar y depurar localmente la aplicación que se ejecutan en Internet Explorer, deberá tener en cuenta las consideraciones siguientes (supongamos que desea ejecutar la aplicación como *http://localhost:1234*):

- Internet Explorer tiene un mecanismo de seguridad llamado "modo protegido", lo que impide que MSAL.js funciona correctamente. Entre los síntomas, después de iniciar sesión, la página se puede redirigir a http://localhost:1234/null.

- Para ejecutar y depurar su aplicación localmente, deberá deshabilitar esta "modo protegido". Para esto:

    1. Haga clic en Internet Explorer **herramientas** (el icono de engranaje).
    1. Seleccione **opciones de Internet** y, a continuación, el **seguridad** ficha.
    1. Haga clic en el **Internet** zona y desactive la opción **habilitar modo protegido (requiere reiniciar Internet Explorer)**. Internet Explorer le advierte que el equipo ya no está protegido. Haga clic en **OK**.
    1. Reinicie Internet Explorer.
    1. Ejecutar y depurar la aplicación.

Cuando haya terminado, restaure la configuración de seguridad de Internet Explorer.  Seleccione **configuración** -> **opciones de Internet** -> **seguridad** -> **restablecer todas las zonas al nivel predeterminado**.

## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre [problemas conocidos al usar MSAL.js en Internet Explorer](msal-js-use-ie-browser.md).