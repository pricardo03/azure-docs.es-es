---
title: Solución de problemas con el servidor de configuración durante la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure con Azure Site Recovery | Microsoft Docs
description: En este artículo se proporciona información para solucionar problemas de la implementación del servidor de configuración para la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure con Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 01/14/2019
ms.author: ramamill
ms.openlocfilehash: 0eebfd8b75f428d3b8f6024ed6ee71c18c1309f6
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54435981"
---
# <a name="troubleshoot-configuration-server-issues"></a>Solución de problemas del servidor de configuración

En este artículo se le ayudará a solucionar problemas al implementar y administrar el servidor de configuración de [Azure Site Recovery](site-recovery-overview.md). El servidor de configuración actúa como servidor de administración. Use el servidor de configuración para configurar la recuperación ante desastres de VM de VMware y servidores físicos locales en Azure con Site Recovery. En las siguientes secciones se habla de los errores más habituales que podría experimentar al agregar un nuevo servidor de configuración y al administrar un servidor de configuración.

## <a name="registration-failures"></a>Errores de registro

La máquina de origen se registra con el servidor de configuración al instalar el agente de movilidad. Puede depurar los errores durante este paso siguiendo estas directrices:

1. Abra el archivo C:\ProgramData\ASR\home\svsystems\var\configurator_register_host_static_info.log  (la carpeta ProgramData podría ser una carpeta oculta. Si no ve la carpeta ProgramData, en el Explorador de archivos, en la pestaña **Ver**, en la sección **Mostrar/ocultar**, active la casilla **Elementos ocultos**). Los errores podrían deberse a varios problemas.

2. Busque la cadena **No se encuentra ninguna dirección IP válida**. Si se encuentra la cadena:
    1. Compruebe que el identificador de host solicitado es el mismo que el identificador de host de la máquina de origen.
    2. Compruebe que la máquina de origen tiene al menos una dirección IP asignada a la NIC física. Para que el registro del agente con el servidor de configuración se realice correctamente, la máquina de origen debe tener al menos una dirección IPv4 asignada a la NIC física.
    3. Ejecute uno de los siguientes comandos en la máquina de origen para obtener todas las direcciones IP de la máquina de origen:
      - En Windows: `> ipconfig /all`
      - En Linux: `# ifconfig -a`

3. Si no se encuentra la cadena **No se encuentra ninguna dirección IP válida** , busque la cadena **Motivo => NULL**. Se produce este error si la máquina de origen utiliza un host vacío para registrarse con el servidor de configuración. Si se encuentra la cadena:
    - Tras resolver los problemas, siga las directrices en [Registrar la máquina de origen con el servidor de configuración](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server) para volver a intentar el registro manualmente.

4. Si no se encuentra la cadena **Motivo => NULL**, en la máquina de origen, abra el archivo C:\ProgramData\ASRSetupLogs\UploadedLogs\ASRUnifiedAgentInstaller.log  (la carpeta ProgramData podría ser una carpeta oculta. Si no ve la carpeta ProgramData, en el Explorador de archivos, en la pestaña **Ver**, en la sección **Mostrar/ocultar**, active la casilla **Elementos ocultos**). Los errores podrían deberse a varios problemas. 

5. Busque la cadena **post request: (7) - Couldn't connect to server**. Si se encuentra la cadena:
    1. Resuelva los problemas de red entre el servidor de configuración y la máquina de origen. Compruebe que el servidor de configuración es accesible desde la máquina de origen con herramientas de red como ping, traceroute o un explorador web. Asegúrese de que la máquina de origen pueda acceder al servidor de configuración a través del puerto 443.
    2. Compruebe si alguna regla de firewall de la máquina de origen bloquea la conexión entre el servidor de configuración y la máquina de origen. Trabaje con el administrador de red para desbloquear los problemas de conexión.
    3. Asegúrese de que las carpetas incluidas en [Exclusiones de carpetas de Site Recovery de programas antivirus](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) se excluyan del software antivirus.
    4. Cuando se resuelvan los problemas de red, vuelva a intentar el registro siguiendo las directrices en [Registrar la máquina de origen con el servidor de configuración](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server).

6. Si no se encuentra la cadena **post request: (7) - Couldn't connect to server**, en el mismo archivo de registro, busque la cadena **request: (60) - Peer certificate cannot be authenticated with given CA certificates**. Este error podría producirse porque el certificado de servidor de configuración ha expirado o la máquina de origen no admite TLS 1.0 o los protocolos SSL posteriores. También puede producirse si un firewall bloquea la comunicación SSL entre la máquina de origen y el servidor de configuración. Si se encuentra la cadena: 
    1. Para resolverlo, conéctese a la dirección IP del servidor de configuración mediante un explorador web en la máquina de origen. Use la URI https:\/\/<dirección IP del servidor de configuración\>:443/. Asegúrese de que la máquina de origen pueda acceder al servidor de configuración a través del puerto 443.
    2. Compruebe si es necesario agregar o quitar alguna regla de firewall de la máquina de origen para que la máquina de origen se comunique con el servidor de configuración. Dada la variedad de software de firewall que podría estar en uso, no podemos incluir todas las configuraciones de firewall necesarias. Trabaje con el administrador de red para desbloquear los problemas de conexión.
    3. Asegúrese de que las carpetas incluidas en [Exclusiones de carpetas de Site Recovery de programas antivirus](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) se excluyan del software antivirus.  
    4. Tras resolver los problemas, vuelva a intentar el registro siguiendo las directrices en [Registrar la máquina de origen con el servidor de configuración](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server).

7. En Linux, si el valor de la plataforma de <INSTALLATION_DIR\>/etc/drscout.conf está dañado, se producirá un error en el registro. Para identificar este problema, abra el archivo /var/log/ua_install.log. Busque la cadena **Aborting configuration as VM_PLATFORM value is either null or it is not VmWare/Azure**. La plataforma debe establecerse en **VmWare** o **Azure**. Si el archivo drscout.conf está dañado, recomendamos que [desinstale el agente de movilidad](vmware-physical-mobility-service-overview.md#uninstall-the-mobility-service) y, a continuación, vuelva a instalarlo. Si se produce un error en la desinstalación, complete los siguientes pasos:
    1. Abra el archivo Installation_Directory/uninstall.sh y convierta en comentario la llamada a la función **StopServices**.
    2. Abra el archivo Installation_Directory/Vx/bin/uninstall.sh y convierta en comentario la llamada a la función **stop_services**.
    3. Abra el archivo Installation_Directory/Fx/uninstall.sh y convierta en comentario la sección completa que intenta detener el servicio Fx.
    4. [Desinstale](vmware-physical-mobility-service-overview.md#uninstall-the-mobility-service) el agente de movilidad. Después de la desinstalación correcta, reinicie el sistema e intente volver a instalar el agente de movilidad.

## <a name="installation-failure-failed-to-load-accounts"></a>Error de instalación: No se pudieron cargar las cuentas

Este error se produce cuando el servicio no puede leer los datos de la conexión de transporte al instalar el agente de movilidad y registrarse con el servidor de configuración. Para resolver el problema, asegúrese de que TLS 1.0 esté habilitado en su máquina de origen.

## <a name="vcenter-discovery-failures"></a>Errores de detección de vCenter

Con el fin de resolver los errores de detección de vCenter, asegúrese de que ese servidor de vCenter se agrega a la configuración de proxy de la lista byPass. Para realizar esta actividad,

- Descargue la herramienta PsExec desde [aquí](https://aka.ms/PsExec) para acceder al contenido de usuario del sistema.
- Abra Internet Explorer en el contenido de usuario del sistema mediante la ejecución de la siguiente línea de comandos: psexec -s -i "%programfiles%\Internet Explorer\iexplore.exe"
- Agregue la configuración de proxy en Internet Explorer y reinicie el servicio tmanssvc.
- Para editar la configuración del proxy DRA, ejecute cd C:\Archivos de programa\Microsoft Azure Site Recovery Provider
- A continuación, ejecute DRCONFIGURATOR. EXE /configure /AddBypassUrls [Agregue la dirección IP/FQDN de vCenter Server que se proporcionó durante el paso **Configurar vCenter Server o vSphere ESXi Server** durante la [implementación del servidor de configuración](vmware-azure-deploy-configuration-server.md#configure-settings)]

## <a name="change-the-ip-address-of-the-configuration-server"></a>Cambie la dirección IP del servidor de configuración

Recomendamos encarecidamente no cambiar la dirección IP de un servidor de configuración. Asegúrese de que todas las direcciones IP que se asignan al servidor de configuración sean direcciones IP estáticas. No use direcciones IP DHCP.

## <a name="acs50008-saml-token-is-invalid"></a>ACS50008: El token SAML no es válido

Para evitar este error, asegúrese de que la hora de su reloj del sistema no sea distinta de la hora local en más de 15 minutos. Vuelva a ejecutar el instalador para completar el registro.

## <a name="failed-to-create-a-certificate"></a>Error al crear un certificado

No se puede crear el certificado necesario para autenticarse en Site Recovery. Vuelva a ejecutar el programa de instalación tras asegurarse de que lo ejecuta como un administrador local.

## <a name="register-source-machine-with-configuration-server"></a>Registrar la máquina de origen con el servidor de configuración

### <a name="if-the-source-machine-runs-windows"></a>Si la máquina de origen ejecuta Windows

Ejecute el siguiente comando en la máquina de origen:

```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe  /CSEndPoint <configuration server IP address> /PassphraseFilePath <passphrase file path>
  ```

Configuración | Detalles
--- | ---
Uso | UnifiedAgentConfigurator.exe  /CSEndPoint <dirección IP del servidor de configuración\> /PassphraseFilePath <ruta de acceso del archivo de frase de contraseña\>
Registros de configuración del agente | Se encuentran en %ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log.
/CSEndPoint | Parámetro obligatorio. Especifique la dirección IP del servidor de configuración. Use una dirección IP válida.
/PassphraseFilePath |  Obligatorio. La ubicación de la frase de contraseña. Use cualquier ruta de acceso de archivo local o UNC válida.

### <a name="if-the-source-machine-runs-linux"></a>Si la máquina de origen ejecuta Linux

Ejecute el siguiente comando en la máquina de origen:

```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <configuration server IP address> -P /var/passphrase.txt
  ```

Configuración | Detalles
--- | ---
Uso | cd /usr/local/ASR/Vx/bin<br /><br /> UnifiedAgentConfigurator.sh -i <dirección IP del servidor de configuración\> -P <ruta de acceso del archivo de frase de contraseña\>
-i | Parámetro obligatorio. Especifique la dirección IP del servidor de configuración. Use una dirección IP válida.
-P |  Obligatorio. La ruta de acceso completa del archivo en que se guarda la frase de contraseña. Uso de cualquier carpeta válida.

