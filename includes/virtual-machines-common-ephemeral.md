---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 05/02/2019
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 3e9885466d422a0428311ed3013e2ab34341cd25
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66391469"
---
Discos de SO efímeros se crean en el almacenamiento local de la máquina Virtual (VM) y no se conservan en el almacenamiento remoto de Azure. Discos de SO efímeros funcionan bien para las cargas de trabajo sin estado, donde las aplicaciones son tolerantes a errores de máquina virtual individuales, pero preocupan más el tiempo necesario para las implementaciones a gran escala o la hora a la imagen inicial de las instancias de máquina virtual individuales. También es adecuado para las aplicaciones implementadas con el modelo de implementación clásica, para mover al modelo de implementación de Resource Manager. Con los discos de sistema operativo efímeros, notará una latencia de lectura y escritura inferior en el disco del sistema operativo y un restablecimiento más rápido de la imagen inicial de la máquina virtual. Además, el disco de SO efímero es gratuito, se incurre en ningún costo de almacenamiento de disco del sistema operativo. 
 
Las características clave de discos efímeros son: 
- Se puede usar con las imágenes de Marketplace e imágenes personalizadas.
- Puede implementar imágenes de máquina virtual hasta el tamaño de la memoria caché de la máquina virtual.
- Capacidad de restablecimiento rápido o la imagen inicial de sus máquinas virtuales en el estado de inicio original.  
- Menor latencia de tiempo de ejecución similar a un disco temporal. 
- Sin costo alguno para el disco del sistema operativo. 
 
 
Diferencias clave entre los discos de SO efímeros y no persistentes:

|                             | Disco del sistema operativo persistente                          | Disco de sistema operativo efímero                              |    |
|-----------------------------|---------------------------------------------|------------------------------------------------|
| Límite de tamaño de disco del sistema operativo      | 2 TiB                                                                                        | Caché de tamaño para el tamaño de máquina virtual o 2TiB, lo que sea menor - [DS](../articles/virtual-machines/linux/sizes-general.md), [ES](../articles/virtual-machines/linux/sizes-memory.md), [M](../articles/virtual-machines/linux/sizes-memory.md), [FS](../articles/virtual-machines/linux/sizes-compute.md), y [GS](../articles/virtual-machines/linux/sizes-memory.md)              |
| Tamaños de máquina virtual admitidos          | Todo                                                                                          | DSv1, DSv2, DSv3, Esv3, Fs, FsV2, GS, M                                               |
| Compatibilidad con tipos de disco           | Disco del sistema operativo administrado y no administrado                                                                | Solo los discos del sistema operativo administrado                                                               |
| Regiones admitidas              | Todas las regiones                                                                                  | Todas las regiones                              |
| Persistencia de los datos            | Datos del disco del sistema operativo escritos en disco del sistema operativo se almacenan en Azure Storage                                  | Los datos escritos en disco del sistema operativo se almacenan en el almacenamiento de máquina virtual local y no se conservan en el almacenamiento de Azure. |
| Estado de detenida (desasignada)      | Máquinas virtuales e instancias de conjunto de escalado pueden estar detenida (desasignada) y reinicia desde el estado de detenida (desasignada) | Máquinas virtuales e instancias de conjunto de escalado no pueden estar detenida (desasignada)                                  |
| Compatibilidad con discos de sistema operativo especializada | Sí                                                                                          | Sin                                                                                  |
| Cambio de tamaño de disco del sistema operativo              | Durante la creación de máquinas virtuales y después la máquina virtual está detenida (desasignada) compatibles                                | Durante la creación de máquinas virtuales solo admite la                                                  |
| Cambiar el tamaño de un nuevo tamaño de máquina virtual   | Se conservan los datos del disco del sistema operativo                                                                    | Se eliminan los datos en el disco del sistema operativo, se vuelven a aprovisionar el sistema operativo                                      |

## <a name="scale-set-deployment"></a>Implementación del conjunto de escalado  
El proceso para crear un conjunto de escalado que usa un disco de SO efímero consiste en agregar el `diffDiskSettings` propiedad a la `Microsoft.Compute/virtualMachineScaleSets/virtualMachineProfile` tipo de recurso en la plantilla. Además, la directiva de caché debe establecerse en `ReadOnly` para el disco de SO efímero. 


```json
{ 
  "type": "Microsoft.Compute/virtualMachineScaleSets", 
  "name": "myScaleSet", 
  "location": "East US 2", 
  "apiVersion": "2018-06-01", 
  "sku": { 
    "name": "Standard_DS2_v2", 
    "capacity": "2" 
  }, 
  "properties": { 
    "upgradePolicy": { 
      "mode": "Automatic" 
    }, 
    "virtualMachineProfile": { 
       "storageProfile": { 
        "osDisk": { 
          "diffDiskSettings": { 
                "option": "Local" 
          }, 
          "caching": "ReadOnly", 
          "createOption": "FromImage" 
        }, 
        "imageReference":  { 
          "publisher": "Canonical", 
          "offer": "UbuntuServer", 
          "sku": "16.04-LTS", 
          "version": "latest" 
        } 
      }, 
      "osProfile": { 
        "computerNamePrefix": "myvmss", 
        "adminUsername": "azureuser", 
        "adminPassword": "P@ssw0rd!" 
      } 
    } 
  } 
}  
```

## <a name="vm-deployment"></a>Implementación de máquinas virtuales 
Puede implementar una máquina virtual con un disco de SO efímero mediante una plantilla. El proceso para crear una máquina virtual que utiliza discos de SO efímeros consiste en agregar el `diffDiskSettings` propiedad al tipo de recurso Microsoft.COMPUTE/virtualmachines en la plantilla. Además, la directiva de caché debe establecerse en `ReadOnly` para el disco de SO efímero. 

```json
{ 
  "type": "Microsoft.Compute/virtualMachines", 
  "name": "myVirtualMachine", 
  "location": "East US 2", 
  "apiVersion": "2018-06-01", 
  "properties": { 
       "storageProfile": { 
            "osDisk": { 
              "diffDiskSettings": { 
                "option": "Local" 
              }, 
              "caching": "ReadOnly", 
              "createOption": "FromImage" 
            }, 
            "imageReference": { 
                "publisher": "MicrosoftWindowsServer", 
                "offer": "WindowsServer", 
                "sku": "2016-Datacenter-smalldisk", 
                "version": "latest" 
            }, 
            "hardwareProfile": { 
                 "vmSize": "Standard_DS2_v2" 
             } 
      }, 
      "osProfile": { 
        "computerNamePrefix": "myvirtualmachine", 
        "adminUsername": "azureuser", 
        "adminPassword": "P@ssw0rd!" 
      } 
    } 
 } 
```


## <a name="reimage-a-vm-using-rest"></a>Restablecer imagen inicial de una máquina virtual mediante REST
Actualmente, el único método para restablecer la imagen inicial de una instancia de máquina Virtual con el disco de SO efímero consiste en usar la API de REST. Para conjuntos de escalado, restablecer la imagen inicial ya está disponible a través de Powershell, CLI y el portal.

```
POST https://management.azure.com/subscriptions/{sub-
id}/resourceGroups/{rgName}/providers/Microsoft.Compute/VirtualMachines/{vmName}/reimage?a pi-version=2018-06-01" 
```
 
## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

**P: ¿Cuál es el tamaño de los discos del sistema operativo local?**

R: La versión preliminar, se admitirán la plataforma o imágenes hasta el tamaño de caché de la máquina virtual, donde todas las lecturas/escrituras en el disco del sistema operativo puede ser locales en el mismo nodo que la máquina Virtual. 

**P: ¿Puede cambiarse el disco de SO efímero?**

R: No, cuando se aprovisiona el disco de SO efímero, no puede cambiarse el disco del sistema operativo. 

**P: ¿Puedo conectar un disco administrado a una máquina virtual efímero?**

R: Sí, puede adjuntar un disco de datos administrado a una máquina virtual que usa un disco de SO efímero. 

**P: ¿Se admitirán todos los tamaños de máquina virtual para los discos de SO efímeros?**

R: No, todos los tamaños de VM de Premium Storage son compatibles (DS, ES, FS, GS y M), excepto la serie B, la serie N y tamaños de la serie H.  
 
**P: ¿El disco de SO efímero se puede aplicar a las máquinas virtuales existentes y conjuntos de escalado?**

R: No, efímero disco del sistema operativo sólo puede usarse durante la máquina virtual y crear el conjunto de escalado. 

**P: ¿Puede combinar discos de SO efímeros y normales en un conjunto de escalado?**

R: No, no puede tener una combinación de instancias del disco del SO efímeras y persistentes dentro del mismo conjunto de escalado. 

**P: ¿El disco de SO efímero se puede crear con Powershell o CLI?**

R: Sí, puede crear máquinas virtuales con disco de SO efímero con REST, plantillas, PowerShell y CLI.

**P: ¿Qué características no son compatibles con el disco de SO efímero?**

R: No se admiten discos efímeros:
- Captura de imágenes de máquina virtual
- Instantáneas de disco. 
- Azure Disk Encryption 
- Azure Backup
- Azure Site Recovery  
- Intercambio del disco del sistema operativo 
