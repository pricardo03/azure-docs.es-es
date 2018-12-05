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
ms.date: 11/14/2018
ms.author: spelluru
ms.openlocfilehash: 30c033b487fe58d017080b02c257502f82338164
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51710047"
---
# <a name="configure-usage-settings-and-policies"></a>Configuración de los valores y directivas de uso
En este artículo se describe cómo configurar el número de usuarios del laboratorio, registrar a esos usuarios en el laboratorio, controlar el número de horas que pueden usar la máquina virtual y mucho más. 


## <a name="specify-the-number-of-users-allowed-into-the-lab"></a>Especificación del máximo número de usuarios permitidos en el laboratorio

1. Seleccione **Política de uso**. 
2. En **Política de uso**, Configuración, escriba el **número de usuarios** con permiso para usar el laboratorio.
3. Seleccione **Guardar**. 

    ![Política de uso](../media/how-to-manage-classroom-labs/usage-policy-settings.png)

## <a name="send-registration-link-to-students"></a>Envío del vínculo de registro a los estudiantes

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

![Lista de usuarios registrados en el laboratorio](../media/how-to-configure-student-usage/users-list.png)

## <a name="set-quotas-per-user"></a>Establecimiento de cuotas por usuario

1. En el menú izquierdo, seleccione **Users** (Usuarios).
2. Seleccione **Quota per user: unlimited** (Cuota por usuario: ilimitada) en la barra de herramientas. 
3. En la página **Quota per user** (Cuota por usuario), seleccione **Limit the number of hours a user can use a VM** (Limitar el número de horas que un usuario puede usar la VM). 
4. Para **How many hours do you want to give to each user?** (¿Cuántas horas quiere dar a cada usuario?), escriba el número de horas y seleccione **Guardar**. 

    ![Número de horas por usuario](../media/how-to-configure-student-usage/number-of-hours-per-user.png)
5. Ahora verá el número de horas en la barra de herramientas: **Quota per user:&lt; (Cuota por usuario:) número de horas&gt;**. 

    ![Quota per user (Cuota por usuario)](../media/how-to-configure-student-usage/quota-per-user.png)

## <a name="add-users-to-the-lab"></a>Incorporación de usuarios al laboratorio
Si tiene la opción **Restringir acceso** habilitada, agregue usuarios (direcciones de correo electrónico) a la lista.

1. En el menú izquierdo, seleccione **Users** (Usuarios).
2. Seleccione **Add users** (Agregar usuarios) en la barra de herramientas. 
3. En la página **Add users** (Agregar usuarios), escriba las direcciones de correo electrónico de los usuarios en líneas independientes, o bien en una sola línea separadas por punto y coma. 

    ![Incorporación de direcciones de correo electrónico de usuarios](../media/how-to-configure-student-usage/add-users-email-addresses.png)
4. Seleccione **Guardar**. Verá las direcciones de correo electrónico de los usuarios y sus estados (registrados o no) en la lista. 

    ![Lista de usuarios](../media/how-to-configure-student-usage/users-list-new.png)

### <a name="add-users-by-uploading-a-csv-file"></a>Adición de usuarios mediante la carga de un archivo CSV
También puede agregar usuarios cargando un archivo CSV con direcciones de correo electrónico de los usuarios.

1. Seleccione **Cargar CSV** en la barra de herramientas.
2. Seleccione el archivo CSV con las direcciones de correo electrónico de los usuarios. Todas las direcciones de correo electrónico deben estar en una columna cuando el archivo se abra con Excel. 

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
Consulte los artículos siguientes:

- [Como administrador, crear y administrar cuentas de laboratorio](how-to-manage-lab-accounts.md)
- [Como propietario del laboratorio, crear y administrar laboratorios](how-to-manage-classroom-labs.md)
- [Como propietario del laboratorio, configurar y publicar plantillas](how-to-create-manage-template.md)
- [Como usuario del laboratorio, obtener acceso a laboratorios educativos](how-to-use-classroom-lab.md)
