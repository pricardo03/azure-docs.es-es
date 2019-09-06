---
title: Detección de amenazas en máquinas virtuales y servidores en Azure Security Center | Microsoft Docs
description: En este tema se presentan las alertas de máquina virtual y servidor disponibles en Azure Security Center.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: dd2eb069-4c76-4154-96bb-6e6ae553ef46
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: v-mohabe
ms.openlocfilehash: 055b578c1d976b7f85c65ab4e028f9d609861cd4
ms.sourcegitcommit: dcf3e03ef228fcbdaf0c83ae1ec2ba996a4b1892
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2019
ms.locfileid: "70013327"
---
# <a name="threat-detection-for-vms-and-servers-in-azure-security-center"></a>Detección de amenazas en máquinas virtuales y servidores en Azure Security Center

En este tema se presentan diferentes tipos de métodos de detección y alertas disponibles para máquinas virtuales y servidores con los siguientes sistemas operativos. Para obtener una lista de versiones admitidas, consulte [Características y plataformas compatibles con Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-os-coverage).

* [Windows](#windows-machines)
* [Linux](#linux-machines)

## Windows <a name="windows-machines"></a>

Azure Security Center se integra con servicios de Azure para supervisar y proteger las máquinas Windows. Security Center presenta las alertas y las sugerencias de corrección de todos estos servicios en un formato fácil de usar.

### ATP de Windows Server Defender <a nanme="windows-atp"></a>

Security Center amplía sus plataformas de protección de cargas de trabajo en la nube mediante la integración con Advanced Threat Protection (ATP) de Windows Server Defender. Esta integración ofrece funcionalidades completas de detección y respuesta (EDR) de puntos de conexión.

> [!NOTE]
> El sensor de ATP de Windows Server Defender se habilita automáticamente en los servidores Windows que usan Security Center.

Cuando ATP de Windows Server Defender detecta una amenaza, desencadena una alerta. La alerta se muestra en el panel de Security Center. En el panel, puede dinamizar hasta la consola de ATP de Windows Defender para realizar una investigación detallada y descubrir el alcance del ataque. Para más información sobre ATP de Windows Server Defender, consulte [Incorporación de servidores al servicio ATP de Windows Defender](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/configure-server-endpoints).

### Análisis del volcado de memoria <a nanme="windows-dump"></a>

En el momento en que el software se bloquea, un volcado de memoria captura una porción de la memoria.

Un bloqueo puede deberse a malware o contener malware. Para evitar que lo detecten los productos de seguridad, diversas formas de malware usan ataques sin archivos, lo que evita tener que escribir en el disco o cifrar los componentes de software escritos en el disco. Este tipo de ataque es difícil de detectar con los enfoques tradicionales basados en disco.

Sin embargo, mediante el análisis de la memoria, puede detectar este tipo de ataque. Mediante el análisis de la memoria del volcado de memoria, Security Center puede detectar las técnicas que está utilizando el ataque. Por ejemplo, el ataque podría estar intentando aprovechar vulnerabilidades del software, acceder a datos confidenciales y persistir clandestinamente dentro de una máquina en peligro. Security Center realiza este trabajo con un impacto mínimo en el rendimiento de los hosts.

> [!div class="mx-tableFixed"]

|Alerta|DESCRIPCIÓN|
|---|---|
|**Inserción de código detectada**|La inserción de código es la inserción de módulos ejecutables en subprocesos o procesos de ejecución. El malware emplea esta técnica para acceder a los datos, al tiempo que se oculta perfectamente para evitar que lo encuentren y eliminen. <br/>Esta alerta indica que un módulo insertado está presente en el volcado de memoria. Para diferenciar entre módulos insertados malintencionados y no malintencionados, Security Center comprueba si el módulo insertado se ajusta a un perfil de comportamiento sospechoso.|
|**Segmento de código sospechoso detectado**|Indica que se ha asignado un segmento de código con métodos no estándar, como el vaciado de proceso y la inserción reflexiva. La alerta ofrece características adicionales del segmento de código que se han procesado para proporcionar un contexto para las funcionalidades y los comportamientos del segmento de código notificado.|
|**Detectado shellcode**|Shellcode es la carga que se ejecuta después de que un malware haya aprovechado una vulnerabilidad en el software.<br/>Esta alerta indica que el análisis del volcado de memoria ha detectado código ejecutable que muestra un comportamiento que normalmente exhiben cargas malintencionadas. Aunque el software no malintencionado puede exhibir también este comportamiento, no es habitual de los procedimientos de desarrollo de software normales.|

### Detección de ataques sin archivos <a nanme="windows-fileless"></a>

Los ataques sin archivos que se dirigen a los puntos de conexión son comunes. Para evitar que se detecten, los ataques sin archivos insertan cargas malintencionadas en la memoria. Las cargas del atacante se conservan dentro de la memoria de los procesos en peligro y realizan una amplia variedad de actividades malintencionadas.

Con la detección de ataques sin archivos, las técnicas forense de memoria automatizadas identifican kits de herramientas, técnicas y comportamientos de los ataques sin archivos. Esta solución examina periódicamente la máquina en tiempo de ejecución y extrae conclusiones directamente de la memoria de los procesos críticos para la seguridad.

Asimismo, encuentra indicios de explotación, inserción de código y ejecución de cargas malintencionadas. La detección de ataques sin archivos genera alertas de seguridad detalladas que aceleran la evaluación de prioridades, la correlación y el tiempo de respuesta de bajada de las alertas. Este enfoque complementa a las soluciones EDR basadas en eventos al proporcionar mayor cobertura de detección.

> [!NOTE]
> Para simular alertas de Windows, descargue [Azure Security Center Playbook: Alertas de seguridad](https://gallery.technet.microsoft.com/Azure-Security-Center-f621a046).

> [!div class="mx-tableFixed"]

|Alerta|DESCRIPCIÓN|
|---|---|
|**Se detectó una técnica de ataque sin archivos**|La memoria del proceso especificado contiene un kit de herramientas de ataques sin archivos: Meterpreter. Los kits de herramientas de ataques sin archivos no tienen presencia en el sistema de archivos, lo que dificulta la detección mediante un software antivirus tradicional.|

### <a name="further-reading"></a>Lecturas adicionales

Para más información sobre la detección de Security Center y ver ejemplos, consulte:

* [How Azure Security Center automates the detection of cyber attack](https://azure.microsoft.com/blog/leverage-azure-security-center-to-detect-when-compromised-linux-machines-attack/) (Cómo Azure Security Center automatiza la detección de ataques cibernéticos)
* [How Azure Security Center detects vulnerabilities using administrative tools](https://azure.microsoft.com/blog/azure-security-center-can-detect-emerging-vulnerabilities-in-linux/) (Cómo Azure Security Center detecta vulnerabilidades mediante herramientas administrativas)

## Linux <a name="linux-machines"></a>

Security Center recopila registros de auditoría de máquinas Linux mediante **auditd**, uno de los marcos de trabajo de Linux más comunes. auditd se encuentra en el kernel de línea principal. 

### Integración de alertas de auditd de Linux y Microsoft Monitoring Agent (MMA) <a name="linux-auditd"></a>

El sistema de auditd está compuesto por un subsistema de nivel de kernel, que es responsable de supervisar las llamadas del sistema. Las filtra según un conjunto de reglas especificado y escribe mensajes para ellas en un socket. Security Center integra funcionalidades del paquete auditd dentro de Microsoft Monitoring Agent (MMA). Esta integración permite una colección de eventos de auditd en todas las distribuciones de Linux admitidas sin requisitos previos.  

Los registros de auditd se recopilan, enriquecen y agregan en eventos mediante el agente de MMA de Linux. Security Center agrega continuamente análisis nuevos que usan señales de Linux para detectar comportamientos malintencionados en máquinas Linux locales y en la nube. De manera similar a las funcionalidades de Windows, estos análisis abarcan varios procesos sospechosos, intentos de inicio de sesión dudosos, carga de módulos de kernel y otras actividades. Estas actividades pueden indicar que una máquina está sufriendo un ataque o se ha vulnerado.  

A continuación se muestran varios ejemplos de análisis que abarcan distintas fases del ciclo de vida del ataque.

> [!div class="mx-tableFixed"]

|Alerta|DESCRIPCIÓN|
|---|---|
|**Se ha observado un proceso accediendo al archivo de claves autorizadas SSH de manera inusual**|Se ha accedido a un archivo de claves autorizadas SSH de una manera similar a campañas de malware conocidas. Este acceso puede indicar que un atacante está intentando conseguir acceso persistente a una máquina.|
|**Se ha detectado un intento de persistencia**|El análisis de datos del host ha detectado que se ha instalado un script de inicio para el modo de usuario único. <br/>Como no es habitual que los procesos legítimos se ejecuten de ese modo, este síntoma podría indicar que un atacante ha agregado un proceso malintencionado a cada nivel de ejecución para garantizar la persistencia.|
|**Se ha detectado la manipulación de tareas programadas**|El análisis de datos del host ha detectado una posible manipulación de las tareas programadas. Con frecuencia, los atacantes agregan tareas programadas a las máquinas que han puesto en peligro a fin de lograr persistencia.|
|**Modificación sospechosa de la marca de tiempo de los archivos**|El análisis de datos del host detectó una modificación sospechosa de la marca de tiempo. Los atacantes suelen copiar las marcas de tiempo de archivos legítimos existentes en nuevas herramientas para evitar la detección de estos archivos recién quitados.|
|**Se ha agregado un nuevo usuario al grupo sudoers**|El análisis de datos del host detectó que un usuario se agregó al grupo sudoers, que permite a sus miembros ejecutar comandos con privilegios elevados.|
|**Probable explotación de la vulnerabilidad DynoRoot en clientes dhcp**|El análisis de datos del host detectó la ejecución de un comando inusual con un proceso primario del script dhclient.|
|**Se detectó un módulo de kernel sospechoso**|El análisis de datos del host detectó la carga de un archivo de objeto compartido como un módulo del kernel. Podría tratarse de una actividad legítima o de una señal de que una de las máquinas se ha puesto en peligro.|
|**Se ha detectado un proceso relacionado con la minería de datos de moneda digital**|El análisis de datos del host detectó la ejecución de un proceso que normalmente se asocia con la minería de datos de moneda digital.|
|**Posible reenvío de puertos a la dirección IP externa**|El análisis de datos del host detectó el inicio del reenvío de puertos a una dirección IP externa.|

> [!NOTE]
> Para simular alertas de Windows, descargue [Azure Security Center Playbook: Detecciones de Linux](https://gallery.technet.microsoft.com/Azure-Security-Center-0ac8a5ef).


Para más información, consulte:  

* [Leverage Azure Security Center to detect when compromised Linux machines attack](https://azure.microsoft.com/blog/leverage-azure-security-center-to-detect-when-compromised-linux-machines-attack/) (Aprovechar Azure Security Center para detectar ataques que ponen en peligro las máquinas virtuales)

* [Azure Security Center can detect emerging vulnerabilities in Linux](https://azure.microsoft.com/blog/azure-security-center-can-detect-emerging-vulnerabilities-in-linux/) (Azure Security Center puede detectar vulnerabilidades emergentes en Linux)

 