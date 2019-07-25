---
title: Habilitación de Escritorio remoto para Linux en Azure Lab Services | Microsoft Docs
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
ms.date: 05/24/2019
ms.author: spelluru
ms.openlocfilehash: 389d467bd9672743d4a086e8a1c505fb0366dba7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66237117"
---
# <a name="enable-and-use-remote-desktop-for-linux-virtual-machines-in-a-lab-in-azure-lab-services"></a>Habilitación y uso de Escritorio remoto para máquinas virtuales Linux en un laboratorio en Azure Lab Services
En este artículo se muestra cómo realizar las tareas siguientes:

- Habilitar Escritorio remoto para una VM Linux
- Cómo los profesores pueden conectarse a la VM de plantillas a través de una Conexión a Escritorio remoto (RDP).
- Cómo los alumnos se conectan a la VM de alumnos a través de RDP

## <a name="enable-remote-desktop-for-linux-vm"></a>Habilitar Escritorio remoto para una VM Linux
Durante la creación del laboratorio, los profesores pueden habilitar la **Conexión a Escritorio remoto** para imágenes **Linux**. La opción **Habilitar Conexión a Escritorio remoto** aparece cuando se selecciona una imagen de Linux para la plantilla. Cuando se habilita esta opción, los profesores pueden conectarse a la VM de plantillas y las VM de alumnos a través de RDP (Escritorio remoto). 

![Habilitación de la conexión a Escritorio remoto para una imagen de Linux](../media/how-to-enable-remote-desktop-linux/enable-rdp-option.png)

En el cuadro de mensaje **Enabling Remote Desktop Connection** (Habilitar conexión a Escritorio remoto), seleccione **Continue with Remote Desktop** (Continuar con Escritorio remoto). 

![Habilitación de la conexión a Escritorio remoto para una imagen de Linux](../media/how-to-enable-remote-desktop-linux/enabling-remote-desktop-connection-dialog.png)

> [!IMPORTANT] 
> Al habilitar una **conexión a Escritorio remoto**, solo se abre el puerto **RDP** en máquinas Linux. Usted, como profesor, se conecta a la máquina Linux mediante SSH por primera vez e instala paquetes RDP y GUI para que pueda conectarse a la máquina de Linux mediante RDP más tarde. Luego, **publica** la imagen para que los alumnos puedan usar RDP para conectarse a las VM Linux de alumnos. 

## <a name="supported-operating-systems"></a>Sistemas operativos compatibles
Actualmente, la conexión a Escritorio remoto se admite para los sistemas operativos siguientes:

- openSUSE Leap 42.3
- Basado en CentOS 7.5
- Debian 9 "Stretch"
- Ubuntu Server 16.04 LTS

## <a name="teachers-connecting-to-the-template-vm-using-rdp"></a>Profesores que se conectan a la VM de plantillas mediante RDP
Los profesores primero deben conectarse a la VM de plantillas mediante SSH e instalar los paquetes de RDP y GUI en ella. Luego, los profesores pueden usar los pasos siguientes para conectarse a las VM Linux mediante RDP: 

Verá la opción **Escritorio remoto** para conectarse a la VM de plantillas en el momento de crear el laboratorio. 

![Conexión a una plantilla a través de RDP en el momento de creación](../media/how-to-enable-remote-desktop-linux/connect-at-creation.png)

Verá la opción **Escritorio remoto** en la página principal del laboratorio cuando este se haya creado y se haya iniciado la VM de plantillas. Si la VM de plantillas aún no se ha iniciado, iníciela. 

![Conexión a una plantilla a través de RDP una vez creado el laboratorio](../media/how-to-enable-remote-desktop-linux/rdp-after-lab-creation.png) 

Para más información sobre cómo conectarse a la VM mediante SSH o RDP, consulte Conexión a SSH o RDP((#connect-using-ssh-or-rdp). 

## <a name="teachers-connecting-to-a-student-vm-using-rdp"></a>Profesores que se conectan a una VM de alumnos mediante RDP
Un profesor puede conectarse a un VM de alumnos al cambiar a la vista **Máquinas virtuales** y seleccionar el icono **Conectar**. Antes de eso, los profesores deben **publicar** la imagen de plantilla con los paquetes de RDP y GUI instalados en ella. 

![Profesores que se conectan a la VM de alumnos](../media/how-to-enable-remote-desktop-linux/teacher-connect-to-student-vm.png)

Para más información sobre cómo conectarse a la VM mediante SSH o RDP, consulte Conexión a SSH o RDP((#connect-using-ssh-or-rdp). 

## <a name="students-connecting-to-the-student-vm"></a>Alumnos que se conectan a la VM de alumnos
Los alumnos pueden usar RDP en conectarse a sus VM Linux después de que el propietario de laboratorio (profesor) **publica** la VM de plantillas con los paquetes de RDP y GUI instalados en la máquina. Estos son los pasos que se deben seguir: 

1. Cuando un alumno inicia sesión en el portal de laboratorios directamente (`https://labs.azure.com`) o mediante un vínculo de registro (`https://labs.azure.com/register/<registrationCode>`), se muestra un icono para cada laboratorio al que el alumno tiene acceso. 
2. En el icono, seleccione **Iniciar**, si la VM se ha detenido. 
3. Seleccione **Conectar**. Verá dos opciones para conectarse a la VM: **SSH** y **Escritorio remoto**.

    ![VM de alumnos: opciones de conexión](../media/how-to-enable-remote-desktop-linux/student-vm-connect-options.png)

## <a name="connect-using-ssh-or-rdp"></a>Conexión mediante SSH o RDP
Si selecciona la opción **SSH**, consulte el siguiente cuadro de diálogo **Conectarse a su máquina virtual**:  

![Cadena de conexión de SSH](../media/how-to-enable-remote-desktop-linux/ssh-connection-string.png)

Seleccione el botón **Copiar** situado junto al cuadro de texto para copiarlo en el Portapapeles. Guarde la cadena de conexión de SSH. Utilice esta cadena de conexión desde un terminal de SSH (como [Putty](https://www.putty.org/)) para conectarse a la máquina virtual.

Si selecciona la opción **RDP**, se descarga un archivo RDP en la máquina. Guarde el archivo y ábralo para conectarse a la máquina. 

## <a name="next-steps"></a>Pasos siguientes
Consulte los artículos siguientes:

- [Como administrador, crear y administrar cuentas de laboratorio](how-to-manage-lab-accounts.md)
- [Como propietario del laboratorio, crear y administrar laboratorios](how-to-manage-classroom-labs.md)
- [Como propietario del laboratorio, configurar y publicar plantillas](how-to-create-manage-template.md)
- [Como usuario del laboratorio, obtener acceso a laboratorios educativos](how-to-use-classroom-lab.md)

