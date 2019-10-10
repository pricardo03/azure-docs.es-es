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
ms.date: 08/20/2019
ms.author: spelluru
ms.openlocfilehash: c67ca111bf87c9dbfa69c93149d29dbd32767fbd
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71350760"
---
# <a name="enable-remote-desktop-for-linux-virtual-machines-in-a-lab-in-azure-lab-services"></a>Habilitación del Escritorio remoto para máquinas virtuales Linux en un laboratorio de Azure Lab Services
En este artículo se muestra cómo realizar las tareas siguientes:

- Habilitar Escritorio remoto para una VM Linux
- Cómo los profesores pueden conectarse a la VM de plantillas a través de una Conexión a Escritorio remoto (RDP).

## <a name="enable-remote-desktop-for-linux-vm"></a>Habilitar Escritorio remoto para una VM Linux
Durante la creación del laboratorio, los profesores pueden habilitar la **Conexión a Escritorio remoto** para imágenes **Linux**. La opción **Habilitar Conexión a Escritorio remoto** aparece cuando se selecciona una imagen de Linux para la plantilla. Cuando se habilita esta opción, los profesores pueden conectarse a la VM de plantillas y las VM de alumnos a través de RDP (Escritorio remoto). 

![Habilitación de la conexión a Escritorio remoto para una imagen de Linux](../media/how-to-enable-remote-desktop-linux/enable-rdp-option.png)

En el cuadro de mensaje **Enabling Remote Desktop Connection** (Habilitar conexión a Escritorio remoto), seleccione **Continue with Remote Desktop** (Continuar con Escritorio remoto). 

![Habilitación de la conexión a Escritorio remoto para una imagen de Linux](../media/how-to-enable-remote-desktop-linux/enabling-remote-desktop-connection-dialog.png)

> [!IMPORTANT] 
> Al habilitar una **conexión a Escritorio remoto**, solo se abre el puerto **RDP** en máquinas Linux. Si RDP ya está instalado y configurado en la imagen de máquina virtual (por ejemplo: imagen de Data Science Virtual Machine de Ubuntu), será posible conectarse a las máquinas virtuales mediante RDP sin seguir pasos adicionales.
> 
> Si la imagen de máquina virtual no tiene RDP instalado y configurado, debe conectarse a la máquina Linux mediante SSH por primera vez e instalar paquetes RDP y GUI para que sea posible conectarse a la máquina Linux mediante RDP más tarde. Para más información, consulte [Instalación y configuración del escritorio remoto para conectarse a una máquina virtual Linux en Azure](../../virtual-machines/linux/use-remote-desktop.md). A continuación, publicará la imagen para que los alumnos puedan usar RDP para conectarse a las máquinas virtuales Linux de alumnos. 

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

## <a name="connect-using-ssh-or-rdp"></a>Conexión mediante SSH o RDP
Si selecciona la opción **SSH**, consulte el siguiente cuadro de diálogo **Conectarse a su máquina virtual**:  

![Cadena de conexión de SSH](../media/how-to-enable-remote-desktop-linux/ssh-connection-string.png)

Seleccione el botón **Copiar** situado junto al cuadro de texto para copiarlo en el Portapapeles. Guarde la cadena de conexión de SSH. Utilice esta cadena de conexión desde un terminal de SSH (como [Putty](https://www.putty.org/)) para conectarse a la máquina virtual.

Si selecciona la opción **RDP**, se descarga un archivo RDP en la máquina. Guarde el archivo y ábralo para conectarse a la máquina. 

## <a name="next-steps"></a>Pasos siguientes
Después de que un instructor habilita la característica de Conexión a Escritorio remoto, los alumnos pueden conectarse a sus máquinas virtuales a través de RDP/SSH. Para obtener más información, consulte [Uso de Escritorio remoto para máquinas virtuales Linux en un laboratorio de clase](how-to-use-remote-desktop-linux-student.md). 