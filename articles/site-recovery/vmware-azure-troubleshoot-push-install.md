---
title: Solución de problemas de instalación de inserción de Mobility Service con Azure Site Recovery
description: Solución de problemas de errores de instalación del servicio de movilidad durante la habilitación de la replicación para recuperación ante desastres con Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.author: ramamill
ms.date: 09/11/2019
ms.openlocfilehash: 3646499ad2104566cb82f3f26c6b55d05f84dc7d
ms.sourcegitcommit: 44c2a964fb8521f9961928f6f7457ae3ed362694
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2019
ms.locfileid: "73953774"
---
# <a name="troubleshoot-mobility-service-push-installation"></a>Solución de problemas de instalación de inserción de Mobility Service 

La instalación del servicio de movilidad es un paso clave durante la habilitación de la replicación. Para realizar este paso correctamente, simplemente se deben cumplir los requisitos previos y usar las configuraciones admitidas. Los errores más comunes que aparecen durante la instalación del servicio de movilidad se deben a:

* [Errores de credenciales/privilegios](#credentials-check-errorid-95107--95108)
* [Errores de inicio de sesión](#login-failures-errorid-95519-95520-95521-95522)
* [Errores de conectividad](#connectivity-failure-errorid-95117--97118)
* [Errores con el uso compartido de archivos e impresoras](#file-and-printer-sharing-services-check-errorid-95105--95106)
* [Errores de WMI](#windows-management-instrumentation-wmi-configuration-check-error-code-95103)
* [Sistemas operativos no admitidos](#unsupported-operating-systems)
* [Configuraciones de aplicaciones no admitidas](#unsupported-boot-disk-configurations-errorid-95309-95310-95311)
* [Errores de instalación de VSS](#vss-installation-failures)
* [Uso del nombre del dispositivo en la configuración de GRUB en lugar del UUID del dispositivo](#enable-protection-failed-as-device-name-mentioned-in-the-grub-configuration-instead-of-uuid-errorid-95320)
* [Volumen LVM](#lvm-support-from-920-version)
* [Advertencias de reinicio](#install-mobility-service-completed-with-warning-to-reboot-errorid-95265--95266)

Al habilitar la replicación, Azure Site Recovery intenta insertar el agente de instalación del servicio de movilidad en su máquina virtual. Como parte de esto, el servidor de configuración intenta conectarse con la máquina virtual y copiar el agente. Para permitir que la instalación se realice correctamente, siga las instrucciones de solución de problemas detalladas que se indican a continuación.

## <a name="credentials-check-errorid-95107--95108"></a>Comprobación de credenciales (ErrorID: 95107 & 95108)

* Compruebe si la cuenta de usuario elegida durante la habilitación de la replicación es **válida y precisa**.
* Azure Site Recovery necesita una cuenta **RAÍZ** o una cuenta de usuario con **privilegios de administrador** para realizar la instalación de inserción. En caso contrario, dicha instalación se bloqueará en la máquina de origen.
  * Para Windows (**error 95107**), compruebe si la cuenta de usuario tiene acceso administrativo, ya sea local o de dominio, en la máquina de origen.
  * Si no utiliza ninguna cuenta de dominio, deberá deshabilitar el control de acceso del usuario remoto en el equipo local.
    * Para ello, en la clave del Registro HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System, agregue un nuevo valor de DWORD: LocalAccountTokenFilterPolicy. Establezca el valor en 1. Para realizar el siguiente paso, ejecute el siguiente comando desde el símbolo del sistema:

         `REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`
  * Para Linux (**error 95108**), debe elegir la cuenta raíz a fin de instalar correctamente el agente de movilidad. Además, los servicios SFTP deben estar en ejecución. Para habilitar la autenticación de la contraseña y el subsistema SFTP en el archivo sshd_config:
    1. Inicie sesión como root.
    2. Vaya al archivo /etc/ssh/sshd_config y busque la línea que comienza por PasswordAuthentication.
    3. Quite la marca de comentario de la línea y cambie el valor a yes.
    4. Busque la línea que comienza por Subsystem y quite la marca de comentario.
    5. Reinicie el servicio sshd.

Si quiere modificar las credenciales de la cuenta de usuario elegida, siga las instrucciones que se indican [aquí](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation).

## <a name="insufficient-privileges-failure-errorid-95517"></a>Error de privilegios insuficientes (ErrorID: 95517)

Cuando el usuario que opta por instalar el agente de movilidad no tiene privilegios de administrador, el servidor de configuración o el servidor del proceso de escalabilidad horizontal no podrá copiar el software de agente de movilidad en la máquina de origen. Por lo tanto, este error es el resultado de un error de acceso denegado. Asegúrese de que la cuenta de usuario tiene privilegios de administrador.

Si quiere modificar las credenciales de la cuenta de usuario elegida, siga las instrucciones que se indican [aquí](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation).

## <a name="insufficient-privileges-failure-errorid-95518"></a>Error de privilegios insuficientes (ErrorID: 95518)

Cuando se produce un error al establecer una relación de confianza de dominio entre el dominio principal y la estación de trabajo al intentar iniciar sesión en la máquina de origen, se produce un error en la instalación del agente de movilidad con el id. de error 95518. Por lo tanto, asegúrese de que la cuenta de usuario usada para instalar al agente de movilidad tiene privilegios administrativos para iniciar sesión a través del dominio principal de la máquina de origen.

Si quiere modificar las credenciales de la cuenta de usuario elegida, siga las instrucciones que se indican [aquí](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation).

## <a name="login-failures-errorid-95519-95520-95521-95522"></a>Error de inicio de sesión (ErrorID: 95519, 95520, 95521 y 95522)

### <a name="credentials-of-the-user-account-have-been-disabled-errorid-95519"></a>Las credenciales de la cuenta de usuario se han deshabilitado (ErrorID: 95519)

La cuenta de usuario elegida durante la habilitación de la replicación se ha deshabilitado. Para habilitar la cuenta de usuario, consulte el artículo [aquí](https://aka.ms/enable_login_user) o ejecute el siguiente comando reemplazando el texto *username* con el nombre de usuario real.
`net user 'username' /active:yes`

### <a name="credentials-locked-out-due-to-multiple-failed-login-attempts-errorid-95520"></a>Se han bloqueado las credenciales después de varios intentos de inicio de sesión fracasados (ErrorID: 95520)

Varios esfuerzos de reintento erróneos para obtener acceso a una máquina bloquearán la cuenta de usuario. El error puede deberse a alguno de los siguientes motivos:

* Las credenciales proporcionadas durante la configuración son incorrectas O
* La cuenta de usuario elegida durante la habilitación de la replicación es incorrecta.

Por lo tanto, modifique las credenciales elegidas siguiendo las instrucciones indicadas [aquí](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation) y reintente la operación más tarde.

### <a name="logon-servers-are-not-available-on-the-source-machine-errorid-95521"></a>Los servidores de inicio de sesión no están disponibles en la máquina de origen (ErrorID: 95521)

Este error se produce cuando los servidores de inicio de sesión no están disponibles en la máquina de origen. La falta de disponibilidad de los servidores de inicio de sesión dará lugar a errores de solicitud de inicio de sesión y, por tanto, no se podrá instalar el agente de movilidad. Para iniciar sesión correctamente, asegúrese de que los servidores de inicio de sesión estén disponibles en la máquina de origen e inicie el servicio de inicio de sesión. Para obtener instrucciones detalladas, consulte KB [139410](https://support.microsoft.com/en-in/help/139410/err-msg-there-are-currently-no-logon-servers-available) Mensaje de error: There are Currently No Logon Servers Available (Actualmente no hay servidores de inicio de sesión disponibles).

### <a name="logon-service-isnt-running-on-the-source-machine-errorid-95522"></a>Los servidores de inicio de sesión no se están ejecutando en la máquina de origen (ErrorID: 95522)

El servicio de inicio de sesión no se ejecuta en la máquina de origen y produce un error de solicitud de inicio de sesión. Por lo tanto, no se puede instalar el agente de movilidad. Para resolver este problema, asegúrese de que el servicio de inicio de sesión se ejecute en la máquina de origen para iniciar sesión correctamente. Para iniciar el servicio de inicio de sesión, ejecute el comando "net start Logon" desde el símbolo del sistema o inicie el servicio "NetLogon" desde el administrador de tareas.

## <a name="connectivity-failure-errorid-95117--97118"></a>**Error de conectividad (ErrorID: 95117 y 97118)**

El servidor de configuración/servidor de procesos de escalabilidad horizontal intenta conectarse a la VM de origen para instalar el agente de movilidad. Este error se produce cuando la máquina de origen no es accesible debido a problemas de conectividad de red. Para resolver este problema:

* Asegúrese de que puede hacer ping a la máquina de origen desde el servidor de configuración. Si ha elegido el servidor de procesos de escalabilidad horizontal durante la habilitación de la replicación, asegúrese de que puede hacer ping a la máquina de origen desde el servidor de procesos.
  * Desde la línea de comandos de la máquina del servidor de origen, utilice Telnet para hacer ping al servidor de configuración/servidor de procesos de escalabilidad horizontal con el puerto HTTPS (135), tal y como se muestra a continuación, para ver si hay problemas de conectividad de red o problemas de bloqueo de puertos en el firewall.

     `telnet <CS/ scale-out PS IP address> <135>`
* Además, para **VM de Linux**,
  * Compruebe que los paquetes openssh, openssh-server y openssl más recientes estén instalados.
  * Compruebe y asegúrese de que Secure Shell (SSH) está habilitado y se ejecuta en el puerto 22.
  * Los servicios SFTP deben estar en ejecución. Para habilitar la autenticación de la contraseña y el subsistema SFTP en el archivo sshd_config:
    * Inicie sesión como root.
    * Vaya al archivo /etc/ssh/sshd_config y busque la línea que comienza por PasswordAuthentication.
    * Quite la marca de comentario de la línea y cambie el valor a yes.
    * Busque la línea que comienza por Subsystem y quite la marca de comentario.
    * Reinicie el servicio sshd.
* Es posible que se haya producido un error en el intento de conexión si no hay ninguna respuesta correcta tras un período de tiempo o se produjo un error en la conexión establecida porque el host conectado no ha podido responder.
* Puede ser un problema relacionado con la conectividad, la red o el dominio. También podría ser debido a un problema de resolución del nombre de DNS o un problema de agotamiento del puerto TCP. Compruebe si hay algún problema conocido en el dominio.

## <a name="connectivity-failure-errorid-95523"></a>Error de conectividad (ErrorID: 95523)

Este error se produce cuando la red en la que reside la máquina de origen no se encuentra o puede que se haya eliminado o ya no está disponible. La única manera de resolver el error es asegurarse de que existe la red.

## <a name="file-and-printer-sharing-services-check-errorid-95105--95106"></a>Comprobación de servicios de uso compartido de archivos e impresoras (ErrorID: 95105 y 95106)

Después de la comprobación de conectividad, compruebe si está habilitado el servicio de uso compartido de archivos e impresoras en la máquina virtual. Esta configuración es necesaria para copiar el agente de movilidad en la máquina de origen.

Para **Windows 2008 R2 y versiones anteriores**,

* Para habilitar el uso compartido de archivos e impresoras a través del Firewall de Windows,
  * Abra el Panel de control -> Sistema y seguridad -> Firewall de Windows -> en el panel izquierdo, haga clic en Configuración avanzada -> haga clic en Reglas de entrada en el árbol de consola.
  * Busque las reglas de uso compartido de archivos e impresoras (sesión NB de entrada) y uso compartido de archivos e impresoras (SMB de entrada). Para cada regla, haga clic con el botón derecho en la regla y, a continuación, haga clic en **Habilitar regla**.
* Para habilitar el uso compartido de archivos con directiva de grupo:
  * Vaya a Inicio, escriba gpmc.msc y busque.
  * En el panel de navegación, abra las siguientes carpetas: Directiva de equipo local, Configuración de usuario, Plantillas administrativas, Componentes de Windows y Uso compartido de red.
  * En el panel de detalles, haga doble clic en **Prevent users from sharing files within their profile** (Evitar que los usuarios compartan archivos con su perfil). Para deshabilitar la configuración de directiva de grupo y habilitar la capacidad del usuario para compartir archivos, haga clic en Deshabilitado. Haga clic en Aceptar para guardar los cambios. Para más información, consulte [Enable or disable File sharing with Group Policy](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754359(v=ws.10)) (Habilitar o deshabilitar el uso compartido de archivos con directiva de grupo).

Para **versiones posteriores**, siga las instrucciones proporcionadas en [Instalación de Mobility Service para la recuperación ante desastres de VM de VMware y servidores físicos](vmware-azure-install-mobility-service.md) para el habilitar uso compartido de impresoras y archivos.

## <a name="windows-management-instrumentation-wmi-configuration-check-error-code-95103"></a>Comprobación de la configuración de Instrumental de administración de Windows (WMI) (Código de error: 95103)

Después de la comprobación de los servicios de archivos e impresoras, habilite el servicio WMI para perfiles privados, públicos y de dominio a través del firewall. Esta configuración es necesaria para completar la ejecución remota en la máquina de origen. Para habilitarla:

* Vaya al Panel de Control, haga clic en Seguridad y, a continuación, haga clic en Firewall de Windows.
* Haga clic en Cambiar configuración y, a continuación, haga clic en la pestaña Excepciones.
* En la ventana Excepciones, seleccione la casilla de verificación para Instrumental de administración de Windows (WMI) para permitir el tráfico de WMI a través del firewall. 

También puede habilitar el tráfico de WMI a través del firewall en el símbolo del sistema. Use el comando siguiente `netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=yes`
Se pueden encontrar otros artículos de solución de problemas de WMI en los siguientes artículos:

* [Basic WMI Testing](https://blogs.technet.microsoft.com/askperf/2007/06/22/basic-wmi-testing/) (Pruebas básicas de WMI)
* [WMI Troubleshooting](https://msdn.microsoft.com/library/aa394603(v=vs.85).aspx) (Solución de problemas de WMI)
* [Troubleshooting problems with WMI scripts and WMI Services](https://technet.microsoft.com/library/ff406382.aspx#H22) (Solución de problemas con los scripts y servicios de WMI)

## <a name="unsupported-operating-systems"></a>Sistemas operativos no admitidos

Otra causa bastante común de error puede deberse a un sistema operativo no admitido. Asegúrese de que está utilizando la versión de kernel o de sistema operativo compatible para conseguir una instalación correcta del servicio de movilidad. Evite el uso de revisiones privadas.
Para ver la lista de sistemas operativos y versiones de kernel compatibles con Azure Site Recovery, consulte nuestro [documento de la matriz de soporte técnico](vmware-physical-azure-support-matrix.md#replicated-machines).

## <a name="unsupported-boot-disk-configurations-errorid-95309-95310-95311"></a>Las configuraciones del disco de arranque no son compatibles (ErrorID: 95309, 95310 y 95311)

### <a name="boot-and-system-partitions--volumes-are-not-the-same-disk-errorid-95309"></a>Los volúmenes o las particiones de arranque y del sistema no están en el mismo disco (ErrorID: 95309)

Antes de la versión 9.20, la ubicación de los volúmenes o las particiones de arranque y del sistema en discos diferentes era una configuración no compatible. Desde la [versión 9.20](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery), esta configuración es compatible. Use la versión más reciente para esta compatibilidad.

### <a name="the-boot-disk-is-not-available-errorid-95310"></a>El disco de arranque no está disponible (ErrorID: 95310)

Una máquina virtual sin disco de arranque no se puede proteger. Esto garantiza la correcta recuperación de la máquina virtual durante una operación de conmutación por error. La ausencia del disco de arranque genera un error al arrancar la máquina después de la conmutación por error. Asegúrese de que la máquina virtual contiene un disco de arranque e intente realizar la operación de nuevo. Asimismo, tenga en cuenta que no se admiten varios discos de arranque en la misma máquina.

### <a name="multiple-boot-disks-present-on-the-source-machine-errorid-95311"></a>Hay varias discos de arranque en la máquina de origen (ErrorID: 95311)

Una máquina virtual con varios discos de arranque no es una [configuración compatible](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage).

## <a name="system-partition-on-multiple-disks-errorid-95313"></a>Partición del sistema en varios discos (ErrorID: 95313)

Antes de la versión 9.20, el volumen o partición raíz en varios discos era una configuración no compatible. Desde la [versión 9.20](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery), esta configuración es compatible. Use la versión más reciente para esta compatibilidad.

## <a name="enable-protection-failed-as-device-name-mentioned-in-the-grub-configuration-instead-of-uuid-errorid-95320"></a>No se pudo habilitar la protección debido a que se ha mencionado el nombre del dispositivo en la configuración de GRUB en lugar del UUID (ErrorID: 95320)

**Causa posible:** </br>
Los archivos de configuración de GRUB ("/boot/grub/menu.lst", "/boot/grub/grub.cfg", "/boot/grub2/grub.cfg" o "/etc/default/grub") pueden contener el valor de los parámetros **root** y **resume** como nombres reales del dispositivo, en lugar del UUID. Site Recovery exige el enfoque de UUID, ya que el nombre de los dispositivos puede cambiar al reiniciar la VM, debido a que la VM puede no mostrar el mismo nombre tras la conmutación por error, lo que puede generar problemas. Por ejemplo: </br>


- La línea siguiente procede del archivo GRUB **/boot/grub2/grub.cfg**. <br>
  *linux   /boot/vmlinuz-3.12.49-11-default **root=/dev/sda2**  ${extra_cmdline} **resume=/dev/sda1** splash=silent quiet showopts*


- La siguiente línea es del archivo GRUB **/boot/grub/menu.lst**
  *kernel /boot/vmlinuz-3.0.101-63-default **root=/dev/sda2** **resume=/dev/sda1** splash=silent crashkernel=256M-:128M showopts vga=0x314*

Si ve la cadena en negrita anterior, GRUB tiene nombres de dispositivos reales para los parámetros "root" y "resume", en lugar del UUID.
 
**Solución:**<br>
Los nombres de dispositivo se deben reemplazar con el UUID correspondiente.<br>


1. Para buscar el UUID del dispositivo, ejecute el comando "blkid \<nombre de dispositivo>". Por ejemplo:<br>
   ```
   blkid /dev/sda1
   /dev/sda1: UUID="6f614b44-433b-431b-9ca1-4dd2f6f74f6b" TYPE="swap"
   blkid /dev/sda2 
   /dev/sda2: UUID="62927e85-f7ba-40bc-9993-cc1feeb191e4" TYPE="ext3" 
   ```

2. Ahora, sustituya el nombre del dispositivo por su UUID con el formato "root=UUID=\<UUID>". Por ejemplo, si reemplazamos los nombres de dispositivo por el UUID de root y reanudamos el parámetro mencionado anteriormente en los archivos "/boot/grub2/grub.cfg", "/boot/grub2/grub.cfg" o "/etc/default/grub:", las líneas de los archivos serán parecidas a las siguientes: <br>
   *kernel /boot/vmlinuz-3.0.101-63-default **root=UUID=62927e85-f7ba-40bc-9993-cc1feeb191e4** **resume=UUID=6f614b44-433b-431b-9ca1-4dd2f6f74f6b** splash=silent crashkernel=256M-:128M showopts vga=0x314*
3. Reinicie la protección

## <a name="install-mobility-service-completed-with-warning-to-reboot-errorid-95265--95266"></a>La instalación de Mobility Service se ha completado con una advertencia de reinicio (ErrorID: 95265 y 95266)

El servicio de movilidad de Site Recovery tiene muchos componentes, uno de los cuales es el controlador del filtro. El controlador del filtro solamente se carga en el sistema cuando se reinicia el sistema. Esto significa que las correcciones del controlador del filtro solo pueden aplicarse cuando se carga un nuevo controlador del filtro, lo que solo ocurre cuando se reinicia el sistema.

**Tenga en cuenta** que se trata de una advertencia y que la replicación existente funcionará incluso después de que se realice la nueva actualización del agente. Puede reiniciar el sistema en cualquier momento para aprovechar las ventajas del nuevo controlador del filtro, pero, si no reinicia, el antiguo controlador seguirá funcionando. De este modo, si actualiza sin reiniciar, salvo el controlador del filtro, **se aplicarán todas las demás mejoras y correcciones del servicio de movilidad**. Por tanto, aunque resulte conveniente, no es obligatorio reiniciar el sistema después de cada actualización. Para información sobre cuándo un reinicio es obligatorio, establezca la sección [Reinicio de la máquina de origen después de actualizar el agente de movilidad](https://aka.ms/v2a_asr_reboot) en Actualizaciones del servicio en Azure Site Recovery.

> [!TIP]
>Para ver los procedimientos recomendados sobre la programación de actualizaciones durante la ventana de mantenimiento, consulte [Soporte técnico para versiones de kernel o del sistema operativo más recientes](https://aka.ms/v2a_asr_upgrade_practice) en Actualizaciones del servicio en Azure Site Recovery.

## <a name="lvm-support-from-920-version"></a>Compatibilidad con LVM desde la versión 9.20

Antes de la versión 9.20, LVM solo era compatible con discos de datos. /boot debe estar en una partición de disco y no estar en un volumen LVM.

Desde la [versión 9.20](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery), se admite el [disco del SO en LVM](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage). Use la versión más reciente para esta compatibilidad.

## <a name="insufficient-space-errorid-95524"></a>Espacio insuficiente (ErrorID: 95524)

Cuando el agente de movilidad se copia en la máquina de origen, es necesario tener 100 MB de espacio disponible por lo menos. Por lo tanto, asegúrese de que la máquina de origen tenga el espacio disponible requerido y reintente la operación.

## <a name="vss-installation-failures"></a>Errores de instalación de VSS

La instalación de VSS es una parte de la instalación del agente de movilidad. Este servicio se usa en el proceso de generar puntos de recuperación coherentes con la aplicación. Pueden producirse errores durante la instalación de VSS debido a varios motivos. Para identificar los errores exactos, consulte **c:\ProgramData\ASRSetupLogs\ASRUnifiedAgentInstaller.log**. En la siguiente sección se destacan algunos errores comunes y los pasos para solucionarlos.

### <a name="vss-error--2147023170-0x800706be---exit-code-511"></a>Error de VSS -2147023170 [0x800706BE]: código de salida 511

Este problema se presenta principalmente cuando software antivirus bloquea las operaciones de servicios de Azure Site Recovery. Para resolver este problema:

1. Excluya todas las carpetas mencionadas [aquí](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program).
2. Siga las instrucciones publicadas por su proveedor de antivirus para desbloquear el registro de DLL en Windows.

### <a name="vss-error-7-0x7---exit-code-511"></a>Error de VSS 7 [0 x 7]: código de salida 511

Se trata de un error en tiempo de ejecución y se produce debido a no hay memoria suficiente para instalar VSS. Asegúrese de aumentar el espacio en disco para la correcta finalización de esta operación.

### <a name="vss-error--2147023824-0x80070430---exit-code-517"></a>Error de VSS -2147023824 [0x80070430]: código de salida 517

Este error se produce cuando el servicio de proveedor de VSS para Azure Site Recovery está [marcado para su eliminación](https://msdn.microsoft.com/library/ms838153.aspx). Pruebe a instalar VSS manualmente en la máquina de origen mediante la ejecución de la siguiente línea de comandos.

`C:\Program Files (x86)\Microsoft Azure Site Recovery\agent>"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

### <a name="vss-error--2147023841-0x8007041f---exit-code-512"></a>Error de VSS -2147023841 [0x8007041F]: código de salida 512

Este error se produce cuando la base de datos de servicio de proveedor de VSS para Azure Site Recovery está [bloqueada](https://msdn.microsoft.com/library/ms833798.aspx). Pruebe a instalar VSS manualmente en la máquina de origen mediante la ejecución de la siguiente línea de comandos.

`C:\Program Files (x86)\Microsoft Azure Site Recovery\agent>"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

En caso de error, compruebe si algún programa antivirus u otros servicios están bloqueados en el estado "Iniciando". Esto podría mantener el bloqueo en los servicios de base de datos. Además, dará lugar a errores en la instalación del proveedor de VSS. Asegúrese de que ningún servicio se encuentra en un estado "Iniciando" y luego vuelva a intentar la operación anterior.

### <a name="vss-exit-code-806"></a>Código de salida VSS 806

Este error se produce cuando la cuenta de usuario usada para la instalación no tiene permisos para ejecutar el comando de CSScript. Proporcione los permisos necesarios para que la cuenta de usuario ejecute el script y vuelva a intentar la operación.

### <a name="other-vss-errors"></a>Otros errores de VSS

Pruebe a instalar el servicio de proveedor de VSS manualmente en la máquina de origen mediante la ejecución de la siguiente línea de comandos

`C:\Program Files (x86)\Microsoft Azure Site Recovery\agent>"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`



## <a name="vss-error---0x8004e00f"></a>Error de VSS 0x8004E00F

Este error normalmente se encuentra durante la instalación del agente de movilidad debido a problemas en DCOM, y DCOM está en estado crítico.

Siga el procedimiento a continuación para determinar la causa del error.

**Examinar los registros de instalación**

1. Abra el registro de instalación ubicado en c:\ProgramData\ASRSetupLogs\ASRUnifiedAgentInstaller.log.
2. La presencia del siguiente error indica este problema:

    Unregistering the existing application...  Create the catalogue object  Get the collection of Applications (No se pudo anular el registro del tipo... Cree el objeto del catálogo Obtenga la colección de aplicaciones) 

    ERROR:

    - Código de error:-2147164145 [0x8004E00F]
    - Código de salida: 802

Para resolver el problema:

Póngase en contacto con el [equipo de la plataforma de Microsoft Windows](https://aka.ms/Windows_Support) para obtener ayuda para resolver el problema de DCOM.

Cuando se resuelva el problema de DCOM, vuelva a instalar el proveedor de VSS de Azure Site Recovery manualmente mediante el comando siguiente:
 
**C:\Program Files (x86)\Microsoft Azure Site Recovery\agent>"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd**
  
Si la coherencia de la aplicación no es fundamental para sus requisitos de recuperación ante desastres, puede omitir la instalación del proveedor de VSS. 

Para omitir la instalación del proveedor de VSS de Azure Site Recovery e instalar manualmente el proveedor de VSS de Azure Site Recovery después de la instalación:

1. Instale Mobility Service. 
   > [!Note]
   > 
   > La instalación producirá un error en el paso de "configuración posterior a la instalación". 
2. Para omitir la instalación de VSS:
   1. Abra el directorio de instalación de Mobility Service de Azure Site Recovery ubicado en:
   
      C:\Archivos de programa (x86)\Microsoft Azure Site Recovery\agent
   2. Modifique los scripts de instalación del proveedor de VSS de Azure Site Recovery **nMageVSSProvider_Install** y **InMageVSSProvider_Uninstall.cmd** para que siempre sean correctos mediante la adición de las líneas siguientes:
    
      ```     
      rem @echo off
      setlocal
      exit /B 0
      ```

3. Vuelva a ejecutar manualmente la instalación del agente de movilidad. 
4. Cuando la instalación se realice correctamente y se mueva al siguiente paso, **Configurar**, quite las líneas que agregó.
5. Para instalar el proveedor de VSS, abra un símbolo del sistema como administrador y ejecute los siguientes comandos:
   
    **C:\Archivos de programa (x86)\Microsoft Azure Site Recovery\agent> .\InMageVSSProvider_Install.cmd**

9. Verifique que el proveedor de VSS de ASR esté instalado como servicio en los Servicios de Windows y, abra la MMC de servicios de componentes para verificar que el proveedor de VSS de ASR figure en la lista.
10. Si la instalación del proveedor de VSS sigue dando errores, trabaje con CX para resolver los errores de permisos en CAPI2.

## <a name="vss-provider-installation-fails-due-to-the-cluster-service-being-enabled-on-non-cluster-machine"></a>No se puede instalar el proveedor de VSS porque el servicio de clúster está habilitando en una máquina sin clústeres

Este problema hace que no se pueda instalar el agente de movilidad de Azure Site Recovery durante el paso de instalación del proveedor de VSS de Azure Site Recovery debido a un problema con COM+ que impide la instalación del proveedor de VSS.
 
### <a name="to-identify-the-issue"></a>Para identificar el problema

En el registro que se encuentra en el servidor de configuración en C:\ProgramData\ASRSetupLogs\UploadedLogs\<fecha-hora>UA_InstallLogFile.log, encontrará la siguiente excepción:

COM+ no pudo realizar la conexión con el Coordinador de transacciones distribuidas de Microsoft (HRESULT: 0x8004E00F)

Para resolver el problema:

1.  Verifique que esta máquina sea una máquina sin clústeres y que los componentes del clúster no se usen.
3.  Si no se usan los componentes, quite los componentes del clúster de la máquina.

## <a name="drivers-are-missing-on-the-source-server"></a>Faltan controladores en el servidor de origen

Si no puede instalar el agente de movilidad, examine los registros de C:\ProgramData\ASRSetupLogs para determinar si faltan algunos de los controladores necesarios en algunos conjuntos de control.
 
Para resolver el problema:
  
1. Con un editor del Registro, como regedit.msc, abra el Registro.
2. Abra el nodo HKEY_LOCAL_MACHINE\SYSTEM.
3. En el nodo SYSTEM, busque los conjuntos de control.
4. Abra cada conjunto de control y verifique que los siguientes controladores de Windows estén presentes:

   - Atapi
   - Vmbus
   - Storflt
   - Storvsc
   - intelide
 
Vuelva a instalar los controladores que falten.

## <a name="next-steps"></a>Pasos siguientes

[Aprenda cómo](vmware-azure-tutorial.md) configurar la recuperación ante desastres para máquinas virtuales de VMware.
