---
title: Configuración de los valores de uso en los laboratorios educativos de Azure Lab Services | Microsoft Docs
description: Obtenga información sobre cómo configurar el número de usuarios del laboratorio, regístrelos en el laboratorio, controle el número de horas que pueden usar la máquina virtual y mucho más.
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
ms.date: 10/01/2018
ms.author: spelluru
ms.openlocfilehash: a91d5826f52b2beb05f2d9a08f3646bd776b294e
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142890"
---
# <a name="configure-usage-settings-and-policies"></a>Configuración de los valores y directivas de uso
En este artículo se describe cómo configurar el número de usuarios del laboratorio, registrar a esos usuarios en el laboratorio, controlar el número de horas que pueden usar la máquina virtual y mucho más. 


## <a name="specify-the-number-of-users-allowed-into-the-lab"></a>Especificación del máximo número de usuarios permitidos en el laboratorio

1. Seleccione **Política de uso**. 
2. En **Política de uso**, Configuración, escriba el **número de usuarios** con permiso para usar el laboratorio.
3. Seleccione **Guardar**. 

    ![Política de uso](../media/how-to-manage-classroom-labs/usage-policy-settings.png)

## <a name="send-registration-link-to-users"></a>Envío del vínculo de registro a los usuarios

1. Cambie a la vista **Panel**. 
2. Seleccione el icono **Registro de usuario**.

    ![Vínculo de registro del estudiante](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. En el cuadro de diálogo **Registro de usuario**, seleccione el botón **Copiar**. El vínculo se copia en el Portapapeles. Péguelo en un editor de correo electrónico y envíe un correo electrónico al estudiante. Los estudiantes usan el vínculo para ir a la página que les ayuda con el registro en el laboratorio. 

    ![Vínculo de registro del estudiante](../media/tutorial-setup-classroom-lab/registration-link.png)
2. En el cuadro de diálogo **Registro de usuario**, seleccione **Cerrar**. 

## <a name="view-users-registered-with-the-lab"></a>Ver los usuarios registrados en el laboratorio

Seleccione **Usuarios** en el menú a la izquierda para ver la lista de usuarios registrados en el laboratorio. 

![Lista de usuarios registrados en el laboratorio](../media/how-to-configure-student-usage/users-list.png)

## <a name="set-quotas-per-user"></a>Establecimiento de cuotas por usuario

1. Seleccione **Configuración de usuario** en el menú a la izquierda.
2. Seleccione el icono **Quota per user** (Cuota por usuario). 
3. Seleccione **Limit the number of hours a user can use a VM** (Limitar el número de horas que un usuario puede utilizar la máquina virtual). 
4. Para **How many hours do you want to give to each user?** (¿Cuántas horas desea dar a cada usuario?), escriba el número de horas y seleccione **Guardar**. 

    ![Número de horas por usuario](../media/how-to-configure-student-usage/number-of-hours-per-user.png)
5. Ahora verá el número de horas en el icono **Quota per user** (Cuota por usuario). 

    ![Quota per user (Cuota por usuario)](../media/how-to-configure-student-usage/quota-per-user.png)

## <a name="manage-user-vms"></a>Administración de las máquinas virtuales de usuario
Una vez que los estudiantes se registren en Azure Lab Services mediante el vínculo de registro que les proporcionó, verá las máquinas virtuales asignadas a los estudiantes en la pestaña **Máquinas virtuales**. 

![Máquinas virtuales asignadas a estudiantes](../media/how-to-manage-classroom-labs/virtual-machines-students.png)

Puede realizar las siguientes tareas en la máquina virtual de un estudiante: 

- Detener una máquina virtual si se está ejecutando. 
- Iniciar una máquina virtual si está detenida. 
- Conéctese a la máquina virtual. 
- Elimine la máquina virtual. 
- Vea el número de horas que los usuarios usan la máquina virtual. 


## <a name="next-steps"></a>Pasos siguientes
Introducción a la configuración de un laboratorio con Azure Lab Services:

- [Configuración de un laboratorio educativo](how-to-manage-classroom-labs.md)
- [Configuración de un laboratorio](../tutorial-create-custom-lab.md)
