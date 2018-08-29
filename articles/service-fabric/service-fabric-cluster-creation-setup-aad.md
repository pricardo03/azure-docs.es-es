---
title: Configurar Azure Active Directory para la autenticación de clientes en Service Fabric | Microsoft Docs
description: Obtenga información sobre cómo configurar Azure Active Directory (Azure AD) para autenticar a los clientes de los clústeres de Service Fabric.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/15/2018
ms.author: aljo
ms.openlocfilehash: 9b00f6e5c4dbd2fe2c6aab4c62cecc2f2e2640f0
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2018
ms.locfileid: "42145022"
---
# <a name="set-up-azure-active-directory-for-client-authentication"></a>Configuración de Azure Active Directory para la autenticación de cliente

Para los clústeres que se ejecutan en Azure, es recomendable proteger el acceso a los puntos de conexión de administración con Azure Active Directory (Azure AD).  En este artículo se describe cómo configurar Azure AD para autenticar a los clientes de un clúster de Service Fabric; esta operación se debe realizar antes [crear el clúster](service-fabric-cluster-creation-via-arm.md).  Azure AD permite a las organizaciones (conocidas como inquilinos) administrar el acceso de los usuarios a las aplicaciones. Las aplicaciones se dividen en las que tienen interfaz de usuario de inicio de sesión basada en web y las que tienen una experiencia de cliente nativa. En este artículo se supone que ya ha creado un inquilino. En caso de que no lo haya hecho, lea [Obtención de un inquilino de Azure Active Directory][active-directory-howto-tenant].

## <a name="create-azure-ad-applications"></a>Crear aplicaciones de Azure AD
Un clúster de Service Fabric ofrece diversos puntos de entrada a su funcionalidad de administración, como [Service Fabric Explorer][service-fabric-visualizing-your-cluster] y [Visual Studio][service-fabric-manage-application-in-visual-studio] basados en web. Como consecuencia, creará dos aplicaciones de Azure AD para controlar el acceso al clúster: una aplicación web y una aplicación nativa.  Una vez creadas las aplicaciones, debe asignar usuarios a los roles de solo lectura y administrador.

Para simplificar algunos de los pasos necesarios para configurar Azure AD con un clúster de Service Fabric, hemos creado un conjunto de scripts de Windows PowerShell.

> [!NOTE]
> Debe realizar los pasos siguientes antes de crear el clúster. Dado que los scripts esperan los puntos de conexión y los nombres de los clústeres, los valores deben planearse y no se pueden usar los que ya haya creado.

1. [Descargue los scripts][sf-aad-ps-script-download] en su equipo.
2. Haga clic con el botón derecho en el archivo zip, seleccione **Propiedades** y la casilla **Desbloquear**, y haga clic en **Aplicar**.
3. Extraiga el archivo ZIP.
4. Ejecute `SetupApplications.ps1` y proporcione los parámetros TenantId, ClusterName y WebApplicationReplyUrl. Por ejemplo: 

```PowerShell
.\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
```

> [!NOTE]
> Para las nubes nacionales (Azure Government, Azure China, Azure Alemania), también debe especificar el parámetro `-Location`.

Ejecute el comando `Get-AzureSubscription` de PowerShell para encontrar su TenantId. Al ejecutar este comando se muestra el valor de TenantId para cada suscripción.

El valor de ClusterName se usa como prefijo en las aplicaciones de Azure AD que crea el script. No es necesario que coincida exactamente con el nombre del clúster real. Está diseñado solo para facilitar la asignación de artefactos de Azure AD al clúster de Service Fabric con el que se utilizan.

El valor de WebApplicationReplyUrl es el punto de conexión predeterminado al que Azure AD devuelve a los usuarios cuando terminan el inicio de sesión. Establézcalo como el punto de conexión de Service Fabric Explorer para el clúster, que de manera predeterminada es el siguiente:

https://&lt;cluster_domain&gt;:19080/Explorer

Se le pedirá que inicie sesión en una cuenta con privilegios administrativos para el inquilino de Azure AD. Una vez iniciada la sesión, el script creará las aplicaciones web y nativa para representar el clúster de Service Fabric. Si examina las aplicaciones del inquilino en [Azure Portal][azure-portal], debería ver dos entradas nuevas:

   * *ClusterName*\_Clúster
   * *ClusterName*\_Cliente

El script imprimirá el código JSON que necesita la plantilla de Azure Resource Manager al crear el clúster en la sección siguiente, por lo que es buena idea mantener abierta la ventana de PowerShell.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

<a name="assign-roles"></a>

## <a name="assign-users-to-roles"></a>Asignación de usuarios a roles
Una vez que haya creado las aplicaciones para representar el clúster, debe asignar los usuarios a los roles compatibles con Service Fabric: solo lectura y administrador. Puede asignar los roles mediante [Azure Portal][azure-portal].

1. En Azure Portal, seleccione el inquilino en la esquina superior derecha.

    ![Botón Seleccionar inquilino][select-tenant-button]
2. Seleccione **Azure Active Directory** en la pestaña de la izquierda y, a continuación, seleccione "Aplicaciones empresariales".
3. Seleccione "Todas las aplicaciones" y, a continuación, busque y seleccione la aplicación web que se llama `myTestCluster_Cluster`.
4. Haga clic en la pestaña **Usuarios y grupos**.

    ![Pestaña Usuarios y grupos][users-and-groups-tab]
5. Haga clic en el botón **Agregar usuario** situado en la página nueva, seleccione un usuario y el rol que desea asignar y, a continuación, haga clic en el botón **Seleccionar** situado en la parte inferior de la página.

    ![Página Asignar usuarios a roles][assign-users-to-roles-page]
6. Haga clic en el botón **Asignar**, situado en la parte inferior de la página.

    ![Confirmación de Agregar asignación][assign-users-to-roles-confirm]

> [!NOTE]
> Para más información sobre los roles de Service Fabric, consulte [Control de acceso basado en roles para clientes de Service Fabric](service-fabric-cluster-security-roles.md).
>
>

## <a name="troubleshooting-help-in-setting-up-azure-active-directory"></a>Ayuda de solución de problemas para configurar Azure Active Directory
Configurar y usar Azure AD puede resultar complicado, así que a continuación se proporcionan algunas indicaciones de lo que puede hacer para depurar el problema.

### <a name="service-fabric-explorer-prompts-you-to-select-a-certificate"></a>Service Fabric Explorer le solicita que seleccione el certificado
#### <a name="problem"></a>Problema
Después de conectarse correctamente a Azure AD en Service Fabric Explorer, el explorador vuelve a la página principal, pero un mensaje le pide que seleccione un certificado.

![Cuadro de diálogo de certificado SFX][sfx-select-certificate-dialog]

#### <a name="reason"></a>Motivo
Al usuario no se le ha asignado un rol en la aplicación de clúster de Azure AD. Por tanto, se produce un error en la autenticación de Azure AD en el clúster de Service Fabric. Service Fabric Explorer recurre a la autenticación de certificado.

#### <a name="solution"></a>Solución
Siga las instrucciones de configuración de Azure AD y de asignación de roles de usuario. Además, se recomienda activar "Asignación de usuario necesaria para acceder a la aplicación", como hace `SetupApplications.ps1`.

### <a name="connection-with-powershell-fails-with-an-error-the-specified-credentials-are-invalid"></a>Error de conexión con PowerShell: "Las credenciales especificadas no son válidas"
#### <a name="problem"></a>Problema
Cuando usa PowerShell para conectarse al clúster mediante el modo de seguridad "AzureActiveDirectory", después de iniciar sesión correctamente en Azure AD, se produce un error de conexión con el mensaje "Las credenciales especificadas no son válidas".

#### <a name="solution"></a>Solución
Esta solución es la mismo que la anterior.

### <a name="service-fabric-explorer-returns-a-failure-when-you-sign-in-aadsts50011"></a>Service Fabric Explorer devuelve un error al iniciar sesión: "AADSTS50011"
#### <a name="problem"></a>Problema
Al intentar iniciar sesión en Azure AD en Service Fabric Explorer, la página devuelve el error: "AADSTS50011: The reply address &lt;url&gt; does not match the reply addresses configured for the application: &lt;guid&gt;" (AADSTS50011: La dirección <url> de respuesta no coincide con las direcciones de respuesta configuradas para la aplicación: <guid>).

![La dirección de respuesta SFX no coincide][sfx-reply-address-not-match]

#### <a name="reason"></a>Motivo
La aplicación del clúster (web) que representa Service Fabric Explorer intenta la autenticación en Azure AD y, como parte de la solicitud, proporciona la dirección URL de retorno de redireccionamiento. Pero no aparece en la lista **URL DE RESPUESTA** de la aplicación Azure AD.

#### <a name="solution"></a>Solución
Seleccione "Registros de aplicaciones" en la página de AAD, seleccione la aplicación de clúster y, a continuación, seleccione el botón **URL de respuesta**. En la página "URL de respuesta", agregue la dirección URL de Service Fabric Explorer a la lista o reemplace algunos de los elementos de esta. Cuando haya terminado, guarde los cambios.

![Dirección URL de respuesta de la aplicación web][web-application-reply-url]

### <a name="connect-the-cluster-by-using-azure-ad-authentication-via-powershell"></a>Conexión del clúster mediante la autenticación de Azure AD a través de PowerShell
Para conectar el clúster de Service Fabric, use el siguiente ejemplo de comando de PowerShell:

```PowerShell
Connect-ServiceFabricCluster -ConnectionEndpoint <endpoint> -KeepAliveIntervalInSec 10 -AzureActiveDirectory -ServerCertThumbprint <thumbprint>
```

Para obtener más información, consulte el [cmdlet Connect-ServiceFabricCluster](https://docs.microsoft.com/powershell/module/servicefabric/connect-servicefabriccluster).

### <a name="can-i-reuse-the-same-azure-ad-tenant-in-multiple-clusters"></a>¿Se puede reutilizar el mismo inquilino de Azure AD para varios clústeres?
Sí. Pero recuerde agregar la dirección URL de Service Fabric Explorer a la aplicación del clúster (web). De lo contrario, Service Fabric Explorer no funciona.

### <a name="why-do-i-still-need-a-server-certificate-while-azure-ad-is-enabled"></a>¿Por qué todavía necesito el certificado de servidor con Azure AD habilitado?
FabricClient y FabricGateway realizan una autenticación mutua. Durante la autenticación de Azure AD, la integración de Azure AD proporciona la identidad del cliente al servidor y el certificado de servidor se utiliza para comprobar la identidad del servidor. Para más información sobre cómo funciona el certificado en Service Fabric, consulte [Certificados X.509 y Service Fabric][x509-certificates-and-service-fabric].

## <a name="next-steps"></a>Pasos siguientes
Después de configurar las aplicaciones de Azure Active Directory y establecer los roles para los usuarios, [configure e implemente un clúster](service-fabric-cluster-creation-via-arm.md).


<!-- Links -->
[azure-CLI]:https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest
[azure-portal]: https://portal.azure.com/
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]:../active-directory/develop/quickstart-create-new-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[select-tenant-button]: ./media/service-fabric-cluster-creation-setup-aad/select-tenant-button.png
[users-and-groups-tab]: ./media/service-fabric-cluster-creation-setup-aad/users-and-groups-tab.png
[assign-users-to-roles-page]: ./media/service-fabric-cluster-creation-setup-aad/assign-users-to-roles-page.png
[assign-users-to-roles-confirm]: ./media/service-fabric-cluster-creation-setup-aad/assign-users-to-roles-confirm.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-setup-aad/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-setup-aad/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-setup-aad/web-application-reply-url.png
