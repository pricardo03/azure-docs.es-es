---
title: Uso de Escritorio remoto para Linux en Azure Lab Services | Microsoft Docs
description: Obtenga información sobre cómo usar Escritorio remoto para máquinas virtuales Linux en un laboratorio en Azure Lab Services.
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
ms.openlocfilehash: 33b4ed3974c3e4e88e5d74ff31a3b8008ab565e9
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2019
ms.locfileid: "69656919"
---
# <a name="use-remote-desktop-for-linux-virtual-machines-in-a-classroom-lab-of-azure-lab-services"></a>Uso de Escritorio remoto para máquinas virtuales Linux en un laboratorio de clase de Azure Lab Services
En este artículo se muestra cómo los alumnos pueden conectarse a una máquina virtual (VM) Linux en un laboratorio mediante RDP/SSH. 

Un instructor debe habilitar la característica de conexión a Escritorio remoto para que los alumnos puedan conectarse a la máquina virtual del laboratorio de clase. Para obtener instrucciones sobre cómo un instructor puede habilitar la característica de conexión a Escritorio remoto, consulte [Habilitación de Escritorio remoto para máquinas virtuales Linux](how-to-enable-remote-desktop-linux.md).

> [!IMPORTANT] 
> Al habilitar una **conexión a Escritorio remoto**, solo se abre el puerto **RDP** en máquinas Linux. Un instructor puede conectarse a la máquina Linux mediante SSH por primera vez e instalar paquetes RDP y GUI para que pueda conectarse a la máquina de Linux mediante RDP más tarde. 

## <a name="connect-to-the-student-vm"></a>Conexión a la máquina virtual del alumno
Los alumnos pueden usar RDP para conectarse a sus VM Linux después de que el propietario de laboratorio (maestro/profesor) **publica** la VM de plantillas con los paquetes de RDP y GUI instalados en la máquina. Estos son los pasos que se deben seguir: 

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
Para obtener información sobre cómo habilitar la característica de conexión a Escritorio remoto para las máquinas virtuales Linux en un laboratorio de clase, consulte [Habilitación de Escritorio remoto para máquinas virtuales Linux](how-to-enable-remote-desktop-linux.md). 

