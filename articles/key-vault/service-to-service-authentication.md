---
title: Autenticación entre servicios en Azure Key Vault mediante .NET
description: Utilice la biblioteca Microsoft.Azure.Services.AppAuthentication para autenticar en Azure Key Vault mediante .NET.
keywords: credenciales locales de autenticación de azure key-vault
author: msmbaldwin
manager: barbkess
services: key-vault
ms.author: mbaldwin
ms.date: 03/05/2019
ms.topic: conceptual
ms.service: key-vault
ms.assetid: 4be434c4-0c99-4800-b775-c9713c973ee9
ms.openlocfilehash: 642c30c4df233476a8f649f7b5f30d0538b0e83f
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58629849"
---
# <a name="service-to-service-authentication-to-azure-key-vault-using-net"></a>Autenticación entre servicios en Azure Key Vault mediante .NET

Para autenticar en Azure Key Vault, necesitará una credencial de Azure Active Directory (AD), un secreto compartido o un certificado. La administración de estas credenciales puede ser complicada y resulta tentador agrupar las credenciales en una aplicación incluyéndolas en archivos de configuración o de origen.

La biblioteca `Microsoft.Azure.Services.AppAuthentication` para .NET simplifica este problema. Utiliza las credenciales del desarrollador para la autenticación durante el desarrollo local. Cuando la solución se implementa más adelante en Azure, la biblioteca cambia automáticamente a las credenciales de la aplicación.  

El uso de credenciales de desarrollador durante el desarrollo local es más seguro porque no es necesario crear credenciales de Azure AD o compartir credenciales entre los programadores.

La biblioteca `Microsoft.Azure.Services.AppAuthentication` administra la autenticación automáticamente, que a su vez le permite centrarse en la solución, en lugar de en las credenciales.

La biblioteca `Microsoft.Azure.Services.AppAuthentication` admite el desarrollo local con Microsoft Visual Studio, la CLI de Azure o Azure AD Integrated Authentication (Autenticación integrada de Azure AD). Cuando se implementa en un recurso de Azure que admite una identidad administrada, la biblioteca usa automáticamente [identidades administradas para los recursos de Azure](../active-directory/msi-overview.md). No se requieren cambios de configuración o código. La biblioteca también admite el uso directo de las [credenciales de cliente](../azure-resource-manager/resource-group-authenticate-service-principal.md) de Azure AD cuando una identidad administrada no está disponible o cuando no se puede determinar el contexto de seguridad del desarrollador durante el desarrollo local.

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

## <a name="samples"></a>Ejemplos

Los siguientes ejemplos muestran la biblioteca `Microsoft.Azure.Services.AppAuthentication` en acción:

1. [Use una identidad administrada para recuperar un secreto de almacén de Azure Key Vault en tiempo de ejecución](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet).

2. [Implemente mediante programación una plantilla de Azure Resource Manager desde una máquina virtual de Azure con una identidad administrada](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet).

3. [Use el ejemplo .NET Core y una identidad administrada para llamar a los servicios de Azure desde una máquina virtual Linux de Azure](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/).

## <a name="local-development-authentication"></a>Autenticación de desarrollo local

Para el desarrollo local, hay dos escenarios de autenticación principales:

- [Autenticación en los servicios de Azure](#authenticating-to-azure-services)
- [Autenticación en los servicios personalizados](#authenticating-to-custom-services)

### <a name="authenticating-to-azure-services"></a>Autenticación en los servicios de Azure

Los equipos locales no admiten identidades administradas para recursos de Azure.  Como resultado, la biblioteca `Microsoft.Azure.Services.AppAuthentication` utiliza las credenciales de desarrollador para la ejecución en el entorno de desarrollo local. Cuando la solución se implementa en Azure, la biblioteca usa una identidad administrada para cambiar a un flujo de concesión de credenciales de cliente de OAuth 2.0.  Esto significa que puede probar el mismo código local y remotamente sin preocuparse.

Para el desarrollo local, `AzureServiceTokenProvider` captura tokens mediante **Visual Studio**, la **interfaz de línea de comandos de Azure** (CLI) o **la autenticación integrada de Azure AD**. Cada opción se prueba secuencialmente y la biblioteca usa la primera opción correcta. Si no funciona ninguna opción, se produce una excepción `AzureServiceTokenProviderException` con información detallada.

### <a name="authenticating-with-visual-studio"></a>Autenticación con Visual Studio

Autenticación con Visual Studio tiene los siguientes requisitos previos:

1. [Visual Studio 2017 v15.5](https://blogs.msdn.microsoft.com/visualstudio/2017/10/11/visual-studio-2017-version-15-5-preview/) o una versión posterior.

2. El [extensión de autenticación de la aplicación para Visual Studio](https://go.microsoft.com/fwlink/?linkid=862354), disponible como una extensión independiente para Visual Studio 2017 Update 5 y empaquetado con el producto en la actualización 6 y versiones posteriores. Con la actualización 6 o posterior, puede comprobar la instalación de la extensión de autenticación de la aplicación mediante la selección de las herramientas de desarrollo de Azure en el instalador de Visual Studio.
 
Inicie sesión en Visual Studio y utilizar **herramientas**&nbsp;>&nbsp;**opciones**&nbsp;>&nbsp;**servicio de Azure Autenticación** para seleccionar una cuenta para el desarrollo local. 

Si experimenta problemas con Visual Studio, como errores relacionados con el archivo del proveedor de tokens, revise atentamente estos pasos. 

Puede que sea necesario volver a autenticar el token del desarrollador. Para ello, vaya a **Herramientas**&nbsp;>&nbsp;**Opciones**>**Azure&nbsp;Service&nbsp;Authentication** (Autenticación de servicio de Azure) y busque un vínculo de **reautenticación** debajo de la cuenta seleccionada.  Selecciónelo para autenticar. 

### <a name="authenticating-with-azure-cli"></a>Autenticación con la CLI de Azure

Para utilizar la CLI de Azure para el desarrollo local:

1. Instale la [CLI de Azure v2.0.12](/cli/azure/install-azure-cli) o posterior. Actualice las versiones anteriores. 

2. Utilice **az login** para iniciar sesión en Azure.

Utilice `az account get-access-token` para comprobar el acceso.  Si recibe un error, compruebe que el paso 1 se haya completado correctamente. 

Si la CLI de Azure no está instalada en el directorio predeterminado, recibirá un error que le notificará que `AzureServiceTokenProvider` no encuentra la ruta de acceso para la CLI de Azure.  Use la **AzureCLIPath** variable de entorno para definir la carpeta de instalación de la CLI de Azure. `AzureServiceTokenProvider` agrega el directorio especificado en la variable de entorno **AzureCLIPath** a la variable de entorno **Path** cuando sea necesario.

Si ha iniciado la sesión en la CLI de Azure con varias cuentas o la cuenta tiene acceso a varias suscripciones, debe especificar la suscripción específica que se va a usar.  Para ello, utilice lo siguiente:

```
az account set --subscription <subscription-id>
```

Este comando genera un resultado solo en caso de error.  Para comprobar la configuración de la cuenta actual, use:

```
az account list
```

### <a name="authenticating-with-azure-ad-integrate-authentication"></a>Autenticación con Azure AD Integrated Authentication (Autenticación integrada de Azure AD)

Para usar la autenticación de Azure AD, compruebe lo siguiente:

- El directorio activo local se [sincroniza con Azure AD](../active-directory/connect/active-directory-aadconnect.md).

- El código se ejecuta en una máquina unidad a un dominio.


### <a name="authenticating-to-custom-services"></a>Autenticación en los servicios personalizados

Cuando un servicio llama a los servicios de Azure, los pasos anteriores funcionan porque los servicios de Azure permiten el acceso a los usuarios y a las aplicaciones.  

Al crear un servicio que llama a un servicio personalizado, use las credenciales de cliente de Azure AD para la autenticación de desarrollo local.  Hay dos opciones: 

1.  Usar a una entidad de servicio para iniciar sesión en Azure:

    1.  [Cree una entidad de servicio](/cli/azure/create-an-azure-service-principal-azure-cli).

    2.  Use la CLI de Azure para iniciar sesión:

        ```
        az login --service-principal -u <principal-id> --password <password>
           --tenant <tenant-id> --allow-no-subscriptions
        ```

        Puesto que la entidad de servicio puede no tener acceso a la suscripción, use el argumento `--allow-no-subscriptions`.

2.  Usar variables de entorno para especificar los detalles de la entidad de servicio.  Para más información, vea [Ejecución de la aplicación con una entidad de servicio](#running-the-application-using-a-service-principal).

Una vez que haya iniciado sesión en Azure, `AzureServiceTokenProvider` usa la entidad de servicio para recuperar un token para el desarrollo local.

Esto se aplica solo a desarrollo local. Cuando la solución se implementa en Azure, la biblioteca cambia a una identidad administrada para la autenticación.

## <a name="running-the-application-using-managed-identity-or-user-assigned-identity"></a>Ejecutar la aplicación usando la identidad administrada o la identidad asignada por el usuario 

Cuando se ejecuta el código en una instancia de Azure App Service o en una máquina virtual de Azure con una identidad administrada habilitada, la biblioteca usa automáticamente dicha identidad. No se requiere ningún cambio de código. 

Como alternativa, puede autenticar con una identidad asignada por el usuario. Para obtener más información sobre las identidades asignadas por el usuario, consulte [sobre identidades administradas para los recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md#how-does-the-managed-identities-for-azure-resources-work). La cadena de conexión se especifica en el [compatibilidad con la cadena de conexión](#connection-string-support) sección más adelante.

## <a name="running-the-application-using-a-service-principal"></a>Ejecución de la aplicación con una entidad de servicio 

Puede ser necesario crear una credencial de cliente de Azure AD para autenticar. Algunos ejemplos frecuentes son:

1. El código se ejecuta en un entorno de desarrollo local, pero no en la identidad del desarrollador.  Service Fabric, por ejemplo, usa la [cuenta de NetworkService](../service-fabric/service-fabric-application-secret-management.md) para el desarrollo local.
 
2. El código se ejecuta en un entorno de desarrollo local y se autentica en un servicio personalizado, por lo que no puede usar la identidad de desarrollador. 
 
3. Se ejecuta el código en un recurso de Azure Compute que aún no admite identidades administradas para recursos de Azure, como Azure Batch.

Para usar un certificado para iniciar sesión en Azure AD:

1. Cree un [certificado de entidad de servicio](../azure-resource-manager/resource-group-authenticate-service-principal.md). 

2. Implemente el certificado en los almacenes *LocalMachine* o *CurrentUser*. 

3. Establezca una variable de entorno denominada **AzureServicesAuthConnectionString** para:

    ```
    RunAs=App;AppId={AppId};TenantId={TenantId};CertificateThumbprint={Thumbprint};
          CertificateStoreLocation={CertificateStore}
    ```
 
    Reemplace *{AppId}*, *{TenantId}* y *{Thumbprint}* por los valores generados en el paso 1. Reemplace *{CertificateStore}* por `LocalMachine` o `CurrentUser`, según su plan de implementación.

4. Ejecute la aplicación. 

Para iniciar sesión con la credencial de secreto compartido de Azure AD:

1. Crear una [entidad de servicio con una contraseña](../azure-resource-manager/resource-group-authenticate-service-principal.md) y concédale acceso a Key Vault. 

2. Establezca una variable de entorno denominada **AzureServicesAuthConnectionString** para:

    ```
    RunAs=App;AppId={AppId};TenantId={TenantId};AppKey={ClientSecret} 
    ```

    Reemplace _{AppId}_, _{TenantId}_ y _{ClientSecret}_ por los valores generados en el paso 1.

3. Ejecute la aplicación. 

Una vez que todo está configurado correctamente, no es necesario realizar más cambios de código.  `AzureServiceTokenProvider` usa la variable de entorno y el certificado para autenticar en Azure AD. 

## <a name="connection-string-support"></a>Compatibilidad de la cadena de conexión

De forma predeterminada, `AzureServiceTokenProvider` utiliza varios métodos para recuperar un token. 

Para controlar el proceso, utilice una cadena de conexión pasada al constructor `AzureServiceTokenProvider` o especificada en la variable de entorno *AzureServicesAuthConnectionString*. 

Se admiten las siguientes opciones:

| Opción de&nbsp;cadena&nbsp;de conexión | Escenario | Comentarios|
|:--------------------------------|:------------------------|:----------------------------|
| `RunAs=Developer; DeveloperTool=AzureCli` | Desarrollo local | AzureServiceTokenProvider utiliza AzureCli para obtener el token. |
| `RunAs=Developer; DeveloperTool=VisualStudio` | Desarrollo local | AzureServiceTokenProvider utiliza Visual Studio para obtener el token. |
| `RunAs=CurrentUser` | Desarrollo local | AzureServiceTokenProvider utiliza Azure AD Integrated Authentication (Autenticación integrada de Azure AD) para obtener el token. |
| `RunAs=App` | [Identidades administradas para recursos de Azure](../active-directory/managed-identities-azure-resources/index.yml) | AzureServiceTokenProvider utiliza una identidad administrada para obtener el token. |
| `RunAs=App;AppId={ClientId of user-assigned identity}` | [Identidad asignada por el usuario para los recursos de Azure](../active-directory/managed-identities-azure-resources/overview.md#how-does-the-managed-identities-for-azure-resources-work) | AzureServiceTokenProvider utiliza una identidad asignada por el usuario para obtener el token. |
| `RunAs=App;AppId={AppId};TenantId={TenantId};CertificateThumbprint={Thumbprint};CertificateStoreLocation={LocalMachine or CurrentUser}`   | Entidad de servicio | `AzureServiceTokenProvider` usa el certificado para obtener el token desde Azure AD. |
| `RunAs=App;AppId={AppId};TenantId={TenantId};CertificateSubjectName={Subject};CertificateStoreLocation={LocalMachine or CurrentUser}` | Entidad de servicio | `AzureServiceTokenProvider` usa el certificado para obtener el token desde Azure AD.|
| `RunAs=App;AppId={AppId};TenantId={TenantId};AppKey={ClientSecret}` | Entidad de servicio |`AzureServiceTokenProvider` usa el secreto para obtener el token desde Azure AD. |



## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre las [identidades administradas para recursos de Azure](../active-directory/managed-identities-azure-resources/index.yml).
- Más información sobre los [escenarios de autenticación de Azure AD](../active-directory/develop/active-directory-authentication-scenarios.md).
