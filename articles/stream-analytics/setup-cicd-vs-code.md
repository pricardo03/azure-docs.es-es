---
title: Integrar continuamente y desarrollar con el paquete de npm de Azure Stream Analytics CI/CD
description: Este artículo describe cómo usar el paquete de npm de Azure Stream Analytics CI/CD para configurar una integración continua y el proceso de implementación.
services: stream-analytics
author: su-jie
ms.author: sujie
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/15/2019
ms.openlocfilehash: fa5a57afa379c6bbe027be80f400fc176800d289
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66158506"
---
# <a name="continuously-integrate-and-develop-with-stream-analytics-cicd-npm-package"></a>Integrar continuamente y desarrollar con el paquete de npm de CI/CD de Stream Analytics
Este artículo describe cómo usar el paquete de npm de Azure Stream Analytics CI/CD para configurar una integración continua y el proceso de implementación.

## <a name="build-the-vs-code-project"></a>Compile el proyecto de VS Code

Puede habilitar la integración continua e implementación para los trabajos de Azure Stream Analytics mediante el **asa-streamanalytics-cicd** paquete npm. El paquete de npm proporciona las herramientas para generar plantillas de Azure Resource Manager de [proyectos de Visual Studio Code de Stream Analytics](quick-create-vs-code.md). Se puede usar en Windows, macOS y Linux sin necesidad de instalar Visual Studio Code.

Una vez que tenga [descargado el paquete](https://www.npmjs.com/package/azure-streamanalytics-cicd), use el siguiente comando para generar las plantillas de Azure Resource Manager. El **scriptPath** argumento es la ruta de acceso absoluta a la **asaql** archivo del proyecto. Asegúrese de que los archivos de JobConfig.json y asaproj.json están en la misma carpeta con el archivo de script. Si el **outputPath** no se especifica, las plantillas se colocarán en el **implementar** carpeta bajo el proyecto **bin** carpeta.

```powershell
azure-streamanalytics-cicd build -scriptPath <scriptFullPath> -outputPath <outputPath>
```
Ejemplo (en macOS)
```powershell
azure-streamanalytics-cicd build -scriptPath "/Users/roger/projects/samplejob/script.asaql" 
```

Cuando se compila correctamente un proyecto de Visual Studio Code de Stream Analytics, genera los dos archivos de plantilla Azure Resource Manager siguiente bajo el **bin / [Debug/Retail] /Deploy** carpeta: 

*  Archivo de plantilla de Resource Manager

       [ProjectName].JobTemplate.json 

*  Archivo de parámetros de Resource Manager

       [ProjectName].JobTemplate.parameters.json   

Los parámetros predeterminados en el archivo parameters.json provienen de la configuración en el proyecto de Visual Studio Code. Si quiere implementar en otro entorno, reemplace los parámetros según corresponda.

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

* [Inicio rápido: Crear un trabajo en la nube de Azure Stream Analytics en Visual Studio Code (versión preliminar)](quick-create-vs-code.md)
* [Probar las consultas de Stream Analytics localmente con Visual Studio Code (versión preliminar)](vscode-local-run.md)
* [Explorar Azure Stream Analytics con Visual Studio Code (versión preliminar)](vscode-explore-jobs.md)
