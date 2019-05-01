---
title: Entregar continuamente actualizaciones de código de función mediante Azure DevOps
description: Obtenga información sobre cómo configurar una canalización de DevOps de Azure como destino Azure Functions.
author: ahmedelnably
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: aelnably
ms.custom: ''
ms.openlocfilehash: 86f1c36bd5f972a6ebd573019a22b0c0d07dc480
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64928098"
---
# <a name="continuous-delivery-using-azure-devops"></a>Entrega continua con Azure DevOps

Puede implementar automáticamente su función en una aplicación de función de Azure con [canalizaciones de Azure](/azure/devops/pipelines/).
Para definir la canalización, puede usar:

- Archivo YAML: Este archivo describe la canalización, puede tener una sección de pasos de compilación y una sección de versión. El archivo YAML debe estar en el mismo repositorio que la aplicación.

- Plantillas: Las plantillas son listas realizan tareas que compilación o implementación la aplicación.

## <a name="yaml-based-pipeline"></a>Canalización de YAML

### <a name="build-your-app"></a>Crear su aplicación

Compilar la aplicación en las canalizaciones de Azure depende del lenguaje de programación de la aplicación.
Cada lenguaje tiene pasos de compilación concreta para crear un artefacto de implementación, que se puede usar para implementar la aplicación de función en Azure.

#### <a name="net"></a>.NET

Puede utilizar el ejemplo siguiente para crear el archivo YAML para compilar la aplicación. NET.

```yaml
jobs:
  - job: Build
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
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)/publish_output/s"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/$(Build.BuildId).zip'
    name: 'drop'
```

#### <a name="javascript"></a>JavaScript

Puede usar el ejemplo siguiente para crear el archivo YAML para compilar la aplicación JavaScript:

```yaml
jobs:
  - job: Build
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
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)/publish_output/s"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/$(Build.BuildId).zip'
    name: 'drop'
```

#### <a name="python"></a>Python

Puede utilizar el ejemplo siguiente para crear el archivo YAML para compilar la aplicación de Python, Python solo se admite para las funciones de Azure de Linux:

```yaml
jobs:
  - job: Build
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
    rootFolderOrFile: "$(System.DefaultWorkingDirectory)/publish_output/s"
    includeRootFolder: false
    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(System.DefaultWorkingDirectory)/$(Build.BuildId).zip'
    name: 'drop'
```

### <a name="deploy-your-app"></a>Implementar la aplicación

Según el sistema operativo host, deberá incluir el siguiente ejemplo YAML en el archivo YAML.

#### <a name="windows-function-app"></a>Aplicación de función de Windows

El siguiente fragmento de código se puede usar para implementar en una aplicación de función de Windows

```yaml
steps:
- task: AzureFunctionApp@1
  inputs:
    azureSubscription: '<Azure service connection>'
    appType: functionApp
    appName: '<Name of function app>'
```

#### <a name="linux-function-app"></a>Aplicación de función de Linux

El siguiente fragmento de código se puede usar para implementar en una aplicación de función Linux

```yaml
steps:
- task: AzureFunctionApp@1
  inputs:
    azureSubscription: '<Azure service connection>'
    appType: functionAppLinux
    appName: '<Name of function app>'
```

## <a name="template-based-pipeline"></a>Canalización basada en plantilla

Las plantillas en Azure, DevOps, son un grupo predefinido de tareas que compilar o implementar una aplicación.

### <a name="build-your-app"></a>Crear su aplicación

Compilar la aplicación en las canalizaciones de Azure depende del lenguaje de programación de la aplicación. Cada lenguaje tiene pasos de compilación concreta para crear un artefacto de implementación, que puede usarse para actualizar la aplicación de función en Azure.
Para usar las plantillas de compilación integradas al crear una nueva canalización de compilación, elija **usar el editor clásico** para crear una canalización mediante las plantillas de diseñador

![Editor clásico de canalizaciones de Azure](media/functions-how-to-azure-devops/classic-editor.png)

Después de configurar el origen del código, busque Azure Functions las plantillas de compilación y elija la plantilla que coincida con el idioma de la aplicación.

![Las plantillas de compilación de las funciones de Azure](media/functions-how-to-azure-devops/build-templates.png)

#### <a name="javascript-apps"></a>Aplicaciones de JavaScript

Si su aplicación de JavaScript tiene una dependencia en los módulos nativos de Windows, necesita actualizar:

- La versión del grupo de agentes para **Hosted VS2017**

  ![Cambiar el agente de compilación del sistema operativo](media/functions-how-to-azure-devops/change-agent.png)

- La secuencia de comandos en el **crear extensiones** paso en la plantilla `IF EXIST *.csproj dotnet build extensions.csproj --output ./bin`

  ![Script de cambio](media/functions-how-to-azure-devops/change-script.png)

### <a name="deploy-your-app"></a>Implementar la aplicación

Al crear una nueva canalización de versiones, busque la plantilla de versión de Azure Functions.

![](media/functions-how-to-azure-devops/release-template.png)

## <a name="creating-an-azure-pipeline-using-the-azure-cli"></a>Creación de una canalización de Azure mediante la CLI de Azure

Mediante el `az functionapp devops-pipeline create` [comando](/cli/azure/functionapp/devops-pipeline#az-functionapp-devops-pipeline-create), se creará una canalización de Azure para compilación y versión de cambios de código en el repositorio. El comando generará un nuevo archivo YAML que define la canalización de compilación y lanzamiento y confirma en el repositorio.
Los requisitos previos para este comando dependen de la ubicación del código:

- Si es el código en GitHub:

    - Debe tener **escribir** permiso a su suscripción.

    - Es el Administrador de proyecto de DevOps de Azure.

    - Tiene permiso para crear un GitHub Token de acceso Personal con permisos suficientes. [Requisitos de permisos de PAT de GitHub.](https://aka.ms/azure-devops-source-repos)

    - Tiene permiso para confirmar en la rama principal en el repositorio de GitHub para confirmar el archivo YAML generado automáticamente.

- Si el código está en repositorios de Azure:

    - Debe tener **escribir** permiso a su suscripción.

    - Es el Administrador de proyecto de DevOps de Azure.

## <a name="next-steps"></a>Pasos siguientes

+ [Información general sobre Azure Functions](functions-overview.md)
+ [Información general de Azure DevOps](/azure/devops/pipelines/)
