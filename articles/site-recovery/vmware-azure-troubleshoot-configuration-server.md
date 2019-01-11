---
title: Solución de problemas con el servidor de configuración durante la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure con Azure Site Recovery | Microsoft Docs
description: En este artículo se proporciona información para solucionar problemas de la implementación del servidor de configuración para la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure con Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 12/17/2018
ms.author: ramamill
ms.openlocfilehash: f5c8241907459a06f0a6206ae6865cdf3fe9ab89
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/03/2019
ms.locfileid: "53998972"
---
# <a name="troubleshoot-configuration-server-issues"></a>Solución de problemas del servidor de configuración

En este artículo se le ayudará a solucionar problemas al implementar y administrar el servidor de configuración de [Azure Site Recovery](site-recovery-overview.md). El servidor de configuración funciona como un servidor de administración y se usa para configurar la recuperación ante desastres para VM de VMware y servidores físicos en Azure con Site Recovery. En las siguientes secciones se describen los errores más comunes que se ven al agregar y administrar un servidor de configuración.

## <a name="registration-failures"></a>Errores de registro

La máquina de origen se registra con el servidor de configuración durante la instalación del agente de movilidad. Los errores durante este paso se pueden depurar siguiendo estas instrucciones:

1. Vaya al archivo C:\ProgramData\ASR\home\svsystems\var\configurator_register_host_static_info.log. ProgramData puede ser una carpeta oculta. Si no lo encuentra, intente mostrar la carpeta. Los errores se pueden deber a varios problemas.
2. Busque la cadena "No se encuentra ninguna dirección IP válida". Si se encuentra la cadena,
    - Validar si el identificador de host solicitado es igual que el de la máquina de origen.
    - A fin de que se realice correctamente, la máquina de origen debe tener al menos una dirección IP asignada a la NIC física para el registro del agente con el CS.
    - Ejecute el comando en la máquina de origen `> ipconfig /all` (para SO de Windows) y `# ifconfig -a` (para SO de Linux) a fin de obtener todas las direcciones IP de la máquina de origen.
    - Tenga en cuenta que el registro del agente requiere una dirección válida IP v4 asignada a la NIC física.
3. Si no se encuentra la cadena anterior, busque la cadena "Reason"=>"NULL". Si se encuentra,
    - Cuando la máquina de origen utiliza un host vacío si se va a registrar con el servidor de configuración, se produce este error.
    - Después de resolver los problemas, siga la instrucciones proporcionadas [aquí](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server) para volver a intentar el registro manualmente.
4. Si no se encuentra la cadena anterior, vaya a la máquina de origen y compruebe el registro de C:\ProgramData\ASRSetupLogs\UploadedLogs\* ASRUnifiedAgentInstaller.log. ProgramData puede ser una carpeta oculta. Si no lo encuentra, intente mostrar la carpeta. Los errores se pueden deber a varios problemas. Busque la cadena “post request: (7) - Couldn't connect to server”. Si la encuentra:
    - Resuelva los problemas de red entre el servidor de configuración y la máquina de origen. Compruebe que el servidor de configuración sea accesible desde la máquina de origen mediante herramientas de red, como ping, traceroute, explorador web, etc. Asegúrese de que esta máquina de origen sea capaz de acceder al servidor de configuración a través del puerto 443.
    - Compruebe si alguna regla de firewall en la máquina de origen está bloqueando la conexión entre el servidor de configuración y la máquina de origen. Trabaje con el administrador de red para desbloquear los problemas de conexión.
    - Asegúrese de que las carpetas mencionadas [aquí](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) se excluyen del software antivirus.
    - Después de resolver los problemas de red, vuelva a intentar el registro siguiendo la instrucciones proporcionadas [aquí](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server).
5. Si no se encuentra, en el mismo registro busque la cadena "request: (60) - Peer certificate cannot be authenticated with given CA certificates". Si la encuentra: 
    - Este error podría deberse a que ha expirado el certificado de servidor de configuración o que la máquina de origen no es compatible con TLS 1.0 y protocolos SSL posteriores o hay un firewall que está bloqueando la comunicación SSL entre el servidor de configuración y la máquina de origen.
    - Para resolverlo, conéctese a la dirección IP del servidor de configuración mediante un explorador web en la máquina de origen con la ayuda del URI https://<CSIPADDRESS>:443/. Asegúrese de que la máquina de origen sea capaz de acceder al servidor de configuración a través del puerto 443.
    - Compruebe si alguna regla de firewall de la máquina de origen está bloqueando la conexión entre el servidor de configuración y la máquina de origen. Trabaje con el administrador de red para desbloquear los problemas de conexión.
    - Asegúrese de que las carpetas mencionadas [aquí](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) se excluyen del software antivirus.  
    - Después de resolver los problemas, vuelva a intentar el registro siguiendo la instrucciones proporcionadas [aquí](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server).
6. En Linux, si el valor de la plataforma de <INSTALLATION_DIR>/etc/drscout.conf está dañado, se producirá un error en el registro. Para identificar, vaya al registro /var/log/ua_install.log. Encontrará la cadena "Aborting configuration as VM_PLATFORM value is either null or it is not VmWare/Azure". La plataforma debe establecerse en "VmWare" o "Azure". Como el archivo drscout.conf está dañado, se recomienda [desinstalar](vmware-physical-mobility-service-overview.md#uninstall-the-mobility-service) el agente de movilidad y volver a instalarlo. Si se produce un error de desinstalación, siga estos pasos:
    - Abra el archivo Installation_Directory/uninstall.sh y convierta en comentario la llamada a la función *StopServices*
    - Abra el archivo Installation_Directory/Vx/bin/uninstall y convierta en comentario la llamada a la función `stop_services`
    - Abra el archivo Installation_Directory/Fx/uninstall y convierta en comentario la sección completa que intenta detener el servicio Fx.
    - Ahora intente [desinstalar](vmware-physical-mobility-service-overview.md#uninstall-the-mobility-service) el agente de movilidad. Después de la desinstalación correcta, reinicie el sistema e intente volver a instalar al agente.

## <a name="installation-failure---failed-to-load-accounts"></a>Error de instalación: no se pudieron cargar las cuentas

Este error se produce cuando el servicio no puede leer los datos de la conexión de transporte al instalar el agente de movilidad y registrarse con el servidor de configuración. Para resolverlo, asegúrese de que TLS 1.0 esté habilitado en la máquina de origen.

## <a name="change-ip-address-of-configuration-server"></a>Cambiar la dirección IP del servidor de configuración

Es absolutamente recomendable no cambiar la dirección IP de un servidor de configuración. Asegúrese de que todas las direcciones IP asignadas al servidor de configuración sean direcciones IP estáticas y no direcciones IP de DHCP.

## <a name="acs50008-saml-token-is-invalid"></a>ACS50008: El token SAML no es válido

Para evitar este error, asegúrese de que la hora de su reloj del sistema no sea más de 15 minutos anterior o posterior a la hora local. Vuelva a ejecutar el instalador para completar el registro.

## <a name="failed-to-create-certificate"></a>Error al crear certificado

No se puede crear el certificado necesario para autenticarse en Site Recovery. Vuelva a ejecutar el programa de instalación tras asegurarse de que lo ejecuta como un administrador local.

## <a name="register-source-machine-with-configuration-server"></a>Registrar la máquina de origen con el servidor de configuración

### <a name="if-source-machine-has-windows-os"></a>Si la máquina de origen tiene el sistema operativo Windows

Ejecute el siguiente comando en la máquina de origen

```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
  ```
**Configuración** | **Detalles**
--- | ---
Uso | UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
Registros de configuración del agente | En %ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log.
/CSEndPoint | Parámetro obligatorio. Especifique la dirección IP del servidor de configuración. Use una dirección IP válida.
/PassphraseFilePath |  Obligatorio. Ubicación de la frase de contraseña. Use cualquier ruta de acceso de archivo local o UNC válida.

### <a name="if-source-machine-has-linux-os"></a>Si la máquina de origen tiene el sistema operativo Linux

Ejecute el siguiente comando en la máquina de origen

```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
  ```
**Configuración** | **Detalles**
--- | ---
Uso | cd /usr/local/ASR/Vx/bin<br/><br/> UnifiedAgentConfigurator.sh -i <CSIP> -P <PassphraseFilePath>
-i | Parámetro obligatorio. Especifique la dirección IP del servidor de configuración. Use una dirección IP válida.
-P |  Obligatorio. Ruta de acceso completa al archivo en que se guarda la frase de contraseña. Uso de cualquier carpeta válida