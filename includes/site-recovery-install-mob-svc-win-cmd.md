---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 9fe3b66de83ebc2cd0bf3a56a45456668c069191
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58116256"
---
1. Copie el instalador en una carpeta local (por ejemplo, C:\Temp) del servidor que desea proteger. Ejecute los siguientes comandos como administrador en un símbolo del sistema:

   ```
   cd C:\Temp
   ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
   MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
   cd C:\Temp\Extracted.
   ```
2. Para instalar Mobility Service, ejecute el siguiente comando:

   ```
   UnifiedAgent.exe /Role "MS" /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
   ```
3. Ahora el agente debe registrarse en el servidor de configuración.

   ```
   cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
   UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
   ```

#### <a name="mobility-service-installer-command-line-arguments"></a>Argumentos de la línea de comandos del instalador de Mobility Service

```
Usage :
UnifiedAgent.exe /Role <MS|MT> /InstallLocation <Install Location> /Platform “VmWare” /Silent
```

| Parámetro|Type|DESCRIPCIÓN|Valores posibles|
|-|-|-|-|
|/Role|Obligatorio|Especifica si se debe instalar Mobility Service (MS) o MasterTarget(MT).|MS </br> MT|
|/InstallLocation|Opcional|Ubicación en que se instala Mobility Service.|Cualquier carpeta del equipo|
|/Platform|Obligatorio|Especifica la plataforma en la que se instala Mobility Service. </br> </br>- **VMware**: Use este valor si instala Mobility Service en una máquina virtual que se ejecutan en *hosts de VMware vSphere ESXi*, *hosts de Hyper-V*, y *servidores físicos*. </br> - **Azure**: Use este valor si instala a un agente en una máquina virtual de IaaS de Azure. | VMware </br> Azure|
|/Silent|Opcional|Especifica que se ejecute el programa de instalación en modo silencioso.| N/D|

>[!TIP]
> Los registros de instalación pueden encontrarse en %ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log.

#### <a name="mobility-service-registration-command-line-arguments"></a>Argumentos de la línea de comandos del registro de Mobility Service

```
Usage :
UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
```

  | Parámetro|Type|DESCRIPCIÓN|Valores posibles|
  |-|-|-|-|
  |/CSEndPoint |Obligatorio|Dirección IP del servidor de configuración| Cualquier dirección IP válida|
  |/PassphraseFilePath|Obligatorio|Ubicación de la frase de contraseña |Cualquier ruta de acceso local o UNC válida|


>[!TIP]
> Los registros de la Configuración de agente pueden encontrarse en %ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log.
