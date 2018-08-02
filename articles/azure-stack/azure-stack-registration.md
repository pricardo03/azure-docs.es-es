---
title: Registro de Azure para sistemas integrados en Azure Stack | Microsoft Docs
description: Describe el proceso de registro de Azure para implementaciones de varios nodos con conexión a Azure en Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2018
ms.author: jeffgilb
ms.reviewer: brbartle
ms.openlocfilehash: e73cd24064f2b0d6197a69251b55639d41e3212c
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2018
ms.locfileid: "39357755"
---
# <a name="register-azure-stack-with-azure"></a>Registro de Azure Stack con Azure

Registrar [Azure Stack](azure-stack-poc.md) en Azure le permite descargar elementos de Marketplace de Azure y configurar informes de datos comerciales para Microsoft. Después de registrar Azure Stack, el uso se informa a Azure Commerce, y puede verlo en la suscripción usada para el registro.

> [!IMPORTANT]  
> Es necesario que el registro admita todas funcionalidades de Azure Stack, incluida la redifusión de Marketplace. Además, infringirá los términos de licencia de Azure Stack si no se registra al usar el modelo de facturación de pago por uso. Para más información acerca de los modelos de licencia de Azure Stack, consulte la [página de compra](https://azure.microsoft.com/overview/azure-stack/how-to-buy/).

## <a name="prerequisites"></a>Requisitos previos

Antes de registrar Azure Stack con Azure, debe tener:

- El identificador de suscripción de una suscripción de Azure. Para obtener el identificador, inicie sesión en Azure, haga clic en **Más servicios** > **Suscripciones**, haga clic en la suscripción que desee usar y, en **Essentials**, encontrará el identificador de la suscripción.

  > [!NOTE]
  > Actualmente no se admiten las suscripciones en la nube en los gobiernos de Alemania y EE. UU.

- El nombre de usuario y la contraseña de una cuenta que sea propietaria de la suscripción (se admiten las cuentas MSA/2FA).
- Haber registrado el proveedor de recursos de Azure Stack (para más información, consulte la sección Registro de un proveedor de recursos de Azure Stack).

Si no tiene una suscripción de Azure que cumpla estos requisitos, puede [crear una cuenta gratuita de Azure aquí](https://azure.microsoft.com/free/?b=17.06). El registro de Azure Stack no supone ningún costo en su suscripción de Azure.

### <a name="powershell-language-mode"></a>Modo de lenguaje de PowerShell

Para registrar correctamente Azure Stack, el modo de lenguaje de PowerShell debe establecerse en **FullLanguageMode**.  Para comprobar que el modo de lenguaje actual está establecido en FullLanguageMode, abra una ventana de PowerShell con privilegios elevados y ejecute los siguientes comandos de PowerShell:

```PowerShell  
$ExecutionContext.SessionState.LanguageMode
```

Asegúrese de que la salida devuelve **FullLanguageMode**. Si se devuelve cualquier otro modo de lenguaje, el tendrá que ejecutarse en otro equipo o el modo de lenguaje tendrá que establecerse en **FullLanguageMode** antes de continuar.

### <a name="bkmk_powershell"></a>Instalación de PowerShell para Azure Stack

Para registrarse en Azure, es preciso usar la versión más reciente de PowerShell para Azure Stack.

Si no está instalado aún, [instale PowerShell para Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install).

### <a name="bkmk_tools"></a>Descarga de las herramientas de Azure Stack

El repositorio de GitHub de las herramientas de Azure Stack contiene módulos de PowerShell que admiten la funcionalidad de Azure Stack; lo que incluye la funcionalidad de registro. Durante el proceso de registro es preciso que importe y use el módulo de PowerShell RegisterWithAzure.psm1, que se encuentra en el repositorio de herramientas de Azure Stack, para registrar la instancia de Azure Stack en Azure.

Para asegurarse de que usa la versión más reciente, debe eliminar las versiones existentes de las herramientas de Azure Stack y [descargar la versión más reciente de GitHub](azure-stack-powershell-download.md) antes de registrarse en Azure.

## <a name="register-azure-stack-in-connected-environments"></a>Registro de Azure Stack en entornos conectados

Los entornos conectados pueden acceder a Internet y a Azure. Para estos entornos, es preciso registrar el proveedor de recursos de Azure Stack en Azure y, después, configurar el modelo de facturación.

> [!NOTE]
> Todos estos pasos se deben ejecutar desde un equipo que tenga acceso al punto de conexión con privilegios.

### <a name="register-the-azure-stack-resource-provider"></a>Registro del proveedor de recursos de Azure Stack

Para registrar el proveedor de recursos de Azure Stack en Azure, inicie PowerShell ISE como administrador y use los siguientes comandos de PowerShell con el parámetro **EnvironmentName** establecido en el tipo de suscripción a Azure apropiad (a continuación puede ver los parámetros).

1. Agregue la cuenta de Azure que usará para registrar Azure Stack. Para agregar la cuenta, ejecute el cmdlet **Add-AzureRmAccount**. Se le pedirá que escriba sus credenciales de cuenta de administrador global de Azure, y puede que tenga que utilizar la autenticación en dos fases en función de la configuración de la cuenta.

   ```PowerShell  
      Add-AzureRmAccount -EnvironmentName "<Either AzureCloud or AzureChinaCloud>"
   ```

   | Parámetro | DESCRIPCIÓN |  
   |-----|-----|
   | EnvironmentName | El nombre de entorno de suscripción de nube de Azure. Los nombres de entorno que se admiten son **AzureCloud** o, si usa una suscripción a Azure de China, **AzureChinaCloud**.  |
   |  |  |

2. Si tiene varias suscripciones, ejecute el siguiente comando para seleccionar la suscripción con la que quiera trabajar:  

   ```PowerShell  
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

3. Ejecute el siguiente comando para registrar el proveedor de recursos de Azure Stack en su suscripción de Azure:

   ```PowerShell  
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

### <a name="register-azure-stack-with-azure-using-the-pay-as-you-use-billing-model"></a>Registro de Azure Stack en Azure mediante el modelo de facturación de pago por uso

Use estos pasos para registrar Azure Stack en Azure mediante el modelo de facturación de pago por uso.

1. Inicie PowerShell ISE como administrador y vaya a la carpeta **Registration**del directorio **AzureStack-Tools-master** que creó cuando [descargó las herramientas de Azure Stack](#bkmk_tools). Importe el módulo **RegisterWithAzure.psm1** mediante PowerShell:

   ```PowerShell  
   Import-Module .\RegisterWithAzure.psm1
   ```

2. A continuación, en la misma sesión de PowerShell, asegúrese de que ha iniciado sesión en el contexto correcto de Azure PowerShell. Se trata de la cuenta de Azure que se usó para registrar el proveedor de recursos de Azure Stack anterior. PowerShell que se ejecuta:

   ```PowerShell  
   Add-AzureRmAccount -Environment "<Either AzureCloud or AzureChinaCloud>"
   ```

3. En la misma sesión de PowerShell, ejecute el cmdlet **Set-AzsRegistration**. PowerShell que se ejecuta:  

   ```PowerShell  
   $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
   $RegistrationName = "<unique-registration-name>"
   Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
      -BillingModel PayAsYouUse `
      -RegistrationName $RegistrationName
   ```

  |Parámetro|DESCRIPCIÓN|
  |-----|-----|
  |PrivilegedEndpointCredential|Las credenciales que se utilizan para [acceder al punto de conexión con privilegios](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). El nombre de usuario tiene el formato **dominioDeAzureStack\AdminNube**.|
  |PrivilegedEndpoint|Es una consola remota de PowerShell previamente configurada que proporciona funcionalidades tales como la recopilación de registros y otras tareas posteriores a la implementación. Para más información, consulte el artículo acerca del [uso del punto de conexión con privilegios](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint).|
  |BillingModel|Es el modelo de facturación que usa la suscripción. Los valores que se permiten en este parámetro son: Capacity, PayAsYouUse y Development.|
  | RegistrationName | Establezca un nombre único para el registro si ejecuta el script de registro en más de una instancia de Azure Stack mediante el mismo identificador de suscripción de Azure. El valor predeterminado del parámetro es **AzureStackRegistration**. Sin embargo, si usa el mismo nombre en más de una instancia de Azure Stack, el script no se ejecutará correctamente. |

  El proceso tardará entre 10 y 15 minutos. Cuando finalice el comando, verá el mensaje **"Your environment is now registered and activated using the provided parameters."** (El entorno ya está registrado y se ha activado mediante los parámetros proporcionados)

### <a name="register-azure-stack-with-azure-using-the-capacity-billing-model"></a>Registro de Azure Stack en Azure mediante el modelo de facturación de capacidad

Siga las mismas instrucciones que usó para realizar el registro mediante el modelo de facturación de pago por uso, pero agregue el número de contrato con la que se adquirió la capacidad y cambie el parámetro **BillingModel** a **Capacity**. Los restantes parámetros son opcionales.

PowerShell que se ejecuta:

```PowerShell  
$CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
$RegistrationName = "<unique-registration-name>"
Set-AzsRegistration `
    -PrivilegedEndpointCredential $CloudAdminCred `
    -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
    -AgreementNumber <EA agreement number> `
    -BillingModel Capacity `
    -RegistrationName $RegistrationName
```

## <a name="register-azure-stack-in-disconnected-environments"></a>Registro de Azure Stack en entornos sin conexión
Si va a registrar Azure Stack en un entorno desconectado (sin conexión a Internet), tendrá que obtener un token de registro del entorno de Azure Stack y, después, utilizarlo en un equipo que puede conectarse a Azure y que tenga [PowerShell para Azure Stack instalado](#bkmk_powershell).  

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Obtención de un token de registro del entorno de Azure Stack

1. Inicie PowerShell ISE como administrador y vaya a la carpeta **Registration**del directorio **AzureStack-Tools-master** que creó cuando [descargó las herramientas de Azure Stack](#bkmk_tools). Importe el módulo **RegisterWithAzure.psm1**:  

   ```PowerShell  
   Import-Module .\RegisterWithAzure.psm1
   ```

2. Para obtener un token de registro, ejecute los siguientes comandos de PowerShell:  

   ```Powershell
   $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
   $RegistrationToken = Get-AzsRegistrationToken -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<EA agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
   ```

   > [!TIP]  
   > El token del registro se guarda en el archivo especificado para *$FilePathForRegistrationToken*. Puede cambiar la ruta del archivo o el nombre de archivo a su entera discreción.

3. Guarde este token de registro para usarlo en la máquina conectada a Azure. El archivo o el texto se pueden copiar de $FilePathForRegistrationToken.

### <a name="connect-to-azure-and-register"></a>Conexión a Azure y posterior registro

En el equipo que esté conectado a Internet, siga los mismos pasos para importar el módulo de RegisterWithAzure.psm1 e inicie sesión en el contexto correcto de Azure Powershell. Luego, llame a Register-AzsEnvironment. Especifique el token de registro que desea registrar en Azure. Si va a registrar más de una instancia de Azure Stack mediante el mismo identificador de suscripción de Azure, especifique un nombre de registro único. Ejecute el siguiente cmdlet:

  ```PowerShell  
  $registrationToken = "<Your Registration Token>"
  $RegistrationName = "<unique-registration-name>"
  Register-AzsEnvironment -RegistrationToken $registrationToken  -RegistrationName $RegistrationName
  ```

Si lo desea, puede usar el cmdlet Get-Content para señalar a un archivo que contenga el token de registro:

  ```PowerShell  
  $registrationToken = Get-Content -Path '<Path>\<Registration Token File>'
  Register-AzsEnvironment -RegistrationToken $registrationToken -RegistrationName $RegistrationName
  ```

  > [!NOTE]  
  > Guarde el nombre de recurso de registro y el token de registro para futuras referencias.

### <a name="retrieve-an-activation-key-from-azure-registration-resource"></a>Recuperación de una clave de activación del recurso de registro de Azure

A continuación, debe recuperar una clave de activación del registro de recurso creado en Azure durante la operación de Register-AzsEnvironment.

Para obtener la clave de activación, ejecute los siguientes comandos de PowerShell:  

  ```Powershell
  $RegistrationResourceName = "AzureStack-<Cloud Id for the Environment to register>"
  $KeyOutputFilePath = "$env:SystemDrive\ActivationKey.txt"
  $ActivationKey = Get-AzsActivationKey -RegistrationName $RegistrationResourceName -KeyOutputFilePath $KeyOutputFilePath
  ```

  > [!TIP]  
  > La clave de activación se guarda en el archivo especificado para *$KeyOutputFilePath*. Puede cambiar la ruta del archivo o el nombre de archivo a su entera discreción.

### <a name="create-an-activation-resource-in-azure-stack"></a>Crear un recurso de activación en Azure Stack

Vuelva al entorno de Azure Stack con el archivo o el texto de la clave de activación creada a partir de Get-AzsActivationKey. Luego, creará un recurso de activación en Azure Stack con dicha clave de activación. Para crear un recurso de activación, ejecute los siguientes comandos de PowerShell:  

  ```Powershell
  $ActivationKey = "<activation key>"
  New-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -ActivationKey $ActivationKey
  ```

Si lo desea, puede usar el cmdlet Get-Content para señalar a un archivo que contenga el token de registro:

  ```Powershell
  $ActivationKey = Get-Content -Path '<Path>\<Activation Key File>'
  New-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -ActivationKey $ActivationKey
  ```

## <a name="verify-azure-stack-registration"></a>Comprobación del registro de Azure Stack

Siga estos pasos para comprobar que Azure Stack se ha registrado correctamente en Azure.

1. Inicie sesión en el [portal de administrador](https://docs.microsoft.com/azure/azure-stack/azure-stack-manage-portals#access-the-administrator-portal) de Azure Stack: https&#58;//adminportal.*&lt;región>.&lt;fqdn>*.
2. Seleccione **Más servicios** > **Marketplace Management** (Administración de Marketplace)  > **Add from Azure** (Agregar desde Azure).

Si aparece una lista de elementos disponibles de Azure (como WordPress), la activación fue correcta. Sin embargo, en entornos desconectados no verá los elementos del Marketplace de Azure en el Marketplace de Azure Stack.

> [!NOTE]
> Una vez completado el registro, ya no aparecerá la advertencia activa para no registrar.

## <a name="renew-or-change-registration"></a>Renovación o cambio de registro

### <a name="renew-or-change-registration-in-connected-environments"></a>Renovación o cambio de registro en entornos conectados

El registro se tendrá que actualizar o renovar en las siguientes circunstancias:

- Después de renovar su suscripción anual basada en capacidad.
- Al cambiar el modelo de facturación.
- Cuando se escalan cambios (se agregan o quitan nodos) para la facturación por capacidad.

#### <a name="change-the-subscription-you-use"></a>Cambio de la suscripción que se usa

Si desea cambiar la suscripción que utiliza, primero debe ejecutar el cmdlet **Remove-AzsRegistration**, después, asegurarse de que inicia una sesión en el contexto correcto de Azure PowerShell y, finalmente, ejecutar **Set-AzsRegistration** con cualquier parámetro c cambiado:

  ```PowerShell  
  Remove-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  Set-AzureRmContext -SubscriptionId $NewSubscriptionId
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse -RegistrationName $RegistrationName
  ```

#### <a name="change-the-billing-model-or-syndication-features"></a>Cambio del modelo de facturación o de las características de redifusión

Si desea cambiar el modelo de facturación o las características de redifusión de la instalación, puede llamar a la función de registro para establecer los valores nuevos. No es preciso eliminar antes la configuración actual:

  ```PowerShell  
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse -RegistrationName $RegistrationName
  ```

### <a name="renew-or-change-registration-in-disconnected-environments"></a>Renovación o cambio de registro en entornos sin conexión

El registro se tendrá que actualizar o renovar en las siguientes circunstancias:

- Después de renovar su suscripción anual basada en capacidad.
- Al cambiar el modelo de facturación.
- Cuando se escalan cambios (se agregan o quitan nodos) para la facturación por capacidad.

#### <a name="remove-the-activation-resource-from-azure-stack"></a>Eliminación del recurso de activación de Azure Stack

En primer lugar, tendrá que eliminar el recurso de activación de Azure Stack y, después, el recurso de registro de Azure.  

Para quitar el recurso de activación de Azure Stack, ejecute los siguientes comandos de PowerShell en su entorno de Azure Stack:  

  ```Powershell
  Remove-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  ```

A continuación, para quitar el recurso de registro en Azure, asegúrese de que está en un equipo con conexión a Azure, inicie sesión en el contexto correcto de Azure PowerShell y ejecute los comandos de PowerShell adecuados, como se describe a continuación.

Puede usar el token de registro que se usó para crear el recurso:  

  ```Powershell
  $registrationToken = "<registration token>"
  Unregister-AzsEnvironment -RegistrationToken $registrationToken
  ```

O bien, puede usar el nombre de registro:

  ```Powershell
  $registrationName = "AzureStack-<Cloud Id of Azure Stack Environment>"
  Unregister-AzsEnvironment -RegistrationName $registrationName
  ```

### <a name="re-register-using-disconnected-steps"></a>Segundo registro mediante pasos desconectados

Ya ha anulado completamente el registro en un escenario sin conexión y debe repetir los pasos para registrar un entorno de Azure Stack en un escenario sin conexión.

## <a name="next-steps"></a>Pasos siguientes

[Descargar elementos de Marketplace desde Azure](azure-stack-download-azure-marketplace-item.md)
