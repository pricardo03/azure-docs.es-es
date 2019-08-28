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
ms.date: 08/20/2019
ms.author: spelluru
ms.openlocfilehash: a4cb2abec429a790f493f95d3d16b2ff7b3eb445
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2019
ms.locfileid: "69645024"
---
# <a name="set-or-reset-password-for-virtual-machines-in-classroom-labs-instructor"></a>Establecimiento o restablecimiento de contraseñas de las máquinas virtuales en laboratorios de clase (instructor)
El propietario de un laboratorio (profesor) puede establecer o restablecer la contraseña de las máquinas virtuales en el momento de crear el laboratorio (asistente para la creación de laboratorio) o después de crearlo (en el panel). 

## <a name="set-password-at-the-time-of-lab-creation"></a>Establecimiento de la contraseña en el momento de crear el laboratorio
El propietario de un laboratorio (profesor) puede establecer una contraseña para las máquinas virtuales del laboratorio en la página **Establecer credenciales** del asistente para la creación de laboratorio.

![Establecer credenciales](../media/tutorial-setup-classroom-lab/set-credentials.png)

Mediante la habilitación/deshabilitación de la opción **Usar la misma contraseña para todas las máquinas virtuales** de esta página, un profesor puede optar por utilizar la misma contraseña para todas las máquinas virtuales del laboratorio o permitir que los alumnos establezcan las contraseñas de sus máquinas virtuales. De forma predeterminada, esta opción está habilitada para todas las imágenes del sistema operativo Windows y Linux, excepto para Ubuntu. Cuando está deshabilitada, se pedirá a los alumnos que establezcan una contraseña al intentar conectarse a la máquina virtual por primera vez. 

El propietario del laboratorio puede restablecer la contraseña (si es necesario) en la página **Configurar plantilla** del asistente para la creación de laboratorio. 

![Página Configurar plantilla una vez terminada](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)

El propietario del laboratorio también puede restablecer la contraseña después de crear el laboratorio (en el panel). 

## <a name="reset-password-on-the-dashboard"></a>Restablecimiento de contraseña en el panel

1. Seleccione el menú de desbordamiento (tres puntos verticales) del icono de laboratorio y elija **Restablecer contraseña**. 

    ![Menú de restablecimiento de contraseña en la página principal](../media/how-to-set-virtual-machine-passwords/reset-password-menu-dashboard.png)
1. En el cuadro de diálogo **Establecer contraseña**, escriba una contraseña y seleccione **Establecer contraseña**.
    
    ![Cuadro de diálogo Establecer contraseña](../media/how-to-set-virtual-machine-passwords/set-password.png)

## <a name="next-steps"></a>Pasos siguientes
Para conocer otras opciones de uso de alumnos que (como propietario del laboratorio) puede configurar, consulte el artículo siguiente: [Configuración del uso de alumnos](how-to-configure-student-usage.md).

Para obtener más información sobre cómo los alumnos puedes restablecer las contraseñas de sus máquinas virtuales, consulte [Set or reset password for virtual machines in classroom labs (students)](how-to-set-virtual-machine-passwords-student.md) (Establecimiento o restablecimiento de contraseñas de las máquinas virtuales en laboratorios de clase [alumnos]).