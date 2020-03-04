---
title: Configuración de los valores de uso de los laboratorios educativos de Azure Lab Services
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
ms.date: 02/20/2020
ms.author: spelluru
ms.openlocfilehash: 9fda1295bcdcd44b8a92c101c2aa445197ce6d4a
ms.sourcegitcommit: 163be411e7cd9c79da3a3b38ac3e0af48d551182
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/21/2020
ms.locfileid: "77539238"
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

    ![Lista de usuarios](../media/how-to-configure-student-usage/list-of-added-users.png)

    > [!NOTE]
    > Una vez registrados los nombres de los usuarios en el laboratorio, aparecen en la lista. El nombre que se muestra en la lista se construye con el nombre y los apellidos del usuario en Azure Active Directory. 

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
1. Cambie a la vista **Users** (Usuarios) si aún no está en la página y seleccione **Invite all** (Invitar a todos) en la barra de herramientas. 

    ![Selección de alumnos](../media/tutorial-setup-classroom-lab/invite-all-button.png)

1. En la página **Send invitation by email** (Enviar invitación por correo electrónico), escriba un mensaje opcional y seleccione **Send** (Enviar). El correo electrónico incluye automáticamente el vínculo de registro. Puede obtener este vínculo de registro al seleccionar **... (puntos suspensivos)** en la barra de herramientas y **Registration link** (Vínculo de registro). 

    ![Envío del vínculo de registro por correo electrónico](../media/tutorial-setup-classroom-lab/send-email.png)
4. Verá el estado de la **invitación** en la lista **Users** (Usuarios). El estado debería cambiar a **Sending** (Enviando) y luego a **Sent on \<date>** (Enviado el <fecha>). 

    Para más información sobre la incorporación de alumnos a una clase y la administración del uso que hacen del laboratorio, consulte [Configuración de los valores de uso de los alumnos](how-to-configure-student-usage.md).

### <a name="invite-selected-users"></a>Invitar a usuarios seleccionados

1. Seleccione uno o varios usuarios de la lista. 
2. A continuación, seleccione el icono de **sobre** que se muestra en la fila seleccionada o seleccione **Invite** (Invitar) en la barra de herramientas. 

    ![Invitar a usuarios seleccionados](../media/how-to-configure-student-usage/invite-selected-users.png)
3. En la página **Send invitation by email** (Enviar invitación por correo electrónico), escriba un **mensaje** opcional y seleccione **Send** (Enviar). 

    ![Enviar un correo electrónico a los usuarios seleccionados](../media/how-to-configure-student-usage/send-invitation-to-selected-users.png)

    El estado de esta operación se ve en la columna **Invitation** (Invitación) de la lista **Users** (Usuarios). El correo electrónico de invitación incluye el vínculo de registro que los usuarios pueden usar para registrarse en el laboratorio.

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

## <a name="view-registered-users"></a>Visualización de usuarios registrados

1. Vaya al [sitio web de Azure Lab Services](https://labs.azure.com). 
2. Seleccione **Iniciar sesión** y escriba las credenciales. Azure Lab Services es compatible con cuentas profesionales y cuentas Microsoft.
3. En la página **My labs** (Mis laboratorios), seleccione el laboratorio de cuyo uso desea realizar un seguimiento. 
4. Seleccione **Users** (Usuarios) en el menú izquierdo o el icono **Usuarios**. Verá los alumnos que se han registrado en el laboratorio.  

    ![Usuarios registrados](../media/tutorial-track-usage/registered-users.png)

## <a name="set-quotas-for-users"></a>Establecimiento de cuotas para los usuarios
Puede establecer cuotas por usuario mediante los pasos siguientes: 

1. Seleccione **Usuarios** en el menú de la izquierda si la página todavía no está activa. 
2. Seleccione **Quota per user (Cuota por usuario): \<número> hours** (horas) en la barra de herramientas. 
3. En la página **Quota per user** (Cuota por usuario), especifique el número de horas que quiere dar a cada usuario (alumno) fuera del tiempo de clase programado y, luego, seleccione **Save** (Guardar).

    ![Quota per user (Cuota por usuario)](../media/how-to-configure-student-usage/quota-per-user.png)    
5. Consulte los valores modificados en la barra de herramientas: **Cuota por usuario: \<número de horas>** y además en la lista de usuarios.

    ![Cuota por usuario: después](../media/how-to-configure-student-usage/quot-per-user-after.png)

    > [!IMPORTANT]
    > El [tiempo de ejecución programado de las máquinas virtuales](how-to-create-schedules.md) no cuentan en la cuota asignada a un usuario. La cuota es el tiempo fuera del horario programado que un alumno pasa en las máquinas virtuales. 

## <a name="set-additional-quota-for-a-specific-user"></a>Establecer una cuota adicional para un usuario concreto
Puede especificar una cuota adicional para un usuario. Esta cuota se suma al conjunto de cuota común para todos los usuarios de la sección anterior. Por ejemplo, si usted (como instructor) establece la cuota de todos los usuarios en 10 horas y establece una cuota adicional de 5 horas para usuarios específicos, esos usuarios obtienen 15 (10 + 5) horas de cuota. Si cambia la cuota común más adelante a, por ejemplo, 15, el usuario obtiene 20 (15 + 5) horas de cuota. Recuerde que esta cuota general está fuera del tiempo programado. El tiempo que los alumnos invierten en una máquina virtual de laboratorio durante el tiempo programado no se cuenta para esta cuota. 

Para hacerlo, siga estos pasos:

1. Seleccione un usuario (alumno) de la lista de usuarios en la página **Users** (Usuarios).
2. A continuación, seleccione **Adjust** (Ajustar) en la barra de herramientas. 

    ![Botón de ajuste de cuota](../media/how-to-configure-student-usage/adjust-quota-button.png)
3. Escriba el número de **horas adicionales** para el usuario o usuarios seleccionados y, después, seleccione **Apply** (Aplicar). 

    ![Cuota adicional para un usuario](../media/how-to-configure-student-usage/additional-quota.png)
4. Se ve el uso actualizado del usuario en la columna **Usage** (Uso). 

    ![Nuevo uso del usuario](../media/how-to-configure-student-usage/new-usage-hours.png)

## <a name="student-accounts"></a>Cuentas de alumno
Para agregar alumnos a un laboratorio de clase, use sus cuentas de correo electrónico. Se pueden usar los siguientes tipos de cuentas de correo electrónico:

- Una cuenta de correo electrónico de alumno proporcionada por la instancia de Azure Active Directory (AAD) de Office 365 de la Universidad. 
- Una cuenta de correo electrónico de Microsoft, como `@outlook.com`, `@hotmail.com`, `@msn.com` o `@live.com`.
- Una cuenta de correo electrónico que no sea de Microsoft, como las proporcionadas por Yahoo o Google. No obstante, estos tipos de cuentas se deben vincular a una cuenta de Microsoft.
- Una cuenta de GitHub. Esta cuenta se debe vincular a una cuenta de Microsoft.

### <a name="using-a-non-microsoft-email-account"></a>Uso de una cuenta de correo electrónico que no sea de Microsoft
Los alumnos pueden usar cuentas de correo electrónico que no sean de Microsoft para registrarse e iniciar sesión en un laboratorio de clase.  Pero el registro exige que los alumnos creen primero una cuenta de Microsoft que esté vinculada a su dirección de correo electrónico que no es de Microsoft.

Es posible que muchos alumnos ya tengan una cuenta de Microsoft vinculada a sus direcciones de correo electrónico que no son de Microsoft. Por ejemplo, los alumnos ya tienen una cuenta de Microsoft si han usado su dirección de correo electrónico con otros productos o servicios de Microsoft, como Office, Skype, OneDrive o Windows.  

Cuando un alumno hace clic en la dirección URL de registro para iniciar sesión en una clase, se le pide su dirección de correo electrónico y contraseña. Si el alumno intenta iniciar sesión con una cuenta que no es de Microsoft y que no tiene una cuenta de Microsoft vinculada, recibe el mensaje de error siguiente: 

![Mensaje de error](../media/how-to-configure-student-usage/cant-find-account.png)

Para suscribirse a una cuenta de Microsoft, los alumnos deben ir a [http://signup.live.com](http://signup.live.com).  

> [!IMPORTANT]
> Cuando los alumnos inician sesión en un laboratorio de clase, no se les ofrece la opción de crear una cuenta de Microsoft. Por tanto, se recomienda incluir este vínculo de suscripción en el correo electrónico de registro del laboratorio de clase que envíe a los alumnos que usen cuentas que no sean de Microsoft.

### <a name="using-a-github-account"></a>Uso de una cuenta de GitHub
Los alumnos también pueden usar una cuenta de GitHub existente para registrarse e iniciar sesión en un laboratorio de clase. Si el alumno ya tiene una cuenta de Microsoft vinculada a su cuenta de GitHub, puede iniciar sesión y proporcionar su contraseña como se muestra en la sección anterior. Si aún no ha vinculado su cuenta de GitHub a una cuenta de Microsoft, debe seleccionar **Opciones de inicio de sesión**:

![Vínculo de opciones de inicio de sesión](../media/how-to-configure-student-usage/signin-options.png)

En la página **Opciones de inicio de sesión**, seleccione **Iniciar sesión con GitHub**.

![Vínculo de inicio de sesión con GitHub](../media/how-to-configure-student-usage/signin-github.png)

Por último, se le pide que cree una cuenta de Microsoft que esté vinculada a su cuenta de GitHub. Sucede automáticamente cuando el alumno selecciona **Siguiente**.  Luego el alumno inicia sesión inmediatamente y se conecta al laboratorio de clase.

## <a name="next-steps"></a>Pasos siguientes
Vea los artículos siguientes:

- [Como administrador, crear y administrar cuentas de laboratorio](how-to-manage-lab-accounts.md)
- [Como propietario del laboratorio, crear y administrar laboratorios](how-to-manage-classroom-labs.md)
- [Como propietario del laboratorio, configurar y publicar plantillas](how-to-create-manage-template.md)
- [Como usuario del laboratorio, obtener acceso a laboratorios educativos](how-to-use-classroom-lab.md)
