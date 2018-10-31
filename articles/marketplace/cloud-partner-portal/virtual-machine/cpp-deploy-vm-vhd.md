---
title: Implementar una máquina virtual a partir de discos duros virtuales para Azure Marketplace | Microsoft Docs
description: Se explica cómo registrar una máquina virtual desde un disco duro virtual implementado en Azure.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 10/19/2018
ms.author: pbutlerm
ms.openlocfilehash: 2771549af29b3e717d117afb42de6db03fbee226
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2018
ms.locfileid: "49639379"
---
# <a name="deploy-a-vm-from-your-vhds"></a>Implementación de una máquina virtual a partir de VHD

En este artículo se explica cómo registrar una máquina virtual (VM) a partir de un disco duro virtual (VHD) implementado en Azure.  Se enumeran las herramientas necesarias y cómo usarlas para crear una imagen de máquina virtual de usuario que luego se implementa en Azure mediante [Microsoft Azure Portal](https://ms.portal.azure.com/) o scripts de PowerShell. 

Una vez que los discos duros virtuales (VHD) se han cargado (el VHD del sistema operativo generalizado y cero o más VHD de discos de datos) en la cuenta de almacenamiento de Azure, puede registrarlos como una imagen de máquina virtual de usuario. A continuación, puede probar esa imagen. Puesto que el VHD del sistema operativo está generalizado, no puede implementar la máquina virtual directamente al proporcionar la dirección URL del VHD.

Para obtener más información sobre las imágenes de máquina virtual, vea las siguientes entradas de blog:

- [Imagen de máquina virtual](https://azure.microsoft.com/blog/vm-image-blog-post/)
- [VM Image PowerShell 'How To'](https://azure.microsoft.com/blog/vm-image-powershell-how-to-blog-post/) (Procedimientos para trabajar con imágenes de máquina virtual en PowerShell)


## <a name="set-up-the-necessary-tools"></a>Configurar las herramientas necesarias

Si aún no lo ha hecho, instale Azure PowerShell y la CLI de Azure mediante las siguientes instrucciones:

<!-- TD: Change the following URLs (in this entire topic) to relative paths.-->

- [Instalación de Azure PowerShell en Windows con PowerShellGet](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)
- [Instalación de la CLI de Azure 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)


## <a name="create-a-user-vm-image"></a>Crear una imagen de máquina virtual de usuario

Luego se crea una imagen no administrada del disco duro virtual generalizado.

#### <a name="capture-the-vm-image"></a>Capturar la imagen de máquina virtual

Siga las instrucciones del artículo siguiente sobre cómo capturar la máquina virtual que correspondan con el método de acceso:

-  PowerShell: [Captura de una imagen administrada de una máquina virtual generalizada en Azure](../../../virtual-machines/windows/capture-image-resource.md)
-  CLI de Azure: [Creación de una imagen de una máquina virtual o un disco duro virtual](../../../virtual-machines/linux/capture-image.md)
-  API: [Virtual Machines - Capture](https://docs.microsoft.com/rest/api/compute/virtualmachines/capture) (Máquinas virtuales: Capture)

### <a name="generalize-the-vm-image"></a>Generalizar la imagen de máquina virtual

Dado que ha generado la imagen de usuario a partir de un VHD generalizado anteriormente, también debería estar generalizada.  Una vez más, seleccione el siguiente artículo que corresponda con el mecanismo de acceso.  (Es posible que ya hubiera generalizado el disco cuando lo capturó).

-  PowerShell: [Generalización de la máquina virtual](https://docs.microsoft.com/azure/virtual-machines/windows/sa-copy-generalized#generalize-the-vm)
-  CLI de Azure: [Paso 2: Crear la imagen de máquina virtual](https://docs.microsoft.com/azure/virtual-machines/linux/capture-image#step-2-create-vm-image)
-  API: [Virtual Machines - Generalize](https://docs.microsoft.com/rest/api/compute/virtualmachines/generalize) (Máquinas virtuales: Generalize)


## <a name="deploy-a-vm-from-a-user-vm-image"></a>Implementar una máquina virtual a partir de una imagen de máquina virtual de usuario

Luego se implementa una máquina virtual a partir de una imagen de máquina virtual de usuario mediante Azure Portal o PowerShell.

<!-- TD: Recapture following hilited images and replace with red-box. -->

### <a name="deploy-a-vm-from-azure-portal"></a>Implementar una máquina virtual desde Azure Portal

Use el siguiente proceso para implementar la máquina virtual de usuario desde Azure Portal.

1.  Inicie sesión en el [Portal de Azure](https://portal.azure.com).

2.  Haga clic en **Nuevo** y busque **Implementación de plantillas**, luego seleccione **Build your own template in Editor** (Compilar plantilla propia en el editor).  <br/>
  ![Compilación de plantilla de implementación de disco duro virtual en Azure Portal](./media/publishvm_021.png)

3. Copie y pegue esta [plantilla JSON](./cpp-deploy-json-template.md) en el editor y haga clic en **Guardar**. <br/>
  ![Guardado de plantilla de implementación de disco duro virtual en Azure Portal](./media/publishvm_022.png)

4. Proporcione los valores de parámetro de las páginas de propiedades **Implementación personalizada** que se muestran.

   <table> <tr> <td valign="top"> <img src="./media/publishvm_023.png" alt="Custom deployment property page 1"> </td> <td valign="top"> <img src="./media/publishvm_024.png" alt="Custom deployment property page 2"> </td> </tr> </table> <br/> 

   |  **Parámetro**              |   **Descripción**                                                            |
   |  -------------              |   ---------------                                                            |
   | User Storage Account Name (Nombre de usuario de la cuenta de almacenamiento)   | Nombre de la cuenta de almacenamiento donde se encuentra el disco duro virtual generalizado                    |
   | User Storage Container Name (Nombre de usuario del contenedor de almacenamiento) | Nombre del contenedor donde se encuentra el disco duro virtual generalizado                          |
   | DNS Name for Public IP (Nombre DNS para IP pública)      | Nombre DNS de IP pública                                                           |
   | Nombre de usuario administrador             | Nombre de usuario de la cuenta de administrador de la nueva máquina virtual                                  |
   | Contraseña de administrador              | Contraseña de la cuenta de administrador de la nueva máquina virtual                                  |
   | OS Type (Tipo de SO)                     | Sistema operativo de la máquina virtual: `Windows` \| `Linux`                                    |
   | Id. de suscripción             | Identificador de la suscripción seleccionada                                      |
   | Ubicación                    | Ubicación geográfica de la implementación                                        |
   | Tamaño de VM                     | [Tamaño de la máquina virtual de Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes), por ejemplo `Standard_A2` |
   | Nombre de dirección IP pública      | Nombre de la dirección IP pública                                               |
   | Nombre de la máquina virtual                     | Nombre de la nueva máquina virtual                                                           |
   | El nombre de la red virtual        | Nombre de la red virtual que usa la máquina virtual                                   |
   | Nombre de NIC                    | Nombre de la tarjeta de interfaz de red que ejecuta la red virtual               |
   | URL del VHD                     | Dirección URL completa del VHD del disco del sistema operativo                                                     |
   |  |  |
            
5. Después de proporcionar estos valores, haga clic en **Adquirir**. 

Azure comienza la implementación: crea una nueva máquina virtual con el VHD no administrado especificado en la ruta de acceso de la cuenta de almacenamiento especificada.  Puede realizar un seguimiento del progreso en Azure Portal si hace clic en **Máquinas virtuales** en el lado izquierdo del portal.  Cuando se ha creado la máquina virtual, el estado cambia de `Starting` a `Running`. 


### <a name="deploy-a-vm-from-powershell"></a>Implementar una máquina virtual desde PowerShell

Para implementar una máquina virtual de gran tamaño a partir de la imagen de máquina virtual generalizada que acaba de crear, use los siguientes cmdlets.

``` powershell
    $img = Get-AzureVMImage -ImageName "myVMImage"
    $user = "user123"
    $pass = "adminPassword123"
    $myVM = New-AzureVMConfig -Name "VMImageVM" -InstanceSize "Large" -ImageName $img.ImageName | Add-AzureProvisioningConfig -Windows -AdminUsername $user -Password $pass
    New-AzureVM -ServiceName "VMImageCloudService" -VMs $myVM -Location "West US" -WaitForBoot
```

<!-- TD: The following is a marketplace-publishing article and may be out-of-date.  TD: update and move topic.
For help with issues, see [Troubleshooting common issues encountered during VHD creation](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation-troubleshooting) for additional assistance.
-->

## <a name="next-steps"></a>Pasos siguientes

Una vez implementada la máquina virtual, está listo para [configurar la máquina virtual](./cpp-configure-vm.md).
