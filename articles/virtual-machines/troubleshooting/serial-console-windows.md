---
title: Consola serie de máquina virtual de Azure para Windows | Microsoft Docs
description: Consola serie bidireccional para máquinas virtuales Windows de Azure.
services: virtual-machines-windows
documentationcenter: ''
author: harijay
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 10/31/2018
ms.author: harijay
ms.openlocfilehash: 61b64b63a53318e0a703678d5525399fe13efa83
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54432770"
---
# <a name="virtual-machine-serial-console-for-windows"></a>Consola serie de máquina virtual para Windows

La consola serie de máquina virtual (VM) de Azure Portal proporciona acceso a una consola basada en texto para máquinas virtuales Windows. Esta conexión serie se conecta al puerto serie COM1 de la máquina virtual y ofrece acceso a la misma, sin estar relacionada con el estado del sistema operativo o de la red de la máquina virtual. Solo se puede acceder a la consola serie de una máquina virtual mediante Azure Portal. Solo se permite a los usuarios que tienen un rol de acceso de Colaborador de la máquina virtual o superior.

Para obtener documentación sobre la consola serie para máquinas virtuales Linux, consulte [Consola serie de máquina virtual para Linux](serial-console-linux.md).

> [!NOTE]
> La consola serie para máquinas virtuales suele estar disponible en las regiones de Azure globales. Aún no está disponible en las nubes de Azure Government ni Azure China.


## <a name="prerequisites"></a>Requisitos previos

* La máquina virtual en la que accede a una consola serie tiene que usar el modelo de implementación de administración de recursos. No se admiten las implementaciones clásicas.

* La máquina virtual en la que accede a una consola serie tiene que tener los [diagnósticos de arranque](boot-diagnostics.md) habilitados.

    ![Configuración de los diagnósticos de arranque](../media/virtual-machines-serial-console/virtual-machine-serial-console-diagnostics-settings.png)

* Una cuenta que usa una consola serie debe tener el [rol Colaborador de la máquina virtual](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) para la máquina virtual y la cuenta de almacenamiento de [diagnósticos de arranque](boot-diagnostics.md).

* La máquina virtual en la que accede a una consola serie tiene que tener una cuenta basada en contraseñas. Puede crear una con la función para [restablecer la contraseña](https://docs.microsoft.com/azure/virtual-machines/extensions/vmaccess#reset-password) de la extensión de acceso de máquina virtual. Seleccione **Restablecer contraseña** en la sección **Soporte técnico y solución de problemas**.


## <a name="get-started-with-the-serial-console"></a>Introducción al uso de la consola serie
La consola serie para las máquinas virtuales solo es accesible mediante Azure Portal:

  1. Abra [Azure Portal](https://portal.azure.com).
  1. En el menú de la izquierda, seleccione **Máquinas virtuales**.
  1. Seleccione la máquina virtual en la lista. Se abrirá la página de información general de la máquina virtual.
  1. Desplácese hacia abajo hasta la sección **Soporte técnico y solución de problemas** y seleccione la opción **Consola serie**. Se abrirá un panel nuevo con la consola serie y se iniciará la conexión.

## <a name="enable-serial-console-functionality"></a>Habilitación de la funcionalidad de la consola serie

> [!NOTE]
> Si no ve nada en la consola serie, asegúrese de que el diagnóstico de arranque está habilitado en la máquina virtual.

### <a name="enable-the-serial-console-in-custom-or-older-images"></a>Habilitación de la consola serie en imágenes personalizadas o antiguas
Las imágenes más nuevas de Windows Server en Azure tienen la [consola de administración especial](https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) (SAC) habilitada de forma predeterminada. SAC se admite en versiones de servidor de Windows, pero no está disponible en versiones de cliente (por ejemplo, Windows 10, Windows 8 o Windows 7).

En el caso de las imágenes de Windows Server anteriores (creadas antes de febrero de 2018), puede habilitar automáticamente la consola serie mediante la característica de ejecución de comandos de Azure Portal. En Azure Portal, seleccione **Ejecutar comando** y, a continuación, seleccione el comando llamado **EnableEM** en la lista.

![Lista de comandos Ejecutar](./media/virtual-machines-serial-console/virtual-machine-windows-serial-console-runcommand.png)

De manera alternativa, para habilitar manualmente la consola serie para las máquinas virtuales Windows creadas antes de febrero de 2018, siga estos pasos:

1. Conéctese a la máquina virtual Windows mediante Escritorio remoto
1. Ejecute los siguientes comandos como administrador en un símbolo del sistema:
    - `bcdedit /ems {current} on`
    - `bcdedit /emssettings EMSPORT:1 EMSBAUDRATE:115200`
1. Reinicie el sistema para que la consola de SAC se habilite.

    ![Consola de SAC](./media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

Si es necesario SAC también puede habilitarse sin conexión:

1. Conecte el disco de Windows para el que desea configurar SAC como un disco de datos a la máquina virtual existente.

1. Ejecute los siguientes comandos como administrador en un símbolo del sistema:
   - `bcdedit /store <mountedvolume>\boot\bcd /ems {default} on`
   - `bcdedit /store <mountedvolume>\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

#### <a name="how-do-i-know-if-sac-is-enabled"></a>¿Cómo se puede saber si SAC está habilitada o no?

Si [SAC](https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) no está habilitado, la consola serie no mostrará el símbolo del sistema de SAC. En algunos casos, se muestra la información de mantenimiento de la máquina virtual y, en otros casos, se queda en blanco. Si utiliza una imagen de Windows Server creada antes de febrero de 2018, es probable que SAC no esté habilitado.

### <a name="enable-the-windows-boot-menu-in-the-serial-console"></a>Habilitación del menú de arranque de Windows en la consola serie

Si tiene que habilitar los mensajes del cargador de arranque de Windows para que se muestren en la consola serie, puede agregar las siguientes opciones adicionales a los datos de la configuración de arranque. Para más información, consulte [bcdedit](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcdedit--set).

1. Conéctese a la máquina virtual Windows mediante Escritorio remoto.

1. Ejecute los siguientes comandos como administrador en un símbolo del sistema:
   - `bcdedit /set {bootmgr} displaybootmenu yes`
   - `bcdedit /set {bootmgr} timeout 10`
   - `bcdedit /set {bootmgr} bootems yes`

1. Reinicie el sistema para que el menú de arranque se habilite

> [!NOTE]
> El tiempo de espera que establezca para que se muestre el menú del administrador de arranque afectará al tiempo de arranque del sistema operativo. Si cree que el valor de tiempo de espera de 10 segundos es demasiado corto o demasiado largo, establézcalo en un valor diferente.

## <a name="use-serial-console"></a>Uso de la consola serie

### <a name="use-cmd-or-powershell-in-serial-console"></a>Uso de CMD o PowerShell en la consola serie

1. Conéctese a la consola serie. Si se conecta correctamente, el símbolo del sistema es **SAC>**:

    ![Conectarse a SAC](./media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect-sac.png)

1.  Escriba `cmd` para crear un canal que contenga una instancia de CMD.

1.  Escriba `ch -si 1` para cambiar al canal que está ejecutando la instancia de CMD.

1.  Presione **Entrar** y, a continuación, escriba las credenciales de inicio de sesión con permisos administrativos.

1.  Después de escribir las credenciales válidas, se abre la instancia de CMD.

1.  Para iniciar una instancia de PowerShell, escriba `PowerShell` en la instancia de CMD y, a continuación, presione **Entrar**.

    ![Abrir una instancia de PowerShell](./media/virtual-machines-serial-console/virtual-machine-windows-serial-console-powershell.png)

### <a name="use-the-serial-console-for-nmi-calls"></a>Uso de la consola serie para llamadas NMI
Una interrupción no enmascarable (NMI) está diseñada para crear una señal que el software de una máquina virtual no pasará por alto. Históricamente, las NMI se han usado para supervisar problemas de hardware en sistemas que necesitaban tiempos de respuesta específicos. En la actualidad, los administradores del sistema y programadores suelen usar NMI como un mecanismo para depurar o solucionar los problemas de sistemas que se han bloqueado.

La consola serie se puede usar para enviar una interrupción NMI a una máquina virtual de Azure mediante el icono de teclado de la barra de comandos. Una vez que se entrega la interrupción NMI, la configuración de la máquina virtual controla cómo responde el sistema. Windows se puede configurar para bloquearse y crear un archivo de volcado de memoria cuando recibe una interrupción NMI.

![Enviar NMI](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-nmi.png) <br>

Para obtener información sobre cómo configurar Windows para crear un archivo de volcado de memoria cuando recibe una interrupción NMI, consulte: [Cómo generar un archivo de volcado de memoria con una interrupción NMI](https://support.microsoft.com/help/927069/how-to-generate-a-complete-crash-dump-file-or-a-kernel-crash-dump-file).

### <a name="use-function-keys-in-serial-console"></a>Uso de las teclas de función en la consola serie
Las teclas de función están habilitadas para usarse en la consola serie en máquinas virtuales Windows. La tecla F8 en la lista desplegable de la consola serie ofrece la comodidad de especificar fácilmente el menú de configuración de arranque avanzado, pero la consola serie es compatible con todas las demás teclas de función. Es posible que tenga que presionar **Fn** + **F1** (o F2, F3, etc.) en el teclado en función del equipo en el que use la consola serie.

### <a name="use-wsl-in-serial-console"></a>Uso de WSL en la consola serie
Subsistema de Windows para Linux (WSL) se ha habilitado para Windows Server 2019 o versiones posteriores, por lo que también es posible habilitar WSL para usarlo dentro de la consola serie si se ejecuta Windows Server 2019 o posterior. Esto puede resultar útil para los usuarios que también conocen los comandos de Linux. Para obtener instrucciones que permitan habilitar WSL para Windows Server, consulte la [Guía de instalación](https://docs.microsoft.com/windows/wsl/install-on-server).

## <a name="disable-serial-console"></a>Deshabilitación de la consola serie
De forma predeterminada, todas las suscripciones tienen el acceso a la consola serie habilitado para todas las VM. Puede deshabilitar la consola serie en el nivel de la suscripción o de la máquina virtual.

> [!NOTE]
> Para habilitar o deshabilitar la consola serie en una suscripción, debe tener permisos de escritura en la suscripción. Estos permisos incluyen, entre otros, los roles de administrador o propietario. Los roles personalizados también pueden tener permisos de escritura.

### <a name="subscription-level-disable"></a>Deshabilitación a nivel de supervisión
La consola serie puede deshabilitarse para toda una suscripción a través de la [llamada API REST Disable Console](/rest/api/serialconsole/console/disableconsole). Puede usar la función **Probarlo** disponible en la página de documentación de la API para deshabilitar y habilitar la consola serie para una suscripción. Escriba el identificador de suscripción para **subscriptionId**, escriba "default" para el valor **predeterminado** y, a continuación, seleccione **Ejecutar**. Los comandos de la CLI de Azure aún no están disponibles.

![Probarlo en API REST](../media/virtual-machines-serial-console/virtual-machine-serial-console-rest-api-try-it.png)

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

### <a name="vm-level-disable"></a>Deshabilitación a nivel de VM
La consola serie puede deshabilitarse para una determinada VM al deshabilitar la configuración de diagnóstico de arranque de la VM. Desactive el diagnóstico de arranque desde Azure Portal a fin de deshabilitar la consola serie para la VM.

## <a name="serial-console-security"></a>Seguridad de la consola serie

### <a name="access-security"></a>Seguridad de acceso
El acceso a la consola serie está limitado a los usuarios que tienen un rol de acceso de [Colaborador de la máquina Virtual](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) o superior. Si el inquilino de Azure Active Directory requiere la autenticación multifactor (MFA), el acceso a la consola serie también requerirá MFA porque el acceso a la consola serie se realiza mediante [Azure Portal](https://portal.azure.com).

### <a name="channel-security"></a>Seguridad del canal
Todos los datos enviados y recibidos se cifran en la conexión.

### <a name="audit-logs"></a>Registros de auditoría
Todo el acceso a la consola serie queda registrado en los registros de los [diagnósticos de arranque](https://docs.microsoft.com/azure/virtual-machines/linux/boot-diagnostics) de la máquina virtual. El administrador de la máquina virtual de Azure es el propietario y el que controla el acceso a estos registros.

>[!CAUTION]
Las contraseñas de acceso de la consola no se registran. Sin embargo, si los comandos ejecutados en la consola contienen o generan contraseñas, secretos, nombres de usuario o cualquier otra forma de información de identificación personal, se escribirán en los registros de diagnóstico de arranque de la máquina virtual. Se escribirán junto con el resto del texto visible, como parte de la implementación de la función de desplazamiento de la consola serie. Estos registros son circulares y solo tienen acceso a ellos aquellas personas con permisos de lectura a la cuenta de almacenamiento de diagnósticos. Sin embargo, recomendamos usar el Escritorio remoto para cualquier operación que pueda implicar secretos o información de identificación personal.

### <a name="concurrent-usage"></a>Uso simultáneo
Si un usuario se conecta a la consola serie y otro usuario solicita correctamente acceso a esa misma máquina virtual, se desconectará el primer usuario y se conectará el segundo a la misma sesión.

>[!CAUTION]
Esto significa que no se cerrará la sesión de un usuario que se haya desconectado. La posibilidad de forzar el cierre de sesión con la desconexión (mediante SIGHUP u otro mecanismo similar) sigue en proceso de valoración. En Windows hay un tiempo de espera automático habilitado en SAC; en Linux puede configurar el valor de tiempo de espera del terminal.

## <a name="accessibility"></a>Accesibilidad
La accesibilidad es un factor clave de la consola serie de Azure. Para ello, nos hemos asegurado de que la consola serie es accesible para quienes tienen dificultades auditivas y visuales, así como para las personas que no pueden utilizar un mouse.

### <a name="keyboard-navigation"></a>Navegación con el teclado
Use la tecla **Tabulador** del teclado para navegar por la interfaz de la consola serie en Azure Portal. La ubicación se resaltará en pantalla. Para salir del enfoque de la ventana de la consola serie, presione **Control**+**F6** en el teclado.

### <a name="use-the-serial-console-with-a-screen-reader"></a>Uso de la consola serie con un lector de pantalla
La consola serie tiene integrada la compatibilidad con el lector de pantalla. Si navega con el lector de pantalla activado, el lector de pantalla podrá leer en voz alta el texto alternativo del botón seleccionado actualmente.

## <a name="common-scenarios-for-accessing-the-serial-console"></a>Escenarios comunes para acceder a la consola de serie
Escenario          | Acciones en la consola serie
:------------------|:-----------------------------------------
Reglas de firewall incorrectas | Accede a la consola de serie y corrige las reglas de firewall de Windows.
Comprobación o daños en el sistema de archivos | Acceda a la consola serie y recupere el sistema de archivos.
Problemas de configuración de RDP | Acceda a la consola serie y cambie la configuración. Para más información, consulte la [documentación de RDP](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access).
Sistema de bloqueo de red | Acceda a la consola serie desde Azure Portal para administrar el sistema. Algunos comandos de red se enumeran en [Comandos de Windows: CMD y PowerShell](serial-console-cmd-ps-commands.md).
Interacción con el cargador de arranque | Acceda a BCD mediante la consola serie. Para obtener información, consulte [Habilitación del menú de arranque de Windows en la consola serie](#enable-the-windows-boot-menu-in-the-serial-console).


## <a name="errors"></a>Errors
Dado que la mayoría de los errores son transitorios, es posible que se solucionen si vuelve a intentar la conexión. En la tabla siguiente se muestra una lista de errores y mitigaciones.

Error                            |   Mitigación
:---------------------------------|:--------------------------------------------|
No se puede recuperar la configuración de diagnóstico de arranque para *&lt;VMNAME&gt;*. Para usar la consola serie, asegúrese de que el diagnóstico de arranque está habilitado para esta máquina virtual. | Asegúrese de que la máquina virtual tiene los [diagnósticos de arranque](boot-diagnostics.md) habilitados.
La máquina virtual se encuentra en un estado desasignado detenido. Inicie la máquina virtual y vuelva a intentar la conexión de la consola serie. | La máquina virtual tiene que estar en un estado iniciado para tener acceso a la consola serie
No tiene los permisos necesarios para usar la consola serie de esta máquina virtual. Asegúrese de tener los permisos del rol de Colaborador de la máquina virtual como mínimo.| El acceso a la consola serie requiere determinados permisos. Para obtener más información, consulte la sección [Requisitos previos](#prerequisites).
No se puede determinar el grupo de recursos para la cuenta de almacenamiento de diagnósticos de arranque *&lt;STORAGEACCOUNTNAME&gt;*. Compruebe que los diagnósticos de arranque están habilitados para esta máquina virtual y acceda a esta cuenta de almacenamiento. | El acceso a la consola serie requiere determinados permisos. Para obtener más información, consulte la sección [Requisitos previos](#prerequisites).
Se encontró una respuesta "Prohibido" al obtener acceso a la cuenta de almacenamiento de diagnósticos de arranque de la VM. | Asegúrese de que el diagnóstico de arranque no tenga un firewall de cuentas. Se necesita una cuenta de almacenamiento de diagnósticos de arranque accesible para que la consola serie funcione.
Socket web está cerrado o no se pudo abrir. | Puede que necesite incluir `*.console.azure.com` en la lista blanca. Un enfoque más detallado, pero más largo, es incluir en la lista blanca los [rangos de IP del centro de datos de Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653), que cambian con bastante frecuencia.
Se muestra solamente información de mantenimiento cuando se conecta a una máquina virtual Windows| Este error se produce si no se ha habilitado la consola de administración especial para la imagen de Windows. Consulte [Habilitación de la consola serie en imágenes personalizadas o antiguas](#enable-the-serial-console-in-custom-or-older-images) para obtener instrucciones sobre cómo habilitar manualmente SAC en la máquina virtual Windows. Para más información, consulte [Señales de mantenimiento de Windows](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Windows_Health_Info.md).

## <a name="known-issues"></a>Problemas conocidos
Somos conscientes de que la consola serie presenta algunos problemas. A continuación, tiene una lista de dichos problemas y los pasos que puede realizar para mitigarlos.

Problema                             |   Mitigación
:---------------------------------|:--------------------------------------------|
Al presionar **Entrar** después del banner de conexión no se muestra ninguna solicitud de inicio de sesión. | Para obtener más información, consulte [Hitting enter does nothing](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md) (Al presionar Entrar, no se realiza ninguna acción). Este error puede ocurrir si está ejecutando una máquina virtual personalizada, un dispositivo protegido o una configuración de arranque que hace que Windows no pueda conectarse correctamente al puerto serie. Este error se producirá también si está ejecutando una máquina virtual cliente de Windows 10, ya que únicamente las máquinas virtuales de Windows Server están configuradas para tener habilitada la opción EMS.
No se puede escribir en el símbolo del sistema de SAC si la depuración del kernel está habilitada. | Conéctese mediante RDP a la máquina virtual y ejecute `bcdedit /debug {current} off` desde un símbolo del sistema con privilegios elevados. Si no puede conectarse mediante RDP, en su lugar puede conectar el disco del sistema operativo a otra máquina virtual de Azure y modificarlo mientras está conectado como un disco de datos mediante la ejecución de `bcdedit /store <drive letter of data disk>:\boot\bcd /debug <identifier> off` para, seguidamente, volver a intercambiar el disco.
Pegar en PowerShell en SAC da como resultado un tercer carácter si el contenido original tenía un carácter repetido. | Una solución alternativa es ejecutar `Remove-Module PSReadLine` para descargar el módulo PSReadLine desde la sesión actual. Esta acción no eliminará ni desinstalará el módulo.
Algunas entradas de teclado generan resultados extraños de SAC (por ejemplo, **[A**, **[3~**). | Las secuencias de escape [VT100](https://aka.ms/vtsequences) no son compatibles con el símbolo del sistema de SAC.
El hecho de pegar cadenas largas no funciona. | La consola serie limita la longitud de las cadenas pegadas en el terminal a 2048 caracteres a fin de impedir que el ancho de banda del puerto serie se sobrecargue.
La consola serie no funciona con un firewall de la cuenta de almacenamiento. | Por su diseño, la consola serie no puede operar con firewalls de la cuenta de almacenamiento habilitados en la cuenta de almacenamiento del diagnóstico de arranque.


## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

**P. ¿Cómo puedo enviar comentarios?**

A. Proporcione comentarios mediante la creación de un problema de GitHub en https://aka.ms/serialconsolefeedback. También tiene la posibilidad (aunque no es tan recomendable) de enviar comentarios a través de azserialhelp@microsoft.com o en la categoría de la máquina virtual de http://feedback.azure.com.

**P. ¿La consola serie admite las operaciones de copiar y pegar?**

A. Sí. Use **Control**+**Mayús**+**C** y **Control**+**Mayús**+**V** para copiar y pegar contenido en el terminal.

**P. ¿Quién puede habilitar o deshabilitar la consola serie en mi suscripción?**

A. Para habilitar o deshabilitar la consola serie en toda una suscripción, es preciso tener permisos de escritura en la suscripción. Los roles que tienen permiso de escritura son, entre otros, los roles de administrador o propietario. Los roles personalizados también pueden tener permisos de escritura.

**P. ¿Quién puede acceder a la consola serie de mi máquina virtual?**

A. Debe tener el rol Colaborador o superior para una máquina virtual a fin de tener acceso a la consola serie de la máquina virtual.

**P. La consola serie no muestra nada, ¿qué hago?**

A. Es probable que la imagen esté mal configurada para el acceso a la consola serie. Consulte [Habilitación de la consola serie en imágenes personalizadas o antiguas](#enable-the-serial-console-in-custom-or-older-images) para obtener información sobre la configuración de la imagen para habilitar la consola serie.

**P. ¿Está disponible la consola serie en los conjuntos de escalado de máquinas virtuales?**

A. En estos momentos no se admite el acceso a la consola serie mediante instancias del conjunto de escalado de máquinas virtuales.

## <a name="next-steps"></a>Pasos siguientes
* Para obtener una guía detallada de los comandos de CMD y PowerShell que puede usar en SAC para Windows, consulte [Comandos de Windows: CMD y PowerShell](serial-console-cmd-ps-commands.md).
* La consola serie también está disponible para máquinas virtuales [Linux](serial-console-linux.md).
* Obtenga más información sobre [diagnósticos de arranque](boot-diagnostics.md).
