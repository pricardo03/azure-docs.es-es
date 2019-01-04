---
title: Flujos de usuario de Azure Active Directory B2C | Microsoft Docs
description: Tema sobre el marco de directiva extensible de Azure Active Directory B2C y cómo crear distintos tipos de flujos de usuario.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: d4a93b04b8ad86a6a6d36a5bdaf3209b49e7a9dc
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52877089"
---
# <a name="azure-active-directory-b2c-user-flows"></a>Azure Active Directory B2C: Flujos de usuario


El marco de directiva extensible de Azure Active Directory (Azure AD) B2C es la fortaleza esencial del servicio. Las directivas describen totalmente las experiencias de identidad del consumidor como el registro, el inicio de sesión y la edición de perfil. Para ayudarle a configurar las tareas más comunes de identidad, el portal de Azure AD B2C incluye directivas predefinidas y configurables denominadas **flujos de usuario**. Por ejemplo, un flujo de usuario de registro le permite controlar los comportamientos configurando los siguientes valores:

* Tipos de cuenta (cuentas sociales como Facebook, o cuentas locales como direcciones de correo electrónico) que los consumidores pueden usar para registrarse en la aplicación
* Atributos (por ejemplo, nombre, código postal, número de calzado, etc.) que se recopilarán del consumidor durante el registro
* Uso de Azure Multi-Factor Authentication
* La apariencia de todas las páginas de registro
* Información (que se manifiesta como notificaciones en un token) que recibe la aplicación cuando se completa la ejecución del flujo de usuario.

Puede crear varios flujos de usuario de diferentes tipos en su inquilino y usarlos en sus aplicaciones según sea necesario. Los flujos de usuario se pueden volver a usar en todas las aplicaciones. Esta flexibilidad permite a los desarrolladores definir y modificar experiencias de identidad de consumidor con cambios mínimos o ningún cambio en su código.

Los flujos de usuario están disponibles para usarlos mediante una interfaz de desarrollador sencilla. Su aplicación desencadena un flujo de usuario mediante una solicitud de autenticación HTTP estándar (pasando un parámetro de flujo de usuario en la solicitud) y recibe un token personalizado como respuesta. Por ejemplo, la única diferencia que hay entre las solicitudes que invocan un flujo de usuario de registro y las que invocan un flujo de usuario de inicio de sesión es el nombre del flujo de usuario que se usa en el parámetro de cadena de consulta "p":

```

https://contosob2c.b2clogin.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up user flow

```

```

https://contosob2c.b2clogin.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in user flow

```

## <a name="create-a-sign-up-or-sign-in-user-flow"></a>Creación de un flujo de usuario de registro o de inicio de sesión

Este flujo de usuario controla las experiencias de registro y de inicio de sesión del cliente con una sola configuración. A los consumidores se les lleva por la ruta correcta (registro o inicio de sesión) según el contexto. También describe el contenido de los tokens que recibirá la aplicación cuando el registro o el inicio de sesión sean correctos.  **Aquí puede encontrar** código de ejemplo del [flujo de usuario de registro o de inicio de sesión](active-directory-b2c-devquickstarts-web-dotnet-susi.md).  Le recomendamos que use este flujo de usuario en vez de un flujo de usuario de **registro** y un flujo de usuario de **inicio de sesión**.  

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

## <a name="create-a-sign-up-user-flow"></a>Creación de un flujo de usuario de registro

[!INCLUDE [active-directory-b2c-create-sign-up-policy](../../includes/active-directory-b2c-create-sign-up-policy.md)]

## <a name="create-a-sign-in-user-flow"></a>Creación de un flujo de usuario de inicio de sesión

[!INCLUDE [active-directory-b2c-create-sign-in-policy](../../includes/active-directory-b2c-create-sign-in-policy.md)]

## <a name="create-a-profile-editing-user-flow"></a>Creación de un flujo de usuario de edición de perfil

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

## <a name="create-a-password-reset-user-flow"></a>Creación de un flujo de usuario de restablecimiento de contraseña

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

## <a name="preview-user-flows"></a>Vista previa de flujos de usuario

Según se publican nuevas características, puede que algunas no estén disponibles en los flujos de usuario o las directivas existentes.  Tenemos previsto reemplazar las versiones anteriores con la versión más reciente del mismo tipo una vez que estos flujos de usuario tengan disponibilidad general.  Las directivas o los flujos de usuario existentes no cambiarán y para sacar provecho de estas nuevas características tendrá que crear nuevos flujos de usuario.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="how-do-i-link-a-sign-up-or-sign-in-user-flow-with-a-password-reset-user-flow"></a>¿Cómo puedo vincular un flujo de usuario de inicio de sesión o de registro con un flujo de usuario de restablecimiento de contraseña?
Al crear un flujo de usuario de **inicio de sesión o de registro** (con cuentas locales), verá el vínculo **¿Olvidó la contraseña?** en la primera página de la experiencia. Al hacer clic en este vínculo, no se desencadena automáticamente ningún flujo de usuario de restablecimiento de contraseña, 

sino que se devuelve a la aplicación el código de error **`AADB2C90118`**. La aplicación debe controlar este código de error invocando un flujo de usuario de restablecimiento de contraseña específico. Para obtener más información, consulte un [ejemplo en el que se muestra el método para vincular flujos de usuario](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI).

### <a name="should-i-use-a-sign-up-or-sign-in-user-flow-or-a-sign-up-user-flow-and-a-sign-in-user-flow"></a>¿Debo usar un flujo de usuario de registro o de inicio de sesión o un flujo de usuario de registro y uno de inicio de sesión?
Se recomienda usar un flujo de usuario de **registro o de inicio de sesión** en lugar de un flujo de usuario de **registro** y un flujo de usuario de **inicio de sesión**.  

El flujo de usuario de **inicio de sesión o de registro** tiene más funcionalidades que el flujo de usuario de **inicio de sesión**. También le permite usar la personalización de la interfaz de usuario de la página y tiene una mejor compatibilidad con la localización. 

El flujo de usuario de **inicio de sesión** se recomienda si no necesita localizar los flujos de usuario, solo necesita unas funcionalidades de personalización secundarias para la personalización de marca y quiere que el restablecimiento de contraseña esté integrado en ella.

## <a name="next-steps"></a>Pasos siguientes
* [Configuración de token, sesión e inicio de sesión único](active-directory-b2c-token-session-sso.md)
* [Deshabilitación de la comprobación de correos electrónicos durante la suscripción de consumidores](active-directory-b2c-reference-disable-ev.md)

