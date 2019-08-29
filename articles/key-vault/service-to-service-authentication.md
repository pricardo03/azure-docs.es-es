---
title: Autenticación entre servicios en Azure Key Vault mediante .NET
description: Utilice la biblioteca Microsoft.Azure.Services.AppAuthentication para autenticar en Azure Key Vault mediante .NET.
keywords: credenciales locales de autenticación de azure key-vault
author: msmbaldwin
manager: barbkess
services: key-vault
ms.author: mbaldwin
ms.date: 07/06/2019
ms.topic: conceptual
ms.service: key-vault
ms.openlocfilehash: 30c99ae4150e0bd4645488b5bf75b8bbac0ee66f
ms.sourcegitcommit: 39d95a11d5937364ca0b01d8ba099752c4128827
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2019
ms.locfileid: "69562442"
---
# <a name="service-to-service-authentication-to-azure-key-vault-using-net"></a>Autenticación entre servicios en Azure Key Vault mediante .NET

Para autenticarse en Azure Key Vault, necesitará una credencial de Azure Active Directory (AD), un secreto compartido o un certificado. 

La administración de estas credenciales puede ser complicada y resulta tentador agrupar las credenciales en una aplicación incluyéndolas en archivos de configuración o de origen.  La biblioteca `Microsoft.Azure.Services.AppAuthentication` para .NET simplifica este problema. Utiliza las credenciales del desarrollador para la autenticación durante el desarrollo local. Cuando la solución se implementa más adelante en Azure, la biblioteca cambia automáticamente a las credenciales de la aplicación.    El uso de credenciales de desarrollador durante el desarrollo local es más seguro porque no es necesario crear credenciales de Azure AD o compartir credenciales entre los programadores.

La biblioteca `Microsoft.Azure.Services.AppAuthentication` administra la autenticación automáticamente, que a su vez le permite centrarse en la solución, en lugar de en las credenciales.  Admite el desarrollo local con Microsoft Visual Studio, la CLI de Azure o la autenticación integrada de Azure AD. Cuando se implementa en un recurso de Azure que admite una identidad administrada, la biblioteca usa automáticamente [identidades administradas para los recursos de Azure](../active-directory/msi-overview.md). No se requieren cambios de configuración o código. La biblioteca también admite el uso directo de las [credenciales de cliente](../azure-resource-manager/resource-group-authenticate-service-principal.md) de Azure AD cuando una identidad administrada no está disponible o cuando no se puede determinar el contexto de seguridad del desarrollador durante el desarrollo local.

## <a name="using-the-library"></a>Uso de la biblioteca

Para aplicaciones. NET, la manera más sencilla de trabajar con una identidad administrada es con el paquete `Microsoft.Azure.Services.AppAuthentication`. Aquí tiene información sobre cómo empezar:

1. Agregue referencias a los paquetes de NuGet [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) y [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) para la aplicación. 

2. Agregue el siguiente código:

    ``` csharp
    using Microsoft.Azure.Services.AppAuthentication;
    using Microsoft.Azure.KeyVault;

    // Instantiate a new KeyVaultClient object, with an access token to Key Vault
    var azureServiceTokenProvider1 = new AzureServiceTokenProvider();
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider1.KeyVaultTokenCallback));

    // Optional: Request an access token to other Azure services
    var azureServiceTokenProvider2 = new AzureServiceTokenProvider();
    string accessToken = await azureServiceTokenProvider2.GetAccessTokenAsync("https://management.azure.com/").ConfigureAwait(false);
    ```

La `AzureServiceTokenProvider` clase almacena el token en la memoria y lo recupera de Azure AD justo antes de que expire. Por lo tanto, ya no tiene que comprobar la expiración antes de llamar al método `GetAccessTokenAsync`. Simplemente llame al método cuando desee utilizar el token. 

El método `GetAccessTokenAsync` requiere un identificador de recursos. Para más información, vea [qué servicios de Azure admiten identidades administradas para recursos de Azure](../active-directory/msi-overview.md).

## <a name="local-development-authentication"></a>Autenticación de desarrollo local

Para el desarrollo local, hay dos escenarios de autenticación principales: la [autenticación en los servicios de Azure](#authenticating-to-azure-services) y la [autenticación en los servicios personalizados](#authenticating-to-custom-services).

### <a name="authenticating-to-azure-services"></a>Autenticación en los servicios de Azure

Los equipos locales no admiten identidades administradas para recursos de Azure.  Como resultado, la biblioteca `Microsoft.Azure.Services.AppAuthentication` utiliza las credenciales de desarrollador para la ejecución en el entorno de desarrollo local. Cuando la solución se implementa en Azure, la biblioteca usa una identidad administrada para cambiar a un flujo de concesión de credenciales de cliente de OAuth 2.0.  Esto significa que puede probar el mismo código local y remotamente sin preocuparse.

Para el desarrollo local, `AzureServiceTokenProvider` captura tokens mediante **Visual Studio**, la **interfaz de línea de comandos de Azure** (CLI) o **la autenticación integrada de Azure AD**. Cada opción se prueba secuencialmente y la biblioteca usa la primera opción correcta. Si no funciona ninguna opción, se produce una excepción `AzureServiceTokenProviderException` con información detallada.

### <a name="authenticating-with-visual-studio"></a>Autenticación con Visual Studio

La autenticación con Visual Studio tiene los siguientes requisitos previos:

1. [Visual Studio 2017 versión 15.5](https://blogs.msdn.microsoft.com/visualstudio/2017/10/11/visual-studio-2017-version-15-5-preview/) o versiones posteriores.

2. La [extensión de autenticación de la aplicación para Visual Studio](https://go.microsoft.com/fwlink/?linkid=862354), disponible como una extensión independiente para Visual Studio 2017 Update 5 e incluida con el producto en Update 6 y versiones posteriores. Con la Update 6 o posterior, puede comprobar la instalación de la extensión de autenticación de la aplicación si selecciona las herramientas de desarrollo de Azure en el instalador de Visual Studio.
 
Inicie sesión en Visual Studio y utilice **Herramientas**&nbsp;>&nbsp;**Opciones**&nbsp;>&nbsp;**Azure Service Authentication** (autenticación del servicio de Azure) para seleccionar una cuenta para desarrollo local. 

Si experimenta problemas con Visual Studio, como errores relacionados con el archivo del proveedor de tokens, revise atentamente estos pasos. 

Puede que sea necesario volver a autenticar el token del desarrollador. Para ello, vaya a **Herramientas**&nbsp;>&nbsp;**Opciones**>**Azure&nbsp;Service&nbsp;Authentication** (Autenticación de servicio de Azure) y busque un vínculo de **reautenticación** debajo de la cuenta seleccionada.  Selecciónelo para autenticar. 

### <a name="authenticating-with-azure-cli"></a>Autenticación con la CLI de Azure

Para utilizar la CLI de Azure para el desarrollo local:

1. Instale la [CLI de Azure v2.0.12](/cli/azure/install-azure-cli) o posterior. Actualice las versiones anteriores. 

2. Utilice **az login** para iniciar sesión en Azure.

Utilice `az account get-access-token` para comprobar el acceso.  Si recibe un error, compruebe que el paso 1 se haya completado correctamente. 

Si la CLI de Azure no está instalada en el directorio predeterminado, recibirá un error que le notificará que `AzureServiceTokenProvider` no encuentra la ruta de acceso para la CLI de Azure.  Use la variable de entorno **AzureCLIPath** para definir la carpeta de instalación de la CLI de Azure. `AzureServiceTokenProvider` agrega el directorio especificado en la variable de entorno **AzureCLIPath** a la variable de entorno **Path** cuando sea necesario.

Si ha iniciado la sesión en la CLI de Azure con varias cuentas o la cuenta tiene acceso a varias suscripciones, debe especificar la suscripción específica que se va a usar.  Para ello, utilice lo siguiente:

```
az account set --subscription <subscription-id>
```

Este comando genera un resultado solo en caso de error.  Para comprobar la configuración de la cuenta actual, use:

```
az account list
```

### <a name="authenticating-with-azure-ad-authentication"></a>Autenticación con la autenticación de Azure AD

Para usar la autenticación de Azure AD, compruebe lo siguiente:

- El directorio activo local se [sincroniza con Azure AD](../active-directory/connect/active-directory-aadconnect.md).

- El código se ejecuta en una máquina unidad a un dominio.


### <a name="authenticating-to-custom-services"></a>Autenticación en los servicios personalizados

Cuando un servicio llama a los servicios de Azure, los pasos anteriores funcionan porque los servicios de Azure permiten el acceso a los usuarios y a las aplicaciones.  

Al crear un servicio que llama a un servicio personalizado, use las credenciales de cliente de Azure AD para la autenticación de desarrollo local.  Hay dos opciones: 

1.  Usar a una entidad de servicio para iniciar sesión en Azure:

    1.  [Cree una entidad de servicio](/cli/azure/create-an-azure-service-principal-azure-cli).

    2.  Use la CLI de Azure para iniciar sesión:

        ```azurecli
        az login --service-principal -u <principal-id> --password <password> --tenant <tenant-id> --allow-no-subscriptions
        ```

        Puesto que la entidad de servicio puede no tener acceso a la suscripción, use el argumento `--allow-no-subscriptions`.

2.  Usar variables de entorno para especificar los detalles de la entidad de servicio.  Para más información, vea [Ejecución de la aplicación con una entidad de servicio](#running-the-application-using-a-service-principal).

Una vez que haya iniciado sesión en Azure, `AzureServiceTokenProvider` usa la entidad de servicio para recuperar un token para el desarrollo local.

Esto se aplica solo a desarrollo local. Cuando la solución se implementa en Azure, la biblioteca cambia a una identidad administrada para la autenticación.

## <a name="running-the-application-using-managed-identity-or-user-assigned-identity"></a>Ejecución de la aplicación con una identidad administrada o una identidad asignada por el usuario 

Cuando se ejecuta el código en una instancia de Azure App Service o en una máquina virtual de Azure con una identidad administrada habilitada, la biblioteca usa automáticamente dicha identidad. No es necesario realizar cambios en el código, pero la identidad administrada debe tener permisos de *obtención* para el almacén de claves. Puede conceder permisos de *obtención* a la identidad administrada mediante las *directivas de acceso* del almacén de claves.

También puede autenticarse con una identidad asignada por el usuario. Para obtener más información sobre las identidades asignadas por el usuario, consulte el artículo sobre [las identidades administradas para recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md#how-does-the-managed-identities-for-azure-resources-work). Para autenticarse con una identidad asignada por el usuario, debe especificar el identificador de cliente de la identidad asignada por el usuario en la cadena de conexión. La cadena de conexión se especifica en la sección [Compatibilidad con la cadena de conexión](#connection-string-support) más adelante.

## <a name="running-the-application-using-a-service-principal"></a>Ejecución de la aplicación con una entidad de servicio 

Puede ser necesario crear una credencial de cliente de Azure AD para autenticar. Algunos ejemplos frecuentes son:

- El código se ejecuta en un entorno de desarrollo local, pero no en la identidad del desarrollador.  Service Fabric, por ejemplo, usa la [cuenta de NetworkService](../service-fabric/service-fabric-application-secret-management.md) para el desarrollo local.
 
- El código se ejecuta en un entorno de desarrollo local y se autentica en un servicio personalizado, por lo que no puede usar la identidad de desarrollador. 
 
- Se ejecuta el código en un recurso de Azure Compute que aún no admite identidades administradas para recursos de Azure, como Azure Batch.

Hay tres métodos principales para usar una entidad de servicio para ejecutar la aplicación. Para usar cualquiera de ellos, primero debe [crear una entidad de servicio](/cli/azure/create-an-azure-service-principal-azure-cli).

### <a name="use-a-certificate-in-local-keystore-to-sign-into-azure-ad"></a>Uso de un certificado en el almacén de claves local para iniciar sesión en Azure AD

1. Cree un certificado de entidad de servicio mediante el comando [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) de la CLI de Azure. 

    ```azurecli
    az ad sp create-for-rbac --create-cert
    ```

    De esta forma, se creará un archivo .pem (clave privada) que se almacenará en el directorio particular. Implemente este certificado en los almacenes *LocalMachine* o *CurrentUser*. 

    > [!Important]
    > El comando de la CLI genera un archivo .pem, pero Windows solo proporciona compatibilidad nativa con los certificados PFX. Para generar un certificado PFX en su lugar, use los comandos de PowerShell que se muestran aquí: [Creación de una entidad de servicio con un certificado autofirmado](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-self-signed-certificate). Estos comandos también implementan el certificado automáticamente.

1. Establezca una variable de entorno denominada **AzureServicesAuthConnectionString** para:

    ```
    RunAs=App;AppId={AppId};TenantId={TenantId};CertificateThumbprint={Thumbprint};
          CertificateStoreLocation={CertificateStore}
    ```
 
    Reemplace *{AppId}* , *{TenantId}* y *{Thumbprint}* por los valores generados en el paso 1. Reemplace *{CertificateStore}* por `LocalMachine` o `CurrentUser`, según su plan de implementación.

1. Ejecute la aplicación. 

### <a name="use-a-shared-secret-credential-to-sign-into-azure-ad"></a>Uso de una credencial compartida secreta para iniciar sesión en Azure AD

1. Cree un certificado de entidad de servicio con una contraseña mediante el comando [az ad sp create-for-rbac --password](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac). 

2. Establezca una variable de entorno denominada **AzureServicesAuthConnectionString** para:

    ```
    RunAs=App;AppId={AppId};TenantId={TenantId};AppKey={ClientSecret} 
    ```

    Reemplace _{AppId}_ , _{TenantId}_ y _{ClientSecret}_ por los valores generados en el paso 1.

3. Ejecute la aplicación. 

Una vez que todo está configurado correctamente, no es necesario realizar más cambios de código.  `AzureServiceTokenProvider` usa la variable de entorno y el certificado para autenticar en Azure AD. 

### <a name="use-a-certificate-in-key-vault-to-sign-into-azure-ad"></a>Uso de un certificado en Key Vault para iniciar sesión en Azure AD

Esta opción le permite almacenar el certificado de cliente de una entidad de servicio en Key Vault y usarlo para la autenticación de la entidad de servicio. Puede usarlo en los escenarios siguientes:

* Autenticación local, en la que quiere autenticarse mediante una entidad de servicio explícita y quiere guardar la credencial de la entidad de servicio de forma segura en un almacén de claves. La cuenta de desarrollador debe tener acceso al almacén de claves. 
* Autenticación desde Azure, donde quiere usar la credencial explícita (por ejemplo, para escenarios entre inquilinos) y quiere guardar la credencial de la entidad de servicio de forma segura en un almacén de claves. La identidad administrada debe tener acceso al almacén de claves. 

La identidad administrada o la identidad del desarrollador deben tener permiso para recuperar el certificado de cliente de Key Vault. La biblioteca AppAuthentication usa el certificado recuperado como credencial de cliente de la entidad de servicio.

Para usar un certificado de cliente para la autenticación de la entidad de servicio

1. Cree un certificado de entidad de servicio y almacénelo automáticamente en el almacén de claves mediante el comando [az ad sp create-for-rbac --keyvault <keyvaultname> --cert <certificatename> --create-cert --skip-assignment](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) de la CLI de Azure:

    ```azurecli
    az ad sp create-for-rbac --keyvault <keyvaultname> --cert <certificatename> --create-cert --skip-assignment
    ```
    
    El identificador del certificado será una URL con el formato `https://<keyvaultname>.vault.azure.net/secrets/<certificatename>`

1. Reemplace `{KeyVaultCertificateSecretIdentifier}` en esta cadena de conexión por el identificador del certificado:

    ```
    RunAs=App;AppId={TestAppId};KeyVaultCertificateSecretIdentifier={KeyVaultCertificateSecretIdentifier}
    ```

    Si, por ejemplo, el almacén de claves se llamaba "myKeyVault" y ha creado un certificado denominado "MyCert", el identificador del certificado sería:

    ```
    RunAs=App;AppId={TestAppId};KeyVaultCertificateSecretIdentifier=https://myKeyVault.vault.azure.net/secrets/myCert
    ```


## <a name="connection-string-support"></a>Compatibilidad de la cadena de conexión

De forma predeterminada, `AzureServiceTokenProvider` utiliza varios métodos para recuperar un token. 

Para controlar el proceso, utilice una cadena de conexión pasada al constructor `AzureServiceTokenProvider` o especificada en la variable de entorno *AzureServicesAuthConnectionString*. 

Se admiten las siguientes opciones:

| Opción de la cadena de conexión | Escenario | Comentarios|
|:--------------------------------|:------------------------|:----------------------------|
| `RunAs=Developer; DeveloperTool=AzureCli` | Desarrollo local | AzureServiceTokenProvider utiliza AzureCli para obtener el token. |
| `RunAs=Developer; DeveloperTool=VisualStudio` | Desarrollo local | AzureServiceTokenProvider utiliza Visual Studio para obtener el token. |
| `RunAs=CurrentUser` | Desarrollo local | AzureServiceTokenProvider utiliza Azure AD Integrated Authentication (Autenticación integrada de Azure AD) para obtener el token. |
| `RunAs=App` | [Identidades administradas para recursos de Azure](../active-directory/managed-identities-azure-resources/index.yml) | AzureServiceTokenProvider utiliza una identidad administrada para obtener el token. |
| `RunAs=App;AppId={ClientId of user-assigned identity}` | [Identidad asignada por el usuario para recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md#how-does-the-managed-identities-for-azure-resources-work) | AzureServiceTokenProvider utiliza una identidad asignada por el usuario para obtener el token. |
| `RunAs=App;AppId={TestAppId};KeyVaultCertificateSecretIdentifier={KeyVaultCertificateSecretIdentifier}` | Autenticación en servicios personalizados | KeyVaultCertificateSecretIdentifier = identificador secreto del certificado. |
| `RunAs=App;AppId={AppId};TenantId={TenantId};CertificateThumbprint={Thumbprint};CertificateStoreLocation={LocalMachine or CurrentUser}`| Entidad de servicio | `AzureServiceTokenProvider` usa el certificado para obtener el token desde Azure AD. |
| `RunAs=App;AppId={AppId};TenantId={TenantId};CertificateSubjectName={Subject};CertificateStoreLocation={LocalMachine or CurrentUser}` | Entidad de servicio | `AzureServiceTokenProvider` usa el certificado para obtener el token desde Azure AD.|
| `RunAs=App;AppId={AppId};TenantId={TenantId};AppKey={ClientSecret}` | Entidad de servicio |`AzureServiceTokenProvider` usa el secreto para obtener el token desde Azure AD. |

## <a name="samples"></a>Ejemplos

Para ver la biblioteca `Microsoft.Azure.Services.AppAuthentication` en acción, consulte los siguientes ejemplos de código.

1. [Use una identidad administrada para recuperar un secreto de almacén de Azure Key Vault en tiempo de ejecución](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet).

2. [Implemente mediante programación una plantilla de Azure Resource Manager desde una máquina virtual de Azure con una identidad administrada](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet).

3. [Use el ejemplo .NET Core y una identidad administrada para llamar a los servicios de Azure desde una máquina virtual Linux de Azure](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/).

## <a name="appauthentication-troubleshooting"></a>Solución de problemas de AppAuthentication

### <a name="common-issues-during-local-development"></a>Problemas comunes durante el desarrollo local

#### <a name="azure-cli-is-not-installed-you-are-not-logged-in-or-you-do-not-have-the-latest-version"></a>La CLI de Azure no está instalada, no se ha iniciado sesión o no se cuenta con la versión más reciente

Ejecute **az account get-access-token** para ver si la CLI de Azure muestra un token automáticamente. Si indica que no se encontró dicho programa, instale la [versión más reciente de la CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest). Si lo ha instalado, es posible que se le pida que inicie sesión. 
 
#### <a name="azureservicetokenprovider-cannot-find-the-path-for-azure-cli"></a>AzureServiceTokenProvider no puede encontrar la ruta de acceso de la CLI de Azure

AzureServiceTokenProvider busca la CLI de Azure en sus ubicaciones de instalación predeterminadas. Si no se puede encontrar la CLI de Azure, establezca la variable de entorno **AzureCLIPath** en la carpeta de instalación de la CLI de Azure. AzureServiceTokenProvider agregará la variable de entorno a la variable de entorno Path.
 
#### <a name="you-are-logged-into-azure-cli-using-multiple-accounts-the-same-account-has-access-to-subscriptions-in-multiple-tenants-or-you-get-an-access-denied-error-when-trying-to-make-calls-during-local-development"></a>Ha iniciado sesión en la CLI de Azure con varias cuentas, la misma cuenta tiene acceso a las suscripciones de varios inquilinos, o se recibe un error de acceso denegado al intentar realizar llamadas durante el desarrollo local.

Con la CLI de Azure, establezca la suscripción predeterminada en una que tenga la cuenta que desea usar y que se encuentra en el mismo inquilino que el recurso al que desea obtener acceso: **az account set --subscription [subscription-id]** . Si no se muestra ninguna salida, significa que se ha realizado correctamente. Compruebe que la cuenta correcta es ahora el valor predeterminado mediante **az account list**.

### <a name="common-issues-across-environments"></a>Problemas comunes en los entornos

#### <a name="unauthorized-access-access-denied-forbidden-etc-error"></a>Acceso no autorizado, acceso denegado, prohibido, etc.
 
La entidad de seguridad utilizada no tiene acceso al recurso al que está intentando acceder. Conceda a su cuenta de usuario el acceso de "colaborador" de MSI de App Service al recurso deseado, en función de si está ejecutando el ejemplo en el equipo de desarrollo local o se ha implementado en Azure en la instancia de App Service. Algunos recursos, como los almacenes de claves, también tienen sus propias [directivas de acceso](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault#data-plane-and-access-policies) que se usan para conceder acceso a las entidades de seguridad (usuarios, aplicaciones, grupos, etc.).

### <a name="common-issues-when-deployed-to-azure-app-service"></a>Problemas comunes al realizar la implementación en Azure App Service

#### <a name="managed-identity-is-not-setup-on-the-app-service"></a>La identidad administrada no se configura en App Service.
 
Compruebe si las variables de entorno MSI_ENDPOINT y MSI_SECRET existen mediante [Kudu debug console](https://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/). Si estas variables de entorno no existen, la identidad administrada no está habilitada en App Service. 
 
### <a name="common-issues-when-deployed-locally-with-iis"></a>Problemas comunes al realizar la implementación localmente con IIS

#### <a name="cant-retrieve-tokens-when-debugging-app-in-iis"></a>No se pueden recuperar los tokens al depurar la aplicación en IIS.

De forma predeterminada, AppAuth se ejecuta en un contexto de usuario diferente de IIS y, por lo tanto, no tiene acceso para usar su identidad de desarrollador para recuperar los tokens de acceso. Puede configurar IIS para que se ejecute con su contexto de usuario con los dos pasos siguientes:
- Configure el grupo de aplicaciones para que la aplicación web se ejecute como su cuenta de usuario actual. Puede obtener más información [aquí](https://docs.microsoft.com/iis/manage/configuring-security/application-pool-identities#configuring-iis-application-pool-identities)
- Configure "setProfileEnvironment" en "True". Puede obtener más información [aquí](https://docs.microsoft.com/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration). 

    - Vaya a %windir%\System32\inetsrv\config\applicationHost.config.
    - Busque "setProfileEnvironment". Si está establecido en "False", cámbielo a "True". Si no está presente, agréguelo como atributo al elemento processModel (/configuration/system.applicationHost/applicationPools/applicationPoolDefaults/processModel/@setProfileEnvironment) y establézcalo en "True".

- Obtenga más información sobre las [identidades administradas para recursos de Azure](../active-directory/managed-identities-azure-resources/index.yml).
- Más información sobre los [escenarios de autenticación de Azure AD](../active-directory/develop/active-directory-authentication-scenarios.md).
