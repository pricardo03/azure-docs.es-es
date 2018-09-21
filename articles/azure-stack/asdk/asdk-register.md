---
title: Registro del Kit de desarrollo de Azure Stack en Azure | Microsoft Docs
description: Describe cómo registrar Azure Stack en Azure para habilitar la redifusión de Marketplace y los informes de uso.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/11/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: eacf8fc9335af2dacffa3e13da47ea39a2776f2b
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2018
ms.locfileid: "44714571"
---
# <a name="azure-stack-registration"></a>Registro de Azure Stack
La instalación del Kit de desarrollo de Azure Stack se puede registrar en Azure para descargar elementos de Marketplace de Azure y configurar informes de datos comerciales para Microsoft. Es necesario que el registro admita todas funcionalidades de Azure Stack, incluida la redifusión de Marketplace. Dicho registro es aconsejable, porque permite probar una funcionalidad importante de Azure Stack, como es la redifusión de Marketplace y los informes de uso. Después de registrar Azure Stack, se informa del uso para el comercio de Azure. Puede verlo en la suscripción que usó para el registro. Sin embargo, a los usuarios del ASDK no se les cobra ningún uso que comuniquen.

Si no registra su ASDK, puede que vea la alerta de advertencia **Activación necesaria** que le informa que debe registrar el Kit de desarrollo de Azure Stack. Este comportamiento es normal.

## <a name="prerequisites"></a>Requisitos previos
Antes de usar estas instrucciones para registrar el ASDK en Azure, asegúrese de que ha instalado Azure Stack PowerShell y ha descargado las herramientas de Azure Stack como se describe en el artículo de [configuración posterior a la implementación](asdk-post-deploy.md).

Además, el modo de lenguaje de PowerShell se debe establecer en **FullLanguageMode** en el equipo que se usa para registrar el ASDK con Azure. Para comprobar que el modo de lenguaje actual está establecido en FullLanguageMode, abra una ventana de PowerShell con privilegios elevados y ejecute los siguientes comandos de PowerShell:

```PowerShell  
$ExecutionContext.SessionState.LanguageMode
```

Asegúrese de que la salida devuelve **FullLanguageMode**. Si se devuelve cualquier otro modo de lenguaje, el tendrá que ejecutarse en otro equipo o el modo de lenguaje tendrá que establecerse en **FullLanguageMode** antes de continuar.

## <a name="register-azure-stack-with-azure"></a>Registro de Azure Stack con Azure
Siga estos pasos para registrar el ASDK en Azure.

> [!NOTE]
> Todos estos pasos se deben ejecutar desde un equipo que tenga acceso al punto de conexión con privilegios. En el caso del Kit de desarrollo de Azure Stack, este es el equipo host del kit de desarrollo.

1. Abra una consola de PowerShell como administrador.  

2. Ejecute los siguientes comandos de PowerShell para registrar la instalación de ASDK con Azure. Tendrá que iniciar sesión tanto en su suscripción de Azure como en la instalación local de ASDK. Si aún no tiene una suscripción de Azure, puede crear una [cuenta gratuita de Azure aquí](https://azure.microsoft.com/free/?b=17.06). El registro de Azure Stack no supone ningún costo en su suscripción de Azure.<br><br>Si el script de registro se ejecuta en más de una instancia de Azure Stack con el mismo identificador de suscripción de Azure, establezca un nombre único para el registro al ejecutar el cmdlet **Set-AzsRegistration**. El parámetro **RegistrationName** tiene el valor predeterminado **AzureStackRegistration**. Sin embargo, si usa el mismo nombre en más de una instancia de Azure Stack, el script no se ejecutará correctamente.

    ```PowerShell  
    # Add the Azure cloud subscription environment name. 
    # Supported environment names are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
    Add-AzureRmAccount -EnvironmentName "AzureCloud"

    # Register the Azure Stack resource provider in your Azure subscription
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack

    #Import the registration module that was downloaded with the GitHub tools
    Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

    #Register Azure Stack
    $AzureContext = Get-AzureRmContext
    $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
    Set-AzsRegistration `
    -PrivilegedEndpointCredential $CloudAdminCred `
    -PrivilegedEndpoint AzS-ERCS01 `
    -BillingModel Development `
    -RegistrationName "<Unique-name>"
    ```
3. Cuando finalice el script, verá el mensaje **Your environment is now registered and activated using the provided parameters** (El entorno ya está registrado y se ha activado mediante los parámetros proporcionados).

    ![El entorno ya está registrado](media/asdk-register/1.PNG)

## <a name="verify-the-registration-was-successful"></a>Comprobación de que el registro fue correcto
Siga estos pasos para comprobar que el registro de ASDK con Azure se realizó correctamente.

1. Inicie sesión en el [portal de administración de Azure Stack](https://adminportal.local.azurestack.external).

2. Haga clic en **Administración de Marketplace** > **Add from Azure** (Agregar desde Azure).

    ![](media/asdk-register/2.PNG)

3. Si aparece una lista de elementos disponibles de Azure, la activación fue correcta.

    ![](media/asdk-register/3.PNG)

## <a name="move-a-registration-resource"></a>Traslado de un recurso de registro
Se **admite** el traslado de un recurso de registro entre grupos de recursos en la misma suscripción. Para más información sobre cómo mover recursos a un nuevo grupo de recursos, consulte [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources).


## <a name="next-steps"></a>Pasos siguientes
[Incorporación de un elemento de Marketplace de Azure Stack](.\.\azure-stack-marketplace.md)
