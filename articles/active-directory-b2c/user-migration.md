---
title: Enfoques de migración de usuarios
titleSuffix: Azure AD B2C
description: Describe tanto los fundamentos como los conceptos avanzados sobre la migración de usuarios mediante Azure AD Graph API y opcionalmente mediante directivas personalizadas de Azure AD B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/26/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 6c70ab718235a67d9f039df638f8ba90b21ab15b
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76850739"
---
# <a name="migrate-users-to-azure-active-directory-b2c"></a>Migrar usuarios a Azure Active Directory B2C

Si migra su proveedor de identidades a Azure Active Directory B2C (Azure AD B2C), también debe migrar las cuentas de usuario. En este artículo se explica cómo migrar las cuentas de usuario existentes desde cualquier proveedor de identidades a Azure AD B2C. El artículo no pretende ser preceptivo, sino describir varios enfoques. El desarrollador es responsable de la idoneidad de cada enfoque.

## <a name="user-migration-flows"></a>Flujos de migración de usuarios

Con Azure AD B2C, puede migrar los usuarios mediante [Azure AD Graph API][B2C-GraphQuickStart]. El proceso de migración de usuarios se divide en dos flujos:

- **Antes de la migración**: este flujo se aplica cuando tiene acceso libre a las credenciales de un usuario (nombre de usuario y contraseña) o las credenciales se cifran, pero puede descifrarlas. El proceso de pre-migración implica leer los usuarios del anterior proveedor de identidades y crear nuevas cuentas en el directorio de Azure AD B2C.

- **Pre-migración y restablecimiento de contraseña**: este flujo se aplica cuando la contraseña de un usuario no es accesible. Por ejemplo:
  - La contraseña se almacena en formato HASH.
  - La contraseña se almacena en un proveedor de identidades al que no tiene acceso. El proveedor de identidades anterior valida las credenciales de usuario mediante una llamada a un servicio web.

En ambos flujos, primero debe ejecutar el proceso de pre-migración, leer los usuarios del proveedor de identidades anterior y crear nuevas cuentas en el directorio de Azure AD B2C. Si no tiene la contraseña, debe crear la cuenta con una contraseña generada de forma aleatoria. A continuación, pida al usuario que cambie la contraseña, o bien, cuando el usuario inicie sesión por primera vez, Azure AD B2C pedirá al usuario que la restablezca.

## <a name="password-policy"></a>Directiva de contraseñas

La directiva de contraseñas de Azure AD B2C (para cuentas locales) se basa en la directiva de Azure AD. Las directivas de restablecimiento de la contraseña, registro e inicio de sesión de Azure AD B2C utilizan la seguridad de la contraseña "segura" y las contraseñas no expiran. Para obtener más información, consulte el artículo sobre la [directiva de contraseñas de Azure AD][AD-PasswordPolicies].

Si las cuentas que se van a migrar tienen menos seguridad de contraseña que la [seguridad de contraseña exigida por Azure AD B2C][AD-PasswordPolicies], puede deshabilitar el requisito de contraseña segura. Para cambiar la directiva de contraseñas predeterminada, establezca la propiedad `passwordPolicies` en `DisableStrongPassword`. Por ejemplo, puede modificar la solicitud de creación de usuario de la siguiente manera:

```JSON
"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"
```

## <a name="step-1-use-azure-ad-graph-api-to-migrate-users"></a>Paso 1: Uso de Azure AD Graph API para migrar usuarios

Debe crear la cuenta de usuario de Azure AD B2C a través de Graph API (con la contraseña o con una contraseña aleatoria). Esta sección describe el proceso de creación de cuentas de usuario en el directorio de Azure AD B2C mediante Graph API.

### <a name="step-11-register-your-application-in-your-tenant"></a>Paso 1.1: Registro de la aplicación en el inquilino

Para comunicarse con Graph API, debe tener una cuenta de servicio con privilegios administrativos. En Azure AD, se registra una aplicación y se habilita el acceso de escritura al directorio. Las credenciales de la aplicación son el **identificador de aplicación** y el **secreto de aplicación**. La aplicación actúa como tal, no como un usuario, para llamar a Graph API.

En primer lugar, registre una aplicación que pueda usar para tareas de administración como la migración de usuarios.

[!INCLUDE [active-directory-b2c-appreg-mgmt](../../includes/active-directory-b2c-appreg-mgmt.md)]

### <a name="step-12-grant-administrative-permission-to-your-application"></a>Paso 1.2: Concesión de permisos administrativos a la aplicación

A continuación, conceda a la aplicación los permisos de Graph API de Azure AD necesarios para escribir en el directorio.

[!INCLUDE [active-directory-b2c-permissions-directory](../../includes/active-directory-b2c-permissions-directory.md)]

### <a name="step-13-create-the-application-secret"></a>Paso 1.3: Creación del secreto de aplicación

Cree un secreto de cliente (clave) para que lo use la aplicación de migración de usuarios que configure en un paso posterior.

[!INCLUDE [active-directory-b2c-client-secret](../../includes/active-directory-b2c-client-secret.md)]

Ahora tiene una aplicación con permisos para crear, leer y actualizar los usuarios en el inquilino de Azure AD B2C.

### <a name="step-14-optional-environment-cleanup"></a>Paso 1.4: (Opcional) Limpieza del entorno

El permiso *Leer y escribir en datos de directorio* *no* incluye el derecho a eliminar usuarios. Para que su aplicación pueda eliminar usuarios (para limpiar el entorno), debe realizar un paso adicional, que implica la ejecución de PowerShell a fin de establecer los permisos de administrador de la cuenta de usuario. En caso contrario, puede saltar a la sección siguiente.

> [!IMPORTANT]
> Debe usar una cuenta de administrador de inquilinos B2C que sea *local* para el inquilino B2C. La sintaxis del nombre de la cuenta es *admin\@contosob2c.onmicrosoft.com*.

En este script de PowerShell, que requiere el [módulo Azure AD PowerShell V2][AD-Powershell], haga lo siguiente:

1. Se conecta a su servicio en línea. Para ello, ejecute el cmdlet `Connect-AzureAD` en el símbolo del sistema de Windows PowerShell y proporcione las credenciales.

1. Se utiliza el **Identificador de aplicación** para asignar el rol de administrador de cuenta de usuario a la aplicación. Estos roles tienen identificadores conocidos, por lo que lo único que debe hacer es escribir el **identificador de aplicación** en el script.

```powershell
# NOTE: This script REQUIRES the Azure AD PowerShell V2 module
#       https://docs.microsoft.com/powershell/azure/active-directory/install-adv2

Connect-AzureAD

$AppId = "<Your application ID>"

# Fetch Azure AD application to assign to role
$roleMember = Get-AzureADServicePrincipal -Filter "AppId eq '$AppId'"

# Fetch User Account Administrator role instance
$role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq 'User Account Administrator'}

# If role instance does not exist, instantiate it based on the role template
if ($role -eq $null) {
    # Instantiate an instance of the role template
    $roleTemplate = Get-AzureADDirectoryRoleTemplate | Where-Object {$_.displayName -eq 'User Account Administrator'}
    Enable-AzureADDirectoryRole -RoleTemplateId $roleTemplate.ObjectId

    # Fetch User Account Administrator role instance again
    $role = Get-AzureADDirectoryRole | Where-Object {$_.displayName -eq 'User Account Administrator'}
}

# Add application to role
Add-AzureADDirectoryRoleMember -ObjectId $role.ObjectId -RefObjectId $roleMember.ObjectId

# Fetch role membership for role to confirm
Get-AzureADDirectoryRoleMember -ObjectId $role.ObjectId
```

Cambie el valor de `$AppId` por el **identificador de aplicación** de Azure AD.

## <a name="step-2-pre-migration-application-sample"></a>Paso 2: Ejemplo de aplicación de pre-migración

Puede encontrar el ejemplo de código previo a la migración en el repositorio de GitHub `azure-ad-b2c/user-migration` mantenido por la comunidad:

[azure-ad-b2c/user-migration/pre-migration][UserMigrationSample-code] (GitHub)

### <a name="step-21-edit-the-migration-data-file"></a>Paso 2.1: Edición del archivo de datos de migración

La aplicación de ejemplo usa un archivo JSON que contiene datos de usuarios ficticios. Después de ejecutar correctamente el ejemplo, puede cambiar el código para consumir los datos de su propia base de datos. O bien puede exportar el perfil de los usuarios a un archivo JSON y configurar la aplicación para usar ese archivo.

Para editar el archivo JSON, abra la solución `AADB2C.UserMigration.sln` de Visual Studio. En el proyecto `AADB2C.UserMigration`, abra el archivo `UsersData.json`.

![Parte del archivo UsersData.json que muestra bloques de JSON de dos usuarios](./media/user-migration/pre-migration-data-file.png)

Como puede ver, el archivo contiene una lista de entidades de usuario. Cada entidad de usuario tiene las siguientes propiedades:

- email
- DisplayName
- firstName
- lastName
- password (puede estar vacío)

> [!NOTE]
> En tiempo de compilación, Visual Studio copia el archivo al directorio `bin`.

### <a name="step-22-configure-the-application-settings"></a>Paso 2.2: Configuración de la aplicación

En el proyecto `AADB2C.UserMigration`, abra el archivo *App.config*. Reemplace los siguientes valores de configuración de la aplicación por sus propios valores:

```XML
<appSettings>
    <add key="b2c:Tenant" value="{Your Tenant Name}" />
    <add key="b2c:ClientId" value="{The ApplicationID from above}" />
    <add key="b2c:ClientSecret" value="{The Client Secret Key from above}" />
    <add key="MigrationFile" value="{Name of a JSON file containing the users' data; for example, UsersData.json}" />
    <add key="BlobStorageConnectionString" value="{Your connection Azure table string}" />
</appSettings>
```

> [!NOTE]
> - El uso de una cadena de conexión de la tabla de Azure se describe en las secciones siguientes.
> - El nombre del inquilino B2C es el dominio que escribió durante la creación del inquilino y se muestra en Azure Portal. El nombre del inquilino normalmente termina con el sufijo *.onmicrosoft.com* (por ejemplo, *contosob2c.onmicrosoft.com*).

### <a name="step-23-run-the-pre-migration-process"></a>Paso 2.3: Ejecución del proceso de pre-migración

Haga clic con el botón derecho en la solución `AADB2C.UserMigration` y vuelva a compilar el ejemplo. Si todo es correcto, ahora debería tener un archivo ejecutable `UserMigration.exe` que se encuentra en `AADB2C.UserMigration\bin\Debug\net461`. Para ejecutar el proceso de migración, use uno de los parámetros de la línea de comandos siguientes:

- Para **migrar usuarios con contraseña**, use el comando `UserMigration.exe 1`.

- Para **migrar usuarios con contraseña aleatoria**, use el comando `UserMigration.exe 2`. Esta operación también crea una entidad de tabla de Azure. Más adelante, se configura la directiva para llamar al servicio de la API de REST. El servicio usa una tabla de Azure para realizar un seguimiento del proceso de migración y administrarlo.

![Ventana del símbolo del sistema que muestra la salida del comando UserMigration.exe](./media/user-migration/pre-migration-demo.png)

### <a name="step-24-check-the-pre-migration-process"></a>Paso 2.4: Comprobación del proceso de pre-migración

Para validar la migración, use uno de los dos métodos siguientes:

- Para buscar un usuario por nombre para mostrar, use Azure Portal:

   1. Abra **Azure AD B2C** y seleccione **Usuarios**.
   1. En el cuadro de búsqueda, escriba el nombre para mostrar del usuario y vea su perfil.

- Para recuperar un usuario por dirección de correo electrónico de inicio de sesión, use la aplicación de ejemplo:

   1. Ejecute el siguiente comando:

      ```Console
          UserMigration.exe 3 {email address} > UserProfile.json
      ```

      > [!TIP]
      > También puede recuperar un usuario mediante el nombre para mostrar con el comando siguiente: `UserMigration.exe 4 "<Display name>"`.

   1. Abra el archivo UserProfile.json en un editor de JSON para ver la información de usuario.

      ![Archivo UserProfile.json abierto en el editor de Visual Studio Code](./media/user-migration/pre-migration-get-by-email2.png)

### <a name="step-25-optional-environment-cleanup"></a>Paso 2.5: (Opcional) Limpieza del entorno

Si quiere limpiar el inquilino de Azure AD y eliminar los usuarios del directorio de Azure AD, ejecute el comando `UserMigration.exe 5`.

> [!NOTE]
> * Para realizar la limpieza de su inquilino, configure permisos de Administrador de cuenta de usuario para la aplicación.
> * La aplicación de ejemplo de migración limpia todos los usuarios que aparecen en el archivo JSON.

### <a name="step-26-sign-in-with-migrated-users-with-password"></a>Paso 2.6: Inicio de sesión con los usuarios migrados (con contraseña)

Después de ejecutar el proceso de pre-migración con las contraseñas de los usuarios, las cuentas están listas para su uso y los usuarios pueden iniciar sesión en la aplicación con Azure AD B2C. Si no tiene acceso a las contraseñas de los usuarios, continúe con la siguiente sección.

## <a name="step-3-help-users-reset-their-password"></a>Paso 3: Ayudar a los usuarios a restablecer su contraseña

Si migra los usuarios con una contraseña aleatoria, estos deberán restablecerla. Para ayudarles a restablecer la contraseña, envíe un correo electrónico de bienvenida con un vínculo para restablecer la contraseña.

Para obtener el vínculo a la directiva de restablecimiento de contraseña, siga estos pasos. En este procedimiento se supone que ha creado previamente una [directiva personalizada](custom-policy-get-started.md) de restablecimiento de contraseña.

1. Seleccione el directorio que contiene el inquilino de Azure AD B2C mediante el filtro **Directorio + suscripción**, que se encuentra en la sección superior derecha de [Azure Portal](https://portal.azure.com).
1. Seleccione **Azure AD B2C** en el menú de la izquierda (o desde dentro de **Todos los servicios**).
1. En **Directivas**, seleccione **Identity Experience Framework**.
1. Seleccione la directiva de restablecimiento de contraseña. Por ejemplo, *B2C_1A_PasswordReset*.
1. Seleccione la aplicación en la lista desplegable **Seleccionar aplicación.**

    > [!NOTE]
    > **Ejecutar ahora** requiere que al menos una aplicación esté registrada en el inquilino. Para aprender a registrar aplicaciones, consulte [Tutorial: Registro de una aplicación en Azure Active Directory B2C][B2C-AppRegister].

1. Copie la dirección URL que se muestra en el cuadro de texto **Ejecutar punto de conexión ahora** y envíela a los usuarios.

    ![Página Directiva de restablecimiento de contraseña con el cuadro Ejecutar punto de conexión ahora resaltado](./media/user-migration/pre-migration-policy-uri.png)

## <a name="step-4-optional-change-your-policy-to-check-and-set-the-user-migration-status"></a>Paso 4: (Opcional) Cambio de la directiva para comprobar y establecer el estado de migración del usuario

> [!NOTE]
> Para comprobar y cambiar el estado de migración del usuario, debe usar una directiva personalizada. Deben completarse las instrucciones de instalación del artículo [Introducción a las directivas personalizadas][B2C-GetStartedCustom].

Cuando los usuarios intentan iniciar sesión sin restablecer la contraseña primero, la directiva debería devolver un mensaje de error descriptivo. Por ejemplo:

> *La contraseña ha expirado. Para restablecerla, seleccione el vínculo Restablecer contraseña.*

Este paso opcional requiere el uso de las directivas personalizadas de Azure AD B2C, como se describe en el artículo [Introducción a las directivas personalizadas][B2C-GetStartedCustom].

En esta sección, se cambia la directiva para comprobar el estado de migración del usuario en el inicio de sesión. Si el usuario no ha cambiado la contraseña, se devuelve el mensaje de error HTTP 409, que pide al usuario que seleccione el vínculo **¿Ha olvidado la contraseña?** .

Para realizar un seguimiento del cambio de contraseña, se usa una tabla de Azure. Cuando ejecuta el proceso de pre-migración con el parámetro de la línea de comandos `2`, crea la entidad de usuario en una tabla de Azure. Su servicio realiza lo siguiente:

- En el inicio de sesión, la directiva de Azure AD B2C invoca el servicio RESTful de migración y envía un correo electrónico como notificación de entrada. El servicio busca la dirección de correo electrónico en la tabla de Azure. Si existe la dirección, el servicio genera el mensaje de error: *Debe cambiar la contraseña*.

- Después de que el usuario cambie correctamente la contraseña, elimine la entidad de la tabla de Azure.

> [!NOTE]
> Usamos una tabla de Azure para simplificar el ejemplo. Puede almacenar el estado de migración en cualquier base de datos o como una propiedad personalizada en la cuenta de Azure AD B2C.

### <a name="41-update-your-application-setting"></a>4.1: Actualización de la configuración de la aplicación

1. Para probar la demostración de API RESTful, abra `AADB2C.UserMigration.sln` en Visual Studio.
1. En el proyecto `AADB2C.UserMigration.API`, abra el archivo *Web.config*. Reemplace la configuración con las opciones que se configuraron en el [paso 2.2](#step-22-configure-the-application-settings):

    ```json
    {
        "BlobStorageConnectionString": "{The Azure Blob storage connection string}",
        ...
    }
    ```

### <a name="step-42-deploy-your-web-application-to-azure-app-service"></a>Paso 4.2: Implementación de la aplicación web en Azure App Service

En el Explorador de soluciones, haga clic con el botón derecho en `AADB2C.UserMigration.API` y seleccione "Publicar...". Siga las instrucciones para publicarla en Azure App Service. Para obtener más información, consulte [Implementación de la aplicación en Azure App Service][AppService-Deploy].

### <a name="step-43-add-a-technical-profile-and-technical-profile-validation-to-your-policy"></a>Paso 4.3: Adición del perfil técnico y validación del perfil técnico a la directiva

1. En el Explorador de soluciones, expanda "Elementos de la solución" y abra el archivo de directiva *TrustFrameworkExtensions.xml*.
1. Cambie los campos `TenantId`, `PublicPolicyUri` y `<TenantId>` de `yourtenant.onmicrosoft.com` al nombre del inquilino.
1. En el elemento `<TechnicalProfile Id="login-NonInteractive">`, reemplace todas las instancias de `ProxyIdentityExperienceFrameworkAppId` y `IdentityExperienceFrameworkAppId` por los identificadores de aplicación configurados en [Introducción a las directivas personalizadas][B2C-GetStartedCustom].
1. En el nodo `<ClaimsProviders>`, agregue el siguiente fragmento de código XML. Cambie el valor de `ServiceUrl` para que apunte a la dirección URL de Azure App Service.

    ```XML
    <ClaimsProvider>
      <DisplayName>REST APIs</DisplayName>
      <TechnicalProfiles>

        <TechnicalProfile Id="LocalAccountSignIn">
          <DisplayName>Local account just in time migration</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
            <Item Key="ServiceUrl">http://{your-app}.azurewebsites.net/api/PrePasswordReset/LoalAccountSignIn</Item>
            <Item Key="AuthenticationType">None</Item>
            <Item Key="SendClaimsIn">Body</Item>
          </Metadata>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="signInName" PartnerClaimType="email" />
          </InputClaims>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>

        <TechnicalProfile Id="LocalAccountPasswordReset">
          <DisplayName>Local account just in time migration</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
            <Item Key="ServiceUrl">http://{your-app}.azurewebsites.net/api/PrePasswordReset/PasswordUpdated</Item>
            <Item Key="AuthenticationType">None</Item>
            <Item Key="SendClaimsIn">Body</Item>
          </Metadata>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
          </InputClaims>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

El perfil técnico anterior define una notificación de entrada: `signInName` (enviar como correo electrónico). En el inicio de sesión, la notificación se envía al punto de conexión de RESTful.

Después de definir el perfil técnico para la API de RESTful, configure el perfil técnico de `SelfAsserted-LocalAccountSignin-Email` existente para que además llame al perfil técnico de la API de REST. Para ello, reemplácelo por el archivo *TrustFrameworkExtensions.xml*:

```XML
<TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
  <ValidationTechnicalProfiles>
    <ValidationTechnicalProfile ReferenceId="LocalAccountUserMigration" />
  </ValidationTechnicalProfiles>
</TechnicalProfile>
```

A continuación, cambie el `Id` del perfil técnico `LocalAccountSignIn` por `LocalAccountUserMigration`.

### <a name="step-44-upload-the-policy-to-your-tenant"></a>Paso 4.4: Carga de la directiva en el inquilino

1. En [Azure Portal][Portal], cambie al [contexto del inquilino de Azure AD B2C][B2C-NavContext] y seleccione **Azure AD B2C**.
1. Seleccione **Marco de experiencia de identidad**.
1. Seleccione **Todas las directivas**.
1. Seleccione **Cargar directiva**.
1. Active la casilla **Sobrescribir la directiva si existe**.
1. Cargue el archivo *TrustFrameworkExtensions.xml* y asegúrese de que pasa la validación.

### <a name="step-45-test-the-custom-policy-by-using-run-now"></a>Paso 4.5: Probar la directiva personalizada con Ejecutar ahora

1. Seleccione **Azure AD B2C** y, después, **Identity Experience Framework**.
1. Abra *B2C_1A_signup_signin*, que es la directiva personalizada del usuario de confianza (RP) que ha cargado, y seleccione **Ejecutar ahora**.
1. Escriba las credenciales de uno de los usuarios migrados y, después, seleccione **Iniciar de sesión**. La API de REST ahora debería generar el mensaje de error siguiente:

    ![Página de registro de inicio de sesión que muestra el mensaje de error relativo al cambio de contraseña](./media/user-migration/pre-migration-error-message.png)

### <a name="step-46-optional-troubleshoot-your-rest-api"></a>Paso 4.6: (Opcional) Solución de problemas de la API REST

Puede ver y supervisar la información de registro prácticamente en tiempo real.

1. En el menú de configuración de la aplicación RESTful, en **Supervisión**, seleccione **Registros de diagnóstico**.
1. Establezca la opción **Registro de la aplicación (sistema de archivos)** en **Activada**.
1. Establezca el **Nivel** en **Detallado**.
1. Seleccione **Guardar**.

    ![Página de configuración de registros de diagnóstico en Azure Portal](./media/user-migration/pre-migration-diagnostic-logs.png)

1. En el menú **Configuración**, seleccione **Secuencia de registro**.
1. Compruebe la salida de la API de RESTful.

> [!IMPORTANT]
> Use los registros de diagnóstico solo durante el desarrollo y pruebas. La salida de la API de RESTful puede contener información confidencial que no debe exponerse en producción.

## <a name="optional-download-the-complete-policy-files"></a>(Opcional) Descarga de los archivos de directiva completos

Una vez completado el tutorial [Introducción a las directivas personalizadas][B2C-GetStartedCustom], le recomendamos que compile el escenario mediante sus archivos de directiva personalizados. Hemos proporcionado [archivos de directiva de ejemplo][UserMigrationSample-policy] para que los tenga como referencia.

[AD-PasswordPolicies]: https://docs.microsoft.com/azure/active-directory/active-directory-passwords-policy
[AD-Powershell]: https://docs.microsoft.com/powershell/azure/active-directory/install-adv2
[AppService-Deploy]: https://docs.microsoft.com/aspnet/core/tutorials/publish-to-azure-webapp-using-vs
[B2C-AppRegister]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-app-registration
[B2C-GetStarted]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-get-started
[B2C-GetStartedCustom]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-get-started-custom
[B2C-GraphQuickStart]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet
[B2C-NavContext]: https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-navigate-to-b2c-context
[Portal]: https://portal.azure.com/
[UserMigrationSample-code]: https://github.com/azure-ad-b2c/user-migration/tree/master/pre-migration/source-code
[UserMigrationSample-policy]: https://github.com/azure-ad-b2c/user-migration/tree/master/pre-migration/policy
