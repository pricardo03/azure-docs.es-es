---
title: 'Conceptos de laboratorios de clase: Azure Lab Services | Microsoft Docs'
description: Obtenga información sobre los conceptos básicos de los servicios de laboratorio y cómo puede facilitar crear y administrar laboratorios.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2019
ms.author: spelluru
ms.openlocfilehash: 82f9d7090d7283e0b00b36e0928fffb3395ca4c0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60679094"
---
# <a name="classroom-labs-concepts"></a>Conceptos de laboratorios educativos
En la lista siguiente contiene las definiciones y los conceptos clave de los servicios de laboratorio:

## <a name="quota"></a>Quota
Cuota es el límite de tiempo (en horas) que puede establecer un profesor de un alumno usar una máquina virtual de laboratorio. Se puede establecer en 0, ilimitado, o un número específico de horas. Si la cuota se establece en 0, un estudiante puede usar solo la máquina virtual cuando se ejecuta una programación o cuando un profesor activa manualmente en la máquina virtual para el alumno.
 
## <a name="schedules"></a>Programaciones
Las programaciones son los intervalos de tiempo (una vez o manera periódica) que puede crear un profesor para la clase. Todas las máquinas virtuales en el laboratorio se inician automáticamente al comienzo a la programación y se detienen al final de la programación. Horas de cuota no se utilizan cuando se ejecuta una programación.

## <a name="template-virtual-machine"></a>Máquina virtual de plantilla
Una máquina virtual de plantilla en un laboratorio es una imagen de máquina virtual base desde la que se crean las máquinas virtuales de todos los usuarios. Creadores de instructores/laboratorio configuración la máquina virtual de plantilla y configuración con el software que desean proporcionar a los asistentes formación hacer laboratorios. Al publicar una plantilla de máquina virtual, Azure Lab Services crea o actualiza las máquinas virtuales de laboratorio según la plantilla de máquina virtual. 


## <a name="user-profiles"></a>Perfiles de usuario
En este artículo se describen distintos perfiles de usuario de Azure Lab Services. 

### <a name="lab-account-owner"></a>Propietario de la cuenta del laboratorio
Por lo general, el administrador de TI de los recursos de nube de la organización, que es el propietario de la suscripción de Azure, actúa como propietario de la cuenta del laboratorio y realiza las siguientes tareas:   

- Configura una cuenta de laboratorio para la organización.
- Administra y configura las directivas en todos los laboratorios.
- Concede permisos a personas de la organización para crear un laboratorio en la cuenta del laboratorio.

### <a name="professor"></a>Profesor
Normalmente, los usuarios como un profesor o un instructor en línea, crean laboratorios en una cuenta de laboratorio. Un profesor realiza las siguientes tareas: 

- Crea un laboratorio educativo.
- Crea máquinas virtuales en el laboratorio. 
- Instala el software adecuado en máquinas virtuales.
- Especifica quién puede obtener acceso al laboratorio.
- Proporciona vínculos de registro en el laboratorio para los alumnos.

### <a name="student"></a>Estudiante
El alumno hace las siguientes tareas:

- Usa el vínculo de registro al laboratorio que el usuario recibe de un creador de laboratorio para registrarse. 
- Se conecta a una máquina virtual del laboratorio y la usa para realizar trabajos de clase, tareas y proyectos. 

## <a name="next-steps"></a>Pasos siguientes
Introducción a la configuración de una cuenta de laboratorio necesaria para crear un laboratorio educativo mediante Azure Lab Services:

- [Configuración de una cuenta de laboratorio](tutorial-setup-lab-account.md)
