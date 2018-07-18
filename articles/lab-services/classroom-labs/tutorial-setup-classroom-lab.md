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
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: 163763bf1203a045326c7163b5f6da9aa417d8cf
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2018
ms.locfileid: "37081863"
---
# <a name="tutorial-set-up-a-classroom-lab"></a>Tutorial: Configuración de un laboratorio de clase 
En este tutorial, se va a configurar un laboratorio de clase con las máquinas virtuales que van a utilizar los estudiantes de la clase.  

En este tutorial realizará lo siguiente:

> [!div class="checklist"]
> * Creación de un laboratorio educativo
> * Configuración del laboratorio de clase
> * Envío del vínculo de registro a los estudiantes

## <a name="prerequisites"></a>requisitos previos
Para configurar un laboratorio de clase en una cuenta de laboratorio, debe ser miembro del rol **Creador de laboratorio** en la cuenta de laboratorio. Un propietario de laboratorio puede agregar un usuario al rol de Creador de laboratorio mediante los pasos descritos en el siguiente artículo: [Incorporación de un usuario al rol de Creador de laboratorio](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).


## <a name="create-a-classroom-lab"></a>Creación de un laboratorio educativo

1. Vaya al [sitio web de Azure Lab Services](https://labs.azure.com).
2. Seleccione **Iniciar sesión** y escriba las credenciales. 
3. En la ventana **Nuevo laboratorio**, lleve a cabo las siguientes acciones: 
    1. Escriba un **nombre** para el laboratorio de clase. 
    2. Seleccione el **tamaño** de la máquina virtual que se va a usar en la clase.
    3. Seleccione la **imagen** utilizada para crear la máquina virtual.
    4. Especifique las **credenciales predeterminadas** para el registro en máquinas virtuales en el laboratorio. 
    7. Seleccione **Guardar**.

        ![Creación de un laboratorio educativo](../media/tutorial-setup-classroom-lab/new-lab-window.png)
1. Verá el **panel** del laboratorio. 
    
    ![Panel del laboratorio de clase](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)

## <a name="configure-usage-policy"></a>Configuración de la política de uso

1. Seleccione **Política de uso**. 
2. En **Política de uso**, Configuración, escriba el **número de usuarios** con permiso para usar el laboratorio.
3. Seleccione **Guardar**. 

    ![Política de uso](../media/tutorial-setup-classroom-lab/usage-policy-settings.png)


## <a name="set-up-the-template"></a>Configuración de la plantilla 
En un laboratorio, una plantilla es una imagen de máquina virtual base a partir de la que se crean las máquinas virtuales de todos los usuarios. Configure la máquina virtual de la plantilla de modo que esté configurada exactamente con lo que desea proporcionar a los usuarios del laboratorio. Puede proporcionar un nombre y una descripción de la plantilla que verán los usuarios del laboratorio. Publique la plantilla si desea que las instancias de la máquina virtual de la plantilla estén disponibles para los usuarios del laboratorio. 

### <a name="set-title-and-description"></a>Establecimiento del título y descripción
1. En la sección **Plantilla**, seleccione **Editar** (icono de lápiz) para la plantilla. 
2. En la ventana **Vista de usuario**, escriba un **título** para la plantilla.
3. Escriba la **descripción** para la plantilla.
4. Seleccione **Guardar**.

    ![Descripción del laboratorio de clase](../media/tutorial-setup-classroom-lab/lab-description.png)

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

### <a name="publish-the-template"></a>Publicación de la plantilla 
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


## <a name="next-steps"></a>Pasos siguientes
En este tutorial, ha creado un laboratorio de clase y ha configurado el laboratorio. Para saber cómo el estudiante puede acceder a una máquina virtual en el laboratorio con el enlace de registro, avance al siguiente tutorial:

> [!div class="nextstepaction"]
> [Conexión a una máquina virtual en el laboratorio de clase](tutorial-connect-virtual-machine-classroom-lab.md)

