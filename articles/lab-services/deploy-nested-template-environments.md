---
title: Implementar entornos de plantilla de Resource Manager anidadas en Azure DevTest Labs | Microsoft Docs
description: Obtenga información sobre cómo implementar las plantillas de Azure Resource Manager anidadas para proporcionar entornos con Azure DevTest Labs.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2019
ms.author: spelluru
ms.openlocfilehash: eec0cde4a36449f85998bfb04d16f1d52c68bb19
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2019
ms.locfileid: "65835291"
---
# <a name="deploy-nested-azure-resource-manager-templates-for-testing-environments"></a>Implementar las plantillas de Azure Resource Manager anidadas para entornos de prueba
Una implementación anidada le permite ejecutar otras plantillas de Azure Resource Manager desde una plantilla de Resource Manager principal. Permite descomponer la implementación en un conjunto de plantillas específicas y propósito específico. Proporciona ventajas en cuanto a las pruebas, la reutilización y la legibilidad. El artículo [uso de plantillas vinculadas al implementar recursos de Azure](../azure-resource-manager/resource-group-linked-templates.md) proporciona una buena descripción general de esta solución con varios ejemplos de código. En este artículo se proporciona un ejemplo que es específico de Azure DevTest Labs. 

## <a name="key-parameters"></a>Parámetros de clave
Si bien puede crear su propia plantilla de Resource Manager desde el principio, se recomienda usar la [proyecto del grupo de recursos de Azure](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) en Visual Studio, lo que facilita a desarrollar y depurar las plantillas. Al agregar un recurso de la implementación anidada a azuredeploy.json, Visual Studio agrega varios elementos para que la plantilla más flexible. Estos elementos incluyen la subcarpeta con el archivo de plantilla y parámetros secundaria, los nombres de variable dentro del archivo de plantilla principal y dos parámetros para la ubicación de almacenamiento para los nuevos archivos. El **_artifactsLocation** y **_artifactsLocationSasToken** son los parámetros claves que se usa DevTest Labs. 

Si no está familiarizado con el funcionamiento de DevTest Labs con entornos, consulte [crear entornos de varias máquinas virtuales y recursos de PaaS con plantillas de Azure Resource Manager](devtest-lab-create-environment-from-arm.md). Las plantillas se almacenan en el repositorio vinculado al laboratorio en DevTest Labs. Cuando se crea un nuevo entorno con esas plantillas, los archivos se mueven a un contenedor de almacenamiento de Azure en el laboratorio. Para poder identificar y copiar los archivos anidados, DevTest Labs identifica los parámetros _artifactsLocation y _artifactsLocationSasToken y las subcarpetas hasta el contenedor de almacenamiento de copia. A continuación, insertan automáticamente la ubicación y el token de firma de acceso compartido (SaS) en los parámetros. 

## <a name="nested-deployment-example"></a>Ejemplo de la implementación anidada
Este es un ejemplo sencillo de una implementación anidada:

```json

"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
"contentVersion": "1.0.0.0",
"parameters": {
    "_artifactsLocation": {
        "type": "string"
    },
    "_artifactsLocationSasToken": {
        "type": "securestring"
    }},
"variables": {
    "NestOneTemplateFolder": "nestedtemplates",
    "NestOneTemplateFileName": "NestOne.json",
    "NestOneTemplateParametersFileName": "NestOne.parameters.json"},
    "resources": [
    {
        "name": "NestOne",
        "type": "Microsoft.Resources/deployments",
        "apiVersion": "2016-09-01",
        "dependsOn": [ ],
        "properties": {
            "mode": "Incremental",
            "templateLink": {
                "uri": "[concat(parameters('_artifactsLocation'), '/', variables('NestOneTemplateFolder'), '/', variables('NestOneTemplateFileName'), parameters('_artifactsLocationSasToken'))]",
                "contentVersion": "1.0.0.0"
            },
            "parametersLink": {
                "uri": "[concat(parameters('_artifactsLocation'), '/', variables('NestOneTemplateFolder'), '/', variables('NestOneTemplateParametersFileName'), parameters('_artifactsLocationSasToken'))]",
                "contentVersion": "1.0.0.0"
            }
        }    
    }],
"outputs": {}
```

La carpeta en el repositorio que contiene esta plantilla tiene una subcarpeta `nestedtemplates` con los archivos **NestOne.json** y **NestOne.parameters.json**. En el **azuredeploy.json**, URI de la plantilla se ha creado mediante la ubicación de artefactos, la carpeta de la plantilla anidada, anidados nombre de archivo de plantilla. De forma similar, el URI para los parámetros se compila con la ubicación de artefactos, la carpeta de la plantilla anidada y el archivo de parámetros para la plantilla anidada. 

Aquí es la imagen de la misma estructura de proyecto en Visual Studio: 

![Estructura del proyecto en Visual Studio](./media/deploy-nested-template-environments/visual-studio-project-structure.png)

Puede agregar carpetas adicionales en la carpeta principal, pero no los más de un solo nivel. 

## <a name="next-steps"></a>Pasos siguientes
Consulte los siguientes artículos para obtener más información acerca de los entornos: 

- [Creación de entornos de varias máquinas virtuales y recursos de PaaS con plantillas de Azure Resource Manager](devtest-lab-create-environment-from-arm.md)
- [Configurar y usar entornos públicos en Azure DevTest Labs](devtest-lab-configure-use-public-environments.md)
- [Conectar un entorno a red virtual de su laboratorio en Azure DevTest Labs](connect-environment-lab-virtual-network.md)