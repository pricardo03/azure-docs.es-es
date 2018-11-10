---
title: Solución de problemas de errores de instalación de inserción del servicio de movilidad durante la habilitación de la replicación para recuperación ante desastres	 | Microsoft Docs
description: Solución de problemas de errores de instalación del servicio de movilidad durante la habilitación de la replicación para recuperación ante desastres
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.author: ramamill
ms.date: 10/29/2018
ms.openlocfilehash: c12683fee7022b84e097c2b71628776271611429
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50210999"
---
# <a name="troubleshoot-mobility-service-push-installation-issues"></a>Solución de problemas de instalación de inserción de Mobility Service

La instalación del servicio de movilidad es un paso clave durante la habilitación de la replicación. Para realizar este paso correctamente, simplemente se deben cumplir los requisitos previos y usar las configuraciones admitidas. Los errores más comunes que aparecen durante la instalación del servicio de movilidad se deben a:

* Errores de credenciales/privilegios
* Errores de conectividad
* Sistemas operativos no admitidos

Al habilitar la replicación, Azure Site Recovery intenta insertar el agente de instalación del servicio de movilidad en su máquina virtual. Como parte de esto, el servidor de configuración intenta conectarse con la máquina virtual y copiar el agente. Para permitir que la instalación se realice correctamente, siga las instrucciones de solución de problemas detalladas que se indican a continuación.

## <a name="credentials-check-errorid-95107--95108"></a>Comprobación de credenciales (ErrorID: 95107 y 95108)

* Compruebe si la cuenta de usuario elegida durante la habilitación de la replicación es **válida y precisa**.
* Azure Site Recovery necesita **privilegios de administrador** para realizar la instalación de inserción.
  * Para Windows, compruebe si la cuenta de usuario tiene acceso administrativo, ya sea local o de dominio, en la máquina de origen.
  * Si no utiliza ninguna cuenta de dominio, deberá deshabilitar el control de acceso del usuario remoto en el equipo local.
    * Para ello, en la clave del Registro HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System, agregue un nuevo valor de DWORD: LocalAccountTokenFilterPolicy. Establezca el valor en 1. Para realizar el siguiente paso, ejecute el siguiente comando desde el símbolo del sistema:

         `REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`
  * Para Linux, debe elegir la cuenta raíz a fin de instalar correctamente el agente de movilidad.

Si quiere modificar las credenciales de la cuenta de usuario elegida, siga las instrucciones que se indican [aquí](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation).

## <a name="connectivity-check-errorid-95117--97118"></a>**Comprobación de conectividad (ErrorID: 95117 y 97118)**

* Asegúrese de que puede hacer ping a la máquina de origen desde el servidor de configuración. Si ha elegido el servidor de procesos de escalabilidad horizontal durante la habilitación de la replicación, asegúrese de que puede hacer ping a la máquina de origen desde el servidor de procesos.
  * Desde la línea de comandos de la máquina del servidor de origen, utilice Telnet para hacer ping al servidor de configuración/servidor de procesos de escalabilidad horizontal con el puerto HTTPS (el valor predeterminado es 9443), tal y como se muestra a continuación para ver si hay problemas de conectividad de red o problemas de bloqueo de puertos en el firewall.

     `telnet <CS/ scale-out PS IP address> <port>`

  * Si no puede conectarse, permita el puerto de entrada 9443 en el servidor de procesos de escalabilidad horizontal/servidor de configuración.
  * Compruebe el estado del servicio **InMage Scout VX Agent-Sentinel/Outpost**. Inicie el servicio, si no está en ejecución.

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

## <a name="file-and-printer-sharing-services-check-errorid-95105--95106"></a>Comprobación de servicios de uso compartido de archivos e impresoras (ErrorID: 95105 y 95106)

Después de la comprobación de conectividad, compruebe si está habilitado el servicio de uso compartido de archivos e impresoras en la máquina virtual.

Para **Windows 2008 R2 y versiones anteriores**,

* Para habilitar el uso compartido de archivos e impresoras a través del Firewall de Windows,
  * Abra el Panel de control -> Sistema y seguridad -> Firewall de Windows -> en el panel izquierdo, haga clic en Configuración avanzada -> haga clic en Reglas de entrada en el árbol de consola.
  * Busque las reglas de uso compartido de archivos e impresoras (sesión NB de entrada) y uso compartido de archivos e impresoras (SMB de entrada). Para cada regla, haga clic con el botón derecho en la regla y, a continuación, haga clic en **Habilitar regla**.
* Para habilitar el uso compartido de archivos con directiva de grupo:
  * Vaya a Inicio, escriba gpmc.msc y busque.
  * En el panel de navegación, abra las siguientes carpetas: Directiva de equipo local, Configuración de usuario, Plantillas administrativas, Componentes de Windows y Uso compartido de red.
  * En el panel de detalles, haga doble clic en **Prevent users from sharing files within their profile** (Evitar que los usuarios compartan archivos con su perfil). Para deshabilitar la configuración de directiva de grupo y habilitar la capacidad del usuario para compartir archivos, haga clic en Deshabilitado. Haga clic en Aceptar para guardar los cambios. Para obtener más información, haga clic [aquí](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754359(v=ws.10)).

Para **versiones posteriores**, siga las instrucciones proporcionadas [aquí](vmware-azure-install-mobility-service.md#install-mobility-service-by-push-installation-from-azure-site-recovery) a fin de habilitar el uso compartido de archivos e impresoras.

## <a name="windows-management-instrumentation-wmi-configuration-check"></a>Comprobación de la configuración de Instrumental de administración de Windows (WMI).

Después de la comprobación de los servicios de archivos e impresoras, habilite el servicio WMI a través del firewall.

* En el Panel de Control, haga clic en Seguridad y, a continuación, haga clic en Firewall de Windows.
* Haga clic en Cambiar configuración y, a continuación, haga clic en la pestaña Excepciones.
* En la ventana Excepciones, seleccione la casilla de verificación para Instrumental de administración de Windows (WMI) para permitir el tráfico de WMI a través del firewall. 

También puede habilitar el tráfico de WMI a través del firewall en el símbolo del sistema. Use el comando siguiente `netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=yes`
Se pueden encontrar otros artículos de solución de problemas de WMI en los siguientes artículos:

* [Basic WMI Testing](https://blogs.technet.microsoft.com/askperf/2007/06/22/basic-wmi-testing/) (Pruebas básicas de WMI)
* [WMI Troubleshooting](https://msdn.microsoft.com/library/aa394603(v=vs.85).aspx) (Solución de problemas de WMI)
* [Troubleshooting problems with WMI scripts and WMI Services](https://technet.microsoft.com/library/ff406382.aspx#H22) (Solución de problemas con los scripts y servicios de WMI)

## <a name="unsupported-operating-systems"></a>Sistemas operativos no admitidos

Otra causa bastante común de error puede deberse a un sistema operativo no admitido. Asegúrese de que está utilizando la versión de kernel o de sistema operativo compatible para conseguir una instalación correcta del servicio de movilidad.

Para obtener información sobre qué sistemas operativos son compatibles con Azure Site Recovery, consulte nuestro [documento de la matriz de soporte técnico](vmware-physical-azure-support-matrix.md#replicated-machines).

## <a name="next-steps"></a>Pasos siguientes

[Aprenda cómo](vmware-azure-tutorial.md) configurar la recuperación ante desastres para máquinas virtuales de VMware.