---
title: Guardar y distribuir imágenes en Azure DevTest Labs | Microsoft Docs
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
ms.openlocfilehash: feabd055833e5f0d850138af528cce1da82cae49
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58440104"
---
# <a name="save-custom-images-and-distribute-to-multiple-labs"></a>Guardar las imágenes personalizadas y distribuir a varios laboratorios
En este artículo abarca le proporciona los pasos necesarios para guardar las imágenes personalizadas de las máquinas virtuales ya se ha creado (VM). También cubre cómo distribuir estas imágenes personalizadas con otros laboratorios de DevTest de la organización.

## <a name="prerequisites"></a>Requisitos previos
Los siguientes elementos ya deben estar en su lugar:

- Un laboratorio para el generador de imágenes en Azure DevTest Labs.
- Un proyecto de DevOps de Azure que se usa para automatizar el generador de imágenes.
- Ubicación del código fuente que contiene los scripts y configuración (en nuestro ejemplo, en el mismo proyecto de DevOps mencionado en el paso anterior).
- Crear definición para organizar las tareas de Powershell de Azure.

Si es necesario, siga los pasos de la [ejecutar un generador de imágenes de Azure DevOps](image-factory-set-up-devops-lab.md) para crear o configurar estos elementos. 

## <a name="save-vms-as-generalized-vhds"></a>Guardar máquinas virtuales como un VHD generalizado
Guardar las máquinas virtuales existentes como VHD generalizados.  Hay un script de PowerShell de ejemplo para guardar las máquinas virtuales existentes como VHD generalizados. Para ello, en primer lugar, agregue otro **Azure Powershell** tareas a la definición de compilación tal como se muestra en la siguiente imagen:

![Agregar paso de Azure PowerShell](./media/save-distribute-custom-images/powershell-step.png)

Una vez que la nueva tarea en la lista, seleccione el elemento, por lo que nos podemos rellenar todos los detalles como se muestra en la siguiente imagen: 

![Configuración de PowerShell](./media/save-distribute-custom-images/powershell-settings.png)


## <a name="generalized-vs-specialized-custom-images"></a>Generalizado frente a especializado imágenes personalizadas
En el [portal Azure](https://portal.azure.com), al crear una imagen personalizada desde una máquina virtual, puede elegir tener generalizada o una imagen especializada personalizada.

- **Imagen personalizada especializado:** NO se ejecutó Sysprep o desaprovisionamiento en el equipo. Significa que la imagen es una copia exacta del disco del sistema operativo en la máquina virtual existente (una instantánea).  El mismo archivos, aplicaciones, las cuentas de usuario, nombre de equipo y así sucesivamente, están presentes cuando se crea una nueva máquina desde esta imagen personalizada.
- **Generalizar la imagen personalizada:** Sysprep o desaprovisionamiento se ejecutó en el equipo.  Cuando se ejecuta este proceso, quita las cuentas de usuario, quita el nombre del equipo, elimina los subárboles del registro de usuario, etc., con el objetivo de generalizar la imagen, por lo que puede personalizarse al crear otra máquina virtual.  Al generalizar una máquina virtual (al ejecutar sysprep), la máquina virtual actual destruye el proceso: ya no será funcional.

La secuencia de comandos para ajustar las imágenes personalizadas en el generador de imágenes guardará los discos duros virtuales para las máquinas virtuales creadas en el paso anterior (identificado basándose en una etiqueta en el recurso en Azure).

## <a name="update-configuration-for-distributing-images"></a>Configuración de actualización para la distribución de imágenes
El siguiente paso en el proceso es insertar la imagen personalizada desde el laboratorio de fábrica de imagen horizontal a otros de los laboratorios que las necesiten. La parte principal de este proceso es el **labs.json** archivo de configuración. Puede encontrar este archivo en el **configuración** carpeta incluida en el generador de imágenes.

Hay dos cuestiones clave que se muestran en el archivo de configuración labs.json:

- Que identifica un laboratorio de destino específico con el identificador de suscripción y el nombre de laboratorio.
- El conjunto específico de imágenes que se debe insertar en el laboratorio como rutas de acceso relativas a la raíz de configuración. Puede especificar una carpeta completa (para obtener todas las imágenes en esa carpeta) demasiado.

Este es un ejemplo de archivo labs.json con dos prácticas que se muestran. En este caso, va a distribuir las imágenes a dos laboratorios diferentes.

```json
{
   "Labs": [
      {
         "SubscriptionId": "<subscription ID that contains the lab>",
         "LabName": "<Name of the DevTest Lab>",
         "ImagePaths": [
               "Win2012R2",
               "Win2016/Datacenter.json"
         ]
      },
      {
         "SubscriptionId": "<subscription ID that contains the lab>",
         "LabName": "<Name of the DevTest Lab>",
         "ImagePaths": [
               "Win2016/Datacenter.json"
         ]
      }
   ]
}
```

## <a name="create-a-build-task"></a>Creación de una tarea de compilación
Con los mismos pasos que ha visto anteriormente en este artículo, agregue más **Azure Powershell** definición de compilación de la tarea de compilación. Rellene los detalles como se muestra en la siguiente imagen: 

![Tarea para distribuir las imágenes de compilación](./media/save-distribute-custom-images/second-build-task-powershell.png)

Los parámetros son: `-ConfigurationLocation $(System.DefaultWorkingDirectory)$(ConfigurationLocation) -SubscriptionId $(SubscriptionId) -DevTestLabName $(DevTestLabName) -maxConcurrentJobs 20`

Esta tarea toma las imágenes personalizadas presentes en el generador de imágenes y expulsa a cualquier laboratorio definido en el archivo Labs.json.

## <a name="queue-the-build"></a>Poner en cola la compilación
Una vez completada la tarea de compilación de distribución, poner en cola una compilación nueva para asegurarse de que todo funciona. Después de la compilación se completa correctamente, las nuevas imágenes personalizadas se mostrará en el laboratorio de destino que se especificó en el archivo de configuración Labs.json.

## <a name="next-steps"></a>Pasos siguientes
En el siguiente artículo de la serie, se actualiza el generador de imágenes con un pasos de limpieza y directivas de retención: [Establecer directiva de retención y ejecutar scripts de limpieza](image-factory-set-retention-policy-cleanup.md).
