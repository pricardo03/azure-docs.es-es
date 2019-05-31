---
title: Configurar una galería de imágenes compartidas en Azure DevTest Labs | Microsoft Docs
description: Aprenda a configurar una galería de imágenes compartidas en Azure DevTest Labs
services: devtest-lab
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2019
ms.author: spelluru
ms.openlocfilehash: 51b394043f88789865edea5be6376ae536f88848
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66420444"
---
# <a name="configure-a-shared-image-gallery-in-azure-devtest-labs"></a>Configurar una galería de imágenes compartidas en Azure DevTest Labs
DevTest Labs ahora admite la [Galería de imágenes compartidas](/virtual-machines/windows/shared-image-galleries.md) característica. Permite a los usuarios del laboratorio tener acceso a imágenes desde una ubicación compartida durante la creación de recursos de laboratorio. También facilita la compilación de la estructura y organización en torno a las imágenes de máquina virtual administrado personalizado. Admite la característica de galería de imágenes compartidas:

- Replicación global administradas de imágenes
- Control de versiones y la agrupación de imágenes para facilitar la administración
- Asegúrese de las imágenes de alta disponibilidad con las cuentas de almacenamiento con redundancia de zona (ZRS) en las regiones que admiten zonas de disponibilidad. ZRS ofrece una mejor resistencia frente a errores de zona.
- Compartir entre suscripciones e incluso entre inquilinos, mediante el control de acceso basado en roles (RBAC).

Para obtener más información, consulte [documentación de la Galería de imágenes compartidas](../virtual-machines/windows/shared-image-galleries.md). 
 
Si tiene un gran número de imágenes administradas que se deben mantener y quiere que estén disponibles en toda la empresa, puede usar una galería de imágenes compartidas como repositorio que facilite la tarea de actualizar y compartir las imágenes. Como propietario del laboratorio, puede adjuntar una galería de imágenes compartidas existente al laboratorio. Una vez que está asociada a esta galería, los usuarios del laboratorio pueden crear las máquinas de estas imágenes más recientes. Una ventaja clave de esta característica es que DevTest Labs ahora puede aprovechar la ventaja de uso compartido de imágenes a través de laboratorios, en las suscripciones y regiones. 

## <a name="considerations"></a>Consideraciones
- Solo puede adjuntar una galería de imágenes compartidas a un laboratorio a la vez. Si desea adjuntar otra galería, necesitará uno existente de separar y adjuntar otro. 
- DevTest Labs no admite actualmente cargar imágenes en la galería a través del laboratorio. 
- Al crear una máquina virtual mediante una imagen de la Galería de imágenes compartidas, DevTest Labs usa siempre la última versión publicada de esta imagen.
- Aunque DevTest Labs automáticamente realiza un intento de mejor para garantizar la Galería de imágenes compartidas replica las imágenes en la región en el que existe el laboratorio, no siempre resulta posible. Para evitar que los usuarios que tiene problemas para crear máquinas virtuales de estas imágenes, asegúrese de que las imágenes ya se replican en la región del laboratorio."

## <a name="use-azure-portal"></a>Usar Azure Portal
1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Seleccione **todos los servicios** en el menú de navegación izquierdo.
1. Seleccione **DevTest Labs** en la lista.
1. En la lista de laboratorios, seleccione su **laboratorio**.
1. Seleccione **configuración y directivas** en el **configuración** sección en el menú izquierdo.
1. Seleccione **galerías de imágenes compartidas** en **bases de máquina Virtual** en el menú izquierdo.
1. Adjuntar una galería de imágenes compartidas existente al laboratorio, haga clic en el **adjuntar** botón y seleccione la galería en la lista desplegable.
1. Vaya a la Galería adjuntada y configure la Galería para **habilitar o deshabilitar** imágenes para la creación de máquinas virtuales compartidas.
1. Los usuarios del laboratorio, a continuación, pueden crear una máquina virtual con las imágenes habilitadas, haga clic en **+ agregar** y buscar la imagen en el **elegir su base** página.

## <a name="use-azure-resource-manager-template"></a>Usar plantillas de Azure Resource Manager

### <a name="attach-a-shared-image-gallery-to-your-lab"></a>Adjuntar una galería de imágenes compartidas al laboratorio
Si usa una plantilla de Azure Resource Manager para asociar una galería de imágenes compartidas al laboratorio, deberá agregarlo en la sección de recursos de la plantilla de Resource Manager, como se muestra en el ejemplo siguiente:

```json
"resources": [
{
    "apiVersion": "2018-10-15-preview",
    "type": "Microsoft.DevTestLab/labs",
    "name": "[parameters('newLabName')]",
    "location": "[resourceGroup (). location]",
    "resources": [
    {
        "apiVersion": "2018-10-15-preview",
        "name": "[variables('labVirtualNetworkName')]",
        "type": "virtualNetworks",
        "dependsOn": [
            "[resourceId('Microsoft.DevTestLab/labs', parameters('newLabName'))]"
        ]
    },
    {
        "apiVersion":"2018-10-15-preview",
        "name":"myGallery",
        "type":"sharedGalleries",
        "properties": {
            "galleryId":"[parameters('existingSharedGalleryId')]",
            "allowAllImages": "Enabled"
        },
        "dependsOn":[
            "[resourceId('Microsoft.DevTestLab/labs', parameters('newLabName'))]"
        ]
    }
    ]
} 

```

Para obtener un ejemplo de plantilla de Resource Manager completa, consulte estos ejemplos de plantilla de Resource Manager en nuestro repositorio de GitHub público: [Configurar una galería de imágenes compartidas al crear un laboratorio](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates/101-dtl-create-lab-shared-gallery-configured).

### <a name="create-a-vm-using-an-image-from-the-shared-image-gallery"></a>Crear una máquina virtual con una imagen desde la Galería de imágenes compartidas
Si usa una plantilla de Azure Resource Manager para crear una máquina virtual mediante una imagen de la Galería de imágenes compartidas, use el ejemplo siguiente:

```json

"resources": [
{
    "apiVersion": "2018-10-15-preview",
    "type": "Microsoft.DevTestLab/labs/virtualMachines",
    "name": "[variables('resourceName')]",
    "location": "[resourceGroup().location]",
    "properties": {
        "sharedImageId": "[parameters('existingSharedImageId')]",
        "size": "[parameters('newVMSize')]",
        "isAuthenticationWithSshKey": false,
        "userName": "[parameters('userName')]",
        "sshKey": "",
        "password": "[parameters('password')]",
        "labVirtualNetworkId": "[variables('labVirtualNetworkId')]",
        "labSubnetName": "[variables('labSubnetName')]"
    }
}
],

```

Para obtener más información, vea estos ejemplos de plantillas de Resource Manager en nuestro público de GitHub.
[Crear una máquina virtual mediante una imagen de la Galería de imágenes compartidas](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates/101-dtl-create-vm-username-pwd-sharedimage).

## <a name="use-api"></a>Use la API

- Use la API versión 2018-10-15-preview.
- Para asociar la galería, envíe la solicitud, como se muestra en el siguiente fragmento de código:
    
    ``` 
    PUT [Lab Resource Id]/SharedGalleries/[newGalleryName]
    Body: 
    {
        “properties”:{
            “galleryId”: “[Shared Image Gallery resource Id]”,
            “allowAllImages”:”Enabled”
        }
    }
    ```
- Para ver todas las imágenes en la Galería de imágenes compartidas, puede enumerar todas las imágenes compartidas junto con sus identificadores de recursos por

    ```
    GET [Lab Resource Id]/SharedGalleries/mySharedGallery/SharedImages
    ````
- Para crear una máquina virtual mediante imágenes compartidas, puede realizar una operación PUT en máquinas virtuales y en las propiedades de la máquina virtual, pase el identificador de la imágenes compartidas que obtuvo de la llamada anterior. Para las propiedades. SharedImageId


## <a name="next-steps"></a>Pasos siguientes
En los artefactos, consulte los siguientes artículos:

- [Especifique los artefactos obligatorios para el laboratorio](devtest-lab-mandatory-artifacts.md)
- [Creación de artefactos personalizados](devtest-lab-artifact-author.md)
- [Agregar un repositorio de artefactos a un laboratorio](devtest-lab-artifact-author.md)
- [Diagnóstico de errores de artefactos](devtest-lab-troubleshoot-artifact-failure.md)
