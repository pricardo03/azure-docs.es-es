---
author: robinsh
ms.author: robin.shahan
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.openlocfilehash: e2d6b6413d1e397eaa3c53f28394dcf321dac729
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2019
ms.locfileid: "57011783"
---
## <a name="obtain-an-azure-resource-manager-token"></a>Obtención de un token de Azure Resource Manager
Azure Active Directory debe autenticar todas las tareas que se realizan en los recursos mediante el Administrador de recursos de Azure. En este ejemplo se usa la autenticación de contraseña; para otros métodos, consulte [Autenticación de solicitudes de Azure Resource Manager][lnk-authenticate-arm].

1. Agregue el código siguiente al método **Main** en Program.cs para recuperar un token de Azure AD mediante el identificador y la contraseña de la aplicación.
   
    ```
    var authContext = new AuthenticationContext(string.Format  
      ("https://login.microsoftonline.com/{0}", tenantId));
    var credential = new ClientCredential(applicationId, password);
    AuthenticationResult token = authContext.AcquireTokenAsync
      ("https://management.core.windows.net/", credential).Result;
   
    if (token == null)
    {
      Console.WriteLine("Failed to obtain the token");
      return;
    }
    ```
2. Cree un objeto **ResourceManagementClient** que use el token, agregando el código siguiente al final del método **Main**:
   
    ```
    var creds = new TokenCredentials(token.AccessToken);
    var client = new ResourceManagementClient(creds);
    client.SubscriptionId = subscriptionId;
    ```
3. Cree u obtenga una referencia al grupo de recursos que está usando:
   
    ```
    var rgResponse = client.ResourceGroups.CreateOrUpdate(rgName,
        new ResourceGroup("East US"));
    if (rgResponse.Properties.ProvisioningState != "Succeeded")
    {
      Console.WriteLine("Problem creating resource group");
      return;
    }
    ```

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx