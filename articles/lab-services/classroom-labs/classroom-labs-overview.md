---
title: Acerca de Azure Lab Services | Microsoft Docs
description: Obtenga información sobre cómo Lab Services puede facilitar la creación, administración y protección de laboratorios con máquinas virtuales que pueden usar los desarrolladores, evaluadores, formadores, alumnos y otros usuarios.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 05/21/2018
ms.author: spelluru
ms.openlocfilehash: e9c3cae7c7129cc489ddd38b5b2de18dd6f52e58
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660596"
---
# <a name="introduction-to-classroom-labs"></a>Introducción a los laboratorios educativos
Azure Lab Services le permite configurar rápidamente un entorno de laboratorio educativo en la nube. Un profesor crea un laboratorio educativo, aprovisiona máquinas virtuales Windows o Linux, instala el software y las herramientas que se necesiten en el aula y las pone a disposición de los alumnos. Los alumnos se conectan a las máquinas virtuales (VM) del laboratorio y las usan para sus proyectos, tareas y ejercicios de clase. 

Los laboratorios educativos los administra Azure. El servicio controla toda la administración de la infraestructura de un laboratorio administrado, desde la sincronización de máquinas virtuales al control de errores y el escalado de infraestructuras. Puede especificar qué tipo de infraestructura va a necesitar e instalar todas las herramientas o software que sea necesario para la clase. Los laboratorios administrados se encuentran actualmente en versión preliminar.  

## <a name="scenarios"></a>Escenarios
Este es el escenario principal que admiten los laboratorios educativos de Azure Lab Services: 

### <a name="set-up-a-resizable-computer-lab-in-the-cloud-for-your-classroom"></a>Configuración de un laboratorio de equipos redimensionables en la nube para clases  

- Creación de un laboratorio educativo administrado. Solo tiene que indicar al servicio exactamente qué necesita y este creará y administrará la infraestructura del laboratorio para que pueda centrarse en la clase, no en los detalles técnicos del laboratorio. 
- Proporcione a los alumnos un laboratorio de máquinas virtuales que se configuran exactamente con todo lo necesario para una clase. Asigne a cada alumno un número limitado de horas para el uso de máquinas virtuales para trabajos de clase.  
- Desplace un laboratorio de equipos físicos del centro educativo a la nube. Escale automáticamente el número de máquinas virtuales solo al umbral de costo y uso máximo que establezca en el laboratorio. 
- Elimine el laboratorio con un solo clic cuando haya terminado. 

## <a name="user-profiles"></a>Perfiles de usuario
En este artículo se describen distintos perfiles de usuario de Azure Lab Services. 

### <a name="lab-account-owner"></a>Propietario de la cuenta del laboratorio
Por lo general, el administrador de TI de los recursos de nube de la organización, que es el propietario de la suscripción de Azure, actúa como propietario de la cuenta del laboratorio y realiza las siguientes tareas:   

- Configura una cuenta de laboratorio para la organización.
- Administra y configura las directivas en todos los laboratorios.
- Concede permisos a personas de la organización para crear un laboratorio en la cuenta del laboratorio.

### <a name="educator"></a>Profesor
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
