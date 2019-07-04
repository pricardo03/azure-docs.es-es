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
ms.openlocfilehash: 24c2bfa4aae94642d3ed66f2cfa6e31ba1e6b19a
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67457534"
---
Los discos del sistema operativo efímeros se crean en el almacenamiento local de la máquina virtual (VM) y no se conservan en la instancia remota de Azure Storage. Estos discos están indicados para cargas de trabajo sin estado, donde las aplicaciones toleran errores de máquinas virtuales individuales y tienen más en cuenta el tiempo empleado en implementaciones a gran escala o el tiempo en restablecer la imagen inicial de dichas máquinas. También son adecuados para migrar las aplicaciones en las que se ha usado el modelo de implementación clásico al modelo de implementación de Resource Manager. Con los discos del sistema operativo efímeros, notará una latencia de lectura y escritura inferior en el disco del sistema operativo y un restablecimiento más rápido de la imagen inicial de la máquina virtual. Además, los discos del sistema operativo efímeros son gratuitos, es decir, no generan costos de almacenamiento. 
 
Las principales características de los discos efímeros son: 
- Se pueden usar con imágenes de Marketplace e imágenes personalizadas.
- Puede implementar imágenes de máquina virtual hasta el tamaño de la caché de máquina virtual.
- Posibilidad de restablecimiento rápido o restablecimiento de la imagen inicial de sus máquinas virtuales al estado de arranque original.  
- Latencia de tiempo de ejecución inferior, similar a la de un disco temporal. 
- Sin costo alguno para el disco del sistema operativo. 
 
 
Principales diferencias entre discos del sistema operativo efímeros y persistentes:

|                             | Disco del sistema operativo persistente                          | Disco de sistema operativo efímero                              |    |
|-----------------------------|---------------------------------------------|------------------------------------------------|
| Límite de tamaño del disco del sistema operativo      | 2 TiB                                                                                        | El tamaño de caché para el tamaño de máquina virtual o 2TiB, el que sea menor: [DS](../articles/virtual-machines/linux/sizes-general.md), [ES](../articles/virtual-machines/linux/sizes-memory.md), [M](../articles/virtual-machines/linux/sizes-memory.md), [FS](../articles/virtual-machines/linux/sizes-compute.md) y [GS](../articles/virtual-machines/linux/sizes-memory.md).              |
| Tamaños de máquina virtual admitidos          | Todo                                                                                          | DSv1, DSv2, DSv3, Esv3, Fs, FsV2, GS, M                                               |
| Compatibilidad con los tipos de discos           | Disco del sistema operativo administrado y no administrado                                                                | Solo disco del sistema operativo administrado                                                               |
| Regiones admitidas              | Todas las regiones                                                                                  | Todas las regiones                              |
| Persistencia de los datos            | Los datos del disco del sistema operativo escritos en un disco del sistema operativo se almacenan en Azure Storage.                                  | Los datos escritos en un disco del sistema operativo se almacenan en el almacenamiento de máquina virtual local y no se conservan en Azure Storage. |
| Estado detenido (desasignado)      | Las máquinas virtuales y las instancias del conjunto de escalado pueden estar detenidas (desasignadas) y reiniciarse a partir de este estado. | Las máquinas virtuales y las instancias del conjunto de escalado no pueden estar detenidas (desasignadas).                                  |
| Compatibilidad con discos del sistema operativo especializados | Sí                                                                                          | Sin                                                                                 |
| Cambio de tamaño del disco del sistema operativo              | Se admite durante la creación de la máquina virtual y después de que esta se detiene (desasigna).                                | Se admite solo durante la creación de la máquina virtual                                                  |
| Cambio a un nuevo tamaño de máquina virtual   | Se conservan los datos del disco del sistema operativo                                                                    | Se eliminan los datos del disco del sistema operativo, se vuelve a aprovisionar el sistema operativo.                                      |

## <a name="scale-set-deployment"></a>Implementación del conjunto de escalado  
El proceso para crear un conjunto de escalado que use un disco del sistema operativo efímero consiste en agregar la propiedad `diffDiskSettings` al tipo de recurso `Microsoft.Compute/virtualMachineScaleSets/virtualMachineProfile` en la plantilla. Además, la directiva de almacenamiento en caché debe establecerse en `ReadOnly` para el disco del sistema operativo efímero. 


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

## <a name="vm-deployment"></a>Implementación de la máquina virtual 
Puede implementar una máquina virtual con un disco del sistema operativo efímero mediante una plantilla. El proceso para crear una máquina virtual que use discos del sistema operativo efímeros consiste en agregar la propiedad `diffDiskSettings` al tipo de recurso Microsoft.COMPUTE/virtualmachines en la plantilla. Además, la directiva de almacenamiento en caché debe establecerse en `ReadOnly` para el disco del sistema operativo efímero. 

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


## <a name="reimage-a-vm-using-rest"></a>Restablecimiento de la imagen inicial de una máquina virtual mediante REST
Actualmente, el único método para restablecer la imagen inicial de una instancia de máquina virtual con el disco del sistema operativo efímero consiste en usar la API REST. Para los conjuntos de escalado, el restablecimiento de la imagen inicial ya está disponible mediante PowerShell, la CLI y el portal.

```
POST https://management.azure.com/subscriptions/{sub-
id}/resourceGroups/{rgName}/providers/Microsoft.Compute/VirtualMachines/{vmName}/reimage?a pi-version=2018-06-01" 
```
 
## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

**P: ¿Cuál es el tamaño de los discos del sistema operativo locales?**

R: En la versión preliminar, se admitirán la plataforma o las imágenes hasta el tamaño de caché de la máquina virtual, donde todas las operaciones de lectura y escritura en el disco del sistema operativo serán locales en el mismo nodo que la máquina virtual. 

**P: ¿Se puede cambiar el tamaño del disco del sistema operativo efímero?**

R: No, una vez que se aprovisiona el disco del sistema operativo efímero, no puede cambiarse su tamaño. 

**P: ¿Puedo asociar un disco administrado a una máquina virtual efímera?**

R: Sí, puede asociar un disco de datos administrado a una máquina virtual que use un disco del sistema operativo efímero. 

**P: ¿Se admitirán todos los tamaños de máquina virtual con los discos del sistema operativo efímeros?**

R: No, se admiten todos los tamaños de máquina virtual Premium Storage (DS, ES, FS, GS y M), excepto los tamaños de las series B, N y H.  
 
**P: ¿Se puede aplicar el disco del sistema operativo efímero a máquinas virtuales y conjuntos de escalado existentes?**

R: No, el disco del sistema operativo efímero solo se puede usar durante la creación de la máquina virtual y el conjunto de escalado. 

**P: ¿Se pueden combinar discos del sistema operativo efímeros y normales en un conjunto de escalado?**

R: No, no se puede tener una combinación de instancias de disco del sistema operativo efímeras y persistentes dentro del mismo conjunto de escalado. 

**P: ¿Se puede crear el disco del sistema operativo efímero con PowerShell o la CLI?**

R: Sí, puede crear máquinas virtuales con discos del sistema operativo efímeros mediante REST, plantillas, PowerShell y la CLI.

**P: ¿Qué características no se admiten con los discos del sistema operativo efímeros?**

R: Los discos efímeros no admiten:
- Captura de imágenes de máquina virtual
- Instantáneas de disco. 
- Azure Disk Encryption 
- Azure Backup
- Azure Site Recovery  
- Intercambio de discos del sistema operativo 
