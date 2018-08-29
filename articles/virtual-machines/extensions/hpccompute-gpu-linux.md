---
title: Extensión del controlador de GPU de NVIDIA. VMs de Azure Linux| Microsoft Docs
description: Extensión de Microsoft Azure para instalar controladores de GPU de NVIDIA en VMs de cálculo de la serie N que ejecutan Linux.
services: virtual-machines-linux
documentationcenter: ''
author: vermagit
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/20/2018
ms.author: roiyz
ms.openlocfilehash: 307bdb5fa7a5d14a77c71d0ea40634a55d8507b6
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2018
ms.locfileid: "42143447"
---
# <a name="nvidia-gpu-driver-extension-for-linux"></a>Extensión del controlador de GPU de NVIDIA para Linux

## <a name="overview"></a>Información general

Esta extensión instala los controladores de GPU de NVIDIA en VMs de serie N de Linux. En función de la familia de VM, la extensión instala los controladores CUDA o GRID. Al instalar controladores de NVIDIA mediante esta extensión, acepta y está de acuerdo con los términos del contrato de licencia de usuario final de NVIDIA. Durante el proceso de instalación, es posible que la máquina virtual se reinicie para completar la instalación del controlador.

También se dispone de una extensión para instalar controladores de GPU de NVIDIA en [MVs de la serie N para Windows](hpccompute-gpu-windows.md).

Los términos de licencia del usuario final de NVIDIA se encuentran aquí: https://go.microsoft.com/fwlink/?linkid=874330

## <a name="prerequisites"></a>Requisitos previos

### <a name="operating-system"></a>Sistema operativo

Esta extensión es compatible con los siguientes sistemas operativos:

| Distribución | Versión |
|---|---|
| Linux: Ubuntu | 16.04 LTS |
| Linux: Red Hat Enterprise Linux | 7.3, 7.4 |
| Linux: CentOS | 7.3, 7.4 |

### <a name="internet-connectivity"></a>Conectividad de Internet

La extensión de Microsoft Azure para los controladores de GPU de NVIDIA requiere que la máquina virtual de destino esté conectada a Internet y tenga acceso.

## <a name="extension-schema"></a>Esquema de extensión

En el siguiente JSON, se muestra el esquema para la extensión.

```json
{
  "name": "<myExtensionName>",
  "type": "extensions",
  "apiVersion": "2015-06-15",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <myVM>)]"
  ],
  "properties": {
    "publisher": "Microsoft.HpcCompute",
    "type": "NvidiaGpuDriverLinux",
    "typeHandlerVersion": "1.1",
    "autoUpgradeMinorVersion": true,
    "settings": {
    }
  }
}
```

### <a name="property-values"></a>Valores de propiedad

| NOMBRE | Valor / ejemplo | Tipo de datos |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | fecha |
| publisher | Microsoft.HpcCompute | string |
| Tipo | NvidiaGpuDriverLinux | string |
| typeHandlerVersion | 1.1 | int |


## <a name="deployment"></a>Implementación


### <a name="azure-resource-manager-template"></a>Plantilla de Azure Resource Manager 

Las extensiones de VM de Azure pueden implementarse con plantillas de Azure Resource Manager. Las plantillas resultan ideales al implementar una o varias máquinas virtuales que requieren configurarse tras la implementación.

La configuración JSON de una extensión de máquina virtual puede estar anidada en el recurso de máquina virtual o colocada en la raíz o nivel superior de una plantilla JSON de Resource Manager. La colocación de la configuración JSON afecta al valor del nombre y tipo del recurso. Para obtener más información, consulte el artículo sobre cómo [establecer el nombre y el tipo de recursos secundarios](../../azure-resource-manager/resource-manager-template-child-resource.md). 

En el siguiente ejemplo se da por supuesto que la extensión está anidada dentro del recurso de máquina virtual. Cuando se anidan los recursos de extensión, la plantilla JSON se coloca en el objeto `"resources": []` de la máquina virtual.

```json
{
  "name": "myExtensionName",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', myVM)]"
  ],
  "properties": {
    "publisher": "Microsoft.HpcCompute",
    "type": "NvidiaGpuDriverLinux",
    "typeHandlerVersion": "1.1",
    "autoUpgradeMinorVersion": true,
    "settings": {
    }
  }
}
```

### <a name="powershell"></a>PowerShell

```powershell
Set-AzureRmVMExtension
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Location "southcentralus" `
    -Publisher "Microsoft.HpcCompute" `
    -ExtensionName "NvidiaGpuDriverLinux" `
    -ExtensionType "NvidiaGpuDriverLinux" `
    -TypeHandlerVersion 1.1 `
    -SettingString '{ `
    }'
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az vm extension set `
  --resource-group myResourceGroup `
  --vm-name myVM `
  --name NvidiaGpuDriverLinux `
  --publisher Microsoft.HpcCompute `
  --version 1.1 `
  --settings '{ `
  }'
```

## <a name="troubleshoot-and-support"></a>Solución de problemas y asistencia

### <a name="troubleshoot"></a>Solución de problemas

Los datos sobre el estado de las implementaciones de extensiones pueden recuperarse desde Azure Portal y mediante Azure PowerShell y la CLI de Azure. Para ver el estado de implementación de las extensiones de una VM determinada, ejecute el comando siguiente.

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

El resultado de la ejecución de las extensiones se registra en el archivo siguiente:

```bash
/var/log/azure/nvidia-vmext-status
```

### <a name="exit-codes"></a>Códigos de salida

| Código de salida | Significado | Acción posible |
| :---: | --- | --- |
| 0 | Operación correcta |
| 1 | Uso incorrecto de la extensión. | Póngase en contacto con el soporte técnico y proporcione el registro de salida de ejecución. |
| 10 | Linux Integration Services para Hyper-V y Azure no están disponibles o no están instalados. | Comprobación de la salida de lspci. |
| 11 | No se encuentra GPU de NVIDIA en este tamaño de VM. | Use un [tamaño de VM y SO admitidos](../linux/n-series-driver-setup.md). |
| 12 | Oferta de imagen no compatible |
| 13 | Tamaño de máquina virtual no compatible | Use una máquina virtual de la serie N para la implementación |
| 14 | La operación no se realizó correctamente | |
| 21 | Error en la actualización de Ubuntu | Compruebe la salida de "actualización de apt-get de sudo" |


### <a name="support"></a>Soporte técnico

Si necesita más ayuda con cualquier aspecto de este artículo, puede ponerse en contacto con los expertos de Azure en los [foros de MSDN Azure o Stack Overflow](https://azure.microsoft.com/support/community/). Como alternativa, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione Obtener soporte. Para obtener información sobre el uso del soporte técnico, lea las [Preguntas más frecuentes de soporte técnico de Microsoft Azure](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre las extensiones, consulte el artículo [Características y extensiones de las máquinas virtuales para Linux](features-linux.md).

Para obtener más información acerca de las VM de serie N, consulte el artículo [Tamaños de máquinas virtuales optimizadas para GPU](../linux/sizes-gpu.md).