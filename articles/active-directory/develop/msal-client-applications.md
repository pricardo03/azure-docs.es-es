---
title: Las aplicaciones cliente (biblioteca de autenticación de Microsoft) | Azure
description: Obtenga información acerca de cliente público y cliente confidencial aplicaciones en la biblioteca de autenticación de Microsoft (MSAL).
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
ms.date: 04/25/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9d09436b9a2ac38e7b07a51f01d65769ed19d08e
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66430832"
---
# <a name="public-client-and-confidential-client-applications"></a>Cliente público y las aplicaciones cliente confidencial
Biblioteca de autenticación de Microsoft (MSAL) define dos tipos de clientes: los clientes públicos y confidenciales. Los tipos de cliente de dos se distinguen por su capacidad de autenticar de forma segura con el servidor de autorización y mantener la confidencialidad de sus credenciales de cliente. En cambio, Azure AD Authentication Library (ADAL) usa lo que se denomina *contexto de autenticación* (que es una conexión a Azure AD).

- **Las aplicaciones cliente confidencial** son aplicaciones que se ejecutan en servidores (aplicaciones web, aplicaciones de API Web o incluso las aplicaciones de servicio o demonio). Se consideran difícil para el acceso y por ese motivo capaces de mantener un secreto de aplicación. Los clientes confidenciales pueden contener los secretos de tiempo de configuración. Cada instancia del cliente tiene una configuración distinta (incluido el Id. de cliente y secreto de cliente). Estos valores son difíciles para los usuarios finales extraer. Una aplicación web es el cliente confidencial más comunes. El identificador de cliente se expone a través del explorador web, pero el secreto se pasa sólo en el canal y se exponen nunca directamente.

    Aplicaciones cliente confidencial: <BR>
    ![Aplicación Web](media/msal-client-applications/web-app.png) ![API Web](media/msal-client-applications/web-api.png) ![demonio/servicio](media/msal-client-applications/daemon-service.png)

- **Las aplicaciones de cliente público** son aplicaciones que se ejecutan en dispositivos o equipos de escritorio o en un explorador web. No son de confianza para mantener segura secretos de aplicación, por lo que solo acceden a las API Web en nombre del usuario. (Admite solo los flujos de cliente público). Los clientes públicos no pueden contener información confidencial de tiempo de configuración, para que no tengan los secretos de cliente.

    Aplicaciones de cliente público: <BR>
    ![Aplicación de escritorio](media/msal-client-applications/desktop-app.png) ![API Browserless](media/msal-client-applications/browserless-app.png) ![aplicación móvil](media/msal-client-applications/mobile-app.png)

> [!NOTE]
> En MSAL.js, no hay ninguna separación de las aplicaciones cliente públicos y confidenciales.  MSAL.js representa las aplicaciones cliente como aplicaciones de usuario basada en agente, los clientes públicos en el que se ejecuta el código de cliente en un agente de usuario, como un explorador web. Estos clientes no almacenan secretos porque el contexto del explorador es accesible públicamente.

## <a name="comparing-the-client-types"></a>Comparación de los tipos de cliente
Estas son algunas similitudes y diferencias entre el cliente público y cliente confidencial aplicaciones:

- Ambos tipos de aplicación mantienen una caché de tokens de usuario y pueden adquirir un token de forma silenciosa (cuando el token ya está en la caché de tokens). Las aplicaciones de cliente confidencial también tienen una caché de tokens de aplicación para los tokens que son para la propia aplicación.
- Ambos tipos de aplicación administrar cuentas de usuario y pueden obtener una cuenta de la caché de tokens de usuario, obtener una cuenta de su identificador o quitar una cuenta.
- Las aplicaciones de cliente público tienen cuatro maneras de adquirir un token (cuatro flujos de autenticación). Las aplicaciones cliente confidencial tienen tres formas de adquirir un token (y extremo de autorización de una manera de calcular la dirección URL del proveedor de identidades). Para obtener más información, consulte [adquisición de tokens](msal-acquire-cache-tokens.md).

Si ha usado ADAL, es posible que observe que, a diferencia de contexto de autenticación de ADAL, en el identificador de cliente MSAL (también denominado el *Id. de aplicación* o *Id. de aplicación*) se pasa una vez en la construcción de la aplicación. No es necesario que se pasarán a intentarlo cuando la aplicación adquiere un token. Esto es cierto tanto para las aplicaciones cliente públicos y confidenciales. Los constructores de las aplicaciones cliente confidencial también se pasan las credenciales del cliente: el secreto que se comparten con el proveedor de identidades.

## <a name="next-steps"></a>Pasos siguientes
Más información sobre lo siguiente:
- [Opciones de configuración de aplicación de cliente](msal-client-application-configuration.md)
- [Crear una instancia de las aplicaciones cliente mediante MSAL.NET](msal-net-initializing-client-applications.md)
- [Crear instancias de las aplicaciones cliente mediante el uso de MSAL.js](msal-js-initializing-client-applications.md)
