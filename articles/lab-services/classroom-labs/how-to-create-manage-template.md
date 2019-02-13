---
title: Administración de plantillas de laboratorios educativos en Azure Lab Services | Microsoft Docs
description: Aprenda a crear y administrar plantillas de laboratorio educativo en Azure Lab Services.
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
ms.date: 01/17/2019
ms.author: spelluru
ms.openlocfilehash: d0942d3465811687937ce113e664fbf3f91277d0
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55815258"
---
# <a name="create-and-manage-a-classroom-template-in-azure-lab-services"></a>Creación y administración de plantillas educativas en Azure Lab Services
En un laboratorio, una plantilla es una imagen de máquina virtual base a partir de la que se crean las máquinas virtuales de todos los usuarios. Configure la máquina virtual de la plantilla de modo que esté configurada exactamente con lo que desea proporcionar a los usuarios del laboratorio. Puede proporcionar un nombre y una descripción de la plantilla que verán los usuarios del laboratorio. Luego, publique la plantilla si desea que las instancias de la plantilla de máquina virtual estén disponibles para los usuarios del laboratorio. Cuando publica una plantilla, Azure Lab Services crea las máquinas virtuales en el laboratorio mediante la plantilla. El número de máquinas virtuales creadas en este proceso es igual al número máximo de usuarios permitidos en el laboratorio, que se puede establecer en la política de uso del laboratorio. Todas las máquinas virtuales tienen la misma configuración que la plantilla.

En este artículo se describe cómo crear y administrar una plantilla de máquina virtual en un laboratorio educativo de Azure Lab Services. 

## <a name="publish-a-template-while-creating-a-classroom-lab"></a>Publicación de una plantilla al crear un laboratorio educativo
En primer lugar, puede configurar y publicar una plantilla al crear un laboratorio educativo.

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
    2. Para publicar más adelante, seleccione **Guardar para más adelante**. Puede publicar la plantilla de máquina virtual una vez finalizado el asistente. Para más información sobre cómo configurar y publicar una vez finalizado el asistente, consulte la sección Publicación de la plantilla del artículo [Administración de laboratorios educativos](how-to-manage-classroom-labs.md).

        ![Publicar plantilla](../media/tutorial-setup-classroom-lab/publish-template.png)
11. Puede ver el **progreso de la publicación** de la plantilla. Este proceso puede tardar hasta una hora. 

    ![Publicar plantilla: progreso](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. Podrá ver la página siguiente cuando la plantilla se haya publicado correctamente. Seleccione **Listo**.

    ![Publicar plantilla: correctamente](../media/tutorial-setup-classroom-lab/publish-success.png)
1. Verá el **panel** del laboratorio. 
    
    ![Panel del laboratorio de clase](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)

 
## <a name="set-or-update-template-title-and-description"></a>Establecimiento o actualización del título y la descripción de la plantilla
Use los pasos siguientes para establecer el título y la descripción por primera vez y actualizarlos más adelante. 

1. En la sección **Plantilla**, mueva el mouse sobre el **nombre** o la **descripción** de la plantilla y selecciónelo. 
2. Escriba el **nuevo nombre** o la **nueva descripción** para la plantilla y presione **ENTRAR**.

    ![Nombre y descripción de la plantilla](../media/how-to-create-manage-template/template-name-description.png)

## <a name="set-up-or-update-a-template-vm"></a>Configuración o actualización de una plantilla de VM
 Puede conectarse a la plantilla de máquina virtual e instalar todo el software necesario en la misma antes de hacerla disponible para los alumnos. Siga estos pasos para configurar una plantilla de máquina virtual por primera vez o para actualizar la máquina virtual. 

1. Espere hasta que la plantilla de máquina virtual esté lista. Una vez que esté lista, el botón **Iniciar** debe estar habilitado. Para iniciar la máquina virtual, seleccione **Iniciar**.

    ![Inicio de la plantilla de máquina virtual](../media/tutorial-setup-classroom-lab/start-template-vm.png)
1. Revise la advertencia y seleccione **Iniciar**. 

    ![Iniciar plantilla: advertencia](../media/how-to-create-manage-template/start-template-warning.png)
1. Una vez iniciada, para conectarse a la máquina virtual, seleccione **Conectar** y siga las instrucciones. 
1. Instale el software necesario para que los estudiantes puedan realizar el laboratorio (por ejemplo, Visual Studio, Explorador de Azure Storage, etc). 
2. Desconéctese (cierre la sesión de escritorio remoto) de la plantilla de máquina virtual. 
3. **Detenga** la plantilla de máquina virtual seleccionando **Detener**. 

## <a name="publish-the-template-vm"></a>Publicación de la plantilla de máquina virtual  
Si no publica la plantilla al crear el laboratorio, puede hacerlo más adelante. Antes de la publicación, puede conectarse a la plantilla de máquina virtual y actualizarla con cualquier software. Cuando publica una plantilla, Azure Lab Services crea las máquinas virtuales en el laboratorio mediante la plantilla. El número de máquinas virtuales creadas en este proceso es igual al número máximo de usuarios permitidos en el laboratorio, que se puede establecer en la política de uso del laboratorio. Todas las máquinas virtuales tienen la misma configuración que la plantilla. 

1. Seleccione **Publicar** en la sección **Plantilla**. 

    ![Publicación de la plantilla de máquina virtual](../media/tutorial-setup-classroom-lab/public-access.png)
1. En el cuadro de mensaje **Publish the template** (Publicar la plantilla), revise el mensaje y seleccione **Publish** (Publicar). Este proceso puede tardar algún tiempo dependiendo de cuántas máquinas virtuales se estén creando.
    
    > [!IMPORTANT]
    > Las plantillas publicadas no pueden dejar de publicarse. Sin embargo, puede volver a publicar la plantilla. 
4. Espere a que el estado de la plantilla cambie a **Publicada**. 

    ![Estado de publicación](../media/how-to-create-manage-template/publish-status.png)
1. Cambie a la página **Máquinas virtuales** y confirme que ve las máquinas virtuales que tienen el estado **Sin asignar**. Estas máquinas virtuales no están asignadas a los alumnos todavía. Espere hasta que se creen las máquinas virtuales. Deberían estar en estado **Detenida**. En esta pagina, puede iniciar una máquina virtual de un alumno, conectarse a la máquina virtual, detener la máquina virtual y eliminar la máquina virtual. Puede iniciarlas en esta página o permitir que los alumnos inicien las máquinas virtuales. 

    ![Máquinas virtuales en estado detenido](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)


## <a name="republish-the-template"></a>Volver a publicar la plantilla 
Después de publicar una plantilla, puede seguir conectándose a la plantilla de máquina virtual, actualizarla y, a continuación, volver a publicarla. Cuando se vuelve a publicar una plantilla de máquina virtual, todas las máquinas virtuales de usuario se desconectan y se vuelven a crear en función de la plantilla actualizada. 

1. En la página del panel del laboratorio, seleccione **Republish** (Volver a publicar) en la sección Template (Plantilla). 

    ![Botón para volver a publicar](../media/how-to-create-manage-template/republish-button.png)
2. En el cuadro de mensaje **Republish the template** (Volver a publicar la plantilla), revise el texto y seleccione **Republish** (Volver a publicar) para continuar. De lo contrario, seleccione **No**. 

    ![Volver a publicar el mensaje de plantilla](../media/how-to-create-manage-template/republish-template-message.png)
3. Verá el estado de la nueva publicación en el icono de la sección **Template** (Plantilla).

    ![Estado de nueva publicación](../media/how-to-create-manage-template/republish-status-publishing.png)
4. Una vez publicada la plantilla, el estado se establece en **Published** (Publicada). 

    ![Nueva publicación correcta](../media/how-to-create-manage-template/republish-success.png)

## <a name="next-steps"></a>Pasos siguientes
Consulte los artículos siguientes:

- [Como administrador, crear y administrar cuentas de laboratorio](how-to-manage-lab-accounts.md)
- [Como propietario del laboratorio, crear y administrar laboratorios](how-to-manage-classroom-labs.md)
- [Como propietario del laboratorio, configurar y controlar el uso de un laboratorio](how-to-configure-student-usage.md)
- [Como usuario del laboratorio, obtener acceso a laboratorios educativos](how-to-use-classroom-lab.md)
