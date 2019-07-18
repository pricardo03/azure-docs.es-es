---
title: Entrega continua de actualizaciones de código de función mediante Azure DevOps
description: Obtenga información sobre cómo configurar una canalización de Azure DevOps con Azure Functions como destino.
author: ahmedelnably
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: aelnably
ms.custom: ''
ms.openlocfilehash: 9806a982982971b1b3ac9c28454e17813b2ad2a5
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2019
ms.locfileid: "67479880"
---
# <a name="continuous-delivery-using-azure-devops"></a>Entrega continua con Azure DevOps

Puede implementar automáticamente su función en una aplicación de función de Azure con [Azure Pipelines](/azure/devops/pipelines/).
Para definir la canalización, puede usar lo siguiente:

- Archivo YAML: este archivo describe la canalización, puede tener una sección de pasos de compilación y una sección de versión. El archivo YAML debe estar en el mismo repositorio que la aplicación.

- Plantillas: las plantillas son tareas preparadas que compilan o implementan la aplicación.

## <a name="yaml-based-pipeline"></a>Canalización basada en YAML

### <a name="build-your-app"></a>Compilación de la aplicación

La compilación de la aplicación en Azure Pipelines depende del lenguaje de programación de la aplicación.
Cada lenguaje tiene pasos de compilación determinados para crear un artefacto de implementación, que se puede usar para implementar la aplicación de funciones en Azure.

#### <a name="net"></a>.NET

Puede utilizar el ejemplo siguiente con el fin de crear el archivo YAML para compilar la aplicación de .NET.

```yaml
pool:
      vmImage: 'VS2017-Win2016'
steps:
- script: |
    dotnet restore
    dotnet build --configuration Release
- task: DotNetCoreCLI@2
  inputs:
    command: publish
    arguments: '--configuration Release --output publish_output'
    projects: '*.csproj'
    publishWebProjects: false
    modifyOutputPath: true
    zipAfterPublish: false
- task: ArchiveFiles@2
  displayName: "Archive files"
  inputs:
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)/publish_output"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip'
    name: 'drop'
```

#### <a name="javascript"></a>JavaScript

Puede utilizar el ejemplo siguiente con el fin de crear el archivo YAML para compilar la aplicación de JavaScript:

```yaml
pool:
      vmImage: ubuntu-16.04 # Use 'VS2017-Win2016' if you have Windows native +Node modules
steps:
- bash: |
    if [ -f extensions.csproj ]
    then
        dotnet build extensions.csproj --output ./bin
    fi
    npm install 
    npm run build --if-present
    npm prune --production
- task: ArchiveFiles@2
  displayName: "Archive files"
  inputs:
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip'
    name: 'drop'
```

#### <a name="python"></a>Python

Puede utilizar el ejemplo siguiente con el fin de crear el archivo YAML para compilar la aplicación de Python, la cual solo se admite para Azure Functions de Linux:

```yaml
pool:
      vmImage: ubuntu-16.04
steps:
- task: UsePythonVersion@0
  displayName: "Setting python version to 3.6 as required by functions"
  inputs:
    versionSpec: '3.6'
    architecture: 'x64'
- bash: |
    if [ -f extensions.csproj ]
    then
        dotnet build extensions.csproj --output ./bin
    fi
    python3.6 -m venv worker_venv
    source worker_venv/bin/activate
    pip3.6 install setuptools
    pip3.6 install -r requirements.txt
- task: ArchiveFiles@2
  displayName: "Archive files"
  inputs:
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip'
    name: 'drop'
```
#### <a name="powershell"></a>PowerShell

Puede utilizar el ejemplo siguiente con el fin de crear el archivo YAML para empaquetar la aplicación de PowerShell, la cual solo se admite para Azure Functions de Windows:

```yaml
pool:
      vmImage: 'VS2017-Win2016'
steps:
- task: ArchiveFiles@2
  displayName: "Archive files"
  inputs:
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip'
    name: 'drop'
```

### <a name="deploy-your-app"></a>Implementación de la aplicación

En función del sistema operativo de hospedaje, deberá incluir el siguiente ejemplo YAML en el archivo YAML.

#### <a name="windows-function-app"></a>Aplicación de funciones de Windows

El siguiente fragmento de código se puede usar para implementar en una aplicación de funciones de Windows.

```yaml
steps:
- task: AzureFunctionApp@1
  inputs:
    azureSubscription: '<Azure service connection>'
    appType: functionApp
    appName: '<Name of function app>'
    #Uncomment the next lines to deploy to a deployment slot
    #deployToSlotOrASE: true
    #resourceGroupName: '<Resource Group Name>'
    #slotName: '<Slot name>'
```

#### <a name="linux-function-app"></a>Aplicación de funciones de Linux

El siguiente fragmento de código se puede usar para implementar en una aplicación de funciones de Linux.

```yaml
steps:
- task: AzureFunctionApp@1
  inputs:
    azureSubscription: '<Azure service connection>'
    appType: functionAppLinux
    appName: '<Name of function app>'
    #Uncomment the next lines to deploy to a deployment slot
    #Note that deployment slots is not supported for Linux Dynamic SKU
    #deployToSlotOrASE: true
    #resourceGroupName: '<Resource Group Name>'
    #slotName: '<Slot name>'
```

## <a name="template-based-pipeline"></a>Canalización basada en plantilla

Las plantillas de Azure DevOps son un grupo predefinido de tareas que compilan o implementan una aplicación.

### <a name="build-your-app"></a>Compilación de la aplicación

La compilación de la aplicación en Azure Pipelines depende del lenguaje de programación de la aplicación. Cada lenguaje tiene pasos de compilación determinados para crear un artefacto de implementación, que se puede usar para actualizar la aplicación de funciones en Azure.
Con el fin de usar las plantillas de compilación integradas, al crear una nueva canalización de compilación, elija **usar el editor clásico** para crear una canalización mediante las plantillas de diseñador.

![Editor clásico de Azure Pipelines](media/functions-how-to-azure-devops/classic-editor.png)

Después de configurar el origen del código, busque las plantillas de compilación de Azure Functions y elija la plantilla que coincida con el lenguaje de la aplicación.

![Plantillas de compilación de Azure Functions](media/functions-how-to-azure-devops/build-templates.png)

En algunos casos, los artefactos de compilación tienen una estructura de carpetas particulares y es posible que tenga que comprobar la opción **Anteponer el nombre de la carpeta raíz a las rutas de acceso del archivo**.

![Anteposición de la carpeta raíz](media/functions-how-to-azure-devops/prepend-root-folder.png)

#### <a name="javascript-apps"></a>Aplicaciones de JavaScript

Si la aplicación de JavaScript tiene una dependencia en los módulos nativos de Windows, necesitará actualizar lo siguiente:

- La versión del Grupo de agentes a la **Hospedado VS2017**

  ![Cambio del sistema operativo del agente de compilación](media/functions-how-to-azure-devops/change-agent.png)

### <a name="deploy-your-app"></a>Implementación de la aplicación

Cuando cree una nueva canalización de versión, busque la plantilla de versión de Azure Functions.

![](media/functions-how-to-azure-devops/release-template.png)

En la plantilla de versión no se admite la implementación en una ranura de implementación.

## <a name="creating-an-azure-pipeline-using-the-azure-cli"></a>Creación de una canalización de Azure mediante la CLI de Azure

Mediante el [comando](/cli/azure/functionapp/devops-pipeline#az-functionapp-devops-pipeline-create) `az functionapp devops-pipeline create`, se creará una canalización de Azure para compilar y liberar los cambios de código en el repositorio. El comando generará un nuevo archivo YAML que define la canalización de versión y compilación y lo confirma en el repositorio. El comando de la CLI de Azure no admite la implementación en una ranura de implementación.
Los requisitos previos para este comando dependen de la ubicación del código:

- Si el código está en GitHub:

    - Debe contar con permiso de **escritura** en su suscripción.

    - Es el administrador del proyecto en Azure DevOps.

    - Tiene permiso para crear un token de acceso personal en GitHub con permisos suficientes. [Requisitos de permiso de token de acceso personal en GitHub](https://aka.ms/azure-devops-source-repos).

    - Tiene permiso para confirmar a la rama maestra en el repositorio de GitHub con el fin de confirmar el archivo YAML generado automáticamente.

- Si el código está en Azure Repos:

    - Debe contar con permiso de **escritura** en su suscripción.

    - Es el administrador del proyecto en Azure DevOps.

## <a name="next-steps"></a>Pasos siguientes

+ [Información general sobre Azure Functions](functions-overview.md)
+ [Introducción a Azure DevOps](/azure/devops/pipelines/)
