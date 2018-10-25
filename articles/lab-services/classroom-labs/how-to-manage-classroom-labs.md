---
title: Administración de laboratorios de clase en Azure Lab Services | Microsoft Docs
description: Obtenga información sobre cómo crear y configurar un laboratorio de clase, ver todos los laboratorios de clase, compartir el vínculo de registro con un usuario del laboratorio o eliminar un laboratorio.
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
ms.openlocfilehash: eebdc98db5ecdf518d3b0b58e6757a2b7ecd5dd7
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2018
ms.locfileid: "48043811"
---
# <a name="manage-classroom-labs-in-azure-lab-services"></a>Administración de laboratorios de clase en Azure Lab Services 
En este artículo se describe cómo crear y configurar un laboratorio de clase, ver todos los laboratorios de clase o eliminar un laboratorio de clase.

## <a name="prerequisites"></a>Requisitos previos
Para configurar un laboratorio de clase en una cuenta de laboratorio, debe ser miembro del rol **Creador de laboratorio** en la cuenta de laboratorio. La cuenta que usó para crear una cuenta de laboratorio se agrega automáticamente a este rol. Un propietario de laboratorio puede agregar otros usuarios al rol de Creador de laboratorio mediante los pasos descritos en el siguiente artículo: [Incorporación de un usuario al rol de Creador de laboratorio](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).

## <a name="create-a-classroom-lab"></a>Creación de un laboratorio educativo

1. Vaya al [sitio web de Azure Lab Services](https://labs.azure.com). 
2. Seleccione **Iniciar sesión** y escriba las credenciales. Azure Lab Services es compatible con cuentas profesionales y cuentas Microsoft. 
3. En la ventana **Nuevo laboratorio**, lleve a cabo las siguientes acciones: 
    1. Especifique un **nombre** para el laboratorio. 
    2. Especifique el máximo **número de usuarios** permitidos en el laboratorio. 
    6. Seleccione **Guardar**.

        ![Creación de un laboratorio educativo](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. En la página **Seleccionar especificaciones de máquina virtual**, realice los pasos siguientes:
    1. Seleccione un **tamaño** para las máquinas virtuales (VM) creadas en el laboratorio. 
    2. Seleccione la **región** en la que quiere que se creen las máquinas virtuales. 
    3. Seleccione la **imagen de máquina virtual** que se usará para crear máquinas virtuales en el laboratorio. 
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
8. Los pasos siguientes son opcionales para este tutorial: 
    1. Seleccione **Iniciar** para iniciar la plantilla de máquina virtual.
    2. Seleccione **Conectar** para conectarse a la plantilla de máquina virtual. 
    3. Instale y configure el software en la plantilla de máquina virtual. 
    4. **Pare** la máquina virtual.  
    5. Escriba una **descripción** para la plantilla.

        ![Siguiente en la página Configurar plantilla](../media/tutorial-setup-classroom-lab/configure-template-next.png)
9. Seleccione **Siguiente** en la página de plantilla. 
10. En la página **Publicar la plantilla**, realice las acciones siguientes. 
    1. Para publicar la plantilla inmediatamente, seleccione la casilla *I understand I can't modify the template after publishing. This process can only be done once and can take up to an hour* (Comprendo que no puedo modificar la plantilla después de la publicación. Este proceso solo se puede realizar una vez y puede tardar hasta una hora), y seleccione **Publicar**.  

        > [!WARNING]
        > Una vez que publique, no se puede cancelar la publicación. 
    2. Para publicar más adelante, seleccione **Guardar para más adelante**. Puede publicar la plantilla de máquina virtual una vez finalizado el asistente. Para más información sobre cómo configurar y publicar una vez finalizado el asistente, consulte la sección [Publicación de la plantilla](#publish-the-template) del artículo [Administración de laboratorios educativos](how-to-manage-classroom-labs.md).

        ![Publicar plantilla](../media/tutorial-setup-classroom-lab/publish-template.png)
11. Puede ver el **progreso de la publicación** de la plantilla. Este proceso puede tardar hasta una hora. 

    ![Publicar plantilla: progreso](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. Podrá ver la página siguiente cuando la plantilla se haya publicado correctamente. Seleccione **Listo**.

    ![Publicar plantilla: correctamente](../media/tutorial-setup-classroom-lab/publish-success.png)
1. Verá el **panel** del laboratorio. 
    
    ![Panel del laboratorio de clase](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)
4. Cambie a la página **Máquinas virtuales** y confirme que ve las máquinas virtuales que tienen el estado **Sin asignar**. Estas máquinas virtuales no están asignadas a los alumnos todavía. Deberían estar en estado **Detenida**. En esta pagina, puede iniciar una máquina virtual de un alumno, conectarse a la máquina virtual, detener la máquina virtual y eliminar la máquina virtual. Puede iniciarlas en esta página o dejar que los alumnos inicien las máquinas virtuales. 

    ![Máquinas virtuales en estado detenido](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)

## <a name="configure-usage-policy"></a>Configuración de la política de uso

1. Seleccione **Política de uso**. 
2. En **Política de uso**, Configuración, escriba el **número de usuarios** con permiso para usar el laboratorio.
3. Seleccione **Guardar**. 

    ![Política de uso](../media/how-to-manage-classroom-labs/usage-policy-settings.png)

## <a name="set-up-the-template"></a>Configuración de la plantilla
En un laboratorio, una plantilla es una imagen de máquina virtual base a partir de la que se crean las máquinas virtuales de todos los usuarios. Configure la máquina virtual de la plantilla de modo que esté configurada exactamente con lo que desea proporcionar a los usuarios del laboratorio. Puede proporcionar un nombre y una descripción de la plantilla que verán los usuarios del laboratorio. Publique la plantilla si desea que las instancias de la máquina virtual de la plantilla estén disponibles para los usuarios del laboratorio.  

### <a name="set-template-title-and-description"></a>Establecimiento del título y descripción de la plantilla
1. En la sección **Plantilla**, seleccione **Editar** (icono de lápiz) para la plantilla. 
2. En la ventana **Vista de usuario**, escriba un **título** para la plantilla.
3. Escriba la **descripción** para la plantilla.
4. Seleccione **Guardar**.

    ![Descripción del laboratorio de clase](../media/how-to-manage-classroom-labs/lab-description.png)

### <a name="set-up-the-template-vm"></a>Configuración de la plantilla de máquina virtual
 Puede conectarse a la plantilla de máquina virtual e instalar todo el software necesario en la misma antes de hacerla disponible para los alumnos. 

1. Espere hasta que la plantilla de máquina virtual esté lista. Una vez que esté lista, el botón **Iniciar** debe estar habilitado. Para iniciar la máquina virtual, seleccione **Iniciar**.

    ![Inicio de la plantilla de máquina virtual](../media/tutorial-setup-classroom-lab/start-template-vm.png)
1. Para conectarse a la máquina virtual, seleccione **Conectar** y siga las instrucciones. 

    ![Conexión a la plantilla de máquina virtual](../media/tutorial-setup-classroom-lab/connect-template-vm.png)
1. Instale el software necesario para que los estudiantes puedan realizar el laboratorio (por ejemplo, Visual Studio, Explorador de Azure Storage, etc). 
2. Desconéctese (cierre la sesión de escritorio remoto) de la plantilla de máquina virtual. 
3. **Detenga** la plantilla de máquina virtual seleccionando **Detener**. 

    ![Detención de la plantilla de máquina virtual](../media/tutorial-setup-classroom-lab/stop-template-vm.png)


## <a name="publish-the-template"></a>Publicación de la plantilla 
Cuando publica una plantilla, Azure Lab Services crea las máquinas virtuales en el laboratorio mediante la plantilla. El número de máquinas virtuales creadas en este proceso es igual al número máximo de usuarios permitidos en el laboratorio, que se puede establecer en la política de uso del laboratorio. Todas las máquinas virtuales tienen la misma configuración que la plantilla. 

1. Seleccione **Publicar** en la sección **Plantilla**. 

    ![Publicación de la plantilla de máquina virtual](../media/tutorial-setup-classroom-lab/public-access.png)
1. En la ventana **Publicar**, seleccione la opción **Publicada**. 
2. A continuación, seleccione el botón **Publicar**. Este proceso puede tardar algún tiempo en función del número de máquinas virtuales que se crean, que es el mismo que el número de usuarios permitidos en el laboratorio.
    
    > [!IMPORTANT]
    > Las plantillas publicadas no pueden dejar de publicarse. 
4. Cambie a la página **Máquinas virtuales** y confirme que ve las máquinas virtuales que tienen el estado **Sin asignar**. Estas máquinas virtuales no están asignadas a los alumnos todavía. 

    ![Máquinas virtuales](../media/tutorial-setup-classroom-lab/virtual-machines.png)
5. Espere hasta que se creen las máquinas virtuales. Deberían estar en estado **Detenida**. En esta pagina, puede iniciar una máquina virtual de un alumno, conectarse a la máquina virtual, detener la máquina virtual y eliminar la máquina virtual. Puede iniciarlas en esta página o dejar que los alumnos inicien las máquinas virtuales. 

    ![Máquinas virtuales en estado detenido](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)


## <a name="send-registration-link-to-students"></a>Envío del vínculo de registro a los estudiantes

1. Cambie a la vista **Panel**. 
2. Seleccione el icono **Registro de usuario**.

    ![Vínculo de registro del estudiante](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. En el cuadro de diálogo **Registro de usuario**, seleccione el botón **Copiar**. El vínculo se copia en el Portapapeles. Péguelo en un editor de correo electrónico y envíe un correo electrónico al estudiante. 

    ![Vínculo de registro del estudiante](../media/tutorial-setup-classroom-lab/registration-link.png)
2. En el cuadro de diálogo **Registro de usuario**, seleccione **Cerrar**. 

## <a name="view-all-classroom-labs"></a>Visualización de todos los laboratorios de clase
1. Vaya al [portal de Azure Lab Services](https://labs.azure.com).
2. Confirme que ve todos los laboratorios en la cuenta de laboratorio seleccionada. 

    ![Todos los laboratorios](../media/how-to-manage-classroom-labs/all-labs.png)
3. Use la lista desplegable de la parte superior para seleccionar una cuenta de laboratorio distinta. Verá los laboratorios en la cuenta de laboratorio seleccionada. 

## <a name="delete-a-classroom-lab"></a>Eliminación de un laboratorio de clase
1. En el icono del laboratorio, seleccione los tres puntos (...) en la esquina. 

    ![Selección del laboratorio](../media/how-to-manage-classroom-labs/select-three-dots.png)
2. Seleccione **Eliminar**. 

    ![Botón Eliminar](../media/how-to-manage-classroom-labs/delete-button.png)
3. En el cuadro de diálogo **Delete lab** (Eliminar laboratorio), seleccione **Delete** (Eliminar). 

    ![Cuadro de diálogo Eliminar](../media/how-to-manage-classroom-labs/delete-lab-dialog-box.png)
 
## <a name="manage-student-vms"></a>Administración de máquinas virtuales de estudiantes
Una vez que los estudiantes se registren en Azure Lab Services mediante el vínculo de registro que les proporcionó, verá las máquinas virtuales asignadas a los estudiantes en la pestaña **Máquinas virtuales**. 

![Máquinas virtuales asignadas a estudiantes](../media/how-to-manage-classroom-labs/virtual-machines-students.png)

Puede realizar las siguientes tareas en la máquina virtual de un estudiante: 

- Detener una máquina virtual si se está ejecutando. 
- Iniciar una máquina virtual si está detenida. 
- Conéctese a la máquina virtual. 
- Elimine la máquina virtual. 

## <a name="next-steps"></a>Pasos siguientes
Introducción a la configuración de un laboratorio con Azure Lab Services:

- [Configuración de un laboratorio educativo](how-to-manage-classroom-labs.md)
- [Configuración de un laboratorio](../tutorial-create-custom-lab.md)
