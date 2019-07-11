---
title: Consola serie de Azure para GRUB y el modo de usuario único | Microsoft Docs
description: Uso de la consola serie para GRUB en máquinas virtuales de Azure.
services: virtual-machines-linux
documentationcenter: ''
author: asinn826
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/08/2019
ms.author: alsin
ms.openlocfilehash: 8a3be6420a91093e060850459ff22fc5823b8cf2
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67710596"
---
# <a name="use-serial-console-to-access-grub-and-single-user-mode"></a>Uso de la consola serie para acceder a GRUB y al modo de usuario único
GRUB son las siglas de GRand Unified Bootloader, que es probablemente lo primero que verá al arrancar una máquina virtual. Al mostrarse antes de que se haya iniciado el sistema operativo, no se puede acceder a él mediante SSH. Con GRUB es posible modificar la configuración de arranque para arrancar en modo usuario único, entre otras cosas.

El modo de usuario único constituye un entorno mínimo con una funcionalidad mínima. Puede ser útil para investigar los problemas de arranque, los problemas del sistema de archivos o los problemas de red. Se pueden ejecutar menos servicios en segundo plano y, en función del nivel de ejecución, es posible que ni siquiera se monte un sistema de archivos de forma automática.

El modo de usuario único también es útil en situaciones en las que la máquina virtual solo se puede configurar para aceptar claves SSH para el inicio de sesión. En este caso, es posible que pueda usar el modo de usuario único para crear una cuenta con autenticación de contraseña. Tenga en cuenta que el servicio de consola serie solo permitirá a los usuarios con acceso de nivel de colaborador, o superior, acceder a la consola serie de una máquina virtual.

Para entrar en el modo de usuario único, tendrá que entrar en GRUB al arrancar la máquina virtual y modificar la configuración de arranque en GRUB. A continuación encontrará instrucciones detalladas para entrar en GRUB. En general, puede usar el botón de reinicio en la consola serie de la máquina virtual para reiniciar la máquina virtual y mostrar GRUB si la máquina virtual se ha configurado para ello.

![Botón de reinicio de la consola serie Linux](./media/virtual-machines-serial-console/virtual-machine-serial-console-restart-button-bar.png)

## <a name="general-grub-access"></a>Acceso a GRUB general
Para acceder a GRUB, tendrá que reiniciar la máquina virtual mientras mantiene abierta la hoja de la consola serie. Algunas distribuciones requerirán la entrada de teclado para mostrar GRUB, mientras que otras lo mostrarán de forma automática durante unos segundos y permitirán la entrada de teclado del usuario para cancelar el tiempo de espera.

Probablemente querrá asegurarse de que GRUB está habilitado en la máquina virtual para poder acceder al modo de usuario único. Según la distribución, puede que haya que realizar algunas operaciones de configuración para asegurarse de que GRUB está habilitado. A continuación, en [este vínculo](https://blogs.msdn.microsoft.com/linuxonazure/2018/10/23/why-proactively-ensuring-you-have-access-to-grub-and-sysrq-in-your-linux-vm-could-save-you-lots-of-down-time/) encontrará información específica de la distribución.

### <a name="restart-your-vm-to-access-grub-in-serial-console"></a>Reinicio de una máquina virtual para acceder a GRUB en la consola serie
Para reiniciar la máquina virtual de la consola serie, vaya al botón de encendido y haga clic en "Reiniciar VM". Así se iniciará un reinicio de la máquina virtual, y verá una notificación en Azure Portal relativa al reinicio.
El reinicio de la máquina virtual también se puede realizar con un comando SysRq `'b'` si [SysRq](./serial-console-nmi-sysrq.md) está habilitado. Siga las siguientes instrucciones específicas de la distribución para saber lo que puede esperar de GRUB al reiniciar el equipo.

![Reinicio de la consola serie Linux](./media/virtual-machines-serial-console/virtual-machine-serial-console-restart-button-ubuntu.gif)

## <a name="general-single-user-mode-access"></a>Acceso en modo de usuario único general
El acceso manual al modo de usuario único puede ser necesario en situaciones en las que no se ha configurado una cuenta con la autenticación de contraseña. Deberá modificar la configuración de GRUB para entrar manualmente al modo de usuario único. Después de hacerlo, consulte Uso del modo de usuario único para restablecer o agregar una contraseña para instrucciones adicionales.

Si la máquina virtual no puede arrancar, algunas distribuciones le pondrán automáticamente en modo de usuario único o modo de emergencia. Pero en otras se requiere configuración adicional antes de ponerle en modo de usuario único o de emergencia de forma automática (por ejemplo la configuración de una contraseña raíz).

### <a name="use-single-user-mode-to-reset-or-add-a-password"></a>Uso del modo de usuario único para restablecer o agregar una contraseña
Una vez en el modo de usuario único, siga estos pasos para agregar un usuario nuevo con privilegios de sudo:
1. Ejecute `useradd <username>` para agregar un usuario.
1. Ejecute `sudo usermod -a -G sudo <username>` para conceder los nuevos privilegios de raíz de usuario.
1. Use `passwd <username>` para establecer la contraseña del nuevo usuario. A continuación, podrá iniciar sesión con el nuevo usuario.


## <a name="access-for-red-hat-enterprise-linux-rhel"></a>Acceso a Red Hat Enterprise Linux (RHEL)
RHEL le pondrá en modo de usuario único automáticamente si no puede arrancar con normalidad. Sin embargo, si no ha configurado el acceso a la raíz para el modo de usuario único, no tendrá una contraseña raíz y no podrá iniciar sesión. Hay una solución alternativa (consulte "Especificación manual del modo de usuario único" a continuación), aunque es recomendable configurar el acceso a la raíz inicialmente.

### <a name="grub-access-in-rhel"></a>Acceso a GRUB en RHEL
RHEL viene con GRUB habilitado listo para su uso. Para especificar GRUB, reinicie la máquina virtual con `sudo reboot` y presione cualquier tecla. Aparecerá la pantalla de GRUB.

> Nota: Red Hat también proporciona documentación para arrancar en modo de rescate, modo de emergencia, modo de depuración y restablecer la contraseña raíz. [Haga clic aquí para acceder a él](https://aka.ms/rhel7grubterminal).

### <a name="set-up-root-access-for-single-user-mode-in-rhel"></a>Configuración del acceso a la raíz para el modo de usuario único en RHEL
El modo de usuario único en RHEL requiere que el usuario raíz esté habilitado (está deshabilitado de manera predeterminada). Si tiene que habilitar el modo de usuario único, siga estas instrucciones:

1. Inicie sesión en el sistema de Red Hat a través de SSH.
1. Cambie a raíz
1. Habilite la contraseña de usuario raíz.
    * `passwd root` (establezca una contraseña de raíz segura)
1. Asegúrese de que el usuario raíz solo puede iniciar sesión a través de ttyS0.
    * `edit /etc/ssh/sshd_config` y asegúrese de que PermitRootLog está establecido en no.
    * `edit /etc/securetty file` para permitir solo el inicio de sesión a través de ttyS0.

Ahora si el sistema arranca en modo de usuario único puede iniciar sesión a través de la contraseña raíz.

En RHEL 7.4+ o 6.9+, alternativamente, puede habilitar el modo de usuario único en las solicitudes de GRUB (vea [aquí](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/5/html/installation_guide/s1-rescuemode-booting-single) las instrucciones).

### <a name="manually-enter-single-user-mode-in-rhel"></a>Especificación manual del modo de usuario único en RHEL
Si ha configurado GRUB y el acceso a la raíz con las instrucciones anteriores, puede especificar el modo de usuario único con las instrucciones siguientes:

1. Presione "Esc" al reiniciar la máquina virtual para especificar GRUB
1. En GRUB, presione "e" para editar el sistema operativo seleccionado en el que desea arrancar (normalmente es la primera línea)
1. Busque la línea de kernel. En Azure, esta empieza por `linux16`
1. Presione Ctrl + E para ir al final de la línea
1. Agregue lo siguiente al final del archivo: `systemd.unit=rescue.target`
    * Esto le permitirá arrancar en modo de usuario único. Si desea usar el modo de emergencia, agregue `systemd.unit=emergency.target` al final de la línea en lugar de `systemd.unit=rescue.target`
1. Presione Ctrl + X para salir y reiniciar con la configuración aplicada
1. Se le pedirá la contraseña de administrador para poder especificar el modo de usuario único: se trata de la misma contraseña que creó en las instrucciones anteriores

    ![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-rhel-enter-emergency-shell.gif)

### <a name="enter-single-user-mode-without-root-account-enabled-in-rhel"></a>Especificación del modo de usuario único sin una cuenta raíz habilitada en RHEL
Si no ha realizado los anteriores pasos para habilitar el usuario raíz, aún puede restablecer la contraseña raíz. Use las siguientes instrucciones:

> Nota: Si va a usar SELinux, asegúrese de que ha seguido los pasos adicionales que se describen en la documentación de Red Hat que puede encontrar [aquí](https://aka.ms/rhel7grubterminal) cuando restablezca la contraseña raíz.

1. Presione "Esc" al reiniciar la máquina virtual para especificar GRUB
1. En GRUB, presione "e" para editar el sistema operativo seleccionado en el que desea arrancar (normalmente es la primera línea)
1. Busque la línea de kernel. En Azure, esta empieza por `linux16`
1. Agregue `rd.break` al final de la línea y asegúrese de que hay un espacio antes de `rd.break` (consulte el ejemplo siguiente)
    - Esto interrumpirá el proceso de arranque antes de que el control pase de `initramfs` a `systemd`, como se indica en la documentación de Red Hat que puede consultar [aquí](https://aka.ms/rhel7rootpassword).
1. Presione Ctrl + X para salir y reiniciar con la configuración aplicada
1. Cuando arranque, se le pondrá en modo de emergencia con un sistema de archivos de solo lectura. Escriba `mount -o remount,rw /sysroot` en el shell para volver a montar el sistema de archivos raíz con permisos de lectura/escritura
1. Cuando arranque en modo de usuario único, escriba en `chroot /sysroot` para cambiar a la cárcel `sysroot`
1. Ahora ya es raíz. Puede restablecer la contraseña raíz con `passwd` y, a continuación, seguir las instrucciones anteriores para especificar el modo de usuario único. Escriba `reboot -f` para reiniciar una vez que haya terminado.

![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-rhel-emergency-mount-no-root.gif)

> Nota: Si ejecuta las instrucciones anteriores se le pondrá en el shell de emergencia, de tal forma que pueda también realizar tareas como editar `fstab`. Sin embargo, la recomendación general es restablecer la contraseña raíz y usarla para especificar el modo de usuario único.


## <a name="access-for-centos"></a>Acceso para CentOS
De forma similar a Red Hat Enterprise Linux, el modo de usuario único en CentOS requiere que GRUB y el usuario raíz estén habilitados.

### <a name="grub-access-in-centos"></a>Acceso a GRUB en CentOS
CentOS viene con GRUB habilitado listo para su uso. Para especificar GRUB, reinicie la máquina virtual con `sudo reboot` y presione cualquier tecla. Aparecerá la pantalla de GRUB.

### <a name="single-user-mode-in-centos"></a>Modo de usuario único en CentOS
Siga las instrucciones anteriores para RHEL para habilitar el modo de usuario único en CentOS.

## <a name="access-for-ubuntu"></a>Acceso para Ubuntu
Las imágenes de Ubuntu no requieren una contraseña raíz. Si el sistema arranca en modo de usuario único, puede usarlo sin necesidad de otras credenciales.

### <a name="grub-access-in-ubuntu"></a>Acceso a GRUB en Ubuntu
Para acceder a GRUB, mantenga presionado "Esc" mientras arranca la máquina virtual.

Puede que las imágenes de Ubuntu no muestren de forma predeterminada la pantalla de GRUB. Esto se puede cambiar con las siguientes instrucciones:
1. Abra `/etc/default/grub.d/50-cloudimg-settings.cfg` en el editor de texto que prefiera.
1. Cambie el valor de `GRUB_TIMEOUT` por un valor distinto de cero.
1. Abra `/etc/default/grub` en el editor de texto que prefiera.
1. Marque como comentario la línea `GRUB_HIDDEN_TIMEOUT=1`.
1. Ejecute `sudo update-grub`

### <a name="single-user-mode-in-ubuntu"></a>Modo de usuario único en Ubuntu
Ubuntu le pondrá en modo de usuario único automáticamente si no puede arrancar con normalidad. Para especificar manualmente el modo de usuario único, siga estas instrucciones:

1. En GRUB, presione "e" para editar la entrada de arranque (la entrada de Ubuntu)
1. Busque la línea que empieza por `linux` y, a continuación, busque `ro`
1. Agregue `single` después de `ro` y asegúrese de que hay un espacio antes y después de `single`
1. Presione Ctrl + X para reiniciar el equipo con esta configuración y especificar el modo de usuario único

### <a name="using-grub-to-invoke-bash-in-ubuntu"></a>Uso de GRUB para invocar Bash en Ubuntu
Puede haber situaciones (por ejemplo, una contraseña raíz olvidada) en las que todavía no pueda tener acceso al modo de usuario único en la VM de Ubuntu después de probar las instrucciones anteriores. También le puede indicar al kernel que ejecute /bin/bash como inicialización, en lugar de la inicialización del sistema, que le asignará un shell de Bash y permitirá el mantenimiento del sistema. Use las siguientes instrucciones:

1. En GRUB, presione "e" para editar la entrada de arranque (la entrada de Ubuntu)
1. Busque la línea que empieza por `linux` y, a continuación, busque `ro`
1. Reemplazar `ro` con `rw init=/bin/bash`
    - Esta acción montará el sistema de archivos como de lectura y escritura y usará /bin/bash como proceso de inicialización.
1. Presione Control + X para reiniciar el equipo con esta configuración.

## <a name="access-for-coreos"></a>Acceso para CoreOS
El modo de usuario único en CoreOS requiere que GRUB esté habilitado.

### <a name="grub-access-in-coreos"></a>Acceso a GRUB en CoreOS
Para acceder a GRUB, presione cualquier tecla cuando se arranque la máquina virtual.

### <a name="single-user-mode-in-coreos"></a>Modo de usuario único en CoreOS
CoreOS le pondrá en modo de usuario único automáticamente si no puede arrancar con normalidad. Para especificar manualmente el modo de usuario único, siga estas instrucciones:
1. En GRUB, presione "e" para editar la entrada de arranque
1. Busque la línea que empieza por `linux$`. Deben ser 2, encapsuladas en diferentes cláusulas if/else
1. Anexe `coreos.autologin=ttyS0` al final de ambas líneas `linux$`
1. Presione Ctrl + X para reiniciar el equipo con esta configuración y especificar el modo de usuario único

## <a name="access-for-suse-sles"></a>Acceso para SUSE Linux Enterprise Server
Las imágenes más recientes de SLES 12 SP3+ permiten el acceso a través de la consola serie en caso de que el sistema arranque en modo de emergencia.

### <a name="grub-access-in-suse-sles"></a>Acceso a GRUB en SUSE Linux Enterprise Server
El acceso a GRUB en SUSE Linux Enterprise Server requiere la configuración del cargador de arranque a través de YaST. Para ello, siga estas instrucciones:

1. Use SSH en la máquina virtual de SLES y ejecute `sudo yast bootloader`. Use la tecla `tab`, la tecla `enter` y las teclas de dirección para desplazarse por el menú.
1. Vaya a `Kernel Parameters` y seleccione `Use serial console`.
1. Agregue `serial --unit=0 --speed=9600 --parity=no` a los argumentos de la consola

1. Presione F10 para guardar la configuración y salir
1. Para especificar GRUB, reinicie la máquina virtual y presione cualquier tecla durante la secuencia de arranque para que GRUB permanezca en pantalla
    - El tiempo de espera predeterminado para GRUB es 1 s. Puede modificar esto cambiando la variable `GRUB_TIMEOUT` en `/etc/default/grub`

![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-sles-yast-grub-config.gif)

### <a name="single-user-mode-in-suse-sles"></a>Modo de usuario único en SUSE Linux Enterprise Server
Se le pondrá automáticamente en el shell de emergencia si SLES no puede arrancar normalmente. Para especificar manualmente el shell de emergencia, siga estas instrucciones:

1. En GRUB, presione "e" para editar la entrada de arranque (la entrada de SLES)
1. Busque la línea de kernel que empieza por `linux`
1. Anexe `systemd.unit=emergency.target` al final de la línea
1. Presione Ctrl + X para reiniciar el equipo con esta configuración y especificar el shell de emergencia
   > Tenga en cuenta que se le pondrá en el shell de emergencia con un sistema de archivos de _solo lectura_. Si desea realizar alguna edición en cualquier archivo, tendrá que volver a montar el sistema de archivos con permisos de lectura y escritura. Para ello, escriba `mount -o remount,rw /` en el shell

## <a name="access-for-oracle-linux"></a>Acceso para Oracle Linux
De forma similar a Red Hat Enterprise Linux, el modo de usuario único en Oracle Linux requiere que GRUB y el usuario raíz estén habilitados.

### <a name="grub-access-in-oracle-linux"></a>Acceso a GRUB en Oracle Linux
Oracle Linux viene con GRUB habilitado listo para su uso. Para especificar GRUB, reinicie la máquina virtual con `sudo reboot` y presione "Esc". Aparecerá la pantalla de GRUB.

### <a name="single-user-mode-in-oracle-linux"></a>Modo de usuario único en Oracle Linux
Siga las instrucciones anteriores para RHEL para habilitar el modo de usuario único en Oracle Linux.

## <a name="next-steps"></a>Pasos siguientes
* La página principal de la documentación de Linux de la consola serie se encuentra [aquí](serial-console-linux.md).
* Aprenda a usar Serial Console para [habilitar GRUB en varias distribuciones](https://blogs.msdn.microsoft.com/linuxonazure/2018/10/23/why-proactively-ensuring-you-have-access-to-grub-and-sysrq-in-your-linux-vm-could-save-you-lots-of-down-time/)
* Uso de la consola serie para [llamadas NMI y SysRq](serial-console-nmi-sysrq.md)
* La consola serie también está disponible para VM [Windows](serial-console-windows.md)
* Obtenga más información sobre [Diagnósticos de arranque](boot-diagnostics.md)
