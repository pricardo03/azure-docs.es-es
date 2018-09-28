---
title: Guía de inicio rápido de Azure AD v2 para UWP de Windows | Microsoft Docs
description: Obtenga más información acerca de cómo una aplicación de la Plataforma universal de Windows (XAML) puede obtener un token de acceso y llamar a una API protegida por un punto de conexión de Azure Active Directory v2.0.
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/25/2018
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 5241df87297fb2e293e2cc828821e66d6f2837b0
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46970834"
---
# <a name="call-the-microsoft-graph-api-from-a-universal-windows-platform-uwp-application"></a>Llamar a Microsoft Graph API desde una aplicación de la Plataforma universal de Windows (UWP)

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

En esta guía de inicio rápido hay código de ejemplo que muestra cómo una aplicación de la Plataforma universal de Windows (UWP) puede iniciar sesión por los usuarios en cuentas personales, profesionales y educativas, obtener un token de acceso y llamar a Microsoft Graph API.

![Funcionamiento de la aplicación de ejemplo generada por esta guía de inicio rápido](media/quickstart-v2-uwp/uwp-intro.png)

> [!div renderon="docs"]
> ## <a name="register-and-download"></a>Registro y descarga
> ### <a name="register-and-configure-your-application-and-code-sample"></a>Registro y configuración de la aplicación y código de ejemplo
> #### <a name="step-1-register-your-application"></a>Paso 1: Registro de la aplicación
> Para registrar la aplicación y agregar la información de registro de aplicación a la solución, siga estos pasos:
> 1. Vaya al [Portal de registro de aplicaciones de Microsoft](https://apps.dev.microsoft.com/portal/register-app) para registrar una aplicación.
> 1. En el cuadro**Application Name** (Nombre de la aplicación) escriba el nombre de su aplicación.
> 1. Asegúrese de que la casilla **Guided Setup** (Instalación guiada) esté desactivada y, luego, seleccione **Create** (Crear).
> 1. Seleccione **Add Platform** (Agregar plataforma), seleccione **Native Application** (Aplicación nativa) y, luego, seleccione **Save** (Guardar).

> [!div renderon="portal" class="sxs-lookup alert alert-info"]
> #### <a name="step-1-configure-your-application"></a>Paso 1: Configuración de la aplicación
> Para que el código de ejemplo de esta guía de inicio rápido funcione, debe agregar una dirección URL de redireccionamiento como **urn:ietf:wg:oauth:2.0:oob**.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Hacer este cambio por mí]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Ya configurada](media/quickstart-v2-uwp/green-check.png) La aplicación está configurada con estos atributos

#### <a name="step-2-download-your-visual-studio-project"></a>Paso 2: Descarga del proyecto de Visual Studio

 - [Descargue el proyecto de Visual Studio 2017](https://github.com/Azure-Samples/active-directory-dotnet-native-uwp-v2/archive/master.zip)

#### <a name="step-3-configure-your-visual-studio-project"></a>Paso 3: Configuración del proyecto de Visual Studio

1. Extraiga el archivo ZIP en una carpeta local (por ejemplo, **C:\Azure-Samples**).
1. Abra el proyecto en Visual Studio.
1. En **App.Xaml.cs**, reemplace la línea que empieza con `private static string ClientId` por:

    ```csharp
    private static string ClientId = "Enter_the_Application_Id_here";
    ```

## <a name="more-information"></a>Más información

A continuación, se muestra información general de este inicio rápido:

### <a name="msalnet"></a>MSAL.NET

MSAL ([Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)) es la biblioteca que se usa para iniciar la sesión de los usuarios y solicitar tokens de acceso a una API protegida por Microsoft Azure Active Directory. Puede instalarlo mediante la ejecución del siguiente comando en la *Consola del Administrador de paquetes* de Visual Studio:

```powershell
Install-Package Microsoft.Identity.Client -Pre
```

### <a name="msal-initialization"></a>Inicialización de MSAL

Puede agregar la referencia de MSAL mediante la adición de la siguiente línea:

```csharp
using Microsoft.Identity.Client;
```

A continuación, realice la inicialización de MSAL con la siguiente línea:

```csharp
public static PublicClientApplication PublicClientApp = new PublicClientApplication(ClientId);
```

> |Donde: ||
> |---------|---------|
> |ClientId | El identificador de aplicación de la aplicación registrada en *portal.microsoft.com* |

### <a name="requesting-tokens"></a>Solicitud de tokens

MSAL tiene dos métodos para adquirir tokens: `AcquireTokenAsync` y `AcquireTokenSilentAsync`:

#### <a name="get-a-user-token-interactively"></a>Obtención de un token de usuario interactivamente

 En algunas situaciones es necesario forzar a los usuarios a interactuar con el punto de conexión de Azure Active Directory v2.0 a través de una ventana emergente para validar sus credenciales o dar su consentimiento. A continuación se incluyen algunos ejemplos:

- La primera vez que los usuarios inician sesión en la aplicación
- Es posible que los usuarios deban volver a escribir las credenciales porque la contraseña expiró
- La aplicación solicita acceso a un recurso para el cual el usuario necesita consentimiento
- Se requiere la autenticación en dos fases

```csharp
authResult = await App.PublicClientApp.AcquireTokenAsync(scopes);
```

> |Donde:||
> |---------|---------|
> |ámbitos | Contiene los ámbitos que se solicitan (es decir, `{ "user.read" }` para Microsoft Graph o `{ "api://<Application ID>/access_as_user" }` para las API web personalizadas) |

#### <a name="get-a-user-token-silently"></a>Obtención de un token de usuario en silencio

No le conviene requerir que los usuarios validen sus credenciales cada vez que necesite acceso a un recurso; la mayoría del tiempo quiere adquisiciones de tokens y renovación sin ninguna interacción del usuario: `AcquireTokenSilentAsync` es el método usado habitualmente para obtener los tokens empleados para obtener acceso a los recursos protegidos después del `AcquireTokenAsync` inicial:

```csharp
var accounts = await App.PublicClientApp.GetAccountsAsync();
authResult = await App.PublicClientApp.AcquireTokenSilentAsync(scopes, accounts.FirstOrDefault());
```

> |Donde: ||
> |---------|---------|
> |ámbitos | Contiene los ámbitos que se solicitan (es decir, `{ "user.read" }` para Microsoft Graph o `{ "api://<Application ID>/access_as_user" }` para las API web personalizadas) |
> |accounts.FirstOrDefault() | El primer usuario en la memoria caché (MSAL admite varios usuarios en una sola aplicación) |

## <a name="next-steps"></a>Pasos siguientes

Visite el tutorial de escritorio de Windows para acceder a una guía completa paso a paso sobre la creación de aplicaciones y nuevas características, que incluye una explicación completa de esta guía de inicio rápido:

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>Obtenga información sobre los pasos para crear la aplicación que se usa en esta guía de inicio rápido

> [!div class="nextstepaction"]
> [UWP: Tutorial de Graph API de llamada](tutorial-v2-windows-uwp.md)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]