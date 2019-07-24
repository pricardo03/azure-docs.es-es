---
title: Establecimiento de contraseñas de las máquinas virtuales en Azure Lab Services | Microsoft Docs
description: Aprenda a establecer y restablecer las contraseñas de las máquinas virtuales (VM) en laboratorios de clase de Azure Lab Services.
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
ms.date: 06/11/2019
ms.author: spelluru
ms.openlocfilehash: c1564fadef35a20d0d87db8439ae1cc3dc923318
ms.sourcegitcommit: 22c97298aa0e8bd848ff949f2886c8ad538c1473
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/14/2019
ms.locfileid: "67144103"
---
# <a name="set-or-reset-password-for-virtual-machines-in-classroom-labs"></a>Establecimiento o restablecimiento de contraseñas de las máquinas virtuales en laboratorios de clase
En este artículo se ofrecen distintas formas de establecer y restablecer contraseñas para acceder a máquinas virtuales en laboratorios de clase. 

## <a name="lab-owners-teachers"></a>Propietarios de laboratorio (profesores)
El propietario de un laboratorio (profesor) puede establecer o restablecer la contraseña de las máquinas virtuales en el momento de crear el laboratorio (asistente para la creación de laboratorio) o después de crearlo (en el panel). 

### <a name="set-password-at-the-time-of-lab-creation"></a>Establecimiento de la contraseña en el momento de crear el laboratorio
El propietario de un laboratorio (profesor) puede establecer una contraseña para las máquinas virtuales del laboratorio en la página **Establecer credenciales** del asistente para la creación de laboratorio.

![Establecer credenciales](../media/tutorial-setup-classroom-lab/set-credentials.png)

Mediante la habilitación/deshabilitación de la opción **Usar la misma contraseña para todas las máquinas virtuales** de esta página, un profesor puede optar por utilizar la misma contraseña para todas las máquinas virtuales del laboratorio o permitir que los alumnos establezcan las contraseñas de sus máquinas virtuales. De forma predeterminada, esta opción está habilitada para todas las imágenes del sistema operativo Windows y Linux, excepto para Ubuntu. Cuando está deshabilitada, se pedirá a los alumnos que establezcan una contraseña al intentar conectarse a la máquina virtual por primera vez. 

El propietario del laboratorio puede restablecer la contraseña (si es necesario) en la página **Configurar plantilla** del asistente para la creación de laboratorio. 

![Página Configurar plantilla una vez terminada](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)

El propietario del laboratorio también puede restablecer la contraseña después de crear el laboratorio (en el panel). 

### <a name="reset-password-on-the-dashboard"></a>Restablecimiento de contraseña en el panel

1. Seleccione el menú de desbordamiento (tres puntos verticales) del icono de laboratorio y elija **Restablecer contraseña**. 

    ![Menú de restablecimiento de contraseña en la página principal](../media/how-to-set-virtual-machine-passwords/reset-password-menu-dashboard.png)
1. En el cuadro de diálogo **Establecer contraseña**, escriba una contraseña y seleccione **Establecer contraseña**.
    
    ![Cuadro de diálogo Establecer contraseña](../media/how-to-set-virtual-machine-passwords/set-password.png)

## <a name="lab-users-students"></a>Usuarios del laboratorio (alumnos)
En el momento de crear el laboratorio, el propietario del laboratorio puede habilitar o deshabilitar la opción **Usar la misma contraseña para todas las máquinas virtuales**. Si está habilitada, los alumnos no pueden restablecer la contraseña. Todas las máquinas virtuales de los laboratorios tendrán la misma contraseña que establece el profesor. 

Si está deshabilitada, los usuarios tendrán que establecer una contraseña al intentar conectarse a la máquina virtual por primera vez. Cuando los usuarios (alumnos) seleccionan el botón **Conectar** del icono de laboratorio en la página **Mis máquinas virtuales**, el usuario ve el siguiente cuadro de diálogo para establecer la contraseña de la máquina virtual: 

![Restablecimiento de contraseña del alumno](../media/how-to-set-virtual-machine-passwords/student-set-password.png)

El alumno también puede establecer la contraseña haciendo clic en el menú de desbordamiento (**tres puntos verticales**) del icono de laboratorio y seleccionando **Restablecer contraseña**. 

## <a name="next-steps"></a>Pasos siguientes
Para conocer otras opciones de uso de alumnos que (como propietario del laboratorio) puede configurar, consulte el artículo siguiente: [Configuración del uso de alumnos](how-to-configure-student-usage.md).
