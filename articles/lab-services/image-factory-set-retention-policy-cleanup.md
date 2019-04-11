---
title: Crear un generador de imágenes en Azure DevTest Labs | Microsoft Docs
description: Obtenga información sobre cómo crear un generador de imágenes personalizadas en Azure DevTest Labs.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: spelluru
ms.openlocfilehash: 48412b3006a462fcc9c77219f42fb41d08f2df61
ms.sourcegitcommit: 6e32f493eb32f93f71d425497752e84763070fad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2019
ms.locfileid: "59469377"
---
# <a name="create-a-custom-image-factory-in-azure-devtest-labs"></a>Crear un generador de imágenes personalizadas en Azure DevTest Labs
Este artículo trata de establecer una directiva de retención, limpiar la fábrica y retirar las imágenes anteriores de todos los otros laboratorios de DevTest de la organización. 

## <a name="prerequisites"></a>Requisitos previos
Asegúrese de que ha seguido estos artículos antes de continuar:

- [Creación de una fábrica de imágenes](image-factory-create.md)
- [Ejecución de una fábrica de imágenes desde AzureDevOps](image-factory-set-up-devops-lab.md)
- [Guardar imágenes personalizadas y distribuirlas a varios laboratorios](image-factory-save-distribute-custom-images.md)

Los siguientes elementos ya deben estar en su lugar:

- Un laboratorio para el generador de imágenes en Azure DevTest Labs
- Uno o varios de destino donde la fábrica distribuirá imágenes golden Azure DevTest Labs
- Un proyecto de DevOps de Azure que se usan para automatizar el generador de imágenes.
- Ubicación del código fuente que contiene los scripts y configuración (en nuestro ejemplo, en el mismo proyecto de DevOps usado anteriormente)
- Una definición de compilación para coordinar las tareas de Azure Powershell
 
## <a name="setting-the-retention-policy"></a>Establecer la directiva de retención
Antes de configurar los pasos de limpieza, definir cuántas imágenes históricas que desee conservar en DevTest Labs. Si ha seguido el [ejecutar un generador de imágenes de Azure DevOps](image-factory-set-up-devops-lab.md) artículo, ha configurado varias Variables de compilación. Uno de ellos estaba **ImageRetention**. Establecer esta variable en `1`, lo que significa que DevTest Labs no conservará un historial de imágenes personalizadas. Solo las imágenes distribuidas más reciente estará disponibles. Si cambia esta variable en `2`, la versión más reciente distribuye la imagen y se mantendrán los anteriores. Puede establecer este valor para definir el número de imágenes históricas que desea mantener en su DevTest Labs.

## <a name="cleaning-up-the-factory"></a>Limpiar la fábrica
El primer paso para limpiar la fábrica es quitar la imagen dorada, las máquinas virtuales desde el generador de imágenes. Hay un script para realizar esta tarea, al igual que los scripts anteriores. El primer paso es agregar otro **Azure Powershell** tareas a la definición de compilación tal como se muestra en la siguiente imagen:

![Paso de PowerShell](./media/set-retention-policy-cleanup/powershell-step.png)

Una vez que la nueva tarea en la lista, seleccione el elemento y rellene todos los detalles, como se muestra en la siguiente imagen:

![Limpiar la tarea anterior de PowerShell de imágenes](./media/set-retention-policy-cleanup/configure-powershell-task.png)

Los parámetros de script son: `-DevTestLabName $(devTestLabName)`.

## <a name="retire-old-images"></a>Retirar imágenes anteriores 
Esta tarea elimina las imágenes anteriores, mantener sólo un historial que coincida con el **ImageRetention** variable de compilación. Agregar otro **Azure Powershell** tarea nuestra definición de compilación de compilación. Una vez agregado, seleccione la tarea y rellene los detalles como se muestra en la siguiente imagen: 

![Retirar la tarea anterior de PowerShell de imágenes](./media/set-retention-policy-cleanup/retire-old-image-task.png)

Los parámetros de script son: `-ConfigurationLocation $(System.DefaultWorkingDirectory)$(ConfigurationLocation) -SubscriptionId $(SubscriptionId) -DevTestLabName $(devTestLabName) -ImagesToSave $(ImageRetention)`

## <a name="queue-the-build"></a>Poner en cola la compilación
Ahora que ha completado la definición de compilación, poner en cola una nueva compilación para asegurarse de que todo funciona. Después de la compilación completa correctamente el nuevas imágenes personalizadas se muestran en el laboratorio de destino y si comprueba el laboratorio de fábrica de imagen, no verá que ninguna máquina virtual aprovisionada. Además, si poner en cola nuevas compilaciones, ver las tareas de limpieza retirar antiguas imágenes personalizadas de los laboratorios de DevTest de acuerdo en el valor de retención establecido en las variables de compilación.

> [!NOTE]
> Si ha ejecutado la canalización de compilación al final del último artículo de la serie, elimine manualmente las máquinas virtuales que se crearon en el laboratorio de fábrica de la imagen antes de poner en cola una compilación nueva.  Solo es necesario el paso de limpieza manual mientras la configuración y comprobar que funciona.



## <a name="summary"></a>Resumen
Ahora tiene un generador de imágenes de ejecución que pueda generar y distribuir imágenes personalizadas para los laboratorios a petición. En este punto, es simplemente una cuestión de obtener sus imágenes configurada correctamente e identificar los laboratorios de destino. Como se mencionó en el artículo anterior, el **Labs.json** archivo se encuentra en su **configuración** carpeta especifica las imágenes que deben estar disponibles en cada uno de los laboratorios de destino. A medida que agrega otros laboratorios de DevTest para su organización, basta con agregar una entrada en el Labs.json para el nuevo laboratorio.

También es sencillo agregar una nueva imagen a la factoría. Cuando van a incluir una nueva imagen de la factoría de abrir el [portal Azure](https://portal.azure.com), vaya a la factoría de DevTest Labs, seleccione el botón para agregar una máquina virtual y elija la imagen de marketplace deseado y los artefactos. En lugar de seleccionar el **crear** botón para realizar la nueva máquina virtual, seleccione **plantilla View Azure Resource Manager**"y guarde la plantilla como un archivo .json en algún lugar dentro de la **GoldenImages** carpeta en el repositorio. La próxima vez que ejecute el generador de imágenes, creará la imagen personalizada.


## <a name="next-steps"></a>Pasos siguientes
1. [Programar la compilación o versión](/azure/devops/pipelines/build/triggers?view=azure-devops&tabs=designer) para ejecutar periódicamente el generador de imágenes. Actualizan sus imágenes generadas por el generador de forma periódica.
2. Hacer más imágenes de oro de la factoría. También puede considerar [crear artefactos](devtest-lab-artifact-author.md) elementos adicionales de las tareas de configuración de máquina virtual de secuencias de comandos e incluir los artefactos de las imágenes de fábrica.
4. Crear un [separar las compilación/versión](/azure/devops/pipelines/overview?view=azure-devops-2019) para ejecutar el **DistributeImages** script por separado. Puede ejecutar esta secuencia de comandos al realizar cambios en Labs.json y obtener las imágenes que se copian en los laboratorios de destino sin tener que volver a crear todas las imágenes de nuevo.

