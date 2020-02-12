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
ms.date: 10/31/2019
ms.author: spelluru
ms.openlocfilehash: 08fbe9565356dc1b7db952fdd265770fef600ca8
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2020
ms.locfileid: "76989049"
---
# <a name="create-and-manage-a-classroom-template-in-azure-lab-services"></a>Creación y administración de plantillas educativas en Azure Lab Services
En un laboratorio, una plantilla es una imagen de máquina virtual base a partir de la que se crean las máquinas virtuales de todos los usuarios. Configure la máquina virtual de la plantilla de modo que esté configurada exactamente con lo que desea proporcionar a los usuarios del laboratorio. Puede proporcionar un nombre y una descripción de la plantilla que verán los usuarios del laboratorio. Luego, publique la plantilla si desea que las instancias de la plantilla de máquina virtual estén disponibles para los usuarios del laboratorio. Cuando publica una plantilla, Azure Lab Services crea las máquinas virtuales en el laboratorio mediante la plantilla. El número de máquinas virtuales creadas en este proceso es igual al número máximo de usuarios permitidos en el laboratorio, que se puede establecer en la política de uso del laboratorio. Todas las máquinas virtuales tienen la misma configuración que la plantilla.

En este artículo se describe cómo crear y administrar una plantilla de máquina virtual en un laboratorio educativo de Azure Lab Services. 

## <a name="publish-a-template-while-creating-a-classroom-lab"></a>Publicación de una plantilla al crear un laboratorio educativo
Para aprender a publicar una plantilla durante la creación de un laboratorio educativo, consulte [Creación de un laboratorio educativo](how-to-manage-classroom-labs.md#create-a-classroom-lab)
 
## <a name="set-or-update-template-title-and-description"></a>Establecimiento o actualización del título y la descripción de la plantilla
Use los pasos siguientes para establecer el título y la descripción por primera vez y actualizarlos más adelante. 

1. En la página **Template** (Plantilla), escriba el **título** nuevo para el laboratorio.  
2. Escriba la **descripción** nueva para la plantilla. Al cambiar el enfoque del cuadro de texto, se guarda automáticamente. 

    ![Nombre y descripción de la plantilla](../media/how-to-create-manage-template/template-name-description.png)

## <a name="update-a-template-vm"></a>Actualización de una máquina virtual de plantilla
Use los pasos siguientes para actualizar una máquina virtual de plantilla.  

1. Espere hasta que la máquina virtual de plantilla se inicie, seleccione **Connect to template** (Conectarse a la plantilla) en la barra de herramientas para conectarse a la máquina virtual de plantilla y siga las instrucciones. Si es una máquina Windows, verá una opción para descargar el archivo RDP. 
1. Una vez que se conecta a la plantilla y realiza cambios, dejará de tener la misma configuración que las máquinas virtuales publicaron la última vez para los usuarios. Los cambios en la plantilla no se reflejarán en las máquinas virtuales existentes de los usuarios hasta que realice otra publicación.

    ![Conexión a la plantilla de máquina virtual](../media/how-to-create-manage-template/connect-template-vm.png)
    
1. Instale el software necesario para que los estudiantes puedan realizar el laboratorio (por ejemplo, Visual Studio, Explorador de Azure Storage, etc). 
1. Desconéctese (cierre la sesión de escritorio remoto) de la plantilla de máquina virtual. 
1. Seleccione **Stop template** (Detener plantilla) para **detener** la máquina virtual de plantilla. 
1. Siga los pasos de la sección siguiente para **publicar** la máquina virtual de plantilla actualizada. 

## <a name="publish-the-template-vm"></a>Publicación de la plantilla de máquina virtual  
Si no publica la plantilla al crear el laboratorio, puede hacerlo más adelante. Antes de la publicación, puede conectarse a la plantilla de máquina virtual y actualizarla con cualquier software. Cuando publica una plantilla, Azure Lab Services crea las máquinas virtuales en el laboratorio mediante la plantilla. El número de máquinas virtuales creadas en este proceso coincide con el que especificó al publicarlas por primera vez o en la página del grupo de máquinas virtuales. Todas las máquinas virtuales tienen la misma configuración que la plantilla. 

1. En la página **Template** (Plantilla), seleccione **Publish** (Publicar) en la barra de herramientas. 
1. En el cuadro de mensaje **Publish the template** (Publicar la plantilla), revise el mensaje y seleccione **Publish** (Publicar). Este proceso puede tardar algún tiempo dependiendo de cuántas máquinas virtuales se estén creando.

    ![Botón Publicar](../media/how-to-create-manage-template/publish-button.png)

    > [!IMPORTANT]
    > Las plantillas publicadas no pueden dejar de publicarse. Sin embargo, puede volver a publicar la plantilla. 
1. Puede ver el estado del proceso de publicación en la página Template (Plantilla). Espere a que el estado de la plantilla cambie a **Publicada**. 

    ![Estado de publicación](../media/how-to-create-manage-template/publish-status.png)
1. Cambie a la página **Máquinas virtuales** y confirme que ve las máquinas virtuales que tienen el estado **Sin asignar**. Estas máquinas virtuales no están asignadas a los alumnos todavía. Espere hasta que se creen las máquinas virtuales. Deberían estar en estado **Detenida**. En esta pagina, puede iniciar una máquina virtual de un alumno, conectarse a la máquina virtual, detener la máquina virtual y eliminar la máquina virtual. Puede iniciarlas en esta página o permitir que los alumnos inicien las máquinas virtuales. 

    ![Máquinas virtuales en estado detenido](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)


## <a name="next-steps"></a>Pasos siguientes
Vea los artículos siguientes:

- [Como administrador, crear y administrar cuentas de laboratorio](how-to-manage-lab-accounts.md)
- [Como propietario del laboratorio, crear y administrar laboratorios](how-to-manage-classroom-labs.md)
- [Como propietario del laboratorio, configurar y controlar el uso de un laboratorio](how-to-configure-student-usage.md)
- [Como usuario del laboratorio, obtener acceso a laboratorios educativos](how-to-use-classroom-lab.md)
