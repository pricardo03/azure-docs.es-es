---
title: 'Tutorial: Habilitación de la autenticación de una aplicación de escritorio con cuentas mediante Azure Active Directory B2C | Microsoft Docs'
description: Tutorial acerca de cómo usar Azure Active Directory B2C para proporcionar inicio de sesión de usuario en una aplicación de escritorio de .NET.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.author: davidmu
ms.date: 11/30/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.component: B2C
ms.openlocfilehash: 357b9f4d307624db838b22581097799d7d7fef4c
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54857002"
---
# <a name="tutorial-enable-desktop-app-authentication-with-accounts-using-azure-active-directory-b2c"></a>Tutorial: Habilitación de la autenticación de una aplicación de escritorio con cuentas mediante Azure Active Directory B2C

En este tutorial se muestra cómo usar Azure Active Directory (Azure AD) B2C para registrar e iniciar sesión con usuarios en una aplicación de escritorio de Windows Presentation Foundation (WPF). Azure AD B2C permite que las aplicaciones puedan autenticarse en cuentas de las redes sociales, cuentas de empresa y cuentas de Azure Active Directory mediante protocolos estándar abiertos.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Registrar la aplicación de escritorio de ejemplo en un inquilino de Azure AD B2C.
> * Crear flujos de usuario para el registro, el inicio de sesión, la edición de un perfil y el restablecimiento de contraseña.
> * Configurar la aplicación de ejemplo para que use el inquilino de Azure AD B2C.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

* Crear su propio [inquilino de Azure AD B2C](active-directory-b2c-get-started.md).
* [Instale Visual Studio 2017](https://www.visualstudio.com/downloads/) con las cargas de trabajo de **desarrollo de escritorio de .NET** y de **desarrollo web y de ASP.NET**.

## <a name="register-desktop-app"></a>Registro de una aplicación de escritorio

Las aplicaciones se deben [registrar](../active-directory/develop/developer-glossary.md#application-registration) en el inquilino antes de que puedan recibir [tokens de acceso](../active-directory/develop/developer-glossary.md#access-token) de Azure Active Directory. El registro de una aplicación crea un [identificador de aplicación](../active-directory/develop/developer-glossary.md#application-id-client-id) para la aplicación en el inquilino. 

Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Seleccione **Azure AD B2C** en la lista de servicios de Azure Portal. 

2. En la configuración de B2C, haga clic en **Aplicaciones** y luego en **Agregar**. 

    Para registrar la aplicación web de ejemplo en el inquilino, utilice la siguiente configuración:
    
    ![Incorporación de una nueva aplicación](media/active-directory-b2c-tutorials-desktop-app/desktop-app-registration.png)
    
    | Configuración      | Valor sugerido  | Descripción                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nombre** | My Sample WPF App | Escriba un **Nombre** que describa la aplicación a los consumidores. | 
    | **Incluir aplicación web o API web** | Sin  | Seleccione **No** en el caso de una aplicación de escritorio. |
    | **Incluir cliente nativo** | SÍ | Ya que al ser una aplicación de escritorio se considera un cliente nativo. |
    | **URI de redirección** | Valores predeterminados | Identificador único al que Azure AD B2C redirige el agente de usuario en una respuesta de OAuth 2.0. |
    | **URI de redireccionamiento personalizado** | `com.onmicrosoft.contoso.appname://redirect/path` | Escriba `com.onmicrosoft.<your tenant name>.<any app name>://redirect/path` Los flujos de usuario envían tokens a este identificador URI. |
    
3. Haga clic en **Crear** para registrar la aplicación.

Las aplicaciones registradas aparecen en la lista de aplicaciones del inquilino de Azure AD B2C. Seleccione la aplicación de escritorio en la lista. Se muestra el panel de propiedades de la aplicación de escritorio registrada.

![Propiedades de la aplicación de escritorio](./media/active-directory-b2c-tutorials-desktop-app/b2c-desktop-app-properties.png)

Tome nota del **Id. del cliente de aplicación**. El identificador identifica la aplicación de forma exclusiva y será necesario al configurar la aplicación más adelante en el tutorial.

## <a name="create-user-flows"></a>Creación de flujos de usuario

Un flujo de usuario de Azure AD B2C define la experiencia del usuario para una tarea de identidad. Por ejemplo, el registro, el inicio de sesión, el cambio de contraseñas y la edición de perfiles son flujos de usuario comunes.

### <a name="create-a-sign-up-or-sign-in-user-flow"></a>Creación de un flujo de usuario de registro o de inicio de sesión

Para registrar usuarios y que estos accedan a la aplicación de escritorio, cree un **flujo de usuario de registro o de inicio de sesión**.

1. En la página del portal de Azure AD B2C, seleccione **Flujos de usuario** y haga clic en **Nuevo flujo de usuario**.
2. En la pestaña **Recomendado**, haga clic en **Registrarse e iniciar sesión**.

    Para configurar el flujo de usuario, utilice la siguiente configuración:

    ![Incorporación de un flujo de usuario de registro o de inicio de sesión](media/active-directory-b2c-tutorials-desktop-app/add-susi-user-flow.png)

    | Configuración      | Valor sugerido  | Descripción                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nombre** | SiUpIn | Escriba un **nombre** para el flujo de usuario. El nombre del flujo de usuario tendrá el prefijo **B2C_1_**. En el código de ejemplo, se utiliza el nombre completo del flujo de usuario **B2C_1_SiUpIn**. | 
    | **Proveedores de identidades** | Registro por correo electrónico | El proveedor de identidades que se usa para identificar al usuario de forma exclusiva. |

3.  En **Atributos y notificaciones de usuario**, haga clic en **Mostrar más** y seleccione la siguiente configuración:

    ![Adición de atributos y notificaciones de usuario](media/active-directory-b2c-tutorials-desktop-app/add-attributes-and-claims.png)

    | Columna      | Valores sugeridos  | DESCRIPCIÓN                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Recopilar atributo** | Nombre para mostrar y Código postal | Seleccione los atributos que se van a recopilar del usuario durante el registro. |
    | **Notificación de devolución** | Nombre para mostrar, Código postal, El usuario es nuevo, Identificador de objeto del usuario | Seleccione las [notificaciones](../active-directory/develop/developer-glossary.md#claim) que desea incluir en el [token de acceso](../active-directory/develop/developer-glossary.md#access-token). |

4. Haga clic en **OK**.

5. Haga clic en **Crear** para crear el flujo de usuario. 

### <a name="create-a-profile-editing-user-flow"></a>Creación de un flujo de usuario de edición de perfil

Para permitir que los usuarios restablezcan la información de su perfil de usuario por su cuenta, cree un **flujo de usuario de edición de perfil**.

1. En la página del portal de Azure AD B2C, seleccione **Flujo de usuario** y haga clic en **Nuevo flujo de usuario**.
2. En la pestaña **Recomendado**, haga clic en **Edición de perfiles**.

    Para configurar el flujo de usuario, utilice la siguiente configuración:

    | Configuración      | Valor sugerido  | Descripción                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nombre** | SiPe | Escriba un **nombre** para el flujo de usuario. El nombre del flujo de usuario tendrá el prefijo **B2C_1_**. En el código de ejemplo, se utiliza el nombre completo del flujo de usuario **B2C_1_SiPe**. | 
    | **Proveedores de identidades** | Inicio de sesión en una cuenta local | El proveedor de identidades que se usa para identificar al usuario de forma exclusiva. |

3. En **Atributos de usuario**, haga clic en **Mostrar más** y seleccione la siguiente configuración:

    | Columna      | Valores sugeridos  | DESCRIPCIÓN                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Recopilar atributo** | Nombre para mostrar y Código postal | Seleccione los atributos que los usuarios pueden modificar durante la edición de un perfil. |
    | **Notificación de devolución** | Nombre para mostrar, código postal, identificador de objeto del usuario | Seleccione las [notificaciones](../active-directory/develop/developer-glossary.md#claim) que desea incluir en el [token de acceso](../active-directory/develop/developer-glossary.md#access-token) después de una edición de perfil correcta. |

4. Haga clic en **OK**.
5. Haga clic en **Crear** para crear el flujo de usuario. 

### <a name="create-a-password-reset-user-flow"></a>Creación de un flujo de usuario de restablecimiento de contraseña

Para habilitar en su aplicación el restablecimiento de contraseña, deberá crear un **flujo de usuario de restablecimiento de contraseña**. Este flujo de usuario describe la experiencia del consumidor durante el restablecimiento de contraseña y el contenido de los tokens que recibe la aplicación al finalizar correctamente.

1. En la página del portal de Azure AD B2C, seleccione **Flujos de usuario** y haga clic en **Nuevo flujo de usuario**.
2. En la pestaña **Recomendado**, haga clic en **Restablecimiento de contraseña**.

    Para configurar el flujo de usuario, utilice la siguiente configuración.

    | Configuración      | Valor sugerido  | Descripción                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nombre** | SSPR | Escriba un **nombre** para el flujo de usuario. El nombre del flujo de usuario tendrá el prefijo **B2C_1_**. En el código de ejemplo, se utiliza el nombre completo del flujo de usuario **B2C_1_SSPR**. | 
    | **Proveedores de identidades** | Restablecer la contraseña mediante la dirección de correo electrónico | Es el proveedor de identidades que se usa para identificar al usuario de forma exclusiva. |

3. En **Notificaciones de la aplicación**, haga clic en **Mostrar más** y seleccione la siguiente configuración:

    | Columna      | Valor sugerido  | DESCRIPCIÓN                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Notificación de devolución** | Identificador de objeto del usuario | Seleccione las [notificaciones](../active-directory/develop/developer-glossary.md#claim) que desea incluir en el [token de acceso](../active-directory/develop/developer-glossary.md#access-token) después de un restablecimiento de contraseña correcto. |

4. Haga clic en **OK**.
5. Haga clic en **Crear** para crear el flujo de usuario. 

## <a name="update-desktop-app-code"></a>Actualización del código de la aplicación de escritorio

Ahora que tiene una aplicación de escritorio registrada y los flujos de usuario creados, debe configurar la aplicación para que use el inquilino de Azure AD B2C. En este tutorial, configurará una aplicación de escritorio de ejemplo. 

[Descargue un archivo zip](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop/archive/master.zip), [examine el repositorio](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop) o clone el ejemplo de GitHub.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop.git
```

La aplicación de escritorio de WPF de ejemplo muestra cómo una aplicación de escritorio puede usar Azure AD B2C para el registro del usuario, el inicio de sesión y llamar a una API web protegida.

Debe cambiar la aplicación para que use el registro de aplicación en el inquilino y configurar los flujos de usuario que ha creado. 

Para cambiar la configuración de la aplicación:

1. Abra la solución `active-directory-b2c-wpf` en Visual Studio.

2. En el proyecto `active-directory-b2c-wpf`, abra el archivo **App.xaml.cs** y realice las siguientes actualizaciones:

    ```C#
    private static string Tenant = "<your-tenant-name>.onmicrosoft.com";
    private static string ClientId = "The Application ID for your desktop app registered in your tenant";
    ```

3. Actualice la variable **PolicySignUpSignIn** con el nombre del *flujo de usuario de registro o de inicio de sesión* que ha creado en un paso anterior. No olvide incluir el prefijo *B2C_1_*.

    ```C#
    public static string PolicySignUpSignIn = "B2C_1_SiUpIn";
    ```

## <a name="run-the-sample-desktop-application"></a>Ejecución de la aplicación de escritorio de ejemplo

Presione **F5** para compilar y ejecutar la aplicación de escritorio. 

La aplicación de ejemplo es compatible con el registro, el inicio de sesión, la edición de un perfil y el restablecimiento de contraseña. Este tutorial ilustra cómo un usuario se registra en la aplicación mediante una dirección de correo electrónico. Puede explorar los demás escenarios por su cuenta.

### <a name="sign-up-using-an-email-address"></a>Registro con una dirección de correo electrónico

1. Haga clic en el botón **Iniciar sesión** para registrarse como usuario de la aplicación de escritorio. Aquí se utiliza el flujo de usuario **B2C_1_SiUpIn** que ha definido en un paso anterior.

2. Azure AD B2C presenta una página de inicio de sesión con un vínculo de registro. Como no tiene aún una cuenta, haga clic en el vínculo **Registrarse ahora**. 

3. El flujo de trabajo del registro presenta una página para recopilar y verificar la identidad del usuario con una dirección de correo electrónico. El flujo de trabajo de registro también recopila la contraseña del usuario y los atributos requeridos definidos en el flujo de usuario.

    Utilice una dirección de correo electrónico válida y valídela mediante el código de verificación. Establezca una contraseña. Especifique valores para los atributos solicitados. 

    ![Flujo de trabajo de registro](media/active-directory-b2c-tutorials-desktop-app/sign-up-workflow.png)

4. Haga clic en **Crear** para crear una cuenta local en el inquilino de Azure AD B2C.

Ahora el usuario puede utilizar su dirección de correo electrónico para iniciar sesión y usar la aplicación de escritorio.

> [!NOTE]
> Si hace clic en el botón **Call API** (Llamar API), aparecerá el error "No autorizado". Dicho error aparece se intenta acceder a un recurso desde el inquilino de demostración. Puesto que su token de acceso solo es válido para su inquilino de Azure AD, la llamada de la API no tiene autorización. Pase al siguiente tutorial para crear una API web protegida para el inquilino. 

## <a name="clean-up-resources"></a>Limpieza de recursos

Puede usar el inquilino de Azure AD B2C si tiene previsto leer otros tutoriales de Azure AD B2C. Cuando ya no sea necesario, puede [eliminar el inquilino de Azure AD B2C](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a crear a un inquilino de Azure AD B2C, a crear flujos de usuario y a actualizar la aplicación de escritorio de ejemplo para que use el inquilino de Azure AD B2C. En el siguiente tutorial aprenderá a registrar, configurar y llamar a una API web protegida desde una aplicación de escritorio.

> [!div class="nextstepaction"]
> 
