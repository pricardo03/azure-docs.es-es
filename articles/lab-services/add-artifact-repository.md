---
title: Agregar un repositorio de artefactos al laboratorio en Azure DevTest Labs | Microsoft Docs
description: Obtenga información sobre cómo agregar un repositorio de artefactos al laboratorio en Azure DevTest labs.
services: devtest-lab
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2019
ms.author: spelluru
ms.openlocfilehash: 42b59c8da9e66b0f749d067e3df867911c7108e9
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58106838"
---
# <a name="add-an-artifact-repository-to-your-lab-in-devtest-labs"></a>Agregar un repositorio de artefactos al laboratorio en DevTest Labs
DevTest Labs le permite especificar un artefacto que se agregarán a una máquina virtual en el momento de crear la máquina virtual o una vez creada la máquina virtual. Este artefacto podría ser una herramienta o una aplicación que desea instalar en la máquina virtual. Los artefactos se definen en un archivo JSON cargado desde un repositorio Git de VSTS o GitHub. 

El [repositorio de artefactos público](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts), mantenida por DevTest Labs, proporciona muchas herramientas comunes para Windows y Linux. Automáticamente se agrega un vínculo a este repositorio al laboratorio. Puede crear su propio repositorio de artefactos con las herramientas específicas que no están disponibles en el repositorio de artefactos público. Para obtener información acerca de cómo crear artefactos personalizados, consulte [crear artefactos personalizados](devtest-lab-artifact-author.md).

En este artículo se proporciona información sobre cómo agregar el repositorio de artefactos personalizados mediante Azure portal, plantillas de administración de recursos de Azure y Azure PowerShell. Puede automatizar la adición de un repositorio de artefactos a un laboratorio mediante la escritura de scripts de PowerShell o CLI. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Requisitos previos
Para agregar un repositorio al laboratorio, obtenga cierta información clave del repositorio. Las secciones siguientes describen cómo obtener la información necesaria para los repositorios que se hospedan en **GitHub** o **Azure DevOps**.

### <a name="get-the-github-repository-clone-url-and-personal-access-token"></a>Obtención de la dirección URL de clonación del repositorio de GitHub y el token de acceso personal

1. Vaya a la página principal del repositorio de GitHub que contiene las definiciones de artefacto o de la plantilla de Azure Resource Manager.
2. Seleccione **Clone or download**(Clonar o descargar).
3. Para copiar la dirección URL en el Portapapeles, seleccione el botón **Dirección URL de clonación de HTTPS**. Guarde la dirección URL para su uso posterior.
4. En la esquina superior derecha de GitHub, seleccione la imagen del perfil y, después, seleccione **Configuración**.
5. En el **configuración Personal** menú de la izquierda, seleccione **opciones del desarrollador**.
6. Seleccione **tokens de acceso Personal** en el menú izquierdo.
7. Seleccione **Generar nuevo token**.
8. En la página **New personal access token** (Nuevo token de acceso personal), bajo **Descripción de token** escriba una descripción. Acepte los elementos predeterminados en **Seleccionar ámbitos** y, a continuación, seleccione **Generar token**.
9. Guarde el token generado. Usará el token más adelante.
10. Cierre GitHub.   

### <a name="get-the-azure-repos-clone-url-and-personal-access-token"></a>Obtención de la dirección URL de clonación de Azure Repos y el token de acceso personal
1. Vaya a la página principal de la colección de equipo (por ejemplo, https://contoso-web-team.visualstudio.com)) y después seleccione el proyecto.
2. En la página de inicio del proyecto, seleccione **Código**.
3. Para ver la dirección URL de clonación, en la página **Código** del proyecto, seleccione **Clonar**.
4. Guarde la dirección URL. Usará la dirección URL más adelante.
5. Para crear un token de acceso personal, en el menú desplegable de la cuenta de usuario, seleccione **Mi perfil**.
6. En la página de información de perfil, seleccione **Seguridad**.
7. En la pestaña **Seguridad**, seleccione **Agregar**.
8. En la página **Crear un token de acceso personal**:
   1. Escriba la información que desee en **Descripción** para el token.
   2. En la lista **Expira en**, seleccione **180 días**.
   3. En la lista **Cuentas accesibles**, seleccione **Todas las cuentas accesibles**.
   4. Seleccione la opción **Todos los ámbitos**.
   5. Seleccione **Crear token**.
9. El nuevo token aparece en la lista **Tokens de acceso personal** . Seleccione **token**y luego guarde el valor del token para usarlo más adelante.
10. Continuar al conectar el laboratorio a la sección de repositorio.

## <a name="use-azure-portal"></a>Usar Azure Portal
Esta sección proporciona los pasos necesarios para agregar un repositorio de artefactos a un laboratorio en Azure portal. 

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **Más servicios** y, luego, **DevTest Labs** en la lista de servicios.
3. En la lista de laboratorios, seleccione el suyo. 
4. Seleccione **configuración y directivas** en el menú izquierdo.
5. Seleccione **repositorios** en **recursos externos** sección en el menú izquierdo.
6. Seleccione **+ agregar** en la barra de herramientas.

    ![Botón Agregar repositorio](./media/devtest-lab-add-repo/devtestlab-add-repo.png)
5. En el **repositorios** , especifique la siguiente información:
   1. **Nombre**. Escriba un nombre para el repositorio.
   2. **URL de clonación de Git**. Escriba la dirección URL de clonación HTTPS de GIT que copió anteriormente de GitHub o de Azure DevOps Services.
   3. **Rama**. Escriba la rama para obtener las definiciones.
   4. **Token de acceso personal**. Especifique el token de acceso personal que obtuvo anteriormente de GitHub o de Azure DevOps Services.
   5. **Rutas de acceso de carpeta**. Escriba al menos una ruta de acceso de carpeta con respecto a la dirección URL de clonación que contenga las definiciones del artefacto o de la plantilla de Azure Resource Manager. Cuando especifique un subdirectorio, asegúrese de incluir la barra diagonal en la ruta de acceso de la carpeta.

        ![Área Repositorios](./media/devtest-lab-add-repo/devtestlab-repo-blade.png)
6. Seleccione **Guardar**.

## <a name="use-azure-resource-manager-template"></a>Usar plantillas de Azure Resource Manager
Las plantillas de Azure Resource Management (Azure Resource Manager) son archivos JSON que describen los recursos de Azure que desea crear. Para obtener más información acerca de estas plantillas, consulte [plantillas Authoring Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).

Esta sección proporcionan pasos para agregar un repositorio de artefactos a un laboratorio mediante una plantilla de Azure Resource Manager.  La plantilla crea el laboratorio si aún no existe. 

### <a name="template"></a>Plantilla
La plantilla de ejemplo usada en este artículo, recopila la siguiente información a través de parámetros. La mayoría de los parámetros tienen valores predeterminados inteligentes, pero hay algunos valores que deben especificarse. Debe especificar el nombre de laboratorio, el URI para el repositorio de artefactos y el token de seguridad para el repositorio. 

- Nombre de laboratorio.
- Nombre para mostrar para el repositorio de artefactos en la interfaz de usuario (UI) de DevTest Labs. El valor predeterminado es: `Team Repository`.
- URI en el repositorio (ejemplo: `https://github.com/<myteam>/<nameofrepo>.git` o `"https://MyProject1.visualstudio.com/DefaultCollection/_git/TeamArtifacts"`.
- Rama del repositorio que contiene los artefactos. El valor predeterminado es: `master`.
- Nombre de la carpeta que contenga los artefactos. El valor predeterminado es: `/Artifacts`.
- Tipo de repositorio. Los valores permitidos son `VsoGit` o `GitHub`.
- Token de acceso para el repositorio. 

    ```json
    {
    
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "labName": {
                "type": "string"
            },
            "artifactRepositoryDisplayName": {
                "type": "string",
                "defaultValue": "Team Repository"
            },
            "artifactRepoUri": {
                "type": "string"
            },
            "artifactRepoBranch": {
                "type": "string",
                "defaultValue": "master"
            },
            "artifactRepoFolder": {
                "type": "string",
                "defaultValue": "/Artifacts"
            },
            "artifactRepoType": {
                "type": "string",
                "allowedValues": ["VsoGit", "GitHub"]
            },
            "artifactRepoSecurityToken": {
                "type": "securestring"
            }
        },
        "variables": {
            "artifactRepositoryName": "[concat('Repo-', uniqueString(subscription().subscriptionId))]"
        },
        "resources": [{
                "apiVersion": "2016-05-15",
                "type": "Microsoft.DevTestLab/labs",
                "name": "[parameters('labName')]",
                "location": "[resourceGroup().location]",
                "resources": [
                    {
                        "apiVersion": "2016-05-15",
                        "name": "[variables('artifactRepositoryName')]",
                        "type": "artifactSources",
                        "dependsOn": [
                            "[resourceId('Microsoft.DevTestLab/labs', parameters('labName'))]"
                        ],
                        "properties": {
                            "uri": "[parameters('artifactRepoUri')]",
                            "folderPath": "[parameters('artifactRepoFolder')]",
                            "branchRef": "[parameters('artifactRepoBranch')]",
                            "displayName": "[parameters('artifactRepositoryDisplayName')]",
                            "securityToken": "[parameters('artifactRepoSecurityToken')]",
                            "sourceType": "[parameters('artifactRepoType')]",
                            "status": "Enabled"
                        }
                    }
                ]
            }
        ]
    }
    ```


### <a name="deploy-the-template"></a>Implementación de la plantilla
Hay varias maneras de implementar la plantilla en Azure y tiene el recurso creado, si no existe, o se actualiza, si existe. Para obtener más información, consulte los artículos siguientes:

- [Implementación de recursos con las plantillas de Resource Manager y Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md)
- [Implementación de recursos con plantillas de Resource Manager y la CLI de Azure](../azure-resource-manager/resource-group-template-deploy-cli.md)
- [Implementación de recursos con las plantillas de Resource Manager y Azure Portal](../azure-resource-manager/resource-group-template-deploy-portal.md)
- [Implementación de recursos con las plantillas de Resource Manager y la API de REST de Resource Manager](../azure-resource-manager/resource-group-template-deploy-rest.md)

Sigamos adelante y vea cómo implementar la plantilla en PowerShell. Cmdlets que se usan para implementar la plantilla son específicos del contexto, por lo que se usan inquilino actual y la suscripción actual. Use [conjunto AzContext](/powershell/module/az.accounts/set-azcontext) antes de implementar la plantilla, si es necesario, para cambiar el contexto.

En primer lugar, cree un grupo de recursos con [New AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Si el grupo de recursos que desea utilizar ya existe, omita este paso.

```powershell
New-AzResourceGroup -Name MyLabResourceGroup1 -Location westus
```

A continuación, cree una implementación para el grupo de recursos con [New AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment). Este cmdlet aplica los cambios de recursos en Azure. Varias implementaciones de recursos pueden realizarse en cualquier grupo de recursos determinado. Si va a implementar varias veces en el mismo grupo de recursos, asegúrese de que el nombre de cada implementación es único.

```powershell
New-AzResourceGroupDeployment `
    -Name MyLabResourceGroup-Deployment1 `
    -ResourceGroupName MyLabResourceGroup1 `
    -TemplateFile azuredeploy.json `
    -TemplateParameterFile azuredeploy.parameters.json
```

Después de ejecutar correctamente la AzResourceGroupDeployment de nuevo, el comando da como resultado información importante, como el estado de aprovisionamiento (debe ser correcto) y los resultados de la plantilla.
 
## <a name="use-azure-powershell"></a>Uso de Azure PowerShell 
En esta sección se proporciona un script de PowerShell de ejemplo que puede usarse para agregar un repositorio de artefactos a un laboratorio. Si no tiene Azure PowerShell, consulte [cómo instalar y configurar Azure PowerShell](/powershell/azure/overview?view=azps-1.2.0) para obtener instrucciones detalladas instalarlo.

### <a name="full-script"></a>Script completo
Este es el script completo, incluidos algunos mensajes detallados y comentarios:

**New-DevTestLabArtifactRepository.ps1**:

```powershell

<#

.SYNOPSIS
This script creates a new custom repository and adds it to an existing DevTest Lab.

.PARAMETER LabName
The name of the lab.

.PARAMETER LabResourceGroupName
The name of the resource group that contains the lab. 

.PARAMETER ArtifactRepositoryName
Name for the new artifact repository.
Script creates a random name for the respository if it is not specified.

.PARAMETER ArtifactRepositoryDisplayName
Display name for the artifact repository.
This is the name that shows up in the Azure portal (https://portal.azure.com) when viewing all the artifact repositories for a lab.

.PARAMETER RepositoryUri
Uri to the repository.

.PARAMETER RepositoryBranch
Branch in which artifact files can be found. Defaults to 'master'.

.PARAMETER FolderPath
Folder under which artifacts can be found. Defaults to '/Artifacts'

.PARAMETER PersonalAccessToken
Security token for access to GitHub or VSOGit repository.
See https://azure.microsoft.com/en-us/documentation/articles/devtest-lab-add-artifact-repo/ for instructions to get personal access token

.PARAMETER SourceType
Whether artifact is VSOGit or GitHub repository.

.EXAMPLE
Set-AzContext -SubscriptionId 11111111-1111-1111-1111-111111111111
.\New-DevTestLabArtifactRepository.ps1 -LabName "mydevtestlab" -LabResourceGroupName "mydtlrg" -ArtifactRepositoryName "MyTeam Repository" -RepositoryUri "https://github.com/<myteam>/<nameofrepo>.git" -PersonalAccessToken "1111...." -SourceType "GitHub"

.NOTES
Script uses the current Az context. To set the context, use the Set-AzContext cmdlet

#>

 
[CmdletBinding()]
Param(

    [Parameter(Mandatory=$true)]
    $LabName,

    [Parameter(Mandatory=$true)]
    $LabResourceGroupName,
    $ArtifactRepositoryName,
    $ArtifactRepositoryDisplayName  = 'Team Artifact Repository',

    [Parameter(Mandatory=$true)]
    $RepositoryUri,
    $RepositoryBranch = 'master',
    $FolderPath = '/Artifacts',
    
    [Parameter(Mandatory=$true)]
    $PersonalAccessToken ,
    
    [Parameter(Mandatory=$true)]
    [ValidateSet('VsoGit', 'GitHub')]
    $SourceType
)


#Set artifact repository internal name,
# if not set by user.

if ($ArtifactRepositoryName -eq $null){
    $ArtifactRepositoryName = "PrivateRepo" + (Get-Random -Maximum 999)
}

# Sign in to Azure
Connect-AzAccount


#Get Lab Resource
$LabResource = Get-AzResource -ResourceType 'Microsoft.DevTestLab/labs' -ResourceName $LabName -ResourceGroupName $LabResourceGroupName

Write-Verbose "Lab Name: $($LabResource.Name)"
Write-Verbose "Lab Resource Group Name: $($LabResource.ResourceGroupName)"
Write-Verbose "Lab Resource Location: $($LabResource.Location)"

Write-Verbose "Artifact Repository Internal Name: $ArtifactRepositoryName"

#Prepare properties object for call to New-AzResource
$propertiesObject = @{
    uri = $RepositoryUri;
    folderPath = $FolderPath;
    branchRef = $RepositoryBranch;
    displayName = $ArtifactRepositoryDisplayName;
    securityToken = $PersonalAccessToken;
    sourceType = $SourceType;
    status = 'Enabled'
}

Write-Verbose @"Properties to be passed to New-AzResource:$($propertiesObject | Out-String)"@

#Resource will be added to current subscription.
$resourcetype = 'Microsoft.DevTestLab/labs/artifactSources'
$resourceName = $LabName + '/' + $ArtifactRepositoryName
Write-Verbose "Az ResourceType: $resourcetype"
Write-Verbose "Az ResourceName: $resourceName"
 
Write-Verbose "Creating artifact repository '$ArtifactRepositoryDisplayName'..."
$result = New-AzResource -Location $LabResource.Location -ResourceGroupName $LabResource.ResourceGroupName -properties $propertiesObject -ResourceType $resourcetype -ResourceName $resourceName -ApiVersion 2016-05-15 -Force


#Alternate implementation:
# Use resourceId rather than resourcetype and resourcename parameters.
# Using resourceId allows you to specify the $SubscriptionId rather than using the
# subscription id of Get-AzContext.
#$resourceId = "/subscriptions/$SubscriptionId/resourceGroups/$($LabResource.ResourceGroupName)/providers/Microsoft.DevTestLab/labs/$LabName/artifactSources/$ArtifactRepositoryName"
#$result = New-AzResource -properties $propertiesObject -ResourceId $resourceId -ApiVersion 2016-05-15 -Force


# Check the result
if ($result.Properties.ProvisioningState -eq "Succeeded") {
    Write-Verbose ("Successfully added artifact repository source '$ArtifactRepositoryDisplayName'")
}
else {
    Write-Error ("Error adding artifact repository source '$ArtifactRepositoryDisplayName'")
}

#Return the newly created resource so it may be used in subsequent scripts
return $result
```

### <a name="run-the-powershell-script"></a>Ejecución del script de PowerShell
El ejemplo siguiente muestra cómo ejecutar la secuencia de comandos: 

```powershell
Set-AzContext -SubscriptionId <Your Azure subscription ID>

.\New-DevTestLabArtifactRepository.ps1 -LabName "mydevtestlab" -LabResourceGroupName "mydtlrg" -ArtifactRepositoryName "MyTeam Repository" -RepositoryUri "https://github.com/<myteam>/<nameofrepo>.git" -PersonalAccessToken "1111...." -SourceType "GitHub"
```


### <a name="parameters"></a>Parámetros
El script de PowerShell de ejemplo en este artículo toma los parámetros siguientes:

| Parámetro | DESCRIPCIÓN | 
| --------- | ----------- | 
| LabName | El nombre del laboratorio. |
| ArtifactRepositoryName | Nombre para el nuevo repositorio de artefactos. El script crea un nombre aleatorio para el repositorio si no se especifica. |
| ArtifactRepositoryDisplayName | Nombre para mostrar para el repositorio de artefactos. Éste es el nombre que aparece en el portal de Azure (https://portal.azure.com) al ver todos los repositorios de artefacto para un laboratorio. |
| RepositoryUri | URI en el repositorio. Ejemplos: `https://github.com/<myteam>/<nameofrepo>.git` o `"https://MyProject1.visualstudio.com/DefaultCollection/_git/TeamArtifacts"`.| 
| RepositoryBranch | Bifurcación en el artefacto de qué archivos se pueden encontrar. El valor predeterminado es 'master'. | 
| FolderPath | Carpeta en la que se pueden encontrar los artefactos. El valor predeterminado es ' / artefactos |
| PersonalAccessToken | Token de seguridad para acceder al repositorio de GitHub o VSOGit. Consulte la sección de requisitos previos para obtener instrucciones para obtener token de acceso personal |
| SourceType | Indica si el artefacto es repositorio VSOGit o GitHub. |

El repositorio en sí necesita una instancia interna de identificación, que es diferente que el nombre para mostrar que se ve en el portal de Azure. No ve el nombre interno mediante Azure portal, pero verá cuando se usa la API de REST de Azure o Azure PowerShell. El script proporciona un nombre, si no se especifica por el usuario de la secuencia de comandos.

```powershell
#Set artifact repository name, if not set by user
if ($ArtifactRepositoryName -eq $null){
    $ArtifactRepositoryName = "PrivateRepo" + (Get-Random -Maximum 999)
}
```

### <a name="powershell-commands-used-in-the-script"></a>Comandos de PowerShell usados en la secuencia de comandos

| Comando de PowerShell | Notas |
| ------------------ | ----- |
| [Get-AzResource](/powershell/module/az.resources/get-azresource) | Este comando se usa para obtener detalles sobre el laboratorio, como su ubicación. |
| [New-AzResource](/powershell/module/az.resources/new-azresource) | No hay ningún comando específico para agregar repositorios de artefactos. La interfaz genérica [New AzResource](/powershell/module/az.resources/new-azresource) cmdlet realiza el trabajo. Este cmdlet necesita el **ResourceId** o **ResourceName** y **ResourceType** par saber el tipo de recurso para crear. Este script de ejemplo usa el nombre del recurso y un par de tipo de recurso. <br/><br/>Tenga en cuenta que va a crear el origen del repositorio de artefactos en la misma ubicación y en el mismo grupo de recursos que el laboratorio.|

El script agrega un nuevo recurso a la suscripción actual. Use [Get AzContext](/powershell/module/az.accounts/get-azcontext) para ver esta información. Use [conjunto AzContext](/powershell/module/az.accounts/set-azcontext) para establecer el inquilino actual y la suscripción.

La mejor manera de detectar el nombre del recurso y la información de tipo de recurso es usar el [API de REST de Azure de prueba de unidad](https://azure.github.io/projects/apis/) sitio Web. Consulte la [DevTest Labs: 2016-05-15](https://aka.ms/dtlrestapis) proveedor para ver las API de REST disponibles para el proveedor de DevTest Labs. Los usuarios de la secuencia de comandos el siguiente identificador de recurso. 

```powershell
"/subscriptions/$SubscriptionId/resourceGroups/$($LabResource.ResourceGroupName)/providers/Microsoft.DevTestLab/labs/$LabName/artifactSources/$ArtifactRepositoryName"
```
 
El tipo de recurso es todo lo que aparece después de "providers" en el URI, excepto para los elementos que aparecen en las llaves. El nombre del recurso es todo lo que ve en las llaves. Si se espera que más de un elemento para el nombre del recurso, separe cada elemento con una barra diagonal, como se ha hecho. 

```powershell
$resourcetype = 'Microsoft.DevTestLab/labs/artifactSources'
$resourceName = $LabName + '/' + $ArtifactRepositoryName
```


## <a name="next-steps"></a>Pasos siguientes
- [Especifique los artefactos obligatorios para el laboratorio en Azure DevTest Labs](devtest-lab-mandatory-artifacts.md)
- [Crear artefactos personalizados para la máquina virtual de DevTest Labs](devtest-lab-artifact-author.md)
- [Diagnosticar errores de artefactos en el laboratorio](devtest-lab-troubleshoot-artifact-failure.md)

