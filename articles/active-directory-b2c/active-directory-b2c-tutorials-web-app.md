---
title: 'Tutorial: Habilitación de una aplicación web para autenticarse con cuentas mediante Azure Active Directory B2C | Microsoft Docs'
description: Tutorial sobre cómo usar Azure Active Directory B2C para proporcionar inicios de sesión de usuario en una aplicación web de ASP.NET.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.author: davidmu
ms.date: 11/30/2018
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.component: B2C
ms.openlocfilehash: 30a94cb5de2d618938f17c4e5733821ac7247785
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54851528"
---
# <a name="tutorial-enable-a-web-application-to-authenticate-with-accounts-using-azure-active-directory-b2c"></a>Tutorial: Habilitación de una aplicación web para autenticarse con cuentas mediante Azure Active Directory B2C

En este tutorial se muestra cómo usar Azure Active Directory (Azure AD) B2C para registrar e iniciar sesión con usuarios en una aplicación web para ASP.NET. Azure AD B2C permite que las aplicaciones puedan autenticarse en cuentas de las redes sociales, cuentas de empresa y cuentas de Azure Active Directory mediante protocolos estándar abiertos.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Registrar una aplicación web para ASP.NET de ejemplo en el inquilino de Azure AD B2C.
> * Crear flujos de usuario para el registro, el inicio de sesión, la edición de un perfil y el restablecimiento de contraseña.
> * Configurar la aplicación web de ejemplo para usar el inquilino de Azure AD B2C. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

* Crear su propio [inquilino de Azure AD B2C](active-directory-b2c-get-started.md).
* Instalar [Visual Studio 2017](https://www.visualstudio.com/downloads/) con la carga de trabajo de **ASP.NET y desarrollo web**.

## <a name="register-web-app"></a>Registro de una aplicación web

Las aplicaciones se deben [registrar](../active-directory/develop/developer-glossary.md#application-registration) en el inquilino antes de que puedan recibir [tokens de acceso](../active-directory/develop/developer-glossary.md#access-token) de Azure Active Directory. El registro de una aplicación crea un [identificador de aplicación](../active-directory/develop/developer-glossary.md#application-id-client-id) para la aplicación en el inquilino. 

Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**. Ahora debería estar utilizando el inquilino que ha creado en el tutorial anterior. 

2. En la configuración de B2C, haga clic en **Aplicaciones** y luego en **Agregar**. 

    Para registrar la aplicación web de ejemplo en el inquilino, utilice la siguiente configuración:

    ![Incorporación de una nueva aplicación](media/active-directory-b2c-tutorials-web-app/web-app-registration.png)
    
    | Configuración      | Valor sugerido  | Descripción                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nombre** | Mi aplicación web de ejemplo | Escriba un **Nombre** que describa la aplicación a los consumidores. | 
    | **Incluir aplicación web o API web** | SÍ | Seleccione **Sí** para una aplicación web. |
    | **Permitir flujo implícito** | SÍ | Seleccione **Sí**, ya que la aplicación utiliza el [inicio de sesión con OpenID Connect](active-directory-b2c-reference-oidc.md). |
    | **URL de respuesta** | `https://localhost:44316` | Las direcciones URL de respuesta son puntos de conexión en los que Azure AD B2C devolverá los tokens que su aplicación solicite. En este tutorial, el ejemplo se ejecuta localmente (localhost) y escucha en el puerto 44316. |
    | **Incluir cliente nativo** | Sin  | Como es una aplicación web y no un cliente nativo, seleccione No. |
    
3. Haga clic en **Crear** para registrar la aplicación.

Las aplicaciones registradas aparecen en la lista de aplicaciones del inquilino de Azure AD B2C. Seleccione la aplicación web en la lista. Se muestra el panel de propiedades de la aplicación web.

![Propiedades de la aplicación web](./media/active-directory-b2c-tutorials-web-app/b2c-web-app-properties.png)

Anote el **Id. de la aplicación**. El identificador identifica la aplicación de forma exclusiva y será necesario al configurar la aplicación más adelante en el tutorial.

### <a name="create-a-client-password"></a>Creación de una contraseña de cliente

Azure AD B2C utiliza la autorización de OAuth2 para [aplicaciones cliente](../active-directory/develop/developer-glossary.md#client-application). Las aplicaciones web son [clientes confidenciales](../active-directory/develop/developer-glossary.md#web-client) y requieren un identificador de cliente o un identificador de aplicación y un secreto de cliente, contraseña de cliente o clave de aplicación.

1. Seleccione la página de claves de la aplicación web registrada y haga clic en **Generar clave**.

2. Haga clic en **Guardar** para mostrar la clave de aplicación.

    ![Página de claves generales de la aplicación](media/active-directory-b2c-tutorials-web-app/app-general-keys-page.png)

La clave se muestra una vez en el portal. Es importante copiar y guardar el valor de la clave. Se necesita este valor para la configuración de la aplicación. Proteja la clave. No comparta la clave de manera pública.

## <a name="create-user-flows"></a>Creación de flujos de usuario

Un flujo de usuario de Azure AD B2C define la experiencia del usuario para una tarea de identidad. Por ejemplo, el registro, el inicio de sesión, el cambio de contraseñas y la edición de perfiles son flujos de usuario comunes.

### <a name="create-a-sign-up-or-sign-in-user-flow"></a>Creación de un flujo de usuario de registro o de inicio de sesión

Para registrar a usuarios y que estos tengan acceso a la aplicación web, cree un **flujo de usuario de registro o de inicio de sesión**.

1. En la página del portal de Azure AD B2C, seleccione **Flujos de usuario** y haga clic en **Nuevo flujo de usuario**.
2. En la pestaña **Recomendado**, haga clic en **Registrarse e iniciar sesión**.

    Para configurar el flujo de usuario, utilice la siguiente configuración:

    ![Incorporación de un flujo de usuario de registro o de inicio de sesión](media/active-directory-b2c-tutorials-web-app/add-susi-user-flow.png)

    | Configuración      | Valor sugerido  | Descripción                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nombre** | SiUpIn | Escriba un **nombre** para el flujo de usuario. El nombre del flujo de usuario tendrá el prefijo **b2c_1_**. En el código de ejemplo, se utiliza el nombre completo del flujo de usuario **b2c_1_SiUpIn**. | 
    | **Proveedores de identidades** | Registro por correo electrónico | El proveedor de identidades que se usa para identificar al usuario de forma exclusiva. |

3. En **Atributos y notificaciones de usuario**, haga clic en **Mostrar más** y seleccione la siguiente configuración:

    ![Incorporación de un flujo de usuario de registro o de inicio de sesión](media/active-directory-b2c-tutorials-web-app/add-attributes-and-claims.png)

    | Columna      | Valores sugeridos  | DESCRIPCIÓN                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Recopilar atributo** | Nombre para mostrar y Código postal | Seleccione los atributos que se van a recopilar del usuario durante el registro. |
    | **Notificación de devolución** | Nombre para mostrar, Código postal, El usuario es nuevo, Identificador de objeto del usuario | Seleccione las [notificaciones](../active-directory/develop/developer-glossary.md#claim) que desea incluir en el [token de acceso](../active-directory/develop/developer-glossary.md#access-token). |

4. Haga clic en **OK**.
5. Haga clic en **Crear** para crear el flujo de usuario. 

### <a name="create-a-profile-editing-user-flow"></a>Creación de un flujo de usuario de edición de perfil

Para permitir que los usuarios restablezcan la información de su perfil de usuario por su cuenta, cree un **flujo de usuario de edición de perfil**.

1. En la página del portal de Azure AD B2C, seleccione **Flujos de usuario** y haga clic en **Nuevo flujo de usuario**.
2. En la pestaña **Recomendado**, haga clic en **Edición de perfiles**.

    Para configurar el flujo de usuario, utilice la siguiente configuración:

    | Configuración      | Valor sugerido  | Descripción                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nombre** | SiPe | Escriba un **nombre** para el flujo de usuario. El nombre del flujo de usuario tendrá el prefijo **b2c_1_**. En el código de ejemplo, se utiliza el nombre completo del flujo de usuario **b2c_1_SiPe**. | 
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

1. En la página del portal de Azure AD B2C, seleccione **Directivas de restablecimiento de contraseña** y haga clic en **Agregar**.
2. En la pestaña **Recomendado**, haga clic en **Restablecimiento de contraseña**.

    Para configurar el flujo de usuario, utilice la siguiente configuración.

    | Configuración      | Valor sugerido  | Descripción                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Nombre** | SSPR | Escriba un **nombre** para el flujo de usuario. El nombre del flujo de usuario tendrá el prefijo **b2c_1_**. En el código de ejemplo, se utiliza el nombre completo del flujo de usuario **b2c_1_SSPR**. | 
    | **Proveedores de identidades** | Restablecer la contraseña mediante la dirección de correo electrónico | Es el proveedor de identidades que se usa para identificar al usuario de forma exclusiva. |

3. En **Notificaciones de la aplicación**, haga clic en **Mostrar más** y seleccione la siguiente configuración:
    | Columna      | Valor sugerido  | DESCRIPCIÓN                                        |
    | ------------ | ------- | -------------------------------------------------- |
    | **Notificación de devolución** | Identificador de objeto del usuario | Seleccione las [notificaciones](../active-directory/develop/developer-glossary.md#claim) que desea incluir en el [token de acceso](../active-directory/develop/developer-glossary.md#access-token) después de un restablecimiento de contraseña correcto. |

4. Haga clic en **OK**.
5. Haga clic en **Crear** para crear el flujo de usuario. 

## <a name="update-web-app-code"></a>Actualización del código de aplicación web

Ahora que tiene una aplicación web registrada y los flujos de usuario creados, debe configurar la aplicación para que use el inquilino de Azure AD B2C. En este tutorial, configurará una aplicación web de ejemplo que puede descargar desde GitHub. 

[Descargue un archivo zip](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi/archive/master.zip) o clone la aplicación web de ejemplo desde GitHub. Asegúrese de extraer el archivo de ejemplo en una carpeta donde la longitud total de caracteres de la ruta de acceso sea inferior a 260.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

La aplicación web para ASP.NET de ejemplo es una aplicación de lista de tareas sencilla para crear y actualizar una lista de tareas pendientes. La aplicación utiliza [componentes de middleware de Microsoft OWIN](https://docs.microsoft.com/aspnet/aspnet/overview/owin-and-katana/) para permitir que los usuarios se registren para usar la aplicación en su inquilino de Azure AD B2C. Al crear un flujo de usuario de Azure AD B2C, los usuarios pueden utilizar una cuenta de las redes sociales o crear una cuenta para utilizarla como su identidad al acceder a la aplicación. 

Hay dos proyectos en la solución de ejemplo:

**Aplicación de ejemplo de aplicación web (TaskWebApp):** aplicación web para crear y editar una lista de tareas. La aplicación web utiliza el flujo de usuario de **registro o de inicio de sesión** para el registro o el inicio de sesión.

**Aplicación de ejemplo de API Web (TaskService):** API web que admite la funcionalidad para crear, leer, actualizar y eliminar la lista de tareas. Azure AD B2C protege la API web y la aplicación web la llama.

Tiene que cambiar la aplicación para que use el registro de aplicaciones de su inquilino, el cual incluye el identificador de aplicación y la clave que registró anteriormente. También debe configurar los flujos de usuario que ha creado. La aplicación web ejemplo define los valores de configuración como configuración de la aplicación en el archivo Web.config. Para cambiar la configuración de la aplicación:

1. Abra la solución **B2C-WebAPI-DotNet** en Visual Studio.

2. En el proyecto de aplicación web **TaskWebApp**, abra el archivo **Web.config**. Reemplace el valor de `ida:Tenant` con el nombre del inquilino que ha creado. Reemplace el valor de `ida:ClientId` con el identificador que ha registrado. Reemplace el valor de `ida:ClientSecret` con la clave que ha registrado.

3. En el archivo **Web.config**, reemplace el valor de `ida:SignUpSignInPolicyId` con `b2c_1_SiUpIn`. Reemplace el valor de `ida:EditProfilePolicyId` con `b2c_1_SiPe`. Reemplace el valor de `ida:ResetPasswordPolicyId` con `b2c_1_SSPR`.

## <a name="run-the-sample-web-app"></a>Ejecución de la aplicación web de ejemplo

En el Explorador de soluciones, haga clic con el botón derecho en el proyecto **TaskWebApp** y haga clic en **Establecer como proyecto de inicio**.

Presione **F5** para iniciar la aplicación web. Inicia el explorador predeterminado en la dirección del sitio web local `https://localhost:44316/`. 

La aplicación de ejemplo es compatible con el registro, el inicio de sesión, la edición de un perfil y el restablecimiento de contraseña. Este tutorial ilustra cómo un usuario se registra en la aplicación mediante una dirección de correo electrónico. Puede explorar los demás escenarios por su cuenta.

### <a name="sign-up-using-an-email-address"></a>Registro con una dirección de correo electrónico

1. Haga clic en el vínculo de **registro/inicio de sesión** en el mensaje emergente superior para registrar un usuario de la aplicación web. Aquí se utiliza el flujo de usuario **b2c_1_SiUpIn** que ha definido en un paso anterior.

2. Azure AD B2C presenta una página de inicio de sesión con un vínculo de registro. Como no tiene aún una cuenta, haga clic en el vínculo **Registrarse ahora**. 

3. El flujo de trabajo del registro presenta una página para recopilar y verificar la identidad del usuario con una dirección de correo electrónico. El flujo de trabajo de registro también recopila la contraseña del usuario y los atributos requeridos definidos en el flujo de usuario.

    Utilice una dirección de correo electrónico válida y valídela mediante el código de verificación. Establezca una contraseña. Especifique valores para los atributos solicitados. 

    ![Flujo de trabajo de registro](media/active-directory-b2c-tutorials-web-app/sign-up-workflow.png)

4. Haga clic en **Crear** para crear una cuenta local en el inquilino de Azure AD B2C.

Ahora el usuario puede utilizar su dirección de correo electrónico para iniciar sesión y usar la aplicación web.

## <a name="clean-up-resources"></a>Limpieza de recursos

Puede usar el inquilino de Azure AD B2C si tiene previsto leer otros tutoriales de Azure AD B2C. Cuando ya no sea necesario, puede [eliminar el inquilino de Azure AD B2C](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a crear un inquilino de Azure AD B2C, crear flujos de trabajo y actualizar la aplicación web de ejemplo para usar el inquilino de Azure AD B2C. Prosiga en el siguiente tutorial para aprender a registrar, configurar y llamar a ASP.NET Web API protegido por el inquilino de Azure AD B2C.

> [!div class="nextstepaction"]
> [Tutorial: Uso de Azure Active Directory B2C para proteger una API web de ASP.NET](active-directory-b2c-tutorials-web-api.md)
