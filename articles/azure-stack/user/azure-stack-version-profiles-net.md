---
title: Uso de perfiles de la versión de la API con .NET SDK en Azure Stack | Microsoft Docs
description: Información acerca de los perfiles de la versión de la API con .NET en Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2018
ms.author: sethm
ms.reviewer: sijuman
ms.openlocfilehash: cfebbdb9b88a1de6a05f06e6ed72ebc9cddddcf6
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53074458"
---
# <a name="use-api-version-profiles-with-net-in-azure-stack"></a>Uso de los perfiles de la versión de la API con .NET en Azure Stack

*Se aplica a: sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

El SDK de .NET para Resource Manager de Azure Stack proporciona herramientas que le ayudarán a crear y administrar su infraestructura. Los proveedores de recursos del SDK incluyen servicios de proceso, redes, almacenamiento, aplicaciones y [KeyVault](../../key-vault/key-vault-whatis.md). El SDK de .NET incluye 14 paquetes NuGet que se deben descargar en la solución de proyecto cada vez que incorpore la información de perfil. Sin embargo, puede descargar de forma específica el proveedor de recursos que utilizará para 2018-03-01-hybrid o 2017-03-09-profile con el fin de optimizar la memoria para la aplicación. Cada paquete consta de un proveedor de recursos, la versión correspondiente de la API y el perfil de la API a la que pertenece. Los perfiles de API en el SDK de .NET habilitan el desarrollo en la nube híbrida ayudándole a cambiar entre los recursos de Azure globales y los recursos de Azure Stack.

## <a name="net-and-api-version-profiles"></a>Perfiles de la versión de API y .NET

Un perfil de API es una combinación de los proveedores de recursos y las versiones de la API. Puede usar un perfil de API para obtener la versión más reciente y más estable de cada tipo de recurso de un paquete de proveedor de recursos.

-   Para usar las últimas versiones de todos los servicios, use el perfil **más reciente** de los paquetes. Este perfil es parte del paquete NuGet **Microsoft.Azure.Management**.

-   Para usar los servicios compatibles con Azure Stack, use los paquetes **Microsoft.Azure.Management.Profiles.hybrid\_2018\_03\_01.*ResourceProvider*.0.9.0-preview.nupkg** o **Microsoft.Azure.Management.Profiles.hybrid\_2017\_03\_09.*ResourceProvider*.0.9.0-preview.nupkg**.

    -   Hay dos paquetes para cada proveedor de recursos de cada perfil.

    -   Asegúrese de que la parte **ResourceProvider** del paquete NuGet anterior se cambia al proveedor correcto.

-   Para usar la última versión de API de un servicio, utilice el perfil **más reciente** del paquete NuGet específico. Por ejemplo, si desea usar solo la **última versión de API** del servicio de proceso, utilice el perfil **latest** del paquete **compute**. El perfil **latest** forma parte del paquete NuGet **Microsoft.Azure.Management**.

-   Para utilizar versiones específicas de API para un tipo de recurso en un proveedor de recursos específico, utilice las versiones específicas de API definidas dentro del paquete.

Tenga en cuenta que puede combinar todas las opciones en la misma aplicación.

## <a name="install-the-azure-net-sdk"></a>Instalación del SDK de .NET para Azure

1.  Instale Git. Para instrucciones, consulte [Introducción: instalación de Git][].

2.  Para instalar los paquetes NuGet correctos, consulte [Búsqueda e instalación de un paquete][].

3.  Los paquetes que se deben instalar dependerán de la versión del perfil que le gustaría utilizar. El nombre del paquete de las versiones de perfiles es:

    1.  **Microsoft.Azure.Management.Profiles.hybrid\_2018\_03\_01.*ResourceProvider*.0.9.0-preview.nupkg**

    2.  **Microsoft.Azure.Management.Profiles.hybrid\_2017\_03\_09.*ResourceProvider*.0.9.0-preview.nupkg**

4.  Para instalar los paquetes NuGet correctos para Visual Studio Code, consulte el siguiente vínculo para descargar las [instrucciones del administrador de paquetes NuGet][].

5.  Si no está disponible, cree una suscripción y guarde su identificador para usarlo más adelante. Para obtener las instrucciones para crear una suscripción, consulte [Creación de suscripciones para ofertas en Azure Stack][].

6.  Cree a una entidad de servicio y guarde el identificador de cliente y el secreto de cliente. Para obtener instrucciones para crear una entidad de servicio para Azure Stack, consulte [Proporcionar a las aplicaciones acceso a Azure Stack][]. Tenga en cuenta que el identificador de cliente es también conocido como el identificador de aplicación al crear una entidad de servicio.

7.  Asegúrese de que la entidad de servicio tenga rol de colaborador o propietario en la suscripción. Para obtener instrucciones sobre cómo asignar roles a la entidad de servicio, consulte [Proporcionar a las aplicaciones acceso a Azure Stack][].

## <a name="prerequisites"></a>Requisitos previos

Para usar el SDK de Azure de .NET con Azure Stack, debe proporcionar los siguientes valores y, a continuación, establecer valores con variables de entorno. Para establecer las variables de entorno, consulte las instrucciones bajo la tabla para su sistema operativo.

| Valor                     | Variables de entorno   | DESCRIPCIÓN                                                                                                             |
|---------------------------|-------------------------|-------------------------------------------------------------------------------------------------------------------------|
| Id. de inquilino                 | AZURE_TENANT_ID       | El valor de su [*identificador de inquilino*][] de Azure Stack.                                                                          |
| Id. de cliente                 | AZURE_CLIENT_ID       | El identificador de aplicación de la entidad de servicio que guardó al crear esta última en la sección anterior de este artículo. |
| Id. de suscripción           | AZURE_SUBSCRIPTION_ID | El [*identificador de suscripción*][] es su forma de acceder a las ofertas de Azure Stack.                                                      |
| Secreto del cliente             | AZURE_CLIENT_SECRET   | El secreto de aplicación de la entidad de servicio que guardó al crear esta última.                                      |
| Punto de conexión de Resource Manager | ARM_ENDPOINT           | Consulte [*el punto de conexión de administración de recursos de Azure Stack*][].                                                                    |

Para buscar el identificador de inquilino de Azure Stack, siga las instrucciones que se encuentran [aquí](../azure-stack-csp-ref-operations.md). Para establecer las variables de entorno, haga lo siguiente:

### <a name="microsoft-windows"></a>Microsoft Windows

Para establecer las variables de entorno, en el símbolo del sistema de Windows, use el siguiente formato:

```shell
Set Azure_Tenant_ID=Your_Tenant_ID
```

### <a name="macos-linux-and-unix-based-systems"></a>Sistemas basados en MacOS, Linux y Unix

En los sistemas basados en Unix, puede usar el comando siguiente:

```shell
Export Azure_Tenant_ID=Your_Tenant_ID
```

### <a name="the-azure-stack-resource-manager-endpoint"></a>Punto de conexión de Resource Manager de Azure Stack

Microsoft Azure Resource Manager es una plataforma de administración que permite a los administradores implementar, administrar y supervisar recursos de Azure. Azure Resource Manager puede controlar estas tareas como grupo, en vez de individualmente, en una sola operación.

Puede obtener la información de metadatos del punto de conexión de Resource Manager. El punto de conexión devuelve un archivo JSON con la información necesaria para ejecutar el código.

Tenga en cuenta las siguientes consideraciones:

- El valor de **ResourceManagerUrl** del Kit de desarrollo de Azure Stack (ASDK) es: https://management.local.azurestack.external/

- El valor de **ResourceManagerUrl** en los sistemas integrados es: `https://management.<location>.ext-<machine-name>.masd.stbtest.microsoft.com/`. Para recuperar los metadatos necesarios: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0`.

Archivo JSON de ejemplo:

```json
{ 
   "galleryEndpoint": "https://portal.local.azurestack.external:30015/",
   "graphEndpoint": "https://graph.windows.net/",
   "portal Endpoint": "https://portal.local.azurestack.external/",
   "authentication": 
      {
      "loginEndpoint": "https://login.windows.net/",
      "audiences": ["https://management.yourtenant.onmicrosoft.com/3cc5febd-e4b7-4a85-a2ed-1d730e2f5928"]
      }
}
```

## <a name="existing-api-profiles"></a>Perfiles de API existentes

1.  **Microsoft.Azure.Management.Profiles.hybrid\_2018\_03\_01.*ResourceProvider*.0.9.0-preview.nupkg**: último perfil creado para Azure Stack. Use este perfil de servicios para mayor compatibilidad con Azure Stack, siempre y cuando tenga una marca de versión 1808 o superior.

2.  **Microsoft.Azure.Management.Profiles.hybrid\_2017\_03\_09.*ResourceProvider*.0.9.0-preview.nupkg**: si utiliza un sello anterior a la compilación 1808, use este perfil.

3.  **Más reciente**: este perfil contiene las versiones más recientes de todos los servicios. Use las versiones más recientes de todos los servicios. Este perfil es parte del paquete NuGet **Microsoft.Azure.Management**.

Para obtener más información sobre los perfiles de API y Azure Stack, consulte [Resumen de perfiles de API][].

## <a name="azure-net-sdk-api-profile-usage"></a>Uso de perfil de API del SDK de .NET de Azure

Se debe usar el siguiente código para crear una instancia de un cliente del perfil. Este parámetro solo es necesario para Azure Stack u otras nubes privadas. Azure global ya tiene esta configuración de forma predeterminada.

Se necesita el código siguiente para autenticar la entidad de servicio en Azure Stack. Crea un token mediante el identificador de inquilino y la base de autenticación, que es específica de Azure.

```csharp
public class CustomLoginCredentials : ServiceClientCredentials
{
    private string clientId;
    private string clientSecret;
    private string resourceId;
    private string tenantId;

    private const string authenticationBase = "https://login.windows.net/{0}";

    public CustomLoginCredentials(string servicePrincipalId, string servicePrincipalSecret, string azureEnvironmentResourceId, string azureEnvironmentTenandId)
    {
        clientId = servicePrincipalId;
        clientSecret = servicePrincipalSecret;
        resourceId = azureEnvironmentResourceId;
        tenantId = azureEnvironmentTenandId;
    }
```

Esto le permitirá usar los paquetes NuGet de perfil de API para implementar la aplicación correctamente en Azure Stack.

## <a name="define-azure-stack-environment-setting-functions"></a>Definición de las funciones de configuración del entorno de Azure Stack

Para autenticar la entidad de servicio en el entorno de Azure Stack, use el siguiente código:

```csharp
private string AuthenticationToken { get; set; }
public override void InitializeServiceClient<T>(ServiceClient<T> client)
{
    var authenticationContext = new AuthenticationContext(String.Format(authenticationBase, tenantId));
    var credential = new ClientCredential(clientId, clientSecret);
    var result = authenticationContext.AcquireTokenAsync(resource: resourceId,
    clientCredential: credential).Result;
    if (result == null)
    {
        throw new InvalidOperationException("Failed to obtain the JWT token");
    }
    AuthenticationToken = result.AccessToken;
}
```

Esto invalida el cliente del servicio de inicialización para autenticarse en Azure Stack.

## <a name="samples-using-api-profiles"></a>Ejemplos donde se usan perfiles de API

Se pueden usar los ejemplos siguientes encontrados en repositorios de GitHub como referencia para crear soluciones con perfiles de API de Azure Stack y .NET.

-   [Proyecto de prueba de la máquina virtual, la red virtual, los grupos de recursos y la cuenta de almacenamiento][]
-   Administración de máquinas virtuales con .NET

### <a name="sample-unit-test-project"></a>Proyecto de prueba unitaria de ejemplo 

1.  Clone el repositorio con el siguiente comando:

    ```shell
    git clone https://github.com/Azure-Samples/hybrid-compute-dotnet-manage-vm.git
    ```

2.  Cree una entidad de servicio de Azure y asigne un rol para acceder a la suscripción. Para obtener instrucciones sobre cómo crear una entidad de servicio, consulte [Uso de Azure PowerShell para crear una entidad de servicio con un certificado][].

3.  Recupere los siguientes valores obligatorios:

    1.  Id. de inquilino
    2.  Id. de cliente
    3.  Secreto del cliente
    4.  Id. de suscripción
    5.  Punto de conexión de Resource Manager

4.  Establezca las siguientes variables de entorno con la información que recuperó de la entidad de servicio que creó con el símbolo del sistema:

    1.  export AZURE_TENANT_ID={your tenant id}
    2.  export AZURE_CLIENT_ID={your client id}
    3.  export AZURE_CLIENT_SECRET={your client secret}
    4.  export AZURE_SUBSCRIPTION_ID={your subscription id}
    5.  export ARM_ENDPOINT={la dirección URL del administrador de recursos de Azure Stack}

   En Windows, use **set** en lugar de **export**.

5.  Asegúrese de que la variable de ubicación está establecida en su ubicación de Azure Stack. Por ejemplo, LOCAL = "local".

6.  Establezca las credenciales de inicio de sesión personalizadas que le permitirán autenticarse en Azure Stack. Tenga en cuenta que esta parte del código está incluida en este ejemplo en la carpeta de autorización.

   ```csharp
   public class CustomLoginCredentials : ServiceClientCredentials
   {
       private string clientId;
       private string clientSecret;
       private string resourceId;
       private string tenantId;
       private const string authenticationBase = "https://login.windows.net/{0}";
       public CustomLoginCredentials(string servicePrincipalId, string servicePrincipalSecret, string azureEnvironmentResourceId, string azureEnvironmentTenandId)
       {
           clientId = servicePrincipalId;
           clientSecret = servicePrincipalSecret;
           resourceId = azureEnvironmentResourceId;
           tenantId = azureEnvironmentTenandId;
       }
   private string AuthenticationToken { get; set; }
   ```

7.  Si va a usar Azure Stack para invalidar el cliente del servicio de inicialización para autenticarse en Azure Stack, agregue el código siguiente. Tenga en cuenta que esta parte del código ya está incluida en este ejemplo en la carpeta de autorización.

   ```csharp
   public override void InitializeServiceClient<T>(ServiceClient<T> client)
   {
      var authenticationContext = new AuthenticationContext(String.Format(authenticationBase, tenantId));
      var credential = new ClientCredential(clientId, clientSecret);
      var result = authenticationContext.AcquireTokenAsync(resource: resourceId,
                clientCredential: credential).Result;
      if (result == null)
      {
          throw new InvalidOperationException("Failed to obtain the JWT token");
      }
      AuthenticationToken = result.AccessToken;
   }
   ```
 
8.  Mediante el administrador de paquetes NuGet, busque "2018-03-01-hybrid" e instale los paquetes asociados a este perfil para los proveedores de recursos de proceso, redes, almacenamiento, KeyVault y App Services.

2.  Dentro de cada tarea del archivo.cs, establezca los parámetros que son necesarios para trabajar con Azure Stack. A continuación, se muestra un ejemplo de la tarea `CreateResourceGroupTest`:

   ```csharp
   var location = Environment.GetEnvironmentVariable("AZURE_LOCATION");
   var baseUriString = Environment.GetEnvironmentVariable("AZURE_BASE_URL");
   var resourceGroupName = Environment.GetEnvironmentVariable("AZURE_RESOURCEGROUP");
   var servicePrincipalId = Environment.GetEnvironmentVariable("AZURE_CLIENT_ID");
   var servicePrincipalSecret = Environment.GetEnvironmentVariable("AZURE_CLIENT_SECRET");
   var azureResourceId = Environment.GetEnvironmentVariable("AZURE_RESOURCE_ID");
   var tenantId = Environment.GetEnvironmentVariable("AZURE_TENANT_ID");
   var subscriptionId = Environment.GetEnvironmentVariable("AZURE_SUBSCRIPTION_ID");
   var credentials = new CustomLoginCredentials(servicePrincipalId, servicePrincipalSecret, azureResourceId, tenantId);
   ```

1.  Haga clic con el botón derecho en cada tarea y seleccione **Ejecutar prueba**.

    1.  Las marcas de verificación verde en la ventana del panel lateral le avisan de que cada tarea se creó correctamente en función de los parámetros proporcionados. Compruebe su suscripción de Azure Stack para asegurarse de que los recursos se crearon correctamente.

    2.  Para más información sobre cómo ejecutar pruebas unitarias, consulte [Ejecutar pruebas unitarias con el Explorador de pruebas.][]

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los perfiles de API, consulte:

- [Administración de perfiles de la versión de API en Azure Stack](azure-stack-version-profiles.md)
- [Versiones de API del proveedor de recursos compatibles con perfiles](azure-stack-profiles-azure-resource-manager-versions.md)

  [Introducción: Instalación de Git]: https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
  [Búsqueda e instalación de un paquete]: /nuget/tools/package-manager-ui
  [Instrucciones del administrador de paquetes NuGet]: https://marketplace.visualstudio.com/items?itemName=jmrog.vscode-nuget-package-manager
  [Creación de suscripciones para ofertas en Azure Stack]: ../azure-stack-subscribe-plan-provision-vm.md
  [Proporcionar a las aplicaciones acceso a Azure Stack]: ../azure-stack-create-service-principals.md
  [*Identificador de inquilino*]: ../azure-stack-identity-overview.md
  [*Identificador de suscripción*]: ../azure-stack-plan-offer-quota-overview.md#subscriptions
  [*Punto de conexión de Resource Manager de Azure Stack*]: ../user/azure-stack-version-profiles-ruby.md#the-azure-stack-resource-manager-endpoint
  [Resumen de perfiles de API]: ../user/azure-stack-version-profiles.md#summary-of-api-profiles
  [Proyecto de prueba de la máquina virtual, la red virtual, los grupos de recursos y la cuenta de almacenamiento]: https://github.com/seyadava/azure-sdk-for-net-samples/tree/master/TestProject
  [Uso de Azure PowerShell para crear una entidad de servicio con un certificado]: ../azure-stack-create-service-principals.md
  [Ejecutar pruebas unitarias con el Explorador de pruebas.]: /visualstudio/test/run-unit-tests-with-test-explorer?view=vs-2017
