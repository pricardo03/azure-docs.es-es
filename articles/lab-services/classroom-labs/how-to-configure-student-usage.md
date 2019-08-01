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
ms.date: 06/11/2019
ms.author: spelluru
ms.openlocfilehash: 86f22864c416ad2a90bea09c02675d6eb3322308
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/22/2019
ms.locfileid: "68385641"
---
# <a name="configure-usage-settings-and-policies"></a>Configuración de los valores y directivas de uso
En este artículo se describe cómo agregar usuarios al laboratorio, registrarlos en el laboratorio, controlar el número de horas que pueden usar la máquina virtual y mucho más. 


## <a name="add-users-to-the-lab"></a>Incorporación de usuarios al laboratorio
Si tiene la opción **Restringir acceso** habilitada, agregue usuarios (direcciones de correo electrónico) a la lista.

1. En el menú izquierdo, seleccione **Users** (Usuarios).
2. Seleccione **Add users** (Agregar usuarios) en la barra de herramientas. 

    ![Botón Agregar usuarios](../media/how-to-configure-student-usage/add-users-button.png)
1. En la página **Add users** (Agregar usuarios), escriba las direcciones de correo electrónico de los usuarios en líneas independientes, o bien en una sola línea separadas por punto y coma. 

    ![Incorporación de direcciones de correo electrónico de usuarios](../media/how-to-configure-student-usage/add-users-email-addresses.png)
4. Seleccione **Guardar**. Verá las direcciones de correo electrónico de los usuarios y sus estados (registrados o no) en la lista. 

    ![Lista de usuarios](../media/how-to-configure-student-usage/users-list-new.png)

## <a name="share-registration-link-with-students"></a>Compartir el vínculo de registro con los alumnos
Para enviar el vínculo de registro a los alumnos, use uno de los métodos siguientes. El primer método muestra cómo enviar correos electrónicos a los alumnos con el vínculo de registro y un mensaje opcional. El segundo método muestra cómo obtener el vínculo de registro que puede compartir con otros usuarios como desee. 

Si el **acceso restringido** se habilita para el laboratorio, solo los usuarios de la lista de usuarios pueden usar el vínculo de registro a fin de registrarse en el laboratorio. Esta opción está habilitada de manera predeterminada. 

### <a name="send-email-to-users"></a>Envío de un correo electrónico a los usuarios
Azure Lab Services permite a los profesores enviar por correo electrónico invitaciones de laboratorio a todos los alumnos, o solo a algunos de ellos, sin tener que usar otro cliente de correo electrónico. Los profesores pueden mantener el puntero sobre cada alumno de la lista para ver el icono de correo electrónico o seleccionar uno o varios alumnos y usar **Send invitation** (Enviar invitación) en la barra de herramientas. Esta característica envía un correo electrónico con un vínculo de registro y agrega un mensaje (si lo hay) del profesor. Después de que se envía la invitación, el estado de la invitación cambia a **invitación enviada** para que los profesores puedan realizar un seguimiento de los alumnos que ya han recibido el vínculo de registro y la fecha en que se envió.

1. Cambie a la vista **Users** (Usuarios) si aún no está en la página. 
2. Seleccione usuarios específicos o todos ellos en la lista. Para seleccionar usuarios específicos, active las casillas de la primera columna de la lista. Para seleccionar todos los usuarios, seleccione la casilla situada delante del título de la primera columna (**Nombre**) o seleccione todas las casillas d todos los usuarios de la lista. Puede ver el **estado de la invitación** en esta lista.  En la siguiente imagen, se establece el estado de la invitación para todos los alumnos en **Invitación no enviada**. 

    ![Selección de alumnos](../media/tutorial-setup-classroom-lab/select-students.png)
1. Seleccione el **icono de correo electrónico (sobre)** en una de las filas o seleccione **Enviar invitación** en la barra de herramientas. También puede mantener el ratón sobre un nombre de alumno en la lista para ver el icono de correo electrónico. 

    ![Envío del vínculo de registro por correo electrónico](../media/tutorial-setup-classroom-lab/send-email.png)
4. En la página **Send registration link by email** (Enviar vínculo de registro por correo electrónico), siga estos pasos: 
    1. Escriba un **mensaje opcional** que desee enviar a los alumnos. El correo electrónico incluye automáticamente el vínculo de registro. 
    2. En la página **Send registration link by email** (Enviar vínculo de registro por correo electrónico), seleccione **Enviar**. Puede ver cómo cambia el estado de la invitación a **Enviando invitación** y, a continuación, a **Invitación enviada**. 
        
        ![Invitaciones enviadas](../media/tutorial-setup-classroom-lab/invitations-sent.png)

## <a name="get-registration-link"></a>Obtención del vínculo de registro
1. Cambie a la vista **Usuarios** seleccionando **Usuarios** en el menú izquierdo. 
2. Seleccione el icono **Get registration link** (Obtener vínculo de registro).

    ![Vínculo de registro del estudiante](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. En el cuadro de diálogo **Registro de usuario**, seleccione el botón **Copiar**. El vínculo se copia en el Portapapeles. Péguelo en un editor de correo electrónico y envíe un correo electrónico al estudiante. 

    ![Vínculo de registro del estudiante](../media/tutorial-setup-classroom-lab/registration-link.png)
2. En el cuadro de diálogo **Registro de usuario**, seleccione **Cerrar**. 
4. Comparta el **vínculo de registro** con un alumno para que este se pueda registrar en la clase. 

## <a name="view-users-registered-with-the-lab"></a>Ver los usuarios registrados en el laboratorio

Seleccione **Usuarios** en el menú a la izquierda para ver la lista de usuarios registrados en el laboratorio. 

![Lista de usuarios registrados en el laboratorio](../media/how-to-configure-student-usage/users-list-new.png)

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



> [!IMPORTANT]
> Antes de enviar el vínculo de registro a los alumnos, los profesores deben establecer la programación de la clase si eligen una cuota de 0 horas o especificar la cuota de horas del laboratorio.
>
> El [tiempo de ejecución programado de las máquinas virtuales](how-to-create-schedules.md) no cuentan en la cuota asignada a un usuario. La cuota es el tiempo fuera del horario programado que un alumno pasa en las máquinas virtuales. 

### <a name="add-users-by-uploading-a-csv-file"></a>Adición de usuarios mediante la carga de un archivo CSV
También puede agregar usuarios cargando un archivo CSV con direcciones de correo electrónico de los usuarios.

1. Cree un archivo CSV con direcciones de correo electrónico de los usuarios en una sola columna.

    ![Quota per user (Cuota por usuario)](../media/how-to-configure-student-usage/csv-file-with-users.png)
2. En la página **Usuarios** del laboratorio, seleccione **Cargar CSV** en la barra de herramientas.

    ![Botón Cargar CSV](../media/how-to-configure-student-usage/upload-csv-button.png)
3. Seleccione el archivo CSV con las direcciones de correo electrónico de los usuarios. Al seleccionar **Abrir** después de seleccionar el archivo CSV, verá la siguiente ventana **Agregar usuarios**. La lista de direcciones de correo electrónico se rellena con las direcciones de correo electrónico del archivo CSV. 

    ![Ventana Agregar usuarios rellenada con direcciones de correo electrónico del archivo CSV](../media/how-to-configure-student-usage/add-users-window.png)
4. Seleccione **Guardar** en la ventana **Agregar usuarios**. 
5. Confirme que ve los usuarios de la lista de usuarios. 

    ![Lista de usuarios agregados](../media/how-to-configure-student-usage/list-of-added-users.png)

## <a name="manage-user-vms"></a>Administración de las máquinas virtuales de usuario
Una vez que los estudiantes se registren en Azure Lab Services mediante el vínculo de registro que les proporcionó, verá las máquinas virtuales asignadas a los estudiantes en la pestaña **Máquinas virtuales**. 

![Máquinas virtuales asignadas a estudiantes](../media/how-to-manage-classroom-labs/virtual-machines-students.png)

Puede realizar las siguientes tareas en la máquina virtual de un estudiante: 

- Detener una máquina virtual si se está ejecutando. 
- Iniciar una máquina virtual si está detenida. 
- Conéctese a la máquina virtual. 
- Elimine la máquina virtual. 
- Vea el número de horas que los usuarios usan la máquina virtual. 

## <a name="update-number-of-virtual-machines-in-lab"></a>Actualización del número de máquinas virtuales en el laboratorio
Para actualizar el número de máquinas virtuales en el laboratorio, siga estos pasos en la página **Virtual Machines**:

1. En el menú de la izquierda, seleccione **Máquinas virtuales**. 
2. Seleccione **Capacidad de laboratorio: &lt;número&gt; máquinas** en la barra de herramientas. 
3. Escriba el **número** de máquinas virtuales.
4. Seleccione **Guardar**.

    ![Máquinas virtuales en el laboratorio](../media/how-to-configure-student-usage/number-virtual-machines.png)


## <a name="next-steps"></a>Pasos siguientes
Consulte los artículos siguientes:

- [Como administrador, crear y administrar cuentas de laboratorio](how-to-manage-lab-accounts.md)
- [Como propietario del laboratorio, crear y administrar laboratorios](how-to-manage-classroom-labs.md)
- [Como propietario del laboratorio, configurar y publicar plantillas](how-to-create-manage-template.md)
- [Como usuario del laboratorio, obtener acceso a laboratorios educativos](how-to-use-classroom-lab.md)
