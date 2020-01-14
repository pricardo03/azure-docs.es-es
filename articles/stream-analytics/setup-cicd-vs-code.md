---
title: Uso del paquete NPM de CI/CD de Azure Stream Analytics
description: En este artículo se describe cómo usar el paquete npm de CI/CD de Azure Stream Analytics para configurar un proceso de integración e implementación continuas.
author: su-jie
ms.author: sujie
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/15/2019
ms.openlocfilehash: ed1f6ebda81a7f036b5e09d15ef4a27323aa9b0d
ms.sourcegitcommit: 51ed913864f11e78a4a98599b55bbb036550d8a5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2020
ms.locfileid: "75660875"
---
# <a name="use-the-stream-analytics-cicd-npm-package"></a>Uso del paquete NPM de CI/CD de Stream Analytics
En este artículo se describe cómo usar el paquete npm de CI/CD de Azure Stream Analytics para configurar un proceso de integración e implementación continuas.

## <a name="build-the-vs-code-project"></a>Compilación del proyecto de VS Code

Puede habilitar la integración e implementación continuas para los trabajos de Azure Stream Analytics mediante el paquete npm **asa-streamanalytics-cicd**. El paquete npm proporciona las herramientas para generar plantillas de Azure Resource Manager de los [proyectos de Visual Studio Code de Stream Analytics](quick-create-vs-code.md). Se puede usar en Windows, macOS y Linux sin instalar Visual Studio Code.

Puede [descargar el paquete](https://www.npmjs.com/package/azure-streamanalytics-cicd) directamente, o bien instalarlo [globalmente](https://docs.npmjs.com/downloading-and-installing-packages-globally) a través del `npm install -g azure-streamanalytics-cicd` comando. Este es el enfoque recomendado, que también se puede usar en una tarea script de PowerShell o CLI de Azure de una canalización de compilación en **canalizaciones de Azure DevOps**.

Una vez que haya instalado el paquete, use el siguiente comando para generar las plantillas de Azure Resource Manager. El argumento **scriptPath** es la ruta de acceso absoluta al archivo **asaql** en el proyecto. Asegúrese de que los archivos asaproj.json y JobConfig.json están en la misma carpeta con el archivo de script. Si no se especifica la **outputPath**, las plantillas se colocarán en la carpeta **Deploy** (Implementación) en la carpeta **bin** del proyecto.

```powershell
azure-streamanalytics-cicd build -scriptPath <scriptFullPath> -outputPath <outputPath>
```
Ejemplo (en macOS)
```powershell
azure-streamanalytics-cicd build -scriptPath "/Users/roger/projects/samplejob/script.asaql" 
```

Cuando un proyecto de Stream Analytics para Visual Studio Code se compila correctamente, genera los dos siguientes archivos de plantilla de Azure Resource Manager en la carpeta **bin/[Debug/Retail]/Deploy**: 

*  Archivo de plantilla de Resource Manager

       [ProjectName].JobTemplate.json 

*  Archivo de parámetros de Resource Manager

       [ProjectName].JobTemplate.parameters.json   

Los parámetros predeterminados en el archivo parameters.json provienen de la configuración del proyecto de Visual Studio Code. Si quiere implementar en otro entorno, reemplace los parámetros según corresponda.

> [!NOTE]
> En todas las credenciales, los valores predeterminados están establecidos en null. Es **imperativo** establecer los valores antes de realizar la implementación en la nube.

```json
"Input_EntryStream_sharedAccessPolicyKey": {
      "value": null
    },
```
Obtenga más información acerca de cómo [implementar con un archivo de plantilla de Resource Manager y Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy). Obtenga más información acerca de cómo [utilizar un objeto como un parámetro en una plantilla de Resource Manager](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/objects-as-parameters).

Para usar la identidad administrada para Azure Data Lake Store Gen1 como receptor de salida, deberá proporcionar acceso a la entidad de servicio con PowerShell antes de la implementación en Azure. Más información acerca de cómo [implementar ADLS Gen1 con identidad administrada con la plantilla de Resource Manager](stream-analytics-managed-identities-adls.md#resource-manager-template-deployment).
## <a name="next-steps"></a>Pasos siguientes

* [Inicio rápido: Creación de un trabajo en la nube de Azure Stream Analytics en Visual Studio Code (versión preliminar)](quick-create-vs-code.md)
* [Prueba de las consultas de Stream Analytics localmente con Visual Studio Code (versión preliminar)](visual-studio-code-local-run.md)
* [Exploración de Azure Stream Analytics con Visual Studio Code (versión preliminar)](visual-studio-code-explore-jobs.md)
