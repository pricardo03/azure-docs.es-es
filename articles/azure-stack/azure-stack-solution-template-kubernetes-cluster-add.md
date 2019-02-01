---
title: Adición de Kubernetes a Marketplace de Azure Stack | Microsoft Docs
description: Obtenga información sobre cómo agregar Kubernetes a Marketplace de Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2019
ms.author: mabrigg
ms.reviewer: waltero
ms.lastreviewed: 01/16/2019
ms.openlocfilehash: b7e293018f2dc248fae8de8bd9e82943606f4ead
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55239509"
---
# <a name="add-kubernetes-to-the-azure-stack-marketplace"></a>Adición de Kubernetes a Marketplace de Azure Stack

*Se aplica a: Sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

> [!note]  
> Kubernetes en Azure Stack está en versión preliminar.

Puede ofrecer Kubernetes como un elemento de Marketplace a los usuarios. A continuación, los usuarios pueden implementar Kubernetes en una sola operación coordinada.

En el artículo siguiente explica cómo usar una plantilla de Azure Resource Manager para implementar y aprovisionar los recursos para un clúster de Kubernetes independiente. El elemento Clúster de Kubernetes de Marketplace 0.3.0 requiere la versión 1808 de Azure Stack. Antes de empezar, compruebe Azure Stack y la configuración de inquilino de Azure global. Recopile la información necesaria sobre su entorno de Azure Stack. Agregue los recursos necesarios a su inquilino y a Marketplace de Azure Stack. El clúster depende de que Ubuntu Server, el script personalizado y los elementos de Kubernetes se encuentren en Marketplace.

## <a name="create-a-plan-an-offer-and-a-subscription"></a>Crear un plan, una oferta y una suscripción

Cree un plan, una oferta y una suscripción para el elemento de Marketplace de Kubernetes. También puede usar un plan y una oferta existentes.

1. Inicie sesión en el [portal de administración](https://adminportal.local.azurestack.external).

1. Cree un plan como plan base. Para obtener instrucciones, vea [Creación de un plan en Azure Stack](azure-stack-create-plan.md).

1. Cree una oferta. Para obtener instrucciones, vea [Creación de una oferta en Azure Stack](azure-stack-create-offer.md).

1. Seleccione **Ofertas** y busque la que ha creado.

1. Seleccione **Información general** en la hoja Oferta.

1. Seleccione **Cambiar estado**. Seleccione **Público**.

1. Seleccione **+ Crear un recurso** > **Offers and Plans** (Ofertas y planes)  > **Suscripción** para crear una suscripción.

     a. Especifique un **Nombre para mostrar**.

    b. Especifique un **Usuario**. Use la cuenta de Azure AD asociada con su inquilino.

    c. **Descripción del proveedor**

    d. Establezca el **Inquilino de directorio** en el inquilino de Azure AD de su entorno de Azure Stack. 

    e. Seleccione **Oferta**. Seleccione el nombre de la oferta que ha creado. Tome nota del identificador de suscripción.

## <a name="create-a-service-principal-and-credentials-in-ad-fs"></a>Creación de una entidad de servicio y las credenciales de AD FS

Si usa Active Directory Federated Services (AD FS) para el servicio de administración de identidades, deberá crear una entidad de servicio para que los usuarios implementen un clúster de Kubernetes.

1. Cree y exporte el certificado que se usará para crear la entidad de servicio. El siguiente fragmento de código muestra cómo crear un certificado autofirmado. 

    - Necesita la siguiente información:

       | Valor | DESCRIPCIÓN |
       | ---   | ---         |
       | Contraseña | Contraseña del certificado. |
       | Ruta de acceso del certificado | Nombre de archivo y ruta de acceso del certificado. Por ejemplo: `path\certfilename.pfx` |
       | Nombre del certificado | Nombre del certificado. |
       | Ubicación del almacén de certificados |  Por ejemplo: `Cert:\LocalMachine\My` |

    - Abra PowerShell con un símbolo del sistema con privilegios elevados. Ejecute el siguiente script con los parámetros actualizados para coincidir con sus valores:

        ```PowerShell  
        # Creates a new self signed certificate 
        $passwordString = "<password>"
        $certlocation = "<local certificate path>.pfx"
        $certificateName = "<certificate name>"
        #certificate store location. Eg. Cert:\LocalMachine\My
        $certStoreLocation="<certificate store location>"
        
        $params = @{
        CertStoreLocation = $certStoreLocation
        DnsName = $certificateName
        FriendlyName = $certificateName
        KeyLength = 2048
        KeyUsageProperty = 'All'
        KeyExportPolicy = 'Exportable'
        Provider = 'Microsoft Enhanced Cryptographic Provider v1.0'
        HashAlgorithm = 'SHA256'
        }
        
        $cert = New-SelfSignedCertificate @params -ErrorAction Stop
        Write-Verbose "Generated new certificate '$($cert.Subject)' ($($cert.Thumbprint))." -Verbose
        
        #Exports certificate with password in a .pfx format
        $pwd = ConvertTo-SecureString -String $passwordString -Force -AsPlainText
        Export-PfxCertificate -cert $cert -FilePath $certlocation -Password $pwd
        ```

2. Cree una entidad de servicio con el certificado.

    - Necesita la siguiente información:

       | Valor | DESCRIPCIÓN                     |
       | ---   | ---                             |
       | IP de ERCS | En el ASDK, el punto de conexión con privilegios es normalmente `AzS-ERCS01`. |
       | Nombre de la aplicación | Nombre sencillo para la entidad de servicio de aplicación. |
       | Ubicación del almacén de certificados | La ruta de acceso en el equipo en el que ha almacenado el certificado. Por ejemplo: `Cert:\LocalMachine\My\<someuid>` |

    - Abra PowerShell con un símbolo del sistema con privilegios elevados. Ejecute el siguiente script con los parámetros actualizados para coincidir con sus valores:

        ```PowerShell  
        #Create service principal using the certificate
        $privilegedendpoint="<ERCS IP>"
        $applicationName="<application name>"
        #certificate store location. Eg. Cert:\LocalMachine\My
        $certStoreLocation="<certificate store location>"
        
        # Get certificate information
        $cert = Get-Item $certStoreLocation
        
        # Credential for accessing the ERCS PrivilegedEndpoint, typically domain\cloudadmin
        $creds = Get-Credential

        # Creating a PSSession to the ERCS PrivilegedEndpoint
        $session = New-PSSession -ComputerName $privilegedendpoint -ConfigurationName PrivilegedEndpoint -Credential $creds

        # Get Service principal Information
        $ServicePrincipal = Invoke-Command -Session $session -ScriptBlock { New-GraphApplication -Name "$using:applicationName" -ClientCertificates $using:cert}

        # Get Stamp information
        $AzureStackInfo = Invoke-Command -Session $session -ScriptBlock { get-azurestackstampinformation }

        # For Azure Stack development kit, this value is set to https://management.local.azurestack.external. This is read from the AzureStackStampInformation output of the ERCS VM.
        $ArmEndpoint = $AzureStackInfo.TenantExternalEndpoints.TenantResourceManager

        # For Azure Stack development kit, this value is set to https://graph.local.azurestack.external/. This is read from the AzureStackStampInformation output of the ERCS VM.
        $GraphAudience = "https://graph." + $AzureStackInfo.ExternalDomainFQDN + "/"

        # TenantID for the stamp. This is read from the AzureStackStampInformation output of the ERCS VM.
        $TenantID = $AzureStackInfo.AADTenantID

        # Register an AzureRM environment that targets your Azure Stack instance
        Add-AzureRMEnvironment `
        -Name "AzureStackUser" `
        -ArmEndpoint $ArmEndpoint

        # Set the GraphEndpointResourceId value
        Set-AzureRmEnvironment `
        -Name "AzureStackUser" `
        -GraphAudience $GraphAudience `
        -EnableAdfsAuthentication:$true
        Add-AzureRmAccount -EnvironmentName "azurestackuser" `
        -ServicePrincipal `
        -CertificateThumbprint $ServicePrincipal.Thumbprint `
        -ApplicationId $ServicePrincipal.ClientId `
        -TenantId $TenantID

        # Output the SPN details
        $ServicePrincipal
        ```

    - La información de la entidad de servicio es similar al fragmento de código que se muestra a continuación:

        ```Text  
        ApplicationIdentifier : S-1-5-21-1512385356-3796245103-1243299919-1356
        ClientId              : 3c87e710-9f91-420b-b009-31fa9e430145
        Thumbprint            : 30202C11BE6864437B64CE36C8D988442082A0F1
        ApplicationName       : Azurestack-MyApp-c30febe7-1311-4fd8-9077-3d869db28342
        PSComputerName        : azs-ercs01
        RunspaceId            : a78c76bb-8cae-4db4-a45a-c1420613e01b
        ```

## <a name="add-an-ubuntu-server-image"></a>Agregar una imagen de Ubuntu Server

Agregue la siguiente imagen de Ubuntu Server en Marketplace:

1. Inicie sesión en el [portal de administración](https://adminportal.local.azurestack.external).

1. Seleccione **Todos los servicios** y, a continuación, en la categoría **Administración**, seleccione **Marketplace management** (Administración de Marketplace).

1. Seleccione **+ Add from Azure** (+ Agregar desde Azure).

1. Escriba `UbuntuServer`.

1. Seleccione la versión más reciente del servidor. Compruebe la versión completa y asegúrese de que tiene instalada la versión más reciente:
    - **Publicador**: Canonical
    - **Oferta**: UbuntuServer
    - **Versión**: 16.04.201806120 (o la versión más reciente)
    - **SKU**: 16.04-LTS

1. Seleccione **Descargar**.

## <a name="add-a-custom-script-for-linux"></a>Agregar un script personalizado para Linux

Agregue Kubernetes desde Marketplace:

1. Abra el [portal de administración](https://adminportal.local.azurestack.external).

1. Seleccione **Todos los servicios** y, a continuación, en la categoría **Administración**, seleccione **Marketplace management** (Administración de Marketplace).

1. Seleccione **+ Add from Azure** (+ Agregar desde Azure).

1. Escriba `Custom Script for Linux`.

1. Seleccione el script con el perfil siguiente:
    - **Oferta**: Script personalizado para Linux 2.0
    - **Versión**: 2.0.6 (o la versión más reciente)
    - **Publicador**: Microsoft Corp

    > [!Note]  
    > Podría aparecer más de una versión del script personalizado para Linux. Deberá agregar la versión más reciente del elemento.

1. Seleccione **Descargar**.


## <a name="add-kubernetes-to-the-marketplace"></a>Adición de Kubernetes a Marketplace

1. Abra el [portal de administración](https://adminportal.local.azurestack.external).

1. Seleccione **Todos los servicios** y, luego, en la categoría **ADMINISTRACIÓN**, seleccione **Administración de Marketplace**.

1. Seleccione **+ Add from Azure** (+ Agregar desde Azure).

1. Escriba `Kubernetes`.

1. Seleccione `Kubernetes Cluster`.

1. Seleccione **Descargar**.

    > [!note]  
    > El elemento podría tardar cinco minutos en aparecer en Marketplace.

    ![kubernetes](user/media/azure-stack-solution-template-kubernetes-deploy/marketplaceitem.png)

## <a name="update-or-remove-the-kubernetes"></a>Actualización o eliminación de Kubernetes 

Al actualizar el elemento de Kubernetes, eliminará el elemento que se encontraba en Marketplace. Siga las instrucciones de este artículo para agregar la actualización de Kubernetes a Marketplace.

Para quitar el elemento de Kubernetes:

1. Conéctese a Azure Stack con PowerShell como operador. Para obtener instrucciones, vea [Configuración del entorno de PowerShell de Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-configure-admin).

2. Busque el elemento actual Clúster de Kubernetes en la galería.

    ```PowerShell  
    Get-AzsGalleryItem | Select Name
    ```
    
3. Anote el nombre del elemento actual, por ejemplo, `Microsoft.AzureStackKubernetesCluster.0.3.0`.

4. Use el siguiente cmdlet de PowerShell para quitar el elemento:

    ```PowerShell  
    $Itemname="Microsoft.AzureStackKubernetesCluster.0.3.0"

    Remove-AzsGalleryItem -Name $Itemname
    ```

## <a name="next-steps"></a>Pasos siguientes

[Implementación de un clúster de Kubernetes en Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)

[Introducción a la oferta de servicios en Azure Stack](azure-stack-offer-services-overview.md)
