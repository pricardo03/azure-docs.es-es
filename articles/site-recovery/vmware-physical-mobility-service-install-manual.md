---
title: Instalación manual de Mobility Service para la recuperación ante desastres de máquinas virtuales VMware y servidores físicos con Azure Site Recovery | Microsoft Docs
description: Aprenda a instalar al agente de Mobility Service para la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure con el servicio Azure Site Recovery.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/04/2018
ms.author: raynew
ms.openlocfilehash: dfed0209131379843b97ff8050c2f2ba7294537d
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51019999"
---
# <a name="install-the-mobility-service-manually-on-vmware-vms-and-physical-servers"></a>Instalación manual de Mobility Service en máquinas virtuales VMware y servidores físicos

Al configurar la recuperación ante desastres para máquinas virtuales de VMware y servidores físicos con [Azure Site Recovery](site-recovery-overview.md), instala [Site Recovery Mobility Service](vmware-physical-mobility-service-overview.md) en cada máquina virtual de VMware local y servidor físico.  Mobility Service captura las escrituras de datos en la máquina y las reenvía al servidor de procesos de Site Recovery.

En este artículo se describe cómo instalar Mobility Service manualmente en cada máquina que desea proteger.

## <a name="create-a-passphrase"></a>Creación de una frase de contraseña

Antes de realizar la instalación, cree una frase de contraseña que se usará durante la instalación.

1. Inicie sesión en el servidor de configuración.
2. Abra un símbolo del sistema como administrador.
3. Cambie el directorio a la carpeta Bin y, a continuación, cree un archivo de frase de contraseña.

    ```
    cd %ProgramData%\ASR\home\svsystems\bin
    genpassphrase.exe -v > MobSvc.passphrase
    ```
3. Almacene el archivo de frase de contraseña en una ubicación segura. 


## <a name="install-the-service-from-the-ui"></a>Instalación del servicio desde la interfaz de usuario

>[!IMPORTANT]
> Si va a replicar máquinas virtuales de IaaS de Azure desde una región de Azure a otra, no use este método. En su lugar, use el método de instalación basado en la línea de comandos.

1. Busque la versión del instalador que necesita para el sistema operativo del equipo. Los instaladores se encuentran en la carpeta %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository. [Compruebe](vmware-physical-mobility-service-overview.md#installer-files) qué instalador necesita.
2. Copie el archivo de instalación en el equipo y ejecútelo.
3. En **Opción de instalación**, seleccione **Instalar Mobility Service**.
4. Seleccione la ubicación de instalación > **Instalar**.

    ![Página Opción de instalación de Mobility Service](./media/vmware-physical-mobility-service-install-manual/mobility1.png)

5. Supervise la instalación en **Progreso de la instalación**. Una vez completada la instalación, seleccione **Continuar con la configuración** para registrar el servicio en el servidor de configuración.

    ![Página de registro de Mobility Service](./media/vmware-physical-mobility-service-install-manual/mobility3.png)

6.  En **Configuration Server Details** (Detalles del servidor de configuración), especifique la dirección IP y la frase de contraseña que ha configurado.  

    ![Página de registro de Mobility Service](./media/vmware-physical-mobility-service-install-manual/mobility4.png)

7. Seleccione **Registrar** para finalizar el registro.

    ![Página de registro final de Mobility Service](./media/vmware-physical-mobility-service-install-manual/mobility5.png)

## <a name="install-the-service-from-the-command-prompt"></a>Instalación del servicio desde el símbolo del sistema

### <a name="on-a-windows-machine"></a>Una máquina Windows

1. Copie el instalador en una carpeta local (por ejemplo, C:\Temp) del servidor que desea proteger. 

  ```
  cd C:\Temp
  ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
  MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
  cd C:\Temp\Extracted.
  ```
2. Haga la instalación de la siguiente manera:

  ```
  UnifiedAgent.exe /Role "MS" /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
  ```

3. Registre el agente en el servidor de configuración.

  ```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
  ```

#### <a name="installation-settings"></a>Configuración de la instalación
**Configuración** | **Detalles**
--- | ---
Uso | UnifiedAgent.exe /Role <MS|MT> /InstallLocation <Install Location> /Platform “VmWare” /Silent
Registros de configuración | En %ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log.
/Role | Parámetro de instalación obligatorio. Especifica si se debe instalar Mobility Service (MS) o el destino maestro (MT).
/InstallLocation| Parámetro opcional. Especifique la ubicación (cualquier carpeta) de la instalación de Mobility Service.
/Platform | Obligatorio. Especifica la plataforma en la que se instala Mobility Service. **VMware** para servidores físicos o máquinas virtuales Mware; **Azure** para máquinas virtuales de Azure. 
/Silent| Opcional. Especifica si se debe ejecutar el instalador en modo silencioso.

#### <a name="registration-settings"></a>Configuración de registro
**Configuración** | **Detalles**
--- | ---
Uso | UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
Registros de configuración del agente | En %ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log.
/CSEndPoint | Parámetro obligatorio. Especifique la dirección IP del servidor de configuración. Use una dirección IP válida.
/PassphraseFilePath |  Obligatorio. Ubicación de la frase de contraseña. Use cualquier ruta de acceso de archivo local o UNC válida.


### <a name="on-a-linux-machine"></a>En un equipo Linux

1. Copie el instalador en una carpeta local (por ejemplo, /tmp) del servidor que desea proteger. En un terminal, ejecute los siguientes comandos:
  ```
  cd /tmp ;

  tar -xvzf Microsoft-ASR_UA*release.tar.gz
  ```
2. Haga la instalación de la siguiente manera:

  ```
  sudo ./install -d <Install Location> -r MS -v VmWare -q
  ```
3. Una vez completada la instalación, Mobility Service debe registrarse en el servidor de configuración. Ejecute el siguiente comando para registrar Mobility Service en el servidor de configuración:

  ```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
  ```


#### <a name="installation-settings"></a>Configuración de la instalación
**Configuración** | **Detalles**
--- | ---
Uso | ./install -d <Install Location> -r <MS|MT> -v VmWare -q
-r | Parámetro de instalación obligatorio. Especifica si se debe instalar Mobility Service (MS) o el destino maestro (MT).
-d | Parámetro opcional. Especifique la ubicación de instalación de Mobility Service: /usr/local/ASR.
-v | Obligatorio. Especifica la plataforma en la que se instala Mobility Service. **VMware** para servidores físicos o máquinas virtuales Mware; **Azure** para máquinas virtuales de Azure. 
-q | Opcional. Especifica si se debe ejecutar el instalador en modo silencioso.

#### <a name="registration-settings"></a>Configuración de registro
**Configuración** | **Detalles**
--- | ---
Uso | cd /usr/local/ASR/Vx/bin<br/><br/> UnifiedAgentConfigurator.sh -i <CSIP> -P <PassphraseFilePath>
-i | Parámetro obligatorio. Especifique la dirección IP del servidor de configuración. Use una dirección IP válida.
-P |  Obligatorio. Ruta de acceso completa al archivo en que se guarda la frase de contraseña. Uso de cualquier carpeta válida

## <a name="next-steps"></a>Pasos siguientes
- [Configuración de la recuperación ante desastres para máquinas virtuales de VMware](vmware-azure-tutorial.md)
- [Configuración de la recuperación ante desastres para servidores físicos](physical-azure-disaster-recovery.md)
