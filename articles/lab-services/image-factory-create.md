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
ms.openlocfilehash: cf1bb31614c04d6073bc40c510fc43b2f8e4e189
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60622651"
---
# <a name="create-a-custom-image-factory-in-azure-devtest-labs"></a>Crear un generador de imágenes personalizadas en Azure DevTest Labs
Este artículo muestra cómo configurar una fábrica de imagen personalizada mediante el uso de scripts de ejemplo disponibles en el [repositorio de Git](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/Scripts/ImageFactory).

## <a name="whats-an-image-factory"></a>¿Qué es un generador de imágenes?
Un generador de imágenes es una solución de configuración como código que crea y distribuye las imágenes automáticamente de forma periódica con todas las configuraciones deseadas. Las imágenes en el generador de imágenes siempre están actualizadas y el mantenimiento en curso es casi igual a cero una vez que todo el proceso se automatiza. Y, dado que todas las configuraciones necesarias ya están en la imagen, guarda la hora de configurar manualmente el sistema una vez creada una máquina virtual con el sistema operativo base.

El Acelerador importante para obtener un escritorio de desarrollador a un estado listo en DevTest Labs usa imágenes personalizadas. La desventaja de imágenes personalizadas es que hay algo adicional para mantener en el laboratorio. Por ejemplo, expiran las versiones de prueba de productos con el tiempo (o) no se aplican las actualizaciones de seguridad más recientes, que forzar que actualice periódicamente la imagen personalizada. Con un generador de imágenes, tiene una definición de la imagen que se protege en el control de código fuente y tiene un proceso automatizado para generar imágenes personalizadas basadas en la definición.

La solución permite que la velocidad de creación de máquinas virtuales desde imágenes personalizadas al tiempo que elimina los costos adicionales de mantenimiento en curso. Con esta solución, automáticamente puede crear imágenes personalizadas, distribuirlos a otros laboratorios de DevTest y retirar las imágenes anteriores. En el siguiente vídeo, obtendrá información sobre el generador de imágenes y cómo se implementa con DevTest Labs.  Todos los scripts de Powershell de Azure están disponibles libremente y se encuentra aquí: [ https://aka.ms/dtlimagefactory ](https://aka.ms/dtlimagefactory).

<br/>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Custom-Image-Factory-with-Azure-DevTest-Labs/player]


## <a name="high-level-view-of-the-solution"></a>Visión general de la solución
La solución permite que la velocidad de creación de máquinas virtuales desde imágenes personalizadas al tiempo que elimina los costos adicionales de mantenimiento en curso. Automáticamente con esta solución, puede crear imágenes personalizadas y distribuirlas a otros DevTest Labs. Usar Azure DevOps (anteriormente Visual Studio Team Services) como el motor de orquestaciones para automatizar todas las operaciones en DevTest Labs.

![Visión general de la solución](./media/create-image-factory/high-level-view-of-solution.png)

Hay un [extensión de VSTS de DevTest Labs](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) que le permite ejecutar estos pasos individuales:

- Creación de una imagen personalizada
- Creación de una máquina virtual
- Eliminar la máquina virtual
- Crear entorno
- Eliminar entorno
- Rellenar el entorno

Uso de la extensión de DevTest Labs es una manera fácil de empezar a crear automáticamente las imágenes personalizadas en DevTest Labs.

Hay una implementación alternativa mediante script de PowerShell para un escenario más complejo. Uso de PowerShell, puede automatizar completamente un generador de imágenes basado en DevTest Labs que se puede usar en la cadena de herramientas de entrega continua (CI/CD) e integración continuas. Son los principios que sigue en esta solución alternativa:

- Actualizaciones común, es necesario ningún cambio en el generador de imágenes. (por ejemplo, agregando un nuevo tipo de imagen personalizada, retiran automáticamente las imágenes anteriores, agregar un nuevo 'endpoint' DevTest Labs para recibir imágenes personalizadas y así sucesivamente).
- Cambios comunes están respaldados por el control de código fuente (infraestructura como código)
- Recibir imágenes personalizadas de DevTest Labs no puede estar en la misma suscripción de Azure (laboratorios abarcan las suscripciones)
- Scripts de PowerShell deben ser reutilizables, por lo que nos podemos poner en marcha generadores adicionales según sea necesario

## <a name="next-steps"></a>Pasos siguientes
Continuar con el siguiente artículo de esta sección: [Ejecute un generador de imágenes de Azure DevOps](image-factory-set-up-devops-lab.md)
