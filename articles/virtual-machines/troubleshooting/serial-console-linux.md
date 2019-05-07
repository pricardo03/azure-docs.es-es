---
title: Consola de serie de Azure para Linux | Microsoft Docs
description: Consola de serie bidireccional para máquinas virtuales y conjuntos de escalado de máquinas virtuales.
services: virtual-machines-linux
documentationcenter: ''
author: asinn826
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 5/1/2019
ms.author: harijay
ms.openlocfilehash: 7019d80c05a1953f4e57f0f42d46588310911791
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65141117"
---
# <a name="azure-serial-console-for-linux"></a>Consola de serie de Azure para Linux

La consola serie en el portal de Azure proporciona acceso a una consola basada en texto para máquinas virtuales (VM) Linux y las instancias de (conjunto de escalado de máquinas virtuales) del conjunto de escalado de máquinas virtuales. Esta conexión en serie se conecta al puerto serie COM1 de la máquina virtual o instancia de conjunto de escalado de máquinas virtuales, que proporciona acceso a los que depende del estado del sistema operativo o de red. La consola serie solo puede obtenerse mediante el portal de Azure y se permite únicamente para los usuarios que tienen un rol de acceso de colaborador o superior para el conjunto de escalado de máquina virtual o una máquina virtual.

Consola serie funciona de la misma manera para las máquinas virtuales e instancias del conjunto de escalado de máquinas virtuales. En este documento, todas las menciones a las máquinas virtuales incluirá implícitamente instancias del conjunto de escalado de máquina virtual, a menos que se indique lo contrario.

Para obtener documentación de la consola serie para Windows, consulte [consola serie para Windows](../windows/serial-console.md).

> [!NOTE]
> La consola serie está disponible con carácter general en las regiones globales de Azure. Aún no está disponible en las nubes de Azure Government ni Azure China.


## <a name="prerequisites"></a>Requisitos previos

- La instancia de conjunto de escalado de máquina virtual o una máquina virtual debe usar el modelo de implementación de resource Manager. No se admiten las implementaciones clásicas.

- La cuenta que usa la consola serie debe tener la [rol Colaborador de máquina Virtual](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) para la máquina virtual y el [diagnósticos de arranque](boot-diagnostics.md) cuenta de almacenamiento

- La instancia de conjunto de escalado de máquina virtual o una máquina virtual debe tener un usuario basado en contraseña. Puede crear una con la función para [restablecer la contraseña](https://docs.microsoft.com/azure/virtual-machines/extensions/vmaccess#reset-password) de la extensión de acceso de máquina virtual. Seleccione **Restablecer contraseña** en la sección **Soporte técnico y solución de problemas**.

- La instancia de conjunto de escalado de máquina virtual o una máquina virtual debe tener [diagnósticos de arranque](boot-diagnostics.md) habilitado.

    ![Configuración de los diagnósticos de arranque](./media/virtual-machines-serial-console/virtual-machine-serial-console-diagnostics-settings.png)

- Para conocer la configuración específica de las distribuciones de Linux, consulte [Disponibilidad de distribuciones de Linux para la consola serie](#serial-console-linux-distribution-availability).



## <a name="get-started-with-the-serial-console"></a>Introducción a la consola serie
La consola serie para las máquinas virtuales y el conjunto de escalado de máquina virtual es accesible a través de Azure portal:

### <a name="serial-console-for-virtual-machines"></a>Consola serie para las máquinas virtuales
Es tan sencillo como hacer clic en la consola serie para las máquinas virtuales **consola serie** dentro de la **soporte técnico y solución de problemas** sección en el portal de Azure.
  1. Abra [Azure Portal](https://portal.azure.com).

  1. Vaya a **todos los recursos** y seleccione una máquina Virtual. Se abrirá la página de información general de la máquina virtual.

  1. Desplácese hacia abajo hasta la sección **Soporte técnico y solución de problemas** y seleccione la opción **Consola serie**. Se abrirá un panel nuevo con la consola serie y se iniciará la conexión.

     ![Ventana de consola serie de Linux](./media/virtual-machines-serial-console/virtual-machine-linux-serial-console-connect.gif)

### <a name="serial-console-for-virtual-machine-scale-sets"></a>Consola serie para conjuntos de escalado de máquinas virtuales
Consola serie está disponible en una base por instancia para conjuntos de escalado de máquinas virtuales. Tendrá que vaya a la instancia individual de un conjunto de escalado de máquinas virtuales antes de ver el **consola serie** botón. Si el conjunto de escalado de máquina virtual no tiene habilitado el diagnóstico de arranque, asegúrese de que actualizar su modelo de conjunto de escalado de máquina virtual para habilitar diagnósticos de arranque y, a continuación, actualice todas las instancias al nuevo modelo con el fin de obtener acceso a la consola serie.
  1. Abra [Azure Portal](https://portal.azure.com).

  1. Vaya a **todos los recursos** y seleccione un conjunto de escalado de máquinas virtuales. Se abre la página de información general para el escalado de máquinas virtuales Set.

  1. Vaya a **instancias**

  1. Seleccione una instancia de conjunto de escalado de máquinas virtuales

  1. Desde el **soporte técnico y solución de problemas** sección, seleccione **consola serie**. Se abrirá un panel nuevo con la consola serie y se iniciará la conexión.

     ![Consola serie de conjunto de escalado de máquinas virtuales de Linux](./media/virtual-machines-serial-console/vmss-start-console.gif)


> [!NOTE]
> La consola serie requiere un usuario local con una contraseña configurada. Las máquinas virtuales o conjuntos de escalado de máquina virtual configurados únicamente con una clave pública SSH no pueden iniciar sesión en la consola serie. Para crear un usuario local con contraseña, utilice la [extensión VMAccess](https://docs.microsoft.com/azure/virtual-machines/linux/using-vmaccess-extension) (también está disponible en el portal al seleccionar **Restablecer contraseña**) y cree un usuario local con una contraseña.
> También puede restablecer la contraseña de administrador en su cuenta si [usa GRUB para iniciar el modo de usuario único](./serial-console-grub-single-user-mode.md).

## <a name="serial-console-linux-distribution-availability"></a>Disponibilidad de distribución de Linux de la consola serie
Para que la consola serie funcione correctamente, el sistema operativo invitado debe configurarse para leer y escribir mensajes de la consola en el puerto serie. La mayoría de [distribuciones de Linux aprobadas por Azure](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) tienen la consola serie configurada de manera predeterminada. Si selecciona la opción **Consola serie** en la sección **Soporte técnico y solución de problemas** de Azure Portal, obtendrá acceso a la consola serie.

Distribución      | Acceso a la consola serie
:-----------|:---------------------
Red Hat Enterprise Linux    | El acceso a la consola serie está habilitado de forma predeterminada.
CentOS      | El acceso a la consola serie está habilitado de forma predeterminada.
Ubuntu      | El acceso a la consola serie está habilitado de forma predeterminada.
CoreOS      | El acceso a la consola serie está habilitado de forma predeterminada.
SUSE        | Las imágenes de SLES más nuevas disponibles en Azure tienen habilitado el acceso a la consola serie de forma predeterminada. Si está utilizando versiones anteriores (10 o inferiores) de SLES en Azure, consulte el [artículo de KB](https://www.novell.com/support/kb/doc.php?id=3456486) para habilitar la consola serie.
Oracle Linux        | El acceso a la consola serie está habilitado de forma predeterminada.
Imágenes personalizadas de Linux     | Para habilitar la consola serie para la imagen de VM de Linux personalizada, habilite el acceso de la consola en el archivo */etc/inittab* a fin de ejecutar un terminal en `ttyS0`. Por ejemplo: `S0:12345:respawn:/sbin/agetty -L 115200 console vt102`. Para obtener más información sobre la creación correcta de imágenes personalizadas, consulte [Creación y carga de un VHD de Linux en Azure](https://aka.ms/createuploadvhd). Si está compilando un kernel personalizado, considere la posibilidad de habilitar las marcas de kernel `CONFIG_SERIAL_8250=y` y `CONFIG_MAGIC_SYSRQ_SERIAL=y`. El archivo de configuración se encuentra normalmente en la ruta de acceso */boot/*.

> [!NOTE]
> Si no ve nada en la consola serie, asegúrese de que el diagnóstico de arranque está habilitado en la máquina virtual. Alcanzar **ENTRAR** a menudo se solucionará problemas donde nada se muestre en la consola serie.

## <a name="common-scenarios-for-accessing-the-serial-console"></a>Escenarios comunes para acceder a la consola serie

Escenario          | Acciones en la consola serie
:------------------|:-----------------------------------------
Archivo *FSTAB* dañado | Presione la tecla **Entrar** para continuar y usar un editor de texto a fin de corregir el archivo *FSTAB*. Para ello, es posible que deba estar en modo de usuario único. Para obtener más información, vea la sección de la consola serie de [cómo solucionar problemas de fstab](https://support.microsoft.com/help/3206699/azure-linux-vm-cannot-start-because-of-fstab-errors) y [Use la consola serie para tener acceso a GRUB y modo de usuario único](serial-console-grub-single-user-mode.md).
Reglas de firewall incorrectas |  Si ha configurado iptables para bloquear la conectividad SSH, puede usar la consola serie para interactuar con la máquina virtual sin necesidad de SSH. Pueden encontrar más detalles en el [iptables man página](https://linux.die.net/man/8/iptables).<br>De forma similar, si su firewalld está bloqueando el acceso SSH, puede tener acceso a la máquina virtual a través de la consola serie y volver a configurar firewalld. Pueden encontrar más detalles en el [firewalld documentación](https://firewalld.org/documentation/).
Comprobación o daños en el sistema de archivos | Consulte la sección de la consola serie de [VM Linux de Azure no se puede iniciar debido a errores de sistema de archivos](https://support.microsoft.com/en-us/help/3213321/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck) para obtener más detalles sobre cómo solucionar dañado los sistemas de archivos mediante la consola serie.
Problemas de configuración de SSH | Acceda a la consola serie y cambie la configuración. Consola serie puede usarse independientemente de la configuración de SSH de una máquina virtual ya no requiere la VM tenga conectividad de red para que funcione. Una guía de solución de problemas está disponible en [solucionar problemas de conexiones SSH a una máquina virtual Linux de Azure que producen error, o se rechazan](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/troubleshoot-ssh-connection). Encontrará más detalles en [detallada SSH pasos para problemas para conectarse a una VM Linux en Azure para solucionar problemas](./detailed-troubleshoot-ssh-connection.md)
Interacción con el cargador de arranque | Reinicie la máquina virtual desde dentro de la hoja de la consola serie para acceder a GRUB en la máquina virtual Linux. Para obtener más detalles e información específica de la distribución, consulte [Use la consola serie para tener acceso a GRUB y modo de usuario único](serial-console-grub-single-user-mode.md).

## <a name="disable-the-serial-console"></a>Deshabilitar la consola serie
De forma predeterminada, todas las suscripciones tienen habilitado el acceso a la consola serie. Puede deshabilitar la consola serie en el nivel de conjunto de escala de máquina virtual/VM o nivel de suscripción. Tenga en cuenta que los diagnósticos de arranque deben estar habilitados en una máquina virtual en el orden de la consola serie para que funcione.

### <a name="vmvirtual-machine-scale-set-level-disable"></a>Deshabilitar de nivel de conjunto de escala de máquina virtual o una máquina virtual
La consola serie puede deshabilitarse para una escala de máquina virtual o una máquina virtual específica establecida deshabilitando la configuración de diagnósticos de arranque. Desactivar el diagnóstico de arranque desde el portal de Azure para deshabilitar la consola serie para la máquina virtual o el conjunto de escalado de máquinas virtuales. Si se utiliza la consola serie en un conjunto de escalado de máquinas virtuales, asegúrese de que actualizar las instancias del conjunto de escalado de máquina virtual para el modelo más reciente.

> [!NOTE]
> Para habilitar o deshabilitar la consola serie en una suscripción, debe tener permisos de escritura en la suscripción. Estos permisos incluyen roles de administrador o propietario. Los roles personalizados también pueden tener permisos de escritura.

### <a name="subscription-level-disable"></a>Deshabilitación a nivel de supervisión
La consola serie puede deshabilitarse para toda una suscripción a través de la [llamada API REST Disable Console](/rest/api/serialconsole/console/disableconsole). Puede usar la función **Probarlo** disponible en la página de documentación de la API para deshabilitar y habilitar la consola serie para una suscripción. Escriba el identificador de suscripción para **subscriptionId**, escriba **predeterminado** para el valor **predeterminado** y, a continuación, seleccione **Ejecutar**. Los comandos de la CLI de Azure aún no están disponibles.

![Probarlo en API REST](./media/virtual-machines-serial-console/virtual-machine-serial-console-rest-api-try-it.png)

Como alternativa, puede usar el conjunto de comandos Bash siguiente en Cloud Shell para deshabilitar, habilitar y ver el estado deshabilitado de la consola serie para una suscripción.

* Para obtener el estado deshabilitado de la consola serie para una suscripción:
    ```azurecli-interactive
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"'))

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s | jq .properties
    ```
* Para deshabilitar la consola serie para una suscripción:
    ```azurecli-interactive
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"'))

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl -X POST "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default/disableConsole?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s -H "Content-Length: 0"
    ```
* Para habilitar la consola serie para una suscripción:
    ```azurecli-interactive
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"'))

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl -X POST "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default/enableConsole?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s -H "Content-Length: 0"
    ```

## <a name="serial-console-security"></a>Seguridad de la consola serie

### <a name="access-security"></a>Seguridad de acceso
El acceso a la consola serie está limitado a los usuarios que tienen un rol de acceso de [Colaborador de la máquina Virtual](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) o superior. Si el inquilino de Azure Active Directory requiere la autenticación multifactor (MFA), el acceso a la consola serie también requerirá MFA porque el acceso a la consola serie se realiza mediante [Azure Portal](https://portal.azure.com).

### <a name="channel-security"></a>Seguridad del canal
Todos los datos enviados y recibidos se cifran en la conexión.

### <a name="audit-logs"></a>Registros de auditoría
Todo el acceso a la consola serie queda registrado en los registros de los [diagnósticos de arranque](https://docs.microsoft.com/azure/virtual-machines/linux/boot-diagnostics) de la máquina virtual. El administrador de la máquina virtual de Azure es el propietario y el que controla el acceso a estos registros.

> [!CAUTION]
> Las contraseñas de acceso de la consola no se registran. Sin embargo, si los comandos ejecutados en la consola contienen o generan contraseñas, secretos, nombres de usuario o cualquier otra forma de información de identificación personal, se escribirán en los registros de diagnóstico de arranque de la máquina virtual. Se escribirán junto con el resto del texto visible, como parte de la implementación de la función de desplazamiento de la consola serie. Estos registros son circulares y solo tienen acceso a ellos aquellas personas con permisos de lectura a la cuenta de almacenamiento de diagnósticos. Sin embargo, recomendamos usar el Escritorio remoto para cualquier operación que pueda implicar secretos o información de identificación personal.

### <a name="concurrent-usage"></a>Uso simultáneo
Si un usuario se conecta a la consola serie y otro usuario solicita correctamente acceso a esa misma máquina virtual, se desconectará el primer usuario y se conectará el segundo a la misma sesión.

> [!CAUTION]
> Esto significa que no se cerrará la sesión de un usuario que se haya desconectado. La capacidad de aplicar un cierre de sesión con la desconexión (mediante SIGHUP u otro mecanismo similar) está todavía en el plan. En Windows hay un tiempo de espera automático habilitado en la consola administrativa especial (SAC); sin embargo, en Linux puede configurar el valor de tiempo de espera del terminal. Para ello, agregue `export TMOUT=600` en su archivo *.bash_profile* o *.profile* para el usuario que use para iniciar sesión en la consola. Esta configuración agotará el tiempo de espera de la sesión en 10 minutos.

## <a name="accessibility"></a>Accesibilidad
Accesibilidad es un factor clave para la consola de serie de Azure. Por este motivo, hemos garantizado que la consola serie sea totalmente accesible.

### <a name="keyboard-navigation"></a>Navegación con el teclado
Use la tecla **Tabulador** del teclado para navegar por la interfaz de la consola serie en Azure Portal. La ubicación se resaltará en pantalla. Para salir del enfoque de la ventana de la consola serie, presione **Control**+**F6** en el teclado.

### <a name="use-serial-console-with-a-screen-reader"></a>Utilice la consola en serie con un lector de pantalla
La consola serie tiene integrada la compatibilidad con el lector de pantalla. Si navega con el lector de pantalla activado, el lector de pantalla podrá leer en voz alta el texto alternativo del botón seleccionado actualmente.

## <a name="errors"></a>Errors
Dado que la mayoría de los errores son transitorios, es posible que se solucionen si vuelve a intentar la conexión. En la tabla siguiente se muestra una lista de errores y mitigaciones. Estos errores y mitigaciones se aplican a ambas máquinas virtuales e instancias del conjunto de escalado de máquinas virtuales.

Error                            |   Mitigación
:---------------------------------|:--------------------------------------------|
No se puede recuperar la configuración de diagnóstico de arranque para *&lt;VMNAME&gt;*. Para usar la consola serie, asegúrese de que el diagnóstico de arranque está habilitado para esta máquina virtual. | Asegúrese de que la máquina virtual tiene los [diagnósticos de arranque](boot-diagnostics.md) habilitados.
La máquina virtual se encuentra en un estado desasignado detenido. Inicie la máquina virtual y vuelva a intentar la conexión de la consola serie. | La máquina virtual tiene que tener el estado iniciado para tener acceso a la consola serie.
No tiene los permisos necesarios para usar esta máquina virtual como consola serie. Asegúrese de tener los permisos del rol de Colaborador de la máquina virtual como mínimo.| El acceso a la consola serie requiere determinados permisos. Para obtener más información, consulte la sección [Requisitos previos](#prerequisites).
No se puede determinar el grupo de recursos para la cuenta de almacenamiento de diagnósticos de arranque *&lt;STORAGEACCOUNTNAME&gt;*. Compruebe que los diagnósticos de arranque están habilitados para esta máquina virtual y acceda a esta cuenta de almacenamiento. | El acceso a la consola serie requiere determinados permisos. Para obtener más información, consulte la sección [Requisitos previos](#prerequisites).
Socket web está cerrado o no se pudo abrir. | Puede que necesite incluir `*.console.azure.com` en la lista blanca. Un enfoque más detallado, pero más largo, es incluir en la lista blanca los [rangos de IP del centro de datos de Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653), que cambian con bastante frecuencia.
Se encontró una respuesta "Prohibido" al obtener acceso a la cuenta de almacenamiento de diagnósticos de arranque de la VM. | Asegúrese de que los diagnósticos de arranque no tengan ningún firewall de cuentas. Se necesita una cuenta de almacenamiento de diagnósticos de arranque accesible para que la consola serie funcione.

## <a name="known-issues"></a>Problemas conocidos
Somos conscientes de que la consola serie presenta algunos problemas. A continuación, tiene una lista de dichos problemas y los pasos que puede realizar para mitigarlos. Estos problemas y las mitigaciones se aplican a ambas máquinas virtuales e instancias del conjunto de escalado de máquinas virtuales.

Problema                           |   Mitigación
:---------------------------------|:--------------------------------------------|
Al presionar **Entrar** después del banner de conexión no se muestra ninguna solicitud de inicio de sesión. | Para obtener más información, consulte [Hitting enter does nothing](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md) (Al presionar Entrar, no se realiza ninguna acción). Este problema puede producirse si está ejecutando una máquina virtual personalizada, dispositivo reforzado o configuración GRUB que hace que Linux no pueda conectarse al puerto serie.
El texto de la consola serie solo ocupa una parte de la pantalla (a menudo después de usar un editor de texto). | Las consolas serie no admiten operaciones para cambiar el tamaño de la ventana ([RFC 1073](https://www.ietf.org/rfc/rfc1073.txt)), lo que significa que no se enviará ninguna señal SIGWINCH para actualizar el tamaño de la pantalla y la máquina virtual no tendrá conocimiento del tamaño del terminal. Instale xterm o una utilidad similar que le proporcione el comando `resize` y, a continuación, ejecute `resize`.
El hecho de pegar cadenas largas no funciona. | La consola serie limita la longitud de las cadenas pegadas en el terminal a 2048 caracteres a fin de impedir que el ancho de banda del puerto serie se sobrecargue.
La consola serie no funciona con un firewall de la cuenta de almacenamiento. | Por su diseño, la consola serie no puede operar con firewalls de la cuenta de almacenamiento habilitados en la cuenta de almacenamiento del diagnóstico de arranque.


## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

**P. ¿Cómo puedo enviar comentarios?**

A. Proporcione comentarios mediante la creación de un problema de GitHub en https://aka.ms/serialconsolefeedback. También tiene la posibilidad (aunque no es tan recomendable) de enviar comentarios a través de azserialhelp@microsoft.com o en la categoría de la máquina virtual de https://feedback.azure.com.

**P. ¿La consola serie admite las operaciones de copiar y pegar?**

A. Sí. Use **Control**+**Mayús**+**C** y **Control**+**Mayús**+**V** para copiar y pegar contenido en el terminal.

**P. ¿Puedo usar la consola serie en lugar de una conexión SSH?**

A. Aunque este uso puede parecer técnicamente posible, la consola serie está pensada para usarse principalmente como una herramienta de solución de problemas en situaciones donde no es posible la conectividad a través de SSH. Se recomienda no usar la consola serie como un reemplazo de SSH por los motivos siguientes:

- La consola serie no tiene el ancho de banda que tiene SSH. Dado que es una conexión de solo texto, las interacciones de GUI más intensivas son complicadas.
- Actualmente, solo se puede acceder a la consola serie con el nombre de usuario y la contraseña. Las claves SSH son mucho más seguras que las combinaciones de nombre de usuario y contraseña; así que, desde una perspectiva de seguridad de inicio de sesión, SSH es más recomendable que la consola serie.

**P. ¿Quién puede habilitar o deshabilitar la consola serie en mi suscripción?**

A. Para habilitar o deshabilitar la consola serie en toda una suscripción, es preciso tener permisos de escritura en la suscripción. Los roles que tienen permiso de escritura son, entre otros, los roles de administrador o propietario. Los roles personalizados también pueden tener permisos de escritura.

**P. ¿Quién puede tener acceso a la consola serie para mi conjunto de escalado de máquina virtual/VM?**

A. Debe tener el rol de colaborador de máquina Virtual o una versión superior para un conjunto para tener acceso a la consola serie de escalado de máquina virtual o una máquina virtual.

**P. La consola serie no muestra nada, ¿qué hago?**

A. Es probable que la imagen esté mal configurada para el acceso a la consola serie. Consulte [Disponibilidad de distribuciones de Linux para la consola serie](#serial-console-linux-distribution-availability) para obtener información sobre la configuración de la imagen a fin de habilitar la consola serie.

**P. ¿Está disponible la consola serie en los conjuntos de escalado de máquinas virtuales?**

A. ¡Sí! Consulte [consola serie para conjuntos de escalado de máquinas virtuales](#serial-console-for-virtual-machine-scale-sets)

**P. Si configuro mi máquina virtual o el escalado de máquinas virtuales que se establece mediante el uso solo autenticación de clave SSH, ¿puedo seguir usando la consola serie para conectarse a mi instancia de conjunto de escalado de máquina virtual/VM?**

A. Sí. La consola serie no requiere claves SSH, por lo que lo único que debe hacer es configurar una combinación de nombre de usuario y contraseña. Para ello, seleccione la opción **Restablecer contraseña** en Azure Portal y use esas credenciales para iniciar sesión en la consola serie.

## <a name="next-steps"></a>Pasos siguientes
* Uso de la consola serie para [acceder a GRUB y al modo de usuario único](serial-console-grub-single-user-mode.md).
* Uso de la consola serie para [llamadas NMI y SysRq](serial-console-nmi-sysrq.md).
* Aprenda a usar la consola serie para [habilitar GRUB en varias distribuciones](https://blogs.msdn.microsoft.com/linuxonazure/2018/10/23/why-proactively-ensuring-you-have-access-to-grub-and-sysrq-in-your-linux-vm-could-save-you-lots-of-down-time/).
* La consola serie también está disponible para las [VM Windows](../windows/serial-console.md).
* Obtenga más información sobre [diagnósticos de arranque](boot-diagnostics.md).

