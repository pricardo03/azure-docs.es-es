---
title: Problemas conocidos en los exploradores (biblioteca de autenticación de Microsoft para JavaScript) | Azure
description: Obtenga información sobre problemas conocidos al usar la biblioteca de autenticación de Microsoft para JavaScript (MSAL.js) con los exploradores Internet Explorer y Microsoft Edge.
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
ms.openlocfilehash: c57ed956ec50c8bac26720a27894c07353928336
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2019
ms.locfileid: "65873906"
---
# <a name="known-issues-on-internet-explorer-and-microsoft-edge-browsers-with-msaljs"></a>Problemas conocidos en los exploradores de Internet Explorer y Microsoft Edge con MSAL.js

## <a name="issues-due-to-security-zones"></a>Problemas debidos a las zonas de seguridad
Teníamos que varios informes de problemas con la autenticación en Internet Explorer y Microsoft Edge (desde la actualización de la *versión del explorador Microsoft Edge para 40.15063.0.0*). Se están realizando el seguimiento estos y mantener el equipo de Microsoft Edge. Aunque Microsoft Edge funciona en una resolución, esta es una descripción de los problemas frecuentes y las soluciones alternativas posibles que se pueden implementar.

### <a name="cause"></a>Causa
La causa de la mayoría de estos problemas es como sigue. El almacenamiento de la sesión y el almacenamiento local se particionan por zonas de seguridad en el explorador Microsoft Edge. En esta versión concreta de Microsoft Edge, cuando se redirige la aplicación en distintas zonas, el almacenamiento de la sesión y el almacenamiento local se desactivan. En concreto, el almacenamiento de la sesión está desactivado en el panel de navegación normal del explorador y se borran la sesión y el almacenamiento local en el modo InPrivate del explorador. MSAL.js guarda el estado determinado en el almacenamiento de la sesión y se basa en la comprobación de este estado durante los flujos de autenticación. Cuando se desactiva el almacenamiento de la sesión, este estado se perderá y da como resultado, por tanto, experiencias rotas.

### <a name="issues"></a>Problemas

- **Bucles de redirección infinito y página vuelve a cargar durante la autenticación**. Cuando los usuarios inician sesión en la aplicación en Microsoft Edge, que se le redirigirá a la recuperación desde la página de inicio de sesión AAD y están bloqueados en un bucle de redirección infinito resultante en las recargas de página repetidos. Esto viene acompañado normalmente por un `invalid_state` error en el almacenamiento de la sesión.

- **Infinito adquirir token bucles y error AADSTS50058**. Cuando una aplicación que se ejecutan en Microsoft Edge intenta adquirir un token para un recurso, la aplicación puede quedarse atascada en un bucle infinito de la adquisición token llamada junto con el siguiente error de AAD en el seguimiento de red:

    `Error :login_required; Error description:AADSTS50058: A silent sign-in request was sent but no user is signed in. The cookies used to represent the user's session were not sent in the request to Azure AD. This can happen if the user is using Internet Explorer or Edge, and the web app sending the silent sign-in request is in different IE security zone than the Azure AD endpoint (login.microsoftonline.com)`

- **Ventana emergente no se cierra o se detuvo cuando se usa el inicio de sesión a través de la ventana emergente para autenticar**. Al autenticar a través de la ventana emergente en Microsoft Edge o IE(InPrivate), después de escribir las credenciales e iniciar sesión, si intervienen varios dominios a través de las zonas de seguridad en el panel de navegación, no se cierra la ventana emergente porque MSAL.js pierde el identificador la ventana emergente.  

    Estos son vínculos a estos problemas en el Rastreador de problemas de Microsoft Edge:  
    - [Error 13861050](https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/13861050/)
    - [Error 13861663](https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/13861663/)

### <a name="update-fix-available-in-msaljs-023"></a>Actualizar: Corrección disponible en MSAL.js 0.2.3
Correcciones para los problemas de bucle de redirección de autenticación se han publicado en [MSAL.js 0.2.3](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases). Habilitar la marca `storeAuthStateInCookie` en la configuración de MSAL.js para aprovechar esta corrección. De forma predeterminada, esta marca se establece en false.

Cuando el `storeAuthStateInCookie` marca está habilitada, MSAL.js usará las cookies del explorador para almacenar el estado de solicitud necesario para la validación de los flujos de autenticación.

> [!NOTE]
> Esta corrección aún no está disponible para los contenedores de msal angular y msal angularjs. Esta corrección no soluciona el problema con ventanas emergentes.

Utilice soluciones alternativas siguientes.

#### <a name="other-workarounds"></a>Otras soluciones
Asegúrese de que el problema se produce solo en la versión específica del explorador Microsoft Edge y funciona con los otros exploradores antes de adoptar estas soluciones de prueba.  
1. Como primer paso para solucionar estos problemas, asegúrese de que el dominio de aplicación, y se agregan otros sitios implicados en las redirecciones del flujo de autenticación como sitios de confianza de la configuración de seguridad del explorador, por lo que pertenecen a la misma zona de seguridad.
Para hacerlo, siga estos pasos:
    - Abra **Internet Explorer** y haga clic en el **configuración** (icono de engranaje) en la esquina superior derecha
    - Seleccione **opciones de Internet**
    - Seleccione el **seguridad** ficha
    - En el **sitios de confianza** opción, haga clic en el **sitios** botón y agregue las direcciones URL en el cuadro de diálogo que aparece.

2. Como se mencionó antes, desde solo la sesión de almacenamiento se borra durante la navegación normal, puede configurar MSAL.js para usar el almacenamiento local en su lugar. Esto se puede establecer como el `cacheLocation` parámetro de configuración al inicializar MSAL.

Tenga en cuenta que esto no solucionará el problema para la exploración de InPrivate puesto que la sesión y almacenamiento local se borran.

## <a name="issues-due-to-popup-blockers"></a>Problemas debidos a los bloqueadores de elementos emergentes

Hay algunos casos en los menús emergentes están bloqueados en Internet Explorer o Microsoft Edge, por ejemplo, cuando se produce un segunda ventana emergente durante la autenticación multifactor. Obtendrá una alerta en el explorador para permitir una sola vez o siempre el elemento emergente. Si decide permitir, el explorador se abre la ventana emergente automáticamente y se devuelve un `null` controlar para él. Como resultado, la biblioteca no tiene un identificador de la ventana y no hay ninguna manera de cerrar la ventana emergente. El mismo problema no ocurre en Chrome cuando le pregunta si desea permitir elementos emergentes porque no se abra automáticamente una ventana emergente.

Como un **solución**, los desarrolladores deben permitir elementos emergentes en Internet Explorer y Microsoft Edge antes de empezar con su aplicación para evitar este problema.

## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre [utilizando MSAL.js en Internet Explorer](msal-js-use-ie-browser.md).
