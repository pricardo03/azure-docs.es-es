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
ms.date: 10/12/2019
ms.author: spelluru
ms.openlocfilehash: 691907d1c221283f99ba59f0937cfbaf673f427a
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72324533"
---
# <a name="add-and-manage-lab-users"></a>Incorporación y administración de usuarios de laboratorio
En este artículo se describe cómo agregar usuarios al laboratorio, registrarlos en el laboratorio, controlar el número de horas que pueden usar la máquina virtual y mucho más. 


## <a name="add-users-to-the-lab"></a>Incorporación de usuarios al laboratorio

1. En el menú izquierdo, seleccione **Users** (Usuarios). De forma predeterminada, está habilitada la opción **Restrict access** (Restringir acceso). Cuando esta opción está activada, los usuarios no se pueden registrar en el laboratorio aunque tengan el vínculo de registro si no aparecen en la lista de usuarios. Solo los usuarios de la lista pueden registrarse en el laboratorio mediante el vínculo de registro enviado. En este procedimiento, agregará usuarios a la lista. Como alternativa, puede desactivar **Restrict access** (Restringir acceso), que permite que los usuarios se registren en el laboratorio, siempre y cuando tengan el vínculo de registro. 
2. Seleccione **Add users** (Agregar usuarios) en la barra de herramientas y, luego, elija **Add by email addresses** (Agregar por dirección de correo electrónico). 

    ![Botón Agregar usuarios](../media/how-to-configure-student-usage/add-users-button.png)
1. En la página **Add users** (Agregar usuarios), escriba las direcciones de correo electrónico de los usuarios en líneas independientes, o bien en una sola línea separadas por punto y coma. 

    ![Incorporación de direcciones de correo electrónico de usuarios](../media/how-to-configure-student-usage/add-users-email-addresses.png)
4. Seleccione **Guardar**. Verá las direcciones de correo electrónico de los usuarios y sus estados (registrados o no) en la lista. 

    ![Lista de usuarios](../media/how-to-configure-student-usage/users-list-new.png)

### <a name="add-users-by-uploading-a-csv-file"></a>Adición de usuarios mediante la carga de un archivo CSV
También puede agregar usuarios cargando un archivo CSV con direcciones de correo electrónico de los usuarios.

1. Cree un archivo CSV con direcciones de correo electrónico de los usuarios en una sola columna.

    ![Archivo CSV con usuarios](../media/how-to-configure-student-usage/csv-file-with-users.png)
2. En la página **Users** (Usuarios) del laboratorio, seleccione **Add users** (Agregar usuarios) en la barra de herramientas y, luego, elija **Upload CSV** (Cargar CSV).

    ![Botón Cargar CSV](../media/how-to-configure-student-usage/upload-csv-button.png)
3. Seleccione el archivo CSV con las direcciones de correo electrónico de los usuarios. Al seleccionar **Abrir** después de seleccionar el archivo CSV, verá la siguiente ventana **Agregar usuarios**. La lista de direcciones de correo electrónico se rellena con las direcciones de correo electrónico del archivo CSV. 

    ![Ventana Agregar usuarios rellenada con direcciones de correo electrónico del archivo CSV](../media/how-to-configure-student-usage/add-users-window.png)
4. Seleccione **Guardar** en la ventana **Agregar usuarios**. 
5. Confirme que ve los usuarios de la lista de usuarios. 

    ![Lista de usuarios agregados](../media/how-to-configure-student-usage/list-of-added-users.png)

## <a name="send-invitations-to-users"></a>Envío de invitaciones a los usuarios
Para enviar el vínculo de registro a los alumnos, use uno de los métodos siguientes. El primer método muestra cómo enviar correos electrónicos a los alumnos con el vínculo de registro y un mensaje opcional. El segundo método muestra cómo obtener el vínculo de registro que puede compartir con otros usuarios como desee. 

Si el **acceso restringido** se habilita para el laboratorio, solo los usuarios de la lista de usuarios pueden usar el vínculo de registro a fin de registrarse en el laboratorio. Esta opción está habilitada de manera predeterminada. 

### <a name="invite-all-users"></a>Invitar a todos los usuarios

1. Cambie a la página **Users** (Usuarios) del laboratorio. 
2. Seleccione **Invite all** (Invitar a todos) en la barra de herramientas. 
3. Escriba un **mensaje** a los usuarios. Se trata de un paso opcional.
4. Luego, seleccione **Send** (Enviar).

    ![Invitar a todos los usuarios](../media/how-to-configure-student-usage/invite-all.png)

    El estado de esta invitación se verá en la columna **Invitation** (Invitación) de la lista **Users** (Usuarios). El correo electrónico de invitación incluirá el vínculo de registro que los usuarios pueden usar para registrarse en el laboratorio. 

### <a name="invite-selected-users"></a>Invitar a usuarios seleccionados

1. Seleccione uno o varios usuarios de la lista. 
2. A continuación, seleccione el icono de **sobre** que se muestra en la fila seleccionada o seleccione **Invite** (Invitar) en la barra de herramientas. 

    ![Invitar a usuarios seleccionados](../media/how-to-configure-student-usage/invite-selected-users.png)
3. En la página **Send invitation by email** (Enviar invitación por correo electrónico), escriba un **mensaje** opcional y seleccione **Send** (Enviar). 

    ![Enviar un correo electrónico a los usuarios seleccionados](../media/how-to-configure-student-usage/send-invitation-to-selected-users.png)

    El estado de esta invitación se verá en la columna **Invitation** (Invitación) de la lista **Users** (Usuarios). El correo electrónico de invitación incluirá el vínculo de registro que los usuarios pueden usar para registrarse en el laboratorio.

1. Cambie a la vista **Users** (Usuarios) si aún no está en la página. 

## <a name="get-registration-link"></a>Obtención del vínculo de registro
También puede obtener el vínculo de registro del portal y enviarlo mediante su propia aplicación cliente de correo electrónico. 

1. Cambie a la vista **Usuarios** seleccionando **Usuarios** en el menú izquierdo. 
2. Seleccione los puntos suspensivos **(...)** en la barra de herramientas y, luego, elija **Registration link** (Vínculo de registro).

    ![Vínculo de registro del estudiante](../media/how-to-configure-student-usage/registration-link-button.png)
1. En el cuadro de diálogo **User registration** (Registro de usuarios), seleccione el botón **Copy** (Copiar). El vínculo se copia en el Portapapeles. Péguelo en un editor de correo electrónico y envíe un correo electrónico al estudiante. 

    ![Vínculo de registro del estudiante](../media/how-to-configure-student-usage/registration-link.png)
2. En el cuadro de diálogo **User registration** (Registro de usuarios), seleccione **Done** (Listo). 
4. Comparta el **vínculo de registro** con un alumno para que este se pueda registrar en la clase. 

## <a name="view-users-registered-with-the-lab"></a>Ver los usuarios registrados en el laboratorio

Seleccione **Usuarios** en el menú a la izquierda para ver la lista de usuarios registrados en el laboratorio. 

![Lista de usuarios registrados en el laboratorio](../media/how-to-configure-student-usage/students-registered.png)

## <a name="set-quotas-for-users"></a>Establecimiento de cuotas para los usuarios
Puede establecer cuotas por usuario mediante los pasos siguientes: 

1. Seleccione **Usuarios** en el menú de la izquierda si la página todavía no está activa. 
2. Seleccione **Quota per user (Cuota por usuario): &lt;número&gt; hours** (horas). 
3. En la página **Quota per user** (Cuota por usuario), especifique el número de horas que quiere dar a cada usuario (alumno) fuera del tiempo de clase programado y, luego, seleccione **Save** (Guardar).

    ![Quota per user (Cuota por usuario)](../media/how-to-configure-student-usage/quota-per-user.png)    
5. Consulte los valores modificados en la barra de herramientas: **Cuota por usuario: &lt;número de horas&gt;** . 

    ![Quota per user (Cuota por usuario): después](../media/how-to-configure-student-usage/quot-per-user-after.png)

    > [!IMPORTANT]
    > El [tiempo de ejecución programado de las máquinas virtuales](how-to-create-schedules.md) no cuentan en la cuota asignada a un usuario. La cuota es el tiempo fuera del horario programado que un alumno pasa en las máquinas virtuales. 

## <a name="set-additional-quota-for-a-specific-user"></a>Establecer una cuota adicional para un usuario concreto
Puede establecer una cuota aparte para un usuario. Para hacerlo, siga estos pasos:

1. Seleccione un usuario (alumno) de la lista de usuarios en la página **Users** (Usuarios).
2. A continuación, seleccione **Adjust** (Ajustar) en la barra de herramientas. 

    ![Botón de ajuste de cuota](../media/how-to-configure-student-usage/adjust-quota-button.png)
3. Escriba el número de **horas adicionales** para el usuario o usuarios seleccionados y, después, seleccione **Apply** (Aplicar). 

    ![Cuota adicional para un usuario](../media/how-to-configure-student-usage/additional-quota.png)
4. Verá el uso actualizado del usuario en la columna **Usage** (Uso). 

    ![Nuevo uso del usuario](../media/how-to-configure-student-usage/new-usage-hours.png)


## <a name="next-steps"></a>Pasos siguientes
Consulte los artículos siguientes:

- [Como administrador, crear y administrar cuentas de laboratorio](how-to-manage-lab-accounts.md)
- [Como propietario del laboratorio, crear y administrar laboratorios](how-to-manage-classroom-labs.md)
- [Como propietario del laboratorio, configurar y publicar plantillas](how-to-create-manage-template.md)
- [Como usuario del laboratorio, obtener acceso a laboratorios educativos](how-to-use-classroom-lab.md)
