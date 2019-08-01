---
title: Configuración de una galería de imágenes compartidas en Azure DevTest Labs | Microsoft Docs
description: Aprenda a configurar una galería de imágenes compartidas en Azure DevTest Labs.
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
ms.openlocfilehash: de857498aeb51c9b3711c90338d983e85b61cb70
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67065428"
---
# <a name="configure-a-shared-image-gallery-in-azure-devtest-labs"></a>Configuración de una galería de imágenes compartidas en Azure DevTest Labs
DevTest Labs ofrece ahora la característica [Shared Image Gallery](../virtual-machines/windows/shared-image-galleries.md). Esta característica permite que los usuarios de laboratorios accedan a imágenes de una ubicación compartida cuando crean recursos de laboratorio. También facilita la estructuración y la organización de las imágenes de máquina virtual administradas y personalizadas. La característica Shared Image Gallery admite lo siguiente:

- Replicación administrada global de imágenes.
- Control de versiones y agrupación de imágenes para facilitar su administración.
- Alta disponibilidad para sus imágenes con cuentas de almacenamiento con redundancia de zona (ZRS) en las regiones que admitan zonas de disponibilidad. ZRS ofrece mejor resistencia a errores de zona.
- Uso compartido entre suscripciones e, incluso, entre inquilinos, con el control de acceso basado en rol (RBAC).

Para más información, consulte la [documentación de Shared Image Gallery](../virtual-machines/windows/shared-image-galleries.md). 
 
Si tiene un gran número de imágenes administradas que se deben mantener y quiere que estén disponibles en toda la empresa, puede usar una galería de imágenes compartidas como repositorio que facilite la tarea de actualizar y compartir las imágenes. Como propietario de un laboratorio, puede asociar al laboratorio una galería de imágenes compartidas que ya exista. Una vez asociada la galería, los usuarios del laboratorio pueden crear máquinas con estas últimas imágenes. Una ventaja fundamental de esta característica es que ahora DevTest Labs puede aprovechar el uso compartido de imágenes entre laboratorios, suscripciones y regiones. 

## <a name="considerations"></a>Consideraciones
- Solo es posible asociar una galería de imágenes compartidas a un laboratorio cada vez. Si quiere asociar otra galería, deberá desasociar la existente y asociar otra. 
- DevTest Labs no admite actualmente la carga de imágenes en la galería mediante el laboratorio. 
- Al crear una máquina virtual mediante una imagen de la galería de imágenes compartidas, DevTest Labs usa siempre la última versión publicada de esta imagen.
- Aunque DevTest Labs hace automáticamente todo lo que puede por garantizar que la galería de imágenes compartidas replica las imágenes en la región en la que existe el laboratorio, no siempre resulta posible. Para evitar que los usuarios tengan problemas al crear máquinas virtuales con estas imágenes, asegúrese de que las imágenes ya se hayan replicado en la región del laboratorio.

## <a name="use-azure-portal"></a>Usar Azure Portal
1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Seleccione **Todos los servicios** en el menú de navegación izquierdo.
1. Seleccione **DevTest Labs** en la lista.
1. En la lista de laboratorios, seleccione el **suyo**.
1. Seleccione **Configuración y directivas** en la sección **Configuración** en el menú de la izquierda.
1. Seleccione **Galerías de imágenes compartidas** en **Bases para máquinas virtuales** en el menú izquierdo.

    ![Menú Galerías de imágenes compartidas](./media/configure-shared-image-gallery/shared-image-galleries-menu.png)
1. Asocie una galería de imágenes compartidas existente al laboratorio; para ello, haga clic en el botón **Asociar** y seleccione su galería en la lista desplegable.

    ![Attach](./media/configure-shared-image-gallery/attach-options.png)
1. Vaya a la galería asociada y configúrela para **habilitar o deshabilitar** imágenes compartidas para la creación de máquinas virtuales.

    ![Habilitar o deshabilitar](./media/configure-shared-image-gallery/enable-disable.png)
1. Los usuarios del laboratorio pueden crear entonces una máquina virtual con las imágenes habilitadas con solo hacer clic en **+ Agregar** y buscar la imagen en la página **choose your base** (Elegir la base).

    ![Usuarios de laboratorios](./media/configure-shared-image-gallery/lab-users.png)
## <a name="use-azure-resource-manager-template"></a>Usar plantillas de Azure Resource Manager

### <a name="attach-a-shared-image-gallery-to-your-lab"></a>Asociación de una galería de imágenes compartidas al laboratorio
Si va a usar una plantilla de Azure Resource Manager para asociar una galería de imágenes compartidas al laboratorio, deberá agregarla en la sección de recursos de la plantilla de Resource Manager, como se muestra en el ejemplo siguiente:

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

Para ver un ejemplo completo de una plantilla de Resource Manager, consulte estos ejemplos de plantilla de Resource Manager en nuestro repositorio público de GitHub: [Configuración de una galería de imágenes compartidas al crear un laboratorio](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates/101-dtl-create-lab-shared-gallery-configured).

### <a name="create-a-vm-using-an-image-from-the-shared-image-gallery"></a>Creación de una máquina virtual con una imagen de la galería de imágenes compartidas
Si va a usar una plantilla de Azure Resource Manager para crear una máquina virtual mediante una imagen de la galería de imágenes compartidas, utilice el ejemplo siguiente:

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

Para más información, vea estos ejemplos de plantillas de Resource Manager en nuestro repositorio público de GitHub.
[Creación de una máquina virtual mediante una imagen de la galería de imágenes compartidas](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates/101-dtl-create-vm-username-pwd-sharedimage).

## <a name="use-api"></a>Uso de la API

- Use la versión de API 2018-10-15-preview.
- Para asociar la galería, envíe la solicitud como se muestra en el siguiente fragmento de código:
    
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
- Para ver todas las imágenes de la galería de imágenes compartidas, puede enumerar todas las imágenes compartidas junto con sus identificadores de recursos.

    ```
    GET [Lab Resource Id]/SharedGalleries/mySharedGallery/SharedImages
    ````
- Para crear una máquina virtual mediante imágenes compartidas, puede realizar una operación PUT en las máquinas virtuales y, en las propiedades de las máquinas virtuales, pasar el identificador de las imágenes compartidas que obtuvo en la llamada anterior. a properties.SharedImageId.


## <a name="next-steps"></a>Pasos siguientes
Consulte los artículos siguientes sobre los artefactos:

- [Especificación de artefactos obligatorios para su laboratorio](devtest-lab-mandatory-artifacts.md)
- [Creación de artefactos personalizados](devtest-lab-artifact-author.md)
- [Incorporación de un repositorio de artefactos a un laboratorio](devtest-lab-artifact-author.md)
- [Diagnóstico de errores de artefactos](devtest-lab-troubleshoot-artifact-failure.md)
