---
title: 'Grupos de Data Science Virtual Machine: Azure | Microsoft Docs'
description: Implementación de grupos de máquinas virtuales de Data Science VM como recurso compartido para un equipo
keywords: aprendizaje profundo, AI, herramientas de ciencia de datos, data science virtual machine, análisis geoespacial, proceso de ciencia de datos en equipo
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/10/2018
ms.author: gokuma
ms.openlocfilehash: 648b0818f07aca8763fa4af01380076ae307b0a2
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2018
ms.locfileid: "53408959"
---
# <a name="create-a-shared-pool-of-data-science-virtual-machines"></a>Crear un grupo compartido de máquinas virtuales de Data Science Virtual Machine

En este artículo se describe la creación de un grupo compartido de máquinas virtuales de Data Science Virtual Machine (DSVM) para que lo use un equipo. La ventaja de utilizar un grupo compartido es que mejora el uso de los recursos, facilita la colaboración y el uso compartido, y permite una administración de los recursos de DSVM más eficaz. 

Puede utilizar muchos métodos y tecnologías para crear un grupo de DSVM. Este artículo se centra en los grupos para las máquinas virtuales interactivas. Una infraestructura de proceso administrado alternativa es Proceso de Azure Machine Learning. Consulte [Configurar los destinos de proceso](../service/how-to-set-up-training-targets.md#amlcompute) para más información.

## <a name="interactive-vm-pool"></a>Grupo de máquinas virtuales interactivas

Un grupo de máquinas virtuales interactivas que comparte todo el equipo de AI o ciencia de datos permite a los usuarios iniciar sesión en una instancia disponible de DSVM en lugar de tener una instancia específica para cada conjunto de usuarios. Esta configuración mejora la disponibilidad y la eficacia del uso de los recursos. 

La tecnología que se usa para crear un grupo de máquinas virtuales interactivas es [Azure Virtual Machine Scale Sets](https://docs.microsoft.com/azure/virtual-machine-scale-sets/). Los conjuntos de escalado se pueden usar para crear y administrar un grupo de máquinas virtuales idénticas, con equilibrio de carga y escalado automático. 

El usuario inicia sesión en la dirección IP o DNS del grupo principal. El conjunto de escalado enruta automáticamente la sesión a una de sus máquinas virtuales DSVM. Dado que los usuarios quieren un entorno similar con independencia de la máquina virtual donde inicien sesión, todas las instancias de la máquina virtual del conjunto de escalado montan una unidad de red compartida, como un recurso compartido de Azure Files o NFS. El área de trabajo compartida del usuario se suele conservar en el almacén de archivos compartidos montado en todas las instancias. 

En [GitHub](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.json) se puede encontrar una plantilla de Azure Resource Manager de ejemplo que crea un conjunto de escalado con instancias de DSVM de Ubuntu. En la misma ubicación también se encuentra un ejemplo de [archivo de parámetros](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.parameters.json) para la plantilla de Azure Resource Manager. 

Para crear el conjunto de escalado a partir de la plantilla de Azure Resource Manager, especifique los valores del archivo de parámetros en la CLI de Azure. 

```
az group create --name [[NAME OF RESOURCE GROUP]] --location [[ Data center. For eg: "West US 2"]
az group deployment create --resource-group  [[NAME OF RESOURCE GROUP ABOVE]]  --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/dsvm-vmss-cluster.json --parameters @[[PARAMETER JSON FILE]]
```
Para los comandos anteriores se supone que tiene:
* Una copia del archivo de parámetros con los valores especificados para su instancia del conjunto de escalado.
* El número de instancias de VM.
* Punteros al recurso compartido de Azure Files.
* Credenciales de la cuenta de almacenamiento que se montará en cada máquina virtual. 

Se hace referencia al archivo de parámetros localmente en los comandos. También puede pasar parámetros alineados o solicitarlos en el script.  

La plantilla anterior habilita el puerto SSH y JupyterHub del conjunto de escalado de front-end en el grupo de back-end de las máquinas virtuales DSVM de Ubuntu. Como usuario, simplemente inicie sesión en el puerto SSH o JupyterHub de la máquina virtual como lo haría normalmente. Puesto que las instancias de máquina virtual se pueden escalar o reducir verticalmente de forma dinámica, es necesario guardar los estados en el recurso compartido montado de Azure Files. Puede usar el mismo enfoque para crear un grupo de máquinas virtuales de DSVM de Windows. 

El [script que monta el recurso compartido de Azure Files](https://raw.githubusercontent.com/Azure/DataScienceVM/master/Extensions/General/mountazurefiles.sh) también está disponible en el repositorio Azure DataScienceVM en Github. El script monta el recurso compartido de Azure Files en el punto de montaje especificado en el archivo de parámetros. El script también crea vínculos simbólicos a la unidad montada en el directorio principal del usuario inicial. Se establece un vínculo simbólico de un directorio de bloc de notas específico del usuario en el recurso compartido de archivos de Azure al directorio `$HOME/notebooks/remote` para que los usuarios pueden acceder a sus blocs de notas de Jupyter, así como ejecutarlos y guardarlos. La misma convención puede usarse al crear usuarios adicionales en la máquina virtual para dirigir el área de trabajo de Jupyter de cada usuario al recurso compartido de Azure Files. 

Virtual Machine Scale Sets es compatible con el escalado automático. Puede establecer reglas sobre cuándo crear instancias adicionales y cuándo reducir verticalmente las instancias. Por ejemplo, puede reducir verticalmente las instancias a cero para ahorrar en costos de uso de hardware de nube si nunca se utilizan máquinas virtuales. Las páginas de documentación de Virtual Machine Scale Sets proporcionan pasos detallados para realizar el [escalado automático](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview).

## <a name="next-steps"></a>Pasos siguientes

* [Configuración de una identidad común](dsvm-common-identity.md)
* [Almacenamiento seguro de las credenciales para acceder a recursos en la nube](dsvm-secure-access-keys.md)















