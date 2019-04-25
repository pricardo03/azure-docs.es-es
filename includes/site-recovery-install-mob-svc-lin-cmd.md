---
ms.openlocfilehash: aa2e6d80620f0a4cf5063919a6de53e4de20f706
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60301450"
---
1. Copie el instalador en una carpeta local (por ejemplo, /tmp) del servidor que desea proteger. En un terminal, ejecute los siguientes comandos:
     ```
     cd /tmp;
     tar -xvzf Microsoft-ASR_UA*release.tar.gz
     ```
2. Para instalar Mobility Service, ejecute el siguiente comando:

     ```
     sudo ./install -d <Install Location> -r MS -v VmWare -q
     ```
3. Una vez completada la instalación, Mobility Service debe registrarse en el servidor de configuración. Ejecute el siguiente comando para registrar Mobility Service en el servidor de configuración:

     ```
     /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
     ```

#### <a name="mobility-service-installer-command-line"></a>Línea de comandos del instalador de Mobility Service

```
Usage:
./install -d <Install Location> -r <MS|MT> -v VmWare -q
```

|Parámetro|Type|DESCRIPCIÓN|Valores posibles|
|-|-|-|-|
|-r |Obligatorio|Especifica si se debe instalar Mobility Service (MS) o MasterTarget(MT).|MS <br /> MT|
|-d |Opcional|Ubicación en que se instala Mobility Service.|/usr/local/ASR|
|-v|Obligatorio|Especifica la plataforma en la que se instala Mobility Service. <br /> <br />- **VMware**: Use este valor si instala Mobility Service en una máquina virtual que se ejecutan en *hosts de VMware vSphere ESXi*, *hosts de Hyper-V*, y *servidores físicos*. <br /> - **Azure**: Use este valor si instala a un agente en una máquina virtual de IaaS de Azure.| VMware <br /> Azure|
|-q|Opcional|Especifica que se ejecute el programa de instalación en modo silencioso.| N/D|

#### <a name="mobility-service-configuration-command-line"></a>Línea de comandos de configuración de Mobility Service

```
Usage:
cd /usr/local/ASR/Vx/bin
UnifiedAgentConfigurator.sh -i <CSIP> -P <PassphraseFilePath>
```

|Parámetro|Type|DESCRIPCIÓN|Valores posibles|
|-|-|-|-|
|-i |Obligatorio|Dirección IP del servidor de configuración|Cualquier dirección IP válida|
|-P |Obligatorio|Ruta de acceso completa del archivo donde se guarda la frase de contraseña de conexión|Cualquier carpeta válida|
<!--Update_Description: wording update-->
<!--ms.date: 03/05/2018-->