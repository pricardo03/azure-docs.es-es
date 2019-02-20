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
ms.date: 02/07/2019
ms.author: spelluru
ms.openlocfilehash: 834674eb63af75088434db0f614b11c7a36e7adf
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2019
ms.locfileid: "55964821"
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

## <a name="send-registration-link-to-students"></a>Envío del vínculo de registro a los estudiantes
El procedimiento siguiente tiene pasos para enviar un vínculo de registro a los usuarios. Si el **acceso restringido** se habilita para el laboratorio, solo los usuarios de la lista de usuarios pueden usar el vínculo de registro a fin de registrarse en el laboratorio. 

1. Cambie a la vista **Usuarios** seleccionando **Usuarios** en el menú izquierdo. 
2. Seleccione el icono **Get registration link** (Obtener vínculo de registro).

    ![Vínculo de registro del estudiante](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. En el cuadro de diálogo **Registro de usuario**, seleccione el botón **Copiar**. El vínculo se copia en el Portapapeles. Péguelo en un editor de correo electrónico y envíe un correo electrónico al estudiante. 

    ![Vínculo de registro del estudiante](../media/tutorial-setup-classroom-lab/registration-link.png)
2. En el cuadro de diálogo **Registro de usuario**, seleccione **Cerrar**. 
4. Comparta el vínculo de registro con un alumno para que este se pueda registrar para la clase. Si tiene habilitada la opción **Restrict access** (Restringir acceso) y tiene una lista de usuarios en la lista, realice las siguientes acciones:
    1. Seleccione la **dirección de correo electrónico** del usuario de la lista. 
    2. Verá una ventana desde el programa de correo electrónico predeterminado que tiene relleno el campo **PARA**. 
    3. Pegue la **dirección URL de registro** que copió anteriormente. 
    4. Envíe el **correo electrónico**. 

## <a name="view-users-registered-with-the-lab"></a>Ver los usuarios registrados en el laboratorio

Seleccione **Usuarios** en el menú a la izquierda para ver la lista de usuarios registrados en el laboratorio. 

![Lista de usuarios registrados en el laboratorio](../media/how-to-configure-student-usage/users-list-new.png)

## <a name="set-quotas-per-user"></a>Establecimiento de cuotas por usuario
Puede establecer cuotas por usuario mediante los pasos siguientes: 

1. En el menú izquierdo, seleccione **Users** (Usuarios).
2. Seleccione **Quota per user: unlimited** (Cuota por usuario: ilimitada) en la barra de herramientas. 
3. En la página **Quota per user** (Cuota por usuario), seleccione una de las siguientes opciones: 
    1. **No**. Los usuarios pueden usar sus máquinas virtuales durante el tiempo programado o cuando el propietario del laboratorio active las máquinas virtuales para ellos.
    2. **Unlimited (default)** (Ilimitado [valor predeterminado]). Los usuarios pueden usar sus máquinas virtuales sin restricciones de tiempo.
    3. **Specify number of hours per user** (Especificar número de horas por usuario). Los usuarios pueden usar sus máquinas virtuales para el número establecido de horas (especificado a continuación), además del tiempo programado. Si selecciona esta opción, escriba el **número de horas** en el cuadro de texto. 

        ![Número de horas por usuario](../media/how-to-configure-student-usage/number-of-hours-per-user.png)
    4. Seleccione **Guardar**. 
5. Consulte los valores modificados en la barra de herramientas: **Cuota por usuario: &lt;número de horas&gt;**. 

    ![Quota per user (Cuota por usuario)](../media/how-to-configure-student-usage/quota-per-user.png)

> [!IMPORTANT]
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
