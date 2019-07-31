---
title: 'Introducción a las directivas personalizadas: Azure Active Directory B2C'
description: Obtenga información sobre cómo empezar a usar directivas personalizadas en Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/16/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 83269a5ae0d2e5fb7ae2651dbc27926c910a0e03
ms.sourcegitcommit: af58483a9c574a10edc546f2737939a93af87b73
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2019
ms.locfileid: "68302489"
---
# <a name="get-started-with-custom-policies-in-azure-active-directory-b2c"></a>Introducción a las directivas personalizadas en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Las [directivas personalizadas](active-directory-b2c-overview-custom.md) son archivos de configuración que definen el comportamiento del inquilino de Azure Active Directory (Azure AD) B2C. En este artículo se crea una directiva personalizada que admite el registro o el inicio de sesión de la cuenta local mediante una dirección de correo electrónico y una contraseña. También debe preparar el entorno para agregar proveedores de identidades.

## <a name="prerequisites"></a>Requisitos previos

- Si todavía no tiene uno, debe [crear un inquilino de Azure AD B2C](tutorial-create-tenant.md) vinculado a la suscripción de Azure.
- [Registre la aplicación](tutorial-register-applications.md) en el inquilino que ha creado para que pueda comunicarse con Azure AD B2C.

## <a name="add-signing-and-encryption-keys"></a>Agregar claves de firma y cifrado

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global del inquilino de Azure AD B2C.
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Haga clic en el **filtro de directorio y suscripciones** del menú superior y elija el directorio que contiene la suscripción.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**.
4. En la página de introducción, seleccione **Identity Experience Framework**.

### <a name="create-the-signing-key"></a>Crear la clave de firma

1. Seleccione **Claves de directiva** y luego **Agregar**.
2. En **Opciones**, elija `Generate`.
3. En **Nombre**, escriba `TokenSigningKeyContainer`. Es posible que se agregue automáticamente el prefijo `B2C_1A_`.
4. En **Tipo de clave**, seleccione **RSA**.
5. En **Uso de claves**, seleccione **Firma**.
6. Haga clic en **Create**(Crear).

### <a name="create-the-encryption-key"></a>Crear la clave de cifrado

1. Seleccione **Claves de directiva** y luego **Agregar**.
2. En **Opciones**, elija `Generate`.
3. En **Nombre**, escriba `TokenEncryptionKeyContainer`. Es posible que se agregue automáticamente el prefijo `B2C_1A`_.
4. En **Tipo de clave**, seleccione **RSA**.
5. En **Uso de la clave**, seleccione **Cifrado**.
6. Haga clic en **Create**(Crear).

### <a name="create-the-facebook-key"></a>Crear la clave de Facebook

Si ya tiene un [secreto de aplicación de Facebook](active-directory-b2c-setup-fb-app.md), agréguelo como clave de directiva al inquilino. De lo contrario, debe crear la clave con un valor de marcador de posición para que las directivas pasen la validación.

1. Seleccione **Claves de directiva** y luego **Agregar**.
2. En **Opciones**, elija `Manual`.
3. En **Nombre**, escriba `FacebookSecret`. Es posible que se agregue automáticamente el prefijo `B2C_1A_`.
4. En **Secreto**, escriba el secreto de Facebook de developers.facebook.com o `0` como marcador de posición. Este valor es el secreto, no el identificador de la aplicación.
5. En **Uso de claves**, seleccione **Firma**.
6. Haga clic en **Create**(Crear).

## <a name="register-identity-experience-framework-applications"></a>Registro de las aplicaciones del marco de experiencia de identidad

Azure AD B2C requiere que registre dos aplicaciones que se usen para registrar usuarios e iniciar su sesión: IdentityExperienceFramework (una aplicación web) y ProxyIdentityExperienceFramework (una aplicación nativa) con permiso delegado de la aplicación IdentityExperienceFramework. Las cuentas locales solo existen en su inquilino. Los usuarios se registran con una combinación única de dirección de correo electrónico/contraseña para obtener acceso a las aplicaciones registradas en el inquilino.

### <a name="register-the-identityexperienceframework-application"></a>Registrar la aplicación IdentityExperienceFramework

1. Seleccione **Todos los servicios** en la esquina superior izquierda de Azure Portal.
1. En el cuadro de búsqueda, escriba `Azure Active Directory`.
1. En la lista de resultados, seleccione **Azure Active Directory**.
1. En **Administrar**, en el menú de la izquierda, seleccione **Registros de aplicaciones (característica heredada)** .
1. Seleccione **Nuevo registro de aplicaciones**.
1. En **Nombre**, escriba `IdentityExperienceFramework`.
1. En **Tipo de aplicación**, elija **Aplicación web o API**.
1. En **URL de inicio de sesión**, escriba `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com`, donde `your-tenant-name` es el nombre de dominio del inquilino de Azure AD B2C. Todas las direcciones URL ahora deberían estar utilizando [b2clogin.com](b2clogin.md).
1. Haga clic en **Create**(Crear). Una vez creada, copie el identificador de aplicación y guárdelo para usarlo más adelante.

### <a name="register-the-proxyidentityexperienceframework-application"></a>Registrar la aplicación ProxyIdentityExperienceFramework

1. En **Registros de aplicaciones (característica heredada)** , seleccione **Nuevo registro de aplicaciones**.
2. En **Nombre**, escriba `ProxyIdentityExperienceFramework`.
3. En **Tipo de aplicación**, elija **Nativa**.
4. En **URI de redirección**, escriba `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com`, donde `your-tenant-name` es el inquilino de Azure AD B2C.
5. Haga clic en **Create**(Crear). Una vez creada, copie el identificador de aplicación y guárdelo para usarlo más adelante.
6. En la página Configuración, seleccione **Permisos necesarios** y luego **Agregar**.
7. Elija **Seleccionar una API**, busque y seleccione **IdentityExperienceFramework** y, después, haga clic en **Seleccionar**.
9. Active la casilla situada junto a **Access IdentityExperienceFramework** (Acceder a IdentityExperienceFramework), haga clic en **Seleccionar** y luego en **Listo**.
10. Seleccione **Conceder permisos** y haga clic en **Sí** para confirmar.

## <a name="custom-policy-starter-pack"></a>Paquete de inicio de directivas personalizadas

Las directivas personalizadas son un conjunto de archivos XML que se cargan en el inquilino de Azure AD B2C para definir perfiles técnicos y recorridos de usuario. Proporcionamos paquetes de inicio con varias directivas predefinidas para que pueda empezar a trabajar rápidamente. Cada uno de estos paquetes de inicio contiene el menor número de perfiles técnicos y recorridos del usuario necesarios para lograr los escenarios descritos:

- **LocalAccounts**: habilita el uso solo de cuentas locales.
- **SocialAccounts**: habilita el uso solo de cuentas sociales (o federadas).
- **SocialAndLocalAccounts**: habilita el uso de cuentas locales y sociales.
- **SocialAndLocalAccountsWithMFA**: habilita opciones sociales, locales y de autenticación multifactor.

Cada paquete de inicio contiene lo siguiente:

- **Archivo base**: se requieren algunas modificaciones en el archivo base. Ejemplo: *TrustFrameworkBase.xml*.
- **Archivo de extensión**: este archivo es donde se hace la mayoría de los cambios de configuración. Ejemplo: *TrustFrameworkExtensions.xml*.
- **Archivos de usuario de confianza**: archivos específicos de la tarea a los que llama la aplicación. Ejemplos: *SignUpOrSignin.xml*, *ProfileEdit.xml*, *PasswordReset.xml*.

En este artículo, editará los archivos de directivas personalizadas XML del paquete de inicio **SocialAndLocalAccounts**. Si necesita un editor XML, [pruebe Visual Studio Code](https://code.visualstudio.com/download), un editor multiplataforma ligero.

### <a name="get-the-starter-pack"></a>Obtención del paquete de inicio

Obtenga los paquetes de inicio de directivas personalizadas en GitHub y, luego, actualice los archivos XML del paquete de inicio SocialAndLocalAccounts con el nombre del inquilino de Azure AD B2C.

1. [Descargue el archivo .zip](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) o clone el repositorio:

    ```console
    git clone https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
    ```

1. En todos los archivos del directorio **SocialAndLocalAccounts**, reemplace la cadena `yourtenant` por el nombre de su inquilino de Azure AD B2C.

    Por ejemplo, si el nombre del inquilino de B2C es *contosotenant*, todas las instancias de `yourtenant.onmicrosoft.com` se convierten en `contosotenant.onmicrosoft.com`.

### <a name="add-application-ids-to-the-custom-policy"></a>Agregar identificadores de aplicación a la directiva personalizada

Agregue los identificadores de aplicación al archivo de extensiones *TrustFrameworkExtensions.xml*.

1. Abra `SocialAndLocalAccounts/` **`TrustFrameworkExtensions.xml`** y busque el elemento `<TechnicalProfile Id="login-NonInteractive">`.
1. Reemplace ambas instancias de `IdentityExperienceFrameworkAppId` por el identificador de la aplicación IdentityExperienceFramework que creó antes.
1. Reemplace ambas instancias de `ProxyIdentityExperienceFrameworkAppId` por el identificador de la aplicación ProxyIdentityExperienceFramework que creó antes.
1. Guarde el archivo.

## <a name="upload-the-policies"></a>Cargar las directivas

1. Seleccione el elemento de menú **Identity Experience Framework** en el inquilino de B2C en Azure Portal.
1. Seleccione **Cargar directiva personalizada**.
1. En este orden, cargue los archivos de directiva:
    1. *TrustFrameworkBase.xml*
    1. *TrustFrameworkExtensions.xml*
    1. *SignUpOrSignin.xml*
    1. *ProfileEdit.xml*
    1. *PasswordReset.xml*

A medida que cargue los archivos, Azure agregará el prefijo `B2C_1A_` a cada uno.

> [!TIP]
> Si el editor XML admite la validación, valide los archivos con el archivo de esquema XML `TrustFrameworkPolicy_0.3.0.0.xsd` que se encuentra en el directorio raíz del paquete de inicio. La validación del esquema XML identifica los errores antes de realizar la carga.

## <a name="test-the-custom-policy"></a>Prueba de la directiva personalizada

1. En **Directivas personalizadas**, seleccione **B2C_1A_signup_signin**.
1. Para **Seleccionar aplicación** en la página Información general de la directiva personalizada, seleccione la aplicación web denominada *webapp1* que ha registrado antes.
1. Asegúrese de que la **URL de respuesta** sea `https://jwt.ms`.
1. Seleccione **Ejecutar ahora**.
1. Regístrese con una dirección de correo electrónico.
1. Vuelva a seleccionar **Ejecutar ahora**.
1. Inicie sesión con la misma cuenta para confirmar que tiene una configuración correcta.

## <a name="add-facebook-as-an-identity-provider"></a>Incorporación de Facebook como proveedor de identidades

1. Complete los pasos descritos en [Configuración de la suscripción y del inicio de sesión con una cuenta de Facebook](active-directory-b2c-setup-fb-app.md) para configurar una aplicación de Facebook.
1. En el archivo `SocialAndLocalAccounts/` **`TrustFrameworkExtensions.xml`** , sustituya el valor de `client_id` por el identificador de aplicación de Facebook:

   ```xml
   <TechnicalProfile Id="Facebook-OAUTH">
     <Metadata>
     <!--Replace the value of client_id in this technical profile with the Facebook app ID"-->
       <Item Key="client_id">00000000000000</Item>
   ```

1. Cargue el archivo *TrustFrameworkExtensions.xml* en el inquilino.
1. En **Directivas personalizadas**, seleccione **B2C_1A_signup_signin**.
1. Seleccione **Ejecutar ahora** y elija Facebook para iniciar sesión con Facebook y probar la directiva personalizada. También puede invocar la directiva directamente desde la aplicación registrada.

## <a name="next-steps"></a>Pasos siguientes

Ahora, pruebe a agregar Azure Active Directory (Azure AD) como proveedor de identidades. El archivo base que se ha usado en esta guía de introducción ya contiene parte del contenido que necesita para agregar otros proveedores de identidades como Azure AD.

Para obtener información sobre cómo configurar Azure AD como proveedor de identidades, vea [Configurar el inicio de sesión con una cuenta de Azure Active Directory mediante directivas personalizadas en Azure Active Directory B2C](active-directory-b2c-setup-aad-custom.md).
