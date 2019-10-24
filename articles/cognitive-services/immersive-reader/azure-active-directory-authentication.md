---
title: Autenticación de Azure Active Directory (Azure AD)
titleSuffix: Azure Cognitive Services
description: En este artículo se mostrará cómo crear un nuevo recurso de Lector inmersivo con un subdominio personalizado y, luego, configurar Azure AD en el inquilino de Azure.
services: cognitive-services
author: rwaller
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: rwaller
ms.openlocfilehash: d51c27b90113679c1547f2d030459a03cc22c80c
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2019
ms.locfileid: "72299812"
---
# <a name="use-azure-active-directory-azure-ad-authentication-with-the-immersive-reader-service"></a>Uso de la autenticación de Azure Active Directory (Azure AD) con el servicio Lector inmersivo

En las secciones siguientes, usará el entorno de Azure Cloud Shell o Azure PowerShell para crear un nuevo recurso de Lector inmersivo con un subdominio personalizado y, después, configure Azure AD en el inquilino de Azure. Después de completar la configuración inicial, llamará a Azure AD para obtener un token de acceso, de forma similar a como se realizará cuando se use el SDK de Lector inmersivo. Si se bloquea, se proporcionan vínculos en cada sección con todas las opciones disponibles para cada uno de los comandos de Azure PowerShell.

## <a name="create-an-immersive-reader-resource-with-a-custom-subdomain"></a>Creación de un recurso de Lector inmersivo con un subdominio personalizado

1. Para empezar, abra [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview). Después [seleccione una suscripción](https://docs.microsoft.com/powershell/module/servicemanagement/azure/select-azuresubscription?view=azuresmps-4.0.0#description):

   ```azurepowershell-interactive
   Select-AzSubscription -SubscriptionName <YOUR_SUBSCRIPTION>
   ```

2. Luego, [cree un recurso de Lector inmersivo](https://docs.microsoft.com/powershell/module/az.cognitiveservices/new-azcognitiveservicesaccount?view=azps-1.8.0) con un subdominio personalizado.

   >[!NOTE]
   > El nombre del subdominio se usa en el SDK de Lector inmersivo al iniciar el lector con la función launchAsync.

   -SkuName puede ser F0 (nivel Gratis) o S0 (nivel Estándar, también gratis durante la versión preliminar pública). El nivel S0 tiene un límite de velocidad de llamadas superior y ninguna cuota mensual en el número de llamadas.

   -Location puede ser cualquiera de las siguientes: `eastus`, `westus`, `australiaeast`, `centralindia`, `japaneast`, `northeurope`, `westeurope`

   -CustomSubdomainName debe ser globalmente único y no puede incluir caracteres especiales, como: ".", "!" o ",".


   ```azurepowershell-interactive
   $resource = New-AzCognitiveServicesAccount -ResourceGroupName <RESOURCE_GROUP_NAME> -name <RESOURCE_NAME> -Type ImmersiveReader -SkuName S0 -Location <REGION> -CustomSubdomainName <UNIQUE_SUBDOMAIN>

   // Display the Resource info
   $resource
   ```

   Si se realiza correctamente, el **punto de conexión** del recurso debe mostrar el nombre de subdominio único del recurso.

   Aquí vamos a capturar el objeto de recurso recién creado en una variable **$resource**, ya que se usará más adelante al conceder acceso a este recurso.


   >[!NOTE]
   > Si crea un recurso en Azure Portal, se usa el recurso "Name" como subdominio personalizado. Puede comprobar el nombre de subdominio en el portal; para ello, vaya a la página de información general de recursos y busque el subdominio en el punto de conexión que aparece en la lista, por ejemplo, `https://[SUBDOMAIN].cognitiveservices.azure.com/`. También podrá comprobarlo aquí cuando necesite obtener el subdominio para integrarlo con el SDK.

   Si el recurso se creó en el portal, también [puede obtener un recurso existente](https://docs.microsoft.com/powershell/module/az.cognitiveservices/get-azcognitiveservicesaccount?view=azps-1.8.0) ahora.

   ```azurepowershell-interactive
   $resource = Get-AzCognitiveServicesAccount -ResourceGroupName <RESOURCE_GROUP_NAME> -name <RESOURCE_NAME>

   // Display the Resource info
   $resource
   ```

## <a name="assign-a-role-to-a-service-principal"></a>Asignación de un rol a una entidad de servicio

Ahora que tiene un subdominio personalizado asociado al recurso, necesita asignar un rol a una entidad de servicio.

1. En primer lugar, va a [crear una aplicación de Azure AD](https://docs.microsoft.com/powershell/module/Az.Resources/New-AzADApplication?view=azps-1.8.0).

   >[!NOTE]
   > La contraseña, también conocida como "secreto de cliente", se utilizará al obtener los tokens de autenticación.

   ```azurepowershell-interactive
   $password = "<YOUR_PASSWORD>"
   $secureStringPassword = ConvertTo-SecureString -String $password -AsPlainText -Force
   $aadApp = New-AzADApplication -DisplayName ImmersiveReaderAAD -IdentifierUris http://ImmersiveReaderAAD -Password $secureStringPassword

   // Display the Azure AD app info
   $aadApp
   ```

   Aquí va a capturar el objeto de aplicación de Azure AD recién creado en una variable **$aadApp** para usarlo en el paso siguiente.

2. Después, tiene que [crear una entidad de servicio](https://docs.microsoft.com/powershell/module/az.resources/new-azadserviceprincipal?view=azps-1.8.0) para la aplicación de Azure AD.

   ```azurepowershell-interactive
   $principal = New-AzADServicePrincipal -ApplicationId $aadApp.ApplicationId

   // Display the service principal info
   $principal
   ```

   Aquí va a capturar el objeto Entidad de servicio recién creado en una variable **$principal** para usarlo en el paso siguiente.


3. El último paso es [asignar el rol "Usuario de Cognitive Services"](https://docs.microsoft.com/powershell/module/az.Resources/New-azRoleAssignment?view=azps-1.8.0) a la entidad de servicio (en el ámbito del recurso). Mediante la asignación de un rol, concede acceso a la entidad de servicio a este recurso. Puede conceder a la misma entidad de servicio acceso a varios recursos de la suscripción.

   ```azurepowershell-interactive
   New-AzRoleAssignment -ObjectId $principal.Id -Scope $resource.Id -RoleDefinitionName "Cognitive Services User"
   ```

   >[!NOTE]
   > Este paso debe realizarse en todos los recursos de Lector inmersivo que cree. Por ejemplo, si crea varios recursos para diferentes regiones globales, tendrá que realizar este paso para cada uno de esos recursos regionales, con el fin de que la autenticación Azure AD funcione para todos ellos. O bien, si crea un recurso más adelante, tendrá que realizar este paso de asignación de rol también para el nuevo recurso.


## <a name="obtain-an-azure-ad-token"></a>Obtención de un token de Azure AD

En este ejemplo, la contraseña se usa para autenticar la entidad de servicio para obtener un token de Azure AD.

1. Obtenga el **TenantId**:
   ```azurepowershell-interactive
   $context = Get-AzContext
   $context.Tenant.Id
   ```

2. Obtenga un token:
   ```azurepowershell-interactive
   $authority = "https://login.windows.net/" + $context.Tenant.Id
   $resource = "https://cognitiveservices.azure.com/"
   $authContext = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext" -ArgumentList $authority
   $clientCredential = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential" -ArgumentList $aadApp.ApplicationId, $password
   $token = $authContext.AcquireTokenAsync($resource, $clientCredential).Result
   $token
   ```

   >[!NOTE]
   > El SDK de Lector inmersivo usa la propiedad AccessToken del token, por ejemplo, $token. AccessToken. Para más información, vea la [referencia ](reference.md) del SDK y los [ejemplos](https://github.com/microsoft/immersive-reader-sdk/tree/master/js/samples).

Como alternativa, la entidad de servicio se puede autenticar con un certificado. Además de una entidad de servicio, las entidades de seguridad de usuario también se admiten si tienen permisos delegados a través de otra aplicación de Azure AD. En este caso, en lugar de contraseñas o certificados, se les pedirá a los usuarios la autenticación en dos fases al adquirir los tokens.

## <a name="next-steps"></a>Pasos siguientes

* Vea el [tutorial para Node.js](./tutorial-nodejs.md) para consultar qué más puede hacer con el SDK del Lector inmersivo con Node.js.
* Vea el [tutorial para Python](./tutorial-python.md) para consultar qué más puede hacer con el SDK del Lector inmersivo con Python.
* Vea el [tutorial para Swift](./tutorial-ios-picture-immersive-reader.md) para consultar qué más puede hacer con el SDK del Lector inmersivo con Swift.
* Explorar el [SDK del Lector inmersivo](https://github.com/microsoft/immersive-reader-sdk) y agregar la [Referencia del SDK del Lector inmersivo](./reference.md)