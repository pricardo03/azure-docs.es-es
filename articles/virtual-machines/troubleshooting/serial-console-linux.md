---
title: Consola serie de Azure para Linux | Microsoft Docs
description: Consola serie bidireccional para máquinas virtuales de Azure y conjuntos de escalado de máquinas virtuales.
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
ms.author: alsin
ms.openlocfilehash: 4fb2fd9503dad4113609b35dbe66496a03272f85
ms.sourcegitcommit: e5dcf12763af358f24e73b9f89ff4088ac63c6cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/14/2019
ms.locfileid: "67137222"
---
# <a name="azure-serial-console-for-linux"></a>Consola serie de Azure para Linux

La consola serie de Azure Portal proporciona acceso a una consola basada en texto para las máquinas virtuales Linux y los conjuntos de escalado de máquinas virtuales. Esta conexión serie se conecta al puerto serie ttys0 de la máquina virtual o del conjunto de escalado de máquinas virtuales y ofrece acceso a ellos sin estar relacionada con el estado del sistema operativo o de la red. A la consola serie solo se puede acceder mediante Azure Portal; además, esta solo se permite a los usuarios que tienen un rol de acceso de colaborador o superior en el conjunto de escalado de máquinas virtuales o la máquina virtual.

La consola serie funciona de la misma manera para las máquinas virtuales y los conjuntos de escalado de máquinas virtuales. En este documento, todas las menciones a las máquinas virtuales incluirán implícitamente los conjuntos de escalado de máquinas virtuales, a menos que se indique lo contrario.

Para la documentación sobre la consola serie para Windows, consulte [Consola serie para Windows](../windows/serial-console.md).

> [!NOTE]
> La consola serie está disponible con carácter general en regiones de Azure globales. Aún no está disponible en las nubes de Azure Government ni Azure China.


## <a name="prerequisites"></a>Requisitos previos

- La máquina virtual o el conjunto de escalado de máquinas virtuales debe usar el modelo de implementación de Resource Manager. No se admiten las implementaciones clásicas.

- La cuenta que usa una consola serie debe tener el [rol Colaborador de la máquina virtual](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) y la cuenta de almacenamiento de [diagnósticos de arranque](boot-diagnostics.md)

- La máquina virtual o el conjunto de escalado de máquinas virtuales deben tener un usuario con contraseña. Puede crear una con la función para [restablecer la contraseña](https://docs.microsoft.com/azure/virtual-machines/extensions/vmaccess#reset-password) de la extensión de acceso de máquina virtual. Seleccione **Restablecer contraseña** en la sección **Soporte técnico y solución de problemas**.

- La máquina virtual o el conjunto de escalado de máquinas virtuales deben tener el [diagnóstico de arranque](boot-diagnostics.md) habilitado.

    ![Configuración de los diagnósticos de arranque](./media/virtual-machines-serial-console/virtual-machine-serial-console-diagnostics-settings.png)

- Para conocer la configuración específica de las distribuciones de Linux, consulte [Disponibilidad de distribuciones de Linux para la consola serie](#serial-console-linux-distribution-availability).

- La máquina virtual o el conjunto de escalado de máquinas virtuales debe configurarse para la salida de serie en `ttys0`. Este es el valor predeterminado para las imágenes de Azure, pero querrá volver a comprobar esto en imágenes personalizadas. Más detalles [a continuación](#custom-linux-images).


## <a name="get-started-with-the-serial-console"></a>Introducción al uso de la consola serie
La consola serie para las máquinas virtuales y los conjuntos de escalado de máquinas virtuales solo es accesible mediante Azure Portal:

### <a name="serial-console-for-virtual-machines"></a>Consola serie para máquinas virtuales
Usar la consola serie para máquinas virtuales es tan sencillo como hacer clic en **Consola serie** en la sección **Soporte técnico y solución de problemas** de Azure Portal.
  1. Abra [Azure Portal](https://portal.azure.com).

  1. Vaya a **Todos los recursos** y seleccione una máquina Virtual. Se abrirá la página de información general de la máquina virtual.

  1. Desplácese hacia abajo hasta la sección **Soporte técnico y solución de problemas** y seleccione la opción **Consola serie**. Se abrirá un panel nuevo con la consola serie y se iniciará la conexión.

     ![Ventana de la consola serie de Linux](./media/virtual-machines-serial-console/virtual-machine-linux-serial-console-connect.gif)

### <a name="serial-console-for-virtual-machine-scale-sets"></a>Consola serie para los conjuntos de escalado de máquinas virtuales
La consola serie está disponible por instancia para los conjuntos de escalado de máquinas virtuales. Tendrá que ir a al conjunto de escalado de máquinas virtuales específico para ver el botón **Consola serie**. Si su conjunto de escalado de máquinas virtuales no tiene habilitado el diagnóstico de arranque, asegúrese de que actualizar el modelo para habilitarlo y actualice todas las instancias al nuevo modelo para acceder a la consola serie.
  1. Abra [Azure Portal](https://portal.azure.com).

  1. Vaya a **Todos los recursos** y seleccione un conjunto de escalado de máquinas virtuales. Se abrirá la página de información general del conjunto de escalado de máquinas virtuales.

  1. Vaya a **Instancias**

  1. Seleccione un conjunto de escalado de máquinas virtuales

  1. En la sección **Soporte técnico y solución de problemas**, seleccione **Consola serie**. Se abrirá un panel nuevo con la consola serie y se iniciará la conexión.

     ![Consola serie para conjuntos de escalado de máquinas virtuales Linux](./media/virtual-machines-serial-console/vmss-start-console.gif)


> [!NOTE]
> La consola serie requiere un usuario local con una contraseña configurada. Las máquinas virtuales o los conjuntos de escalado de máquinas virtuales configurados solo con una clave pública SSH no podrán iniciar sesión en la consola serie. Para crear un usuario local con contraseña, utilice la [extensión VMAccess](https://docs.microsoft.com/azure/virtual-machines/linux/using-vmaccess-extension) (también está disponible en el portal al seleccionar **Restablecer contraseña**) y cree un usuario local con una contraseña.
> También puede restablecer la contraseña de administrador en su cuenta si [usa GRUB para iniciar el modo de usuario único](./serial-console-grub-single-user-mode.md).

## <a name="serial-console-linux-distribution-availability"></a>Disponibilidad de la distribución de Linux para la consola serie
Para que la consola serie funcione correctamente, el sistema operativo invitado debe configurarse para leer y escribir mensajes de la consola en el puerto serie. La mayoría de [distribuciones de Linux aprobadas por Azure](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) tienen la consola serie configurada de manera predeterminada. Si selecciona la opción **Consola serie** en la sección **Soporte técnico y solución de problemas** de Azure Portal, obtendrá acceso a la consola serie.

> [!NOTE]
> Si no ve nada en la consola serie, asegúrese de que el diagnóstico de arranque está habilitado en la máquina virtual. Presionar **Entrar** a menudo soluciona los problemas en los que no aparece nada en la consola serie.

Distribución      | Acceso a la consola serie
:-----------|:---------------------
Red Hat Enterprise Linux    | El acceso a la consola serie está habilitado de forma predeterminada.
CentOS      | El acceso a la consola serie está habilitado de forma predeterminada.
Ubuntu      | El acceso a la consola serie está habilitado de forma predeterminada.
CoreOS      | El acceso a la consola serie está habilitado de forma predeterminada.
SUSE        | Las imágenes de SLES más nuevas disponibles en Azure tienen habilitado el acceso a la consola serie de forma predeterminada. Si está utilizando versiones anteriores (10 o inferiores) de SLES en Azure, consulte el [artículo de KB](https://www.novell.com/support/kb/doc.php?id=3456486) para habilitar la consola serie.
Oracle Linux        | El acceso a la consola serie está habilitado de forma predeterminada.

### <a name="custom-linux-images"></a>Imágenes personalizadas de Linux
Para habilitar la consola serie para la imagen de VM de Linux personalizada, habilite el acceso de la consola en el archivo */etc/inittab* a fin de ejecutar un terminal en `ttyS0`. Por ejemplo: `S0:12345:respawn:/sbin/agetty -L 115200 console vt102`.

También deberá agregar ttys0 como destino para la salida de serie. Para más información sobre cómo configurar una imagen personalizada para trabajar con la consola serie, consulte los requisitos generales del sistema en el artículo de [Creación y carga de un disco duro virtual Linux en Azure](https://aka.ms/createuploadvhd#general-linux-system-requirements).

Si está compilando un kernel personalizado, considere la posibilidad de habilitar las marcas de kernel `CONFIG_SERIAL_8250=y` y `CONFIG_MAGIC_SYSRQ_SERIAL=y`. El archivo de configuración se encuentra normalmente en la ruta de acceso */boot/* . |

## <a name="common-scenarios-for-accessing-the-serial-console"></a>Escenarios habituales de acceso a la consola serie

Escenario          | Acciones en la consola serie
:------------------|:-----------------------------------------
Archivo *FSTAB* dañado | Presione la tecla **Entrar** para continuar y usar un editor de texto a fin de corregir el archivo *FSTAB*. Para ello, es posible que deba estar en modo de usuario único. Para más información, consulte la sección de la consola serie sobre [cómo solucionar problemas de fstab](https://support.microsoft.com/help/3206699/azure-linux-vm-cannot-start-because-of-fstab-errors) y [Use serial console to access GRUB and single user mode](serial-console-grub-single-user-mode.md) (Uso de la consola serie para acceder a GRUB y al modo de usuario único).
Reglas de firewall incorrectas |  Si ha configurado iptables para bloquear la conectividad SSH, puede usar la consola serie para interactuar con la máquina virtual sin necesidad de SSH. Más detalles en la [página del manual de iptables](https://linux.die.net/man/8/iptables).<br>De forma similar, si su firewalld bloquea el acceso SSH, puede acceder a la máquina virtual mediante la consola serie y volver a configurar el firewalld. Se puede encontrar más información en la [documentación del firewalld](https://firewalld.org/documentation/).
Comprobación o daños en el sistema de archivos | Consulte la sección de la consola serie de [Azure VM Linux no se puede iniciar debido a errores de sistema de archivos](https://support.microsoft.com/en-us/help/3213321/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck) para más detalles sobre cómo solucionar sistemas de archivos dañados mediante la consola serie.
Problemas de configuración de SSH | Acceda a la consola serie y cambie la configuración. La consola serie puede usarse independientemente de la configuración de SSH de una máquina virtual, ya que no requiere que la máquina virtual tenga conectividad de red para que funcione. Hay una guía de solución de problemas disponible en [Troubleshoot SSH connections to an Azure Linux VM that fails, errors out, or is refused](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/troubleshoot-ssh-connection) (Solución de problemas de conexión SSH a una máquina virtual Linux de Azure que producen error o se rechazan). Más detalles disponibles en [Detailed SSH troubleshooting steps for issues connecting to a Linux VM in Azure](./detailed-troubleshoot-ssh-connection.md) (Pasos detallados de solución de problemas de SSH para los problemas de conexión a una máquina virtual Linux en Azure).
Interacción con el cargador de arranque | Reinicie la máquina virtual desde dentro de la hoja de la consola serie para acceder a GRUB en la máquina virtual Linux. Para más detalles e información específica de la distribución, consulte [Use serial console to access GRUB and single user mode](serial-console-grub-single-user-mode.md) (Uso de la consola serie para acceder a GRUB y al modo de usuario único).

## <a name="disable-the-serial-console"></a>Deshabilitación de la consola serie
De forma predeterminada, todas las suscripciones tienen el acceso a la consola serie habilitado. Puede deshabilitar la consola serie a nivel de la suscripción o de la máquina virtual o del conjunto de escalado de máquinas virtuales. Tenga en cuenta que el diagnóstico de arranque debe estar habilitado en una máquina virtual para que la consola serie funcione.

### <a name="vmvirtual-machine-scale-set-level-disable"></a>Nivel de la máquina virtual o del conjunto de escalado de máquinas virtuales deshabilitados
La consola serie puede deshabilitarse para una determinada máquina virtual o conjunto de escalado de máquinas virtuales al deshabilitar la configuración de diagnóstico de arranque. Desactive el diagnóstico de arranque desde Azure Portal para deshabilitar la consola serie para la máquina virtual o el conjunto de escalado de máquinas virtuales. Si utiliza la consola serie en un conjunto de escalado de máquinas virtuales, asegúrese de actualizar las instancias al modelo más reciente.

> [!NOTE]
> Para habilitar o deshabilitar la consola serie en una suscripción, debe tener permisos de escritura en la suscripción. Estos permisos incluyen roles de administrador o propietario. Los roles personalizados también pueden tener permisos de escritura.

### <a name="subscription-level-disable"></a>Deshabilitación a nivel de supervisión
La consola serie puede deshabilitarse para toda una suscripción a través de la [llamada API REST Disable Console](/rest/api/serialconsole/console/disableconsole). Esta acción requiere acceso de nivel de colaborador o superior en la suscripción. Puede usar la función **Probarlo** disponible en la página de documentación de la API para deshabilitar y habilitar la consola serie para una suscripción. Escriba el identificador de suscripción para **subscriptionId**, escriba **predeterminado** para el valor **predeterminado** y, a continuación, seleccione **Ejecutar**. Los comandos de la CLI de Azure aún no están disponibles.

Para volver a habilitar la consola serie para una suscripción, utilice la [llamada de API REST Enable Console](/rest/api/serialconsole/console/enableconsole).

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
> Esto significa que no se cerrará la sesión de un usuario que se haya desconectado. La posibilidad de forzar el cierre de sesión con la desconexión (mediante SIGHUP u otro mecanismo similar) sigue en proceso de valoración. En Windows hay un tiempo de espera automático habilitado en la consola administrativa especial (SAC); sin embargo, en Linux puede configurar el valor de tiempo de espera del terminal. Para ello, agregue `export TMOUT=600` en su archivo *.bash_profile* o *.profile* para el usuario que use para iniciar sesión en la consola. Esta configuración agotará el tiempo de espera de la sesión en 10 minutos.

## <a name="accessibility"></a>Accesibilidad
La accesibilidad es un factor clave de la consola serie de Azure. Por este motivo, hemos garantizado que la consola serie sea totalmente accesible.

### <a name="keyboard-navigation"></a>Navegación con el teclado
Use la tecla **Tabulador** del teclado para navegar por la interfaz de la consola serie en Azure Portal. La ubicación se resaltará en pantalla. Para salir del enfoque de la ventana de la consola serie, presione **Control**+**F6** en el teclado.

### <a name="use-serial-console-with-a-screen-reader"></a>Uso de la consola serie con un lector de pantalla
La consola serie tiene integrada la compatibilidad con el lector de pantalla. Si navega con el lector de pantalla activado, el lector de pantalla podrá leer en voz alta el texto alternativo del botón seleccionado actualmente.

## <a name="errors"></a>Errors
Dado que la mayoría de los errores son transitorios, es posible que se solucionen si vuelve a intentar la conexión. En la tabla siguiente se muestra una lista de errores y mitigaciones. Estos errores y mitigaciones se aplican tanto a las máquinas virtuales como a los conjuntos de escalado de máquinas virtuales.

Error                            |   Mitigación
:---------------------------------|:--------------------------------------------|
No se puede recuperar la configuración de diagnóstico de arranque para *&lt;VMNAME&gt;* . Para usar la consola serie, asegúrese de que el diagnóstico de arranque está habilitado para esta máquina virtual. | Asegúrese de que la máquina virtual tiene los [diagnósticos de arranque](boot-diagnostics.md) habilitados.
La máquina virtual se encuentra en un estado desasignado detenido. Inicie la máquina virtual y vuelva a intentar la conexión de la consola serie. | La máquina virtual tiene que tener el estado iniciado para tener acceso a la consola serie.
No tiene los permisos necesarios para usar esta máquina virtual como consola serie. Asegúrese de tener los permisos del rol de Colaborador de la máquina virtual como mínimo.| El acceso a la consola serie requiere determinados permisos. Para obtener más información, consulte la sección [Requisitos previos](#prerequisites).
No se puede determinar el grupo de recursos para la cuenta de almacenamiento de diagnósticos de arranque *&lt;STORAGEACCOUNTNAME&gt;* . Compruebe que los diagnósticos de arranque están habilitados para esta máquina virtual y acceda a esta cuenta de almacenamiento. | El acceso a la consola serie requiere determinados permisos. Para obtener más información, consulte la sección [Requisitos previos](#prerequisites).
Socket web está cerrado o no se pudo abrir. | Puede que necesite incluir `*.console.azure.com` en la lista blanca. Un enfoque más detallado, pero más largo, es incluir en la lista blanca los [rangos de IP del centro de datos de Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653), que cambian con bastante frecuencia.
Se encontró una respuesta "Prohibido" al obtener acceso a la cuenta de almacenamiento de diagnósticos de arranque de la VM. | Asegúrese de que los diagnósticos de arranque no tengan ningún firewall de cuentas. Se necesita una cuenta de almacenamiento de diagnósticos de arranque accesible para que la consola serie funcione.

## <a name="known-issues"></a>Problemas conocidos
Somos conscientes de que la consola serie presenta algunos problemas. A continuación, tiene una lista de dichos problemas y los pasos que puede realizar para mitigarlos. Estos problemas y mitigaciones se aplican tanto a las máquinas virtuales como a los conjuntos de escalado de máquinas virtuales.

Problema                           |   Mitigación
:---------------------------------|:--------------------------------------------|
Al presionar **Entrar** después del banner de conexión no se muestra ninguna solicitud de inicio de sesión. | Para obtener más información, consulte [Hitting enter does nothing](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md) (Al presionar Entrar, no se realiza ninguna acción). Este problema puede ocurrir si está ejecutando una máquina virtual personalizada, un dispositivo protegido o una configuración de GRUB que hace que Linux no pueda conectarse al puerto serie.
El texto de la consola serie solo ocupa una parte de la pantalla (a menudo después de usar un editor de texto). | Las consolas serie no admiten operaciones para cambiar el tamaño de la ventana ([RFC 1073](https://www.ietf.org/rfc/rfc1073.txt)), lo que significa que no se enviará ninguna señal SIGWINCH para actualizar el tamaño de la pantalla y la máquina virtual no tendrá conocimiento del tamaño del terminal. Instale xterm o una utilidad similar que le proporcione el comando `resize` y, a continuación, ejecute `resize`.
El hecho de pegar cadenas largas no funciona. | La consola serie limita la longitud de las cadenas pegadas en el terminal a 2048 caracteres a fin de impedir que el ancho de banda del puerto serie se sobrecargue.
La consola serie no funciona con un firewall de la cuenta de almacenamiento. | Por su diseño, la consola serie no puede operar con firewalls de la cuenta de almacenamiento habilitados en la cuenta de almacenamiento del diagnóstico de arranque.
La consola serie no funciona con una cuenta de almacenamiento que use Azure Data Lake Storage Gen2 con espacios de nombres jerárquicos. | Se trata de un problema conocido con los espacios de nombres jerárquicos. Para mitigar el problema, asegúrese de que cuenta de almacenamiento con diagnóstico de arranque de la máquina virtual no se crea mediante Azure Data Lake Storage Gen2. Esta opción solo puede establecerse durante la creación de la cuenta de almacenamiento. Es posible que deba crear cuenta de almacenamiento con diagnóstico de arranque independiente sin Azure Data Lake Storage Gen2 habilitado para mitigar este problema.
Entradas de teclado erráticas en las imágenes de SLES BYOS. La entrada mediante el teclado solo se reconoce esporádicamente. | Se trata de un problema con el paquete de Plymouth. No se debe ejecutar Plymouth en Azure, ya que no necesita una pantalla de presentación y Plymouth afecta la capacidad de la plataforma para usar la consola serie. Quite Plymouth con `sudo zypper remove plymouth` y reinicie el equipo. Como alternativa, modifique la línea de kernel de la configuración GRUB anexando `plymouth.enable=0` al final de esta. Para ello, puede [editar la entrada de arranque en el tiempo de arranque](https://aka.ms/serialconsolegrub#single-user-mode-in-suse-sles) o editar la línea GRUB_CMDLINE_LINUX en `/etc/default/grub`, reconstruir GRUB con `grub2-mkconfig -o /boot/grub2/grub.cfg` y reiniciar el equipo.


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

**P. ¿Quién puede acceder a la consola serie de mi máquina virtual o conjunto de escalado de máquinas virtuales?**

A. Para acceder a la consola serie, debe tener el rol Colaborador o superior para una máquina virtual o conjunto de escalado de máquinas virtuales.

**P. La consola serie no muestra nada, ¿qué hago?**

A. Es probable que la imagen esté mal configurada para el acceso a la consola serie. Consulte [Disponibilidad de distribuciones de Linux para la consola serie](#serial-console-linux-distribution-availability) para obtener información sobre la configuración de la imagen a fin de habilitar la consola serie.

**P. ¿Está disponible la consola serie en los conjuntos de escalado de máquinas virtuales?**

A. Sí. Consulte [Serial Console for Virtual Machine Scale Sets](#serial-console-for-virtual-machine-scale-sets) (Consola serie para los conjuntos de escalado de máquinas virtuales).

**P. Si configuro mi máquina virtual o conjunto de escalado de máquinas virtuales usando solo la autenticación de clave SSH, ¿puedo seguir usando la consola serie para conectarme a mi máquina virtual o conjunto de escalado de máquinas virtuales?**

A. Sí. La consola serie no requiere claves SSH, por lo que lo único que debe hacer es configurar una combinación de nombre de usuario y contraseña. Para ello, seleccione la opción **Restablecer contraseña** en Azure Portal y use esas credenciales para iniciar sesión en la consola serie.

## <a name="next-steps"></a>Pasos siguientes
* Uso de la consola serie para [acceder a GRUB y al modo de usuario único](serial-console-grub-single-user-mode.md).
* Uso de la consola serie para [llamadas NMI y SysRq](serial-console-nmi-sysrq.md).
* Aprenda a usar la consola serie para [habilitar GRUB en varias distribuciones](https://blogs.msdn.microsoft.com/linuxonazure/2018/10/23/why-proactively-ensuring-you-have-access-to-grub-and-sysrq-in-your-linux-vm-could-save-you-lots-of-down-time/).
* La consola serie también está disponible para las [VM Windows](../windows/serial-console.md).
* Obtenga más información sobre [diagnósticos de arranque](boot-diagnostics.md).

