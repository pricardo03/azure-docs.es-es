---
title: Creación de entornos de varias máquinas virtuales y recursos de PaaS con plantillas de Azure Resource Manager | Microsoft Docs
description: Obtenga información acerca de cómo crear entornos de varias máquinas virtuales y recursos de PaaS en Azure DevTest Labs a partir de una plantilla de Azure Resource Manager
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2018
ms.author: spelluru
ms.openlocfilehash: 144fd11e9c1ee3e00412320840e864a3190ccdb0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65833978"
---
# <a name="create-multi-vm-environments-and-paas-resources-with-azure-resource-manager-templates"></a>Creación de entornos de varias máquinas virtuales y recursos de PaaS con plantillas de Azure Resource Manager

[Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) le permite [agregar una máquina virtual a la vez a un laboratorio](https://docs.microsoft.com/azure/devtest-lab/devtest-lab-add-vm) fácilmente. Sin embargo, si el entorno contiene varias máquinas virtuales, cada una tiene que crearse por separado. Para escenarios como una aplicación web de varios niveles o una granja de SharePoint, se necesita un mecanismo para permitir la creación de varias máquinas virtuales en un solo paso. Mediante el uso de plantillas de Azure Resource Manager, puede definir ahora la infraestructura y la configuración de la solución de Azure e implementar repetidamente varias máquinas virtuales de manera coherente. Esta característica proporciona las siguientes ventajas:

- Las plantillas de Azure Resource Manager se cargan directamente desde el repositorio de control de código fuente (GitHub o el Git de Azure DevOps Services).
- Con la configuración finalizada, los usuarios pueden crear un entorno al seleccionar una plantilla de Azure Resource Manager desde Azure Portal como lo hacen con otros tipos de [bases de máquinas virtuales](./devtest-lab-comparing-vm-base-image-types.md).
- Los recursos de PaaS de Azure se pueden aprovisionar en un entorno a partir de una plantilla de Azure Resource Manager además de desde las máquinas virtuales de IaaS.
- El coste de los entornos puede controlarse desde el laboratorio además del de las máquinas virtuales individuales creadas por otros tipos de bases de datos.
- Los recursos de PaaS se crean y se muestran en el seguimiento del costo; sin embargo, el cierre automático de máquina virtual no se aplica a ellos.

Más información acerca de las muchas [ventajas del uso de plantillas de Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#the-benefits-of-using-resource-manager) para implementar, actualizar o eliminar todos los recursos de laboratorio en una sola operación.

> [!NOTE]
> Al usar una plantilla de Resource Manager como punto de partida para crear más máquinas virtuales de laboratorio, existen algunas diferencias entre crear una o varias. En el artículo sobre el [uso de una plantilla de máquina virtual de Azure Resource Manager](devtest-lab-use-resource-manager-template.md) se explican estas diferencias con mayor detalle.
>

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="devtest-labs-public-environments"></a>Entornos públicos de DevTest Labs
Azure DevTest Labs tiene un [repositorio público de plantillas de Azure Resource Manager](https://github.com/Azure/azure-devtestlab/tree/master/Environments) que puede usar para crear entornos sin tener que conectarse a un origen externo de GitHub por sí mismo. Este repositorio incluye plantillas usadas con frecuencia, como Azure Web Apps, Clúster de Service Fabric y entorno de desarrollo de Granja de SharePoint. Esta característica es similar al repositorio público de artefactos que se incluye para todos los laboratorios que cree. El repositorio de entorno le permite empezar a trabajar rápidamente con las plantillas de entorno ya creadas, con el mínimo de parámetros de entrada, para proporcionarle una experiencia de introducción sin problemas a los recursos de PaaS en laboratorios. Para más información, consulte [Configuración y uso de entornos públicos en Azure DevTest Labs](devtest-lab-configure-use-public-environments.md).

## <a name="configure-your-own-template-repositories"></a>Configuración de sus propios repositorios de plantilla
Como uno de los procedimientos recomendados con infraestructura como código y configuración como código, las plantillas de entorno deben administrarse con control de código fuente. Azure DevTest Labs sigue este procedimiento y carga todas las plantillas de Azure Resource Manager directamente desde los repositorios de GitHub o el Git de Azure DevOps Services. Como resultado, las plantillas de Resource Manager pueden utilizarse en todo el ciclo de versiones, desde el entorno de prueba hasta el entorno de producción.

Consulte las plantillas creadas por el equipo de DevTest Labs en el [repositorio público de GitHub](https://github.com/Azure/azure-devtestlab/tree/master/Environments). En este repositorio público, puede ver las plantillas compartidas por otros usuarios que puede utilizar directamente o personalizarlas para adaptarlas a sus necesidades. Después de crear la plantilla, almacénela en este repositorio para compartirla con otros usuarios. También puede configurar su propio repositorio de Git con plantillas que se pueden usar para configurar los entornos en la nube. 

Hay un par de reglas que seguir para organizar las plantillas de Azure Resource Manager en un repositorio:

- El archivo de plantilla principal debe llamarse `azuredeploy.json`. 

    ![Archivos de plantilla principal de Azure Resource Manager](./media/devtest-lab-create-environment-from-arm/master-template.png)

- Si desea usar valores de parámetro definidos en un archivo de parámetros, este debe llamarse `azuredeploy.parameters.json`.
- Puede usar los parámetros `_artifactsLocation` y `_artifactsLocationSasToken` para construir el valor de identificador URI parametersLink y permitir a DevTest Labs administrar automáticamente las plantillas anidadas. Para obtener más información, consulte [Implementar plantillas anidadas de Azure Resource Manager para probar entornos](deploy-nested-template-environments.md).
- Los metadatos pueden definirse para especificar el nombre para mostrar y la descripción de la plantilla. Estos metadatos deben estar en un archivo denominado `metadata.json`. El archivo de metadatos de ejemplo siguiente muestra cómo especificar el nombre para mostrar y la descripción: 

    ```json
    { 
        "itemDisplayName": "<your template name>", 
        "description": "<description of the template>" 
    }
    ```

Los siguientes pasos le ayudarán a agregar un repositorio al laboratorio a través de Azure Portal. 

1. Inicie sesión en el [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Seleccione **Todos los servicios** y, luego, **DevTest Labs** en la lista.
1. En la lista de laboratorios, seleccione el laboratorio que desee.   
1. En el panel **Introducción** del laboratorio, seleccione **Configuración y directivas**.

    ![Directivas y configuración](./media/devtest-lab-create-environment-from-arm/configuration-and-policies-menu.png)

1. Desde la lista de configuración **Configuration and Policies** (Directivas y configuración), seleccione **Repositories** (Repositorios). En el panel **Repositorios** se enumeran los repositorios que se han agregado al laboratorio. Un repositorio denominado `Public Repo` se genera automáticamente para todos los laboratorios y se conecta al [repositorio de GitHub de DevTest Labs](https://github.com/Azure/azure-devtestlab), que contiene varios artefactos de máquina virtual para su uso.

    ![Repositorio público](./media/devtest-lab-create-environment-from-arm/public-repo.png)

1. Seleccione **+ Add** (+ Agregar) para agregar el repositorio de plantillas de Azure Resource Manager.
1. Cuando se abra el segundo panel **Repositorios**, escriba la información necesaria como se indica a continuación:
    - **Name** (Nombre): nombre del repositorio que se utiliza en el laboratorio.
    - **URL de clonación de Git**: escriba la URL de clonación HTTPS de GIT de GitHub o Azure DevOps Services.  
    - **Branch** (Rama): nombre de la rama para acceder a las definiciones de plantilla de Azure Resource Manager. 
    - **Personal access token** (Token de acceso personal): se usa para acceder de forma segura al repositorio. Para obtener el token de Azure DevOps Services, seleccione **&lt;SuNombre> > Mi perfil > Seguridad > Public access token** (Token de acceso público). Para obtener el token de GitHub, seleccione su avatar seguido de la selección **Configuración > Public access token** (Token de acceso público). 
    - **Rutas de acceso de carpeta**: en uno de los dos campos de entrada, escriba la ruta de acceso de carpeta que comienza con una barra diagonal (/) correspondiente a la URI de clonación de Git de cualquiera de las definiciones de artefacto (primer campo de entrada) o las definiciones de plantilla de Azure Resource Manager.   
    
        ![Repositorio público](./media/devtest-lab-create-environment-from-arm/repo-values.png)


1. Una vez rellenos y validados todos los campos obligatorios, seleccione **Save** (Guardar).

La siguiente sección le guiará a través de la creación de entornos a partir de una plantilla de Azure Resource Manager.

## <a name="create-an-environment-from-a-resource-manager-template-using-the-azure-portal"></a>Creación de un entorno a partir de una plantilla de Resource Manager con Azure Portal

Una vez configurado un repositorio de plantillas de Azure Resource Manager en el laboratorio, los usuarios de este pueden crear un entorno a través de Azure Portal mediante los pasos siguientes:

1. Inicie sesión en el [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Seleccione **Todos los servicios** y, luego, **DevTest Labs** en la lista.
1. En la lista de laboratorios, seleccione el laboratorio que desee.   
1. En el panel del laboratorio, seleccione **+ Agregar**.
1. En el panel **Elegir una base** se muestran las imágenes de las bases de datos que se pueden usar con las plantillas de Azure Resource Manager en primer lugar. Seleccione la plantilla de Azure Resource Manager que desee.

    ![Elegir base de datos](./media/devtest-lab-create-environment-from-arm/choose-a-base.png)
  
1. En el panel **Agregar**, escriba el valor de **Nombre del entorno**. El nombre del entorno es lo que se muestra a los usuarios del laboratorio. Los campos de entrada restantes se definen en la plantilla de Azure Resource Manager. Si se han definido valores predeterminados en la plantilla o el archivo `azuredeploy.parameter.json` está presente, en esos campos de entrada se muestran los valores predeterminados. Para los parámetros de tipo *cadena segura*, puede usar los secretos almacenados en el almacén de claves de Azure. Para información sobre cómo guardar secretos en un almacén de claves y utilizarlos al crear recursos de laboratorio, consulte [Store secrets in Azure Key Vault](devtest-lab-store-secrets-in-key-vault.md) (Almacenamiento de secretos en un almacén de claves de Azure).  

    ![Agregar panel](./media/devtest-lab-create-environment-from-arm/add.png)

    > [!NOTE]
    > Hay varios valores de parámetro que, aunque se especifiquen, aparecen vacíos. Por lo tanto, si los usuarios asignan dichos valores a parámetros de una plantilla de Azure Resource Manager, DevTest Labs no muestra los valores. En su lugar, se muestran los campos de entrada en blanco donde los usuarios del laboratorio deben especificar un valor al crear el entorno.
    > 
    > - GEN-UNIQUE
    > - GEN-UNIQUE-[N]
    > - GEN-SSH-PUB-KEY
    > - GEN-PASSWORD 
 
1. Seleccione **Add** (Agregar) para crear el entorno. El entorno empieza el aprovisionamiento inmediatamente y el estado se muestra en la lista **My virtual machines** (Mis máquinas virtuales). El laboratorio crea automáticamente un grupo de recursos nuevo para aprovisionar todos los recursos definidos en la plantilla de Azure Resource Manager.
1. Una vez creado el entorno, selecciónelo en la lista **Mis máquinas virtuales** para abrir el panel del grupo de recursos y examinar todos los recursos aprovisionados en el entorno.
    
    ![Lista My virtual machines (Mis máquinas virtuales)](./media/devtest-lab-create-environment-from-arm/all-environment-resources.png)
   
   También puede expandir el entorno para ver solo la lista de máquinas virtuales aprovisionadas en el entorno.
    
    ![Lista My virtual machines (Mis máquinas virtuales)](./media/devtest-lab-create-environment-from-arm/my-vm-list.png)

1. Haga clic en cualquiera de los entornos para ver las acciones disponibles, por ejemplo, se puede aplicar artefactos, adjuntar discos de datos, cambiar el tiempo de apagado automático y mucho más.

    ![Acciones de entorno](./media/devtest-lab-create-environment-from-arm/environment-actions.png)

## <a name="automate-deployment-of-environments"></a>Automatizar la implementación de entornos
Azure DevTest Labs ofrece la posibilidad de usar una [plantilla de administración de Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) para crear un entorno con un conjunto de recursos en el laboratorio. Estos entornos pueden contener los recursos de Azure que se pueden crear mediante plantillas de Resource Manager. Los entornos de DevTest Labs permiten a los usuarios implementar fácilmente infraestructuras complejas de forma coherente dentro de los confines del laboratorio. Actualmente, es posible agregar un entorno a un laboratorio mediante Azure Portal una vez creado, pero en una situación de desarrollo o de pruebas, donde se producen varias creaciones, una implementación automatizada permite una mejor experiencia.

Complete los siguientes pasos de la sección [Configurar sus propios repositorios de plantilla](#configure-your-own-template-repositories) antes de continuar: 

1. Cree la plantilla de Resource Manager que define los recursos que se van a crear. 
2. Configure la plantilla de Resource Manager en un repositorio de Git. 
3. Conecte el repositorio de Git al laboratorio. 

### <a name="powershell-script-to-deploy-the-resource-manager-template"></a>Script de PowerShell para implementar la plantilla de Resource Manager
Guarde el script de PowerShell en la siguiente sección del disco duro (por ejemplo: deployenv.ps1) y ejecute el script después de especificar valores para SubscriptionId, ResourceGroupName, LabName, RepositoryName y TemplateName (carpeta) en el repositorio de Git, EnvironmentName.

```powershell
./deployenv.ps1 -SubscriptionId "000000000-0000-0000-0000-0000000000000" -LabName "mydevtestlab" -ResourceGroupName "mydevtestlabRG994248" -RepositoryName "SP Repository" -TemplateName "My Environment template name" -EnvironmentName "SPResourceGroupEnv"  
```

#### <a name="sample-script"></a>Script de ejemplo
Este es el script de ejemplo para crear un entorno en el laboratorio. Los comentarios del script ayudan a comprender mejor la secuencia de comandos. 

```powershell
#Requires -Module Az.Resources

[CmdletBinding()]

param (
# ID of the Azure Subscription where the lab is created.
[string] [Parameter(Mandatory=$true)] $SubscriptionId,

# Name of the lab (existing) in which to create the environment.
[string] [Parameter(Mandatory=$true)] $LabName,

# Name of the connected repository in the lab. 
[string] [Parameter(Mandatory=$true)] $RepositoryName,

# Name of the template (folder name in the Git repository) based on which the environment will be created.
[string] [Parameter(Mandatory=$true)] $TemplateName,

# Name of the environment to be created in the lab.
[string] [Parameter(Mandatory=$true)] $EnvironmentName,

# The parameters to be passed to the template. Each parameter is prefixed with “-param_”. 
# For example, if the template has a parameter named “TestVMName” with a value of “MyVMName”, the string in $Params will have the form: `-param_TestVMName MyVMName`. 
# This convention allows the script to dynamically handle different templates.
[Parameter(ValueFromRemainingArguments=$true)]
    $Params
)

# Save this script as the deployenv.ps1 file
# Run the script after you specify values for SubscriptionId, ResourceGroupName, LabName, RepositoryName, TemplateName (folder) in the Git repo, EnvironmentName
# ./deployenv.ps1 -SubscriptionId "000000000-0000-0000-0000-0000000000000" -LabName "mydevtestlab" -ResourceGroupName "mydevtestlabRG994248" -RepositoryName "SP Repository" -TemplateName "My Environment template name" -EnvironmentName "SPResourceGroupEnv"    

# Comment this statement to completely automate the environment creation.    
# Sign in to Azure. 
Connect-AzAccount

# Select the subscription that has the lab.  
Set-AzContext -SubscriptionId $SubscriptionId | Out-Null

# Get information about the user, specifically the user ID, which is used later in the script.  
$UserId = $((Get-AzADUser -UserPrincipalName (Get-AzContext).Account).Id.Guid)
        
# Get information about the lab such as lab location. 
$lab = Get-AzResource -ResourceType "Microsoft.DevTestLab/labs" -Name $LabName -ResourceGroupName $ResourceGroupName 
if ($lab -eq $null) { throw "Unable to find lab $LabName in subscription $SubscriptionId." } 
    
# Get information about the repository in the lab. 
$repository = Get-AzResource -ResourceGroupName $lab.ResourceGroupName `
    -ResourceType 'Microsoft.DevTestLab/labs/artifactsources' `
    -ResourceName $LabName `
    -ApiVersion 2016-05-15 `
    | Where-Object { $RepositoryName -in ($_.Name, $_.Properties.displayName) } `
    | Select-Object -First 1
if ($repository -eq $null) { throw "Unable to find repository $RepositoryName in lab $LabName." } 

# Get information about the Resource Manager template based on which the environment will be created. 
$template = Get-AzResource -ResourceGroupName $lab.ResourceGroupName `
    -ResourceType "Microsoft.DevTestLab/labs/artifactSources/armTemplates" `
    -ResourceName "$LabName/$($repository.Name)" `
    -ApiVersion 2016-05-15 `
    | Where-Object { $TemplateName -in ($_.Name, $_.Properties.displayName) } `
    | Select-Object -First 1
if ($template -eq $null) { throw "Unable to find template $TemplateName in lab $LabName." } 

# Build the template parameters with parameter name and values.     
$parameters = Get-Member -InputObject $template.Properties.contents.parameters -MemberType NoteProperty | Select-Object -ExpandProperty Name
$templateParameters = @()

# The custom parameters need to be extracted from $Params and formatted as name/value pairs.
$Params | ForEach-Object {
    if ($_ -match '^-param_(.*)' -and $Matches[1] -in $parameters) {
        $name = $Matches[1]                
    } elseif ( $name ) {
        $templateParameters += @{ "name" = "$name"; "value" = "$_" }
        $name = $null #reset name variable
    }
}

# Once name/value pairs are isolated, create an object to hold the necessary template properties
$templateProperties = @{ "deploymentProperties" = @{ "armTemplateId" = "$($template.ResourceId)"; "parameters" = $templateParameters }; } 

# Now, create or deploy the environment in the lab by using the New-AzResource command. 
New-AzResource -Location $Lab.Location `
    -ResourceGroupName $lab.ResourceGroupName `
    -Properties $templateProperties `
    -ResourceType 'Microsoft.DevTestLab/labs/users/environments' `
    -ResourceName "$LabName/$UserId/$EnvironmentName" `
    -ApiVersion '2016-05-15' -Force 
 
Write-Output "Environment $EnvironmentName completed."
```

También puede usar la CLI de Azure para implementar recursos con plantillas de Resource Manager. Para más información, consulte [Implementación de recursos con plantillas de Resource Manager y la CLI de Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-cli).

> [!NOTE]
> Solo un usuario con permisos de propietario de laboratorio puede crear máquinas virtuales desde una plantilla de Resource Manager con Azure PowerShell. Si desea automatizar la creación de máquinas virtuales mediante una plantilla de Resource Manager y solo tiene permisos de usuario, puede usar el comando [ **az lab vm create** de la CLI ](https://docs.microsoft.com/cli/azure/lab/vm#az-lab-vm-create).

### <a name="general-limitations"></a>Limitaciones generales 

Tenga en cuenta estas limitaciones cuando use una plantilla de Resource Manager en DevTest Labs:

- Cualquier plantilla de Resource Manager que cree no puede hacer referencia a los recursos existentes; solo puede hacer referencia a los nuevos recursos. Además, si tiene una plantilla de Resource Manager existente que normalmente implementa fuera de DevTest Labs y contiene referencias a los recursos existentes, no puede usarla en el laboratorio.

   La única excepción a eso es que **puede** hacer referencia a una red virtual existente. 

- No se pueden crear fórmulas desde las máquinas virtuales de laboratorio que se crearon a partir de una plantilla de Resource Manager. 

- No se pueden crear imágenes personalizadas desde las máquinas virtuales de laboratorio que se crearon a partir de una plantilla de Resource Manager. 

- La mayoría de las directivas no se evalúan al implementar plantillas de Resource Manager.

   Por ejemplo, es posible que tenga una directiva de laboratorio que especifique que un usuario solo puede crear cinco máquinas virtuales. Sin embargo, un usuario puede implementar una plantilla de Resource Manager que cree docenas de máquinas virtuales. Entre las directivas que no se evalúan se incluyen:

   - Número de máquinas virtuales por usuario
   - Número de máquinas virtuales premium por usuario de laboratorio
   - Número de discos premium por usuario de laboratorio


### <a name="configure-environment-resource-group-access-rights-for-lab-users"></a>Configuración de los derechos de acceso a grupos de recursos de entorno para los usuarios del laboratorio

Los usuarios del laboratorio pueden implementar una plantilla de Resource Manager. Sin embargo, de forma predeterminada, tienen derechos de acceso de lectura, lo que significa que no pueden cambiar los recursos de un grupo de recursos del entorno. Por ejemplo, no pueden detener ni iniciar sus recursos.

Siga estos pasos para conceder derechos de acceso de colaborador a los usuarios del laboratorio. A continuación, cuando implementen una plantilla de Resource Manager, podrán editar los recursos en ese entorno. 


1. En el panel **Introducción** del laboratorio, seleccione **Configuración y directivas**.
1. Seleccione **Configuración del laboratorio**.
1. En el panel Configuración de laboratorio, seleccione **Colaborador** para conceder permisos de escritura a los usuarios del laboratorio.

    ![Configuración de los derechos de acceso de los usuarios del laboratorio](./media/devtest-lab-create-environment-from-arm/configure-access-rights.png)

1. Seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes
* Una vez creada la máquina virtual, puede conectarse a esta al seleccionar **Conectar** en el panel de administración de la máquina virtual.
* Consulte y administre recursos en un entorno al seleccionarlo de la lista **My virtual machines** (Mis máquinas virtuales) del laboratorio. 
* Explore las [plantillas de Azure Resource Manager de la galería de plantillas de inicio rápido de Azure](https://github.com/Azure/azure-quickstart-templates).
