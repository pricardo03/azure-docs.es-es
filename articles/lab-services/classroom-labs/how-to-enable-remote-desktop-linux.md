---
title: Habilitar Escritorio remoto para Linux en Azure Lab Services | Microsoft Docs
description: Obtenga información sobre cómo habilitar Escritorio remoto para máquinas virtuales Linux en un laboratorio en Azure Lab Services.
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
ms.date: 03/28/2019
ms.author: spelluru
ms.openlocfilehash: 28f963ab807cbd679b7efe8b034ab91f2893198e
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58659463"
---
# <a name="enable-and-use-remote-desktop-for-linux-virtual-machines-in-a-lab-in-azure-lab-services"></a>Habilitar y usar Escritorio remoto para máquinas virtuales Linux en un laboratorio en Azure Lab Services
Este artículo muestra cómo realizar las tareas siguientes:

- Habilitar Escritorio remoto para VM de Linux
- Cómo los profesores pueden conectarse a la plantilla de máquina virtual a través de la conexión de escritorio remoto (RDP).
- Cómo conectarán a los alumnos al alumno con la máquina virtual a través de RDP

## <a name="enable-remote-desktop-for-linux-vm"></a>Habilitar Escritorio remoto para VM de Linux
Durante la creación del laboratorio, pueden permitir que los profesores **conexión a Escritorio remoto** para **Linux** imágenes. El **Habilitar conexión a Escritorio remoto** opción aparece cuando se selecciona una imagen de Linux para la plantilla. Cuando esta opción está habilitada, los profesores pueden conectarse a la plantilla de máquina virtual y las máquinas virtuales de alumno a través de RDP (escritorio remoto). 

![Habilitar la conexión a Escritorio remoto para una imagen de Linux](../media/how-to-enable-remote-desktop-linux/enable-rdp-option.png)

> [!IMPORTANT] 
> Habilitar **conexión a Escritorio remoto** sólo abre el **RDP** puerto en máquinas Linux. Usted, como profesor, conéctese a la máquina de Linux mediante SSH por primera vez e instala paquetes RDP y la interfaz gráfica de usuario para que puedan conectarse a la máquina de Linux mediante RDP más tarde. A continuación, se **publicar** la imagen para que los estudiantes puedan RDP al alumno con máquinas virtuales Linux. 

Actualmente, se admite la conexión a escritorio remota para los sistemas operativos siguientes:

- openSUSE Leap 42.3
- Basado en CentOS 7.5
- Debian 9 "Stretch"
- Ubuntu Server 16.04 LTS

## <a name="teachers-connecting-to-the-template-vm-using-rdp"></a>Los profesores conectarse a la plantilla de máquina virtual mediante RDP
Los profesores deben conectarse a la plantilla de máquina virtual mediante SSH en primer lugar e instalar paquetes de RDP y la interfaz gráfica de usuario en ella. A continuación, los profesores pueden usar los pasos siguientes para conectarse a las máquinas virtuales de Linux mediante RDP: 

Verá el **escritorio remoto** opción para conectarse a la plantilla de máquina virtual en el momento de crear el laboratorio. 

![Conectarse a la plantilla a través de RDP en el momento de creación](../media/how-to-enable-remote-desktop-linux/connect-at-creation.png)

Verá el **escritorio remoto** opción en la página del laboratorio principal una vez creado el laboratorio y la plantilla de máquina virtual se ha iniciado. Inicie la plantilla de máquina virtual si aún no está iniciado. 

![Conectarse a la plantilla a través de RDP una vez creado el laboratorio](../media/how-to-enable-remote-desktop-linux/rdp-after-lab-creation.png) 

Cuando se selecciona el **RDP** opción, descargará un archivo RDP. Abrir para conectarse a la máquina de Linux. 

## <a name="teachers-connecting-to-a-student-vm-using-rdp"></a>Conectarse a un máquina virtual de estudiante de profesores mediante RDP
Un propietario de laboratorio (profesor) puede conectarse a un máquina virtual de estudiante al cambiar a la **máquinas virtuales** ver y seleccionar el **conectar** icono. Antes de eso, los profesores deben **publicar** la imagen de plantilla con RDP y la interfaz gráfica de usuario de los paquetes instalados en él. 

![Conectarse a la máquina virtual de estudiante de profesores](../media/how-to-enable-remote-desktop-linux/teacher-connect-to-student-vm.png)

## <a name="students-connecting-to-the-student-vm"></a>Conectarse a la máquina virtual de estudiante de estudiantes
Estudiante puede usar RDP en para sus máquinas virtuales de Linux después el propietario de laboratorio (profesor) **publica** la plantilla de máquina virtual con RDP y la GUI de paquetes instalados en el equipo. Estos son los pasos que se deben seguir: 

1. Cuando un alumno inicia sesión en el portal laboratorios directamente (`http://labs.azure.com`) o mediante un vínculo de registro (`http://labs.azure.com/register/<registrationCode>`), se muestra un icono para cada alumno de laboratorio tiene acceso a. 
2. En el icono, seleccione **iniciar** si se detiene la máquina virtual. 
3. Seleccione **Conectar**. Esta acción descarga el archivo RDP en su máquina. Guárdelo y abrirla para conectarse a la máquina Linux a través de RDP. 

    ![Descargar de RDP de estudiante de una máquina virtual:](../media/how-to-enable-remote-desktop-linux/student-rdp-download.png)

    Todavía puede conectarse a la VM de Linux mediante SSH. Seleccione **... (puntos suspensivos)**  para ver la opción de SSH. 
    
    ![Estudiante de una máquina virtual: SSH](../media/how-to-enable-remote-desktop-linux/student-ssh.png)

    Copie y guarde la cadena de conexión SSH en el **conectar a la máquina virtual** cuadro de diálogo. Utilice esta cadena de conexión desde un terminal de SSH (como [Putty](https://www.putty.org/)) para conectarse a la máquina virtual. 

## <a name="next-steps"></a>Pasos siguientes
Consulte los artículos siguientes:

- [Como administrador, crear y administrar cuentas de laboratorio](how-to-manage-lab-accounts.md)
- [Como propietario del laboratorio, crear y administrar laboratorios](how-to-manage-classroom-labs.md)
- [Como propietario del laboratorio, configurar y publicar plantillas](how-to-create-manage-template.md)
- [Como usuario del laboratorio, obtener acceso a laboratorios educativos](how-to-use-classroom-lab.md)

