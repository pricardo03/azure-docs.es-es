---
title: Configuración de un laboratorio con Azure Lab Services | Microsoft Docs
description: En este tutorial, va a configurar un laboratorio para usarlo en una clase.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 06/11/2019
ms.author: spelluru
ms.openlocfilehash: 0c50a321cbeb0d07a5039038ff796df00463ac8a
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/22/2019
ms.locfileid: "68385675"
---
# <a name="tutorial-set-up-a-classroom-lab"></a>Tutorial: Configuración de un laboratorio educativo 
En este tutorial, se va a configurar un laboratorio de clase con las máquinas virtuales que van a utilizar los estudiantes de la clase.  

En este tutorial realizará lo siguiente:

> [!div class="checklist"]
> * Creación de un laboratorio educativo
> * Incorporación de usuarios al laboratorio
> * Envío del vínculo de registro a los estudiantes

## <a name="prerequisites"></a>Requisitos previos
Para configurar un laboratorio educativo en una cuenta de laboratorio, debe ser miembro de alguno de estos roles en la cuenta de laboratorio: Propietario, Creador de laboratorio o Colaborador. La cuenta que usó para crear una cuenta de laboratorio se agrega automáticamente al rol de propietario.

El propietario de un laboratorio puede agregar a otros usuarios al rol **Creador de laboratorio**. Por ejemplo, un propietario de laboratorio agrega profesores al rol Creador de laboratorio. A continuación, los profesores pueden crear laboratorios con máquinas virtuales para sus clases. Los estudiantes pueden usar el vínculo de registro que reciban de los profesores para registrar el laboratorio. Una vez registrados, pueden usar las máquinas virtuales de los laboratorios para hacer las tareas de clase y de casa. Para obtener instrucciones detalladas sobre cómo agregar usuarios al rol Creador de laboratorio, consulte [Incorporación de un usuario al rol Creador de laboratorio](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).


## <a name="create-a-classroom-lab"></a>Creación de un laboratorio educativo

1. Vaya al [sitio web de Azure Lab Services](https://labs.azure.com). Tenga en cuenta que Internet Explorer 11 aún no se admite. 
2. Seleccione **Iniciar sesión** y escriba las credenciales. Azure Lab Services es compatible con cuentas profesionales y cuentas Microsoft. 
3. En la ventana **Nuevo laboratorio**, lleve a cabo las siguientes acciones: 
    1. Especifique un **nombre** para el laboratorio. 
    2. Especifique el máximo **número de máquinas virtuales** del laboratorio. Puede aumentar o reducir el número de máquinas virtuales después de crear el laboratorio o en un laboratorio existente. Para más información, consulte [Actualización del número de máquinas virtuales en un laboratorio](how-to-configure-student-usage.md#update-number-of-virtual-machines-in-lab).
    6. Seleccione **Guardar**.

        ![Creación de un laboratorio educativo](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. En la página **Seleccionar especificaciones de máquina virtual**, realice los pasos siguientes:
    1. Seleccione un **tamaño** para las máquinas virtuales (VM) creadas en el laboratorio. Actualmente, se permiten los tamaños **pequeño**, **medio**, **medio (virtualización)** , **grande** y **GPU**.
    3. Seleccione la **imagen de máquina virtual** que se usará para crear máquinas virtuales en el laboratorio. Si selecciona una imagen de Linux, verá una opción para habilitar la conexión a Escritorio remoto para ella. Para más información, consulte [Habilitar conexión de escritorio remoto para Linux](how-to-enable-remote-desktop-linux.md).
    4. Seleccione **Next** (Siguiente).

        ![Especificaciones de máquina virtual](../media/tutorial-setup-classroom-lab/select-vm-specifications.png)    
5. En la página **Establecer credenciales**, especifique las credenciales predeterminadas de todas las máquinas virtuales del laboratorio. 
    1. Especifique el **nombre del usuario** para todas las máquinas virtuales del laboratorio.
    2. Especifique la **contraseña** del usuario. 

        > [!IMPORTANT]
        > Tome nota de ambos. No se volverán a mostrar.
    3. Seleccione **Crear**. 

        ![Establecer credenciales](../media/tutorial-setup-classroom-lab/set-credentials.png)
6. En la página **Configurar plantilla**, puede ver el estado del proceso de creación del laboratorio. La creación de la plantilla en el laboratorio tarda un máximo de 20 minutos. 

    ![Configurar plantilla](../media/tutorial-setup-classroom-lab/configure-template.png)
7. Una vez completada la configuración de la plantilla, verá la siguiente página: 

    ![Página Configurar plantilla una vez terminada](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)
8. En la página **Configurar plantilla**, siga estos pasos: Estos pasos son **opcionales** para el tutorial.
    2. Seleccione **Conectar** para conectarse a la plantilla de máquina virtual. Si es una plantilla de máquina virtual Linux, decida si desea conectarse mediante SSH o RDP (si RDP está habilitado).
    1. Seleccione **Restablecer contraseña** para restablecer la contraseña de la máquina virtual. 
    1. Instale y configure el software en la plantilla de máquina virtual. 
    1. **Pare** la máquina virtual.  
    1. Escriba una **descripción** para la plantilla.
9. Seleccione **Siguiente** en la página de plantilla. 
10. En la página **Publicar la plantilla**, realice las acciones siguientes. 
    1. Para publicar inmediatamente la plantilla, seleccione **Publicar**.  

        > [!WARNING]
        > Una vez que publique, no se puede cancelar la publicación. 
    2. Para publicar más adelante, seleccione **Guardar para más adelante**. Puede publicar la plantilla de máquina virtual una vez finalizado el asistente. Para más información sobre cómo configurar y publicar una vez finalizado el asistente, consulte la sección [Publicación de la plantilla](how-to-create-manage-template.md#publish-the-template-vm) del artículo [Administración de laboratorios educativos](how-to-manage-classroom-labs.md).

        ![Publicar plantilla](../media/tutorial-setup-classroom-lab/publish-template.png)
11. Puede ver el **progreso de la publicación** de la plantilla. Este proceso puede tardar hasta una hora. 

    ![Publicar plantilla: progreso](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. Podrá ver la página siguiente cuando la plantilla se haya publicado correctamente. Seleccione **Listo**.

    ![Publicar plantilla: correctamente](../media/tutorial-setup-classroom-lab/publish-success.png)
1. Verá el **panel** del laboratorio. 
    
    ![Panel del laboratorio de clase](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)
4. Cambie a la página **Máquinas virtuales**, para lo que debe seleccionar Máquinas virtuales en el menú izquierdo o icono de las máquinas virtuales. Confirme que ve las máquinas virtuales que están en el estado **Sin asignar**. Estas máquinas virtuales no están asignadas a los alumnos todavía. Deberían estar en estado **Detenida**. En esta pagina, puede iniciar una máquina virtual de un alumno, conectarse a la máquina virtual, detener la máquina virtual y eliminar la máquina virtual. Puede iniciarlas en esta página o dejar que los alumnos inicien las máquinas virtuales. 

    ![Máquinas virtuales en estado detenido](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)

## <a name="add-users-to-the-lab"></a>Incorporación de usuarios al laboratorio

1. En el menú izquierdo, seleccione **Users** (Usuarios). De forma predeterminada, está habilitada la opción **Restrict access** (Restringir acceso). Cuando esta opción está activada, los usuarios no se pueden registrar en el laboratorio aunque tengan el vínculo de registro si no aparecen en la lista de usuarios. Solo los usuarios de la lista pueden registrarse en el laboratorio mediante el vínculo de registro enviado. En este procedimiento, agregará usuarios a la lista. Como alternativa, puede desactivar **Restrict access** (Restringir acceso), que permite que los usuarios se registren en el laboratorio, siempre y cuando tengan el vínculo de registro. 
2. Seleccione **Add users** (Agregar usuarios) en la barra de herramientas. 

    ![Botón Agregar usuarios](../media/how-to-configure-student-usage/add-users-button.png)
1. En la página **Add users** (Agregar usuarios), escriba las direcciones de correo electrónico de los usuarios en líneas independientes, o bien en una sola línea separadas por punto y coma. 

    ![Incorporación de direcciones de correo electrónico de usuarios](../media/how-to-configure-student-usage/add-users-email-addresses.png)
4. Seleccione **Guardar**. Verá las direcciones de correo electrónico de los usuarios y sus estados (registrados o no) en la lista. 

    ![Lista de usuarios](../media/how-to-configure-student-usage/users-list-new.png)

## <a name="set-quotas-for-users"></a>Establecimiento de cuotas para los usuarios
Puede establecer cuotas por usuario mediante los pasos siguientes: 

1. Seleccione **Usuarios** en el menú de la izquierda si la página todavía no está activa. 
2. Seleccione **Quota per user: (Cuota por usuario:) 10 horas** en la barra de herramientas. 
3. En la página **Cuota por usuario**, especifique el número de horas que quiere dar a cada usuario (alumno): 
    1. **Número total de horas de laboratorio por usuario**. Los usuarios pueden usar sus máquinas virtuales para el número establecido de horas (que se especifica para este campo) **además del tiempo programado**. Si selecciona esta opción, escriba el **número de horas** en el cuadro de texto. 

        ![Número de horas por usuario](../media/how-to-configure-student-usage/number-of-hours-per-user.png). 
    1. **0 horas (solo programación)** . Los usuarios pueden usar sus máquinas virtuales durante el tiempo programado o cuando usted, como el propietario del laboratorio, les enciende las máquinas virtuales.

        ![Cero horas: solo el tiempo programado](../media/how-to-configure-student-usage/zero-hours.png)
    4. Seleccione **Guardar**. 
5. Consulte los valores modificados en la barra de herramientas: **Cuota por usuario: &lt;número de horas&gt;** . 

    ![Quota per user (Cuota por usuario)](../media/how-to-configure-student-usage/quota-per-user.png)

## <a name="set-a-schedule-for-the-lab"></a>Configuración de una programación para el laboratorio
Si configuró la cuota en **0 horas (solo programación)** , debe establecer una programación para el laboratorio. En este tutorial, establece la programación como una programación semanal recurrente.

1. Cambie a la página **Schedules** (Programaciones) y seleccione **Add schedule** (Agregar programación) en la barra de herramientas. 

    ![Botón Add schedule (Agregar programación) en la página Schedules (Programaciones)](../media/how-to-create-schedules/add-schedule-button.png)
2. En la página **Add schedule** (Agregar programación), cambie a **Weekly** (Semanal) en la parte superior. 
3. En **Schedule days (required)** (Días de programación [obligatorio]), seleccione los días en los que quiere que se aplique la programación. En el ejemplo siguiente, está seleccionado Monday-Friday (Lunes a viernes). 
4. En el campo **From** (De), escriba la **fecha de inicio de la programación** o use el botón de **calendario** para seleccionar una fecha. Este campo es obligatorio. 
5. En **Schedule end date** (Fecha de finalización de programación), escriba o seleccione una fecha de finalización en la que se apagarán las máquinas virtuales. 
6. En **Start time** (Hora de inicio), seleccione la hora a la que se iniciarán las máquinas virtuales. La hora de inicio es obligatoria si no se establece la hora de finalización. Seleccione **Remove start event** (Quitar evento de inicio) si solo quiere especificar la hora de finalización. Si **Start time** (Hora de inicio) está deshabilitada, seleccione **Add start event** (Agregar evento de inicio) junto a la lista desplegable para habilitarla. 
7. En **Stop time** (Hora de finalización), seleccione la hora a la que se apagarán las máquinas virtuales. La hora de finalización es obligatoria si no se establece la hora de inicio. Seleccione **Remove stop event** (Quitar evento de finalización) si solo quiere especificar la hora de inicio. Si **Stop time** (Hora de finalización) está deshabilitada, seleccione **Add stop event** (Agregar evento de finalización) junto a la lista desplegable para habilitarla.
8. En **Time zone (required)** (Zona horaria [obligatorio]), seleccione la zona horaria de las horas de inicio y finalización que ha especificado.  
9. En **Notes** (Notas), escriba alguna descripción o nota para la programación. 
10. Seleccione **Guardar**. 

    ![Programación semanal](../media/how-to-create-schedules/add-schedule-page-weekly.png)

## <a name="send-an-email-with-the-registration-link"></a>Envío de un correo electrónico con el vínculo de registro

1. Cambie a la vista **Users** (Usuarios) si aún no está en la página. 
2. Seleccione usuarios específicos o todos ellos en la lista. Para seleccionar usuarios específicos, active las casillas de la primera columna de la lista. Para seleccionar todos los usuarios, seleccione la casilla situada delante del título de la primera columna (**Nombre**) o seleccione todas las casillas d todos los usuarios de la lista. Puede ver el **estado de la invitación** en esta lista.  En la siguiente imagen, se establece el estado de la invitación para todos los alumnos en **Invitación no enviada**. 

    ![Selección de alumnos](../media/tutorial-setup-classroom-lab/select-students.png)
1. Seleccione el **icono de correo electrónico (sobre)** en una de las filas o seleccione **Enviar invitación** en la barra de herramientas. También puede mantener el ratón sobre un nombre de alumno en la lista para ver el icono de correo electrónico. 

    ![Envío del vínculo de registro por correo electrónico](../media/tutorial-setup-classroom-lab/send-email.png)
4. En la página **Send registration link by email** (Enviar vínculo de registro por correo electrónico), siga estos pasos: 
    1. Escriba un **mensaje opcional** que desee enviar a los alumnos. El correo electrónico incluye automáticamente el vínculo de registro. 
    2. En la página **Send registration link by email** (Enviar vínculo de registro por correo electrónico), seleccione **Enviar**. Puede ver cómo cambia el estado de la invitación a **Enviando invitación** y, a continuación, a **Invitación enviada**. 
        
        ![Invitaciones enviadas](../media/tutorial-setup-classroom-lab/invitations-sent.png)

## <a name="next-steps"></a>Pasos siguientes
En este tutorial, ha creado un laboratorio de clase y ha configurado el laboratorio. Para saber cómo el estudiante puede acceder a una máquina virtual en el laboratorio con el enlace de registro, avance al siguiente tutorial:

> [!div class="nextstepaction"]
> [Conexión a una máquina virtual en el laboratorio de clase](tutorial-connect-virtual-machine-classroom-lab.md)

