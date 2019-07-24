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
ms.openlocfilehash: f23865fc0a1943a5157e4ff8eb8de10a71ef0883
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2019
ms.locfileid: "68295790"
---
# <a name="threat-detection-for-vms--servers-in-azure-security-center"></a>Detección de amenazas en máquinas virtuales y servidores en Azure Security Center

En este tema se presentan diferentes tipos de métodos de detección y alertas disponibles para máquinas virtuales y servidores con los siguientes sistemas operativos. Para obtener una lista de versiones admitidas, consulte [Características y plataformas compatibles con Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-os-coverage).

* [Windows](#windows-machines)
* [Linux](#linux-machines)

## Windows <a name="windows-machines"></a>

Security Center se integra con servicios de Azure para supervisar y proteger las máquinas Windows.  Security Center presenta las alertas y las sugerencias de corrección de todos estos servicios en un formato fácil de usar.

### ATP de Microsoft Server Defender<a nanme="windows-atp"></a>

Azure Security Center amplía sus plataformas de protección de cargas de trabajo en la nube mediante la integración con Advanced Threat Protection (ATP) de Windows Defender. Esta integración ofrece funcionalidades completas de detección y respuesta (EDR) de puntos de conexión.

> [!NOTE]
> El sensor de ATP de Windows Server Defender se habilita automáticamente en los servidores Windows que se incorporan a Azure Security Center.

Cuando ATP de Windows Server Defender detecta una amenaza, desencadena una alerta. La alerta se muestra en el panel de Security Center. En el panel, puede dinamizar hasta la consola de ATP de Windows Defender para realizar una investigación detallada y descubrir el alcance del ataque. Para más información sobre ATP de Windows Server Defender, consulte [Incorporación de servidores al servicio ATP de Windows Defender](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/configure-server-endpoints).

### Análisis del volcado de memoria <a nanme="windows-dump"></a>

En el momento en que el software se bloquea, un volcado de memoria captura una porción de la memoria.

Un bloqueo puede deberse a malware o contener malware. Para evitar que lo detecten los productos de seguridad, diversas formas de malware usan ataques sin archivos, lo que evita tener que escribir en el disco o cifrar los componentes de software escritos en el disco. Este tipo de ataque es difícil de detectar con los enfoques tradicionales basados en disco.

Sin embargo, este tipo de ataque se puede detectar mediante el análisis de memoria. Al analizar la memoria en el volcado, Security Center puede detectar las técnicas usadas en el ataque para aprovechar las vulnerabilidades del software, acceder a información confidencial y permanecer subrepticiamente en una máquina en peligro. Para ello se usa el back-end de Security Center con un impacto mínimo en el rendimiento de los hosts.

> [!div class="mx-tableFixed"]

|Alerta|DESCRIPCIÓN|
|---|---|
|**Inserción de código detectada**|La inserción de código es la inserción de módulos ejecutables en subprocesos o procesos de ejecución. El malware emplea esta técnica para acceder a los datos, al tiempo que se oculta perfectamente para evitar que lo encuentren y eliminen. <br/>Esta alerta indica que un módulo insertado está presente en el volcado de memoria. Para diferenciar entre módulos insertados malintencionados y no malintencionados, Security Center comprueba si el módulo insertado se ajusta a un perfil de comportamiento sospechoso.|
|**Segmento de código sospechoso detectado**|Indica que se ha asignado un segmento de código con métodos no estándar, como el vaciado de proceso y la inserción reflexiva. La alerta ofrece características adicionales del segmento de código que se han procesado para proporcionar un contexto para las funcionalidades y los comportamientos del segmento de código notificado.|
|**Detectado shellcode**|Shellcode es la carga que se ejecuta después de que un malware haya aprovechado una vulnerabilidad en el software.<br/>Esta alerta indica que el análisis del volcado de memoria ha detectado código ejecutable que muestra un comportamiento que normalmente exhiben cargas malintencionadas. Aunque el software no malintencionado puede exhibir también este comportamiento, no es habitual de los procedimientos de desarrollo de software normales.|

### Detección de ataques sin archivos <a nanme="windows-fileless"></a>

En Azure, se pueden observar con frecuencia ataques sin archivos dirigidos a los puntos de conexión de los clientes.

Para evitar que se detecten, los ataques sin archivos insertan cargas malintencionadas en la memoria. Las cargas del atacante se conservan dentro de la memoria de los procesos en peligro y realizan una amplia variedad de actividades malintencionadas.

Con la detección de ataques sin archivos, las técnicas forense de memoria automatizadas identifican kits de herramientas, técnicas y comportamientos de los ataques sin archivos. Esta solución examina periódicamente la máquina en tiempo de ejecución y extrae conclusiones directamente de la memoria de los procesos críticos para la seguridad.

Asimismo, encuentra indicios de explotación, inserción de código y ejecución de cargas malintencionadas. La detección de ataques sin archivos genera alertas de seguridad detalladas que aceleran la evaluación de prioridades, la correlación y el tiempo de respuesta de bajada de las alertas. Este enfoque complementa a las soluciones EDR basadas en eventos al proporcionar mayor cobertura de detección.

> [!NOTE]
> Para simular las alertas de Windows, descargue [Azure Security Center Playbook](https://gallery.technet.microsoft.com/Azure-Security-Center-0ac8a5ef): Security Alerts (Alertas de seguridad) y siga las instrucciones proporcionadas

> [!div class="mx-tableFixed"]

|Alerta|DESCRIPCIÓN|
|---|---|
|**Se detectó una técnica de ataque sin archivos**|La memoria del proceso especificado a continuación contiene un kit de herramientas de ataques sin archivos: Meterpreter. Los kits de herramientas de ataques sin archivos no tienen presencia en el sistema de archivos, lo que dificulta la detección mediante las soluciones antivirus tradicionales.|

### <a name="further-reading"></a>Lecturas adicionales

Para más información sobre la detección de Security Center y ver ejemplos, consulte:

* [How Azure Security Center automates the detection of cyber attack](https://azure.microsoft.com/blog/leverage-azure-security-center-to-detect-when-compromised-linux-machines-attack/) (Cómo Azure Security Center automatiza la detección de ataques cibernéticos)
* [How Azure Security Center detects vulnerabilities using administrative tools](https://azure.microsoft.com/blog/azure-security-center-can-detect-emerging-vulnerabilities-in-linux/) (Cómo Azure Security Center detecta vulnerabilidades mediante herramientas administrativas)

## Linux <a name="linux-machines"></a>

Security Center recopila registros de auditoría de máquinas Linux mediante **auditd**, uno de los marcos de trabajo de Linux más comunes. auditd tiene la ventaja de que existe desde hace mucho tiempo y vive en el kérnel de línea principal. 

### Integración de alertas de auditd de Linux y Microsoft Monitoring Agent (MMA) <a name="linux-auditd"></a>

El sistema auditd consta de un subsistema de nivel de kernel, que es responsable de supervisar las llamadas del sistema, filtrarlas por un conjunto determinado de reglas y escribirles mensajes en un socket. Security Center integra funcionalidades del paquete auditd dentro de Microsoft Monitoring Agent (MMA). Esta integración permite la recopilación de eventos de auditd en todas las distribuciones de Linux admitidas sin requisitos previos.  

Los registros de auditd se recopilan, enriquecen y agregan en eventos mediante el agente de MMA de Linux. Security Center trabaja constantemente para agregar nuevos análisis que aprovechan las señales de Linux para detectar comportamientos malintencionados en la nube y en máquinas Linux. Al igual que las funcionalidades de Windows, estos análisis abarcan procesos sospechosos, intentos de inicio de sesión dudosos, carga de módulos del kernel y otras actividades, que indican que una máquina está sufriendo ataques o los ha sufrido ya.  

A continuación se muestran varios ejemplos de análisis, que ilustran la expensión entre diferentes fases del ciclo de vida del ataque.

> [!div class="mx-tableFixed"]

|Alerta|DESCRIPCIÓN|
|---|---|
|**Se ha observado un proceso accediendo al archivo de claves autorizadas SSH de manera inusual**|Se ha accedido a un archivo de claves autorizadas SSH de una manera similar a campañas de malware conocidas. Este acceso podría indicar que un atacante está intentando conseguir acceso persistente a una máquina.|
|**Se ha detectado un intento de persistencia**|El análisis de datos del host ha detectado que se ha instalado un script de inicio para el modo de usuario único. <br/>Puesto que no es habitual que los procesos legítimos se ejecuten de ese modo, este síntoma podría indicar que un atacante ha agregado un proceso malintencionado a cada nivel de ejecución para garantizar la persistencia.|
|**Se ha detectado la manipulación de tareas programadas**|El análisis de datos del host ha detectado una posible manipulación de las tareas programadas. Con frecuencia, los atacantes agregan tareas programadas a las máquinas que han puesto en peligro a fin de lograr persistencia.|
|**Modificación sospechosa de la marca de tiempo de los archivos**|El análisis de datos del host detectó una modificación sospechosa de la marca de tiempo. Los atacantes suelen copiar las marcas de tiempo de archivos legítimos existentes en nuevas herramientas para evitar la detección de estos archivos recién quitados.|
|**Se ha agregado un nuevo usuario al grupo sudoers**|El análisis de datos del host detectó que un usuario se agregó al grupo sudoers, que permite a sus miembros ejecutar comandos con privilegios elevados.|
|**Probable explotación de la vulnerabilidad DynoRoot en clientes dhcp**|El análisis de datos del host detectó la ejecución de un comando inusual con el proceso primario del script dhclient.|
|**Se detectó un módulo de kernel sospechoso**|El análisis de datos del host detectó la carga de un archivo de objeto compartido como un módulo del kernel. Podría tratarse de una actividad legítima o de una señal de que una de las máquinas se ha puesto en peligro.|
|**Se ha detectado un proceso relacionado con la minería de datos de moneda digital**|El análisis de datos del host detectó la ejecución de un proceso que normalmente se asocia con la minería de datos de moneda digital.|
|**Posible reenvío de puertos a la dirección IP externa**|El análisis de datos del host detectó el inicio del reenvío de puertos a una dirección IP externa.|

> [!NOTE]
> Para simular alertas de Windows, descargue [Azure Security Center Playbook: Security Alerts](https://gallery.technet.microsoft.com/Azure-Security-Center-0ac8a5ef) (Alertas de seguridad) y siga las instrucciones proporcionadas.


Para obtener más información, consulte estos artículos:  

* [Leverage Azure Security Center to detect when compromised Linux machines attack](https://azure.microsoft.com/blog/leverage-azure-security-center-to-detect-when-compromised-linux-machines-attack/) (Aprovechar Azure Security Center para detectar ataques que ponen en peligro las máquinas virtuales)

* [Azure Security Center can detect emerging vulnerabilities in Linux](https://azure.microsoft.com/blog/azure-security-center-can-detect-emerging-vulnerabilities-in-linux/) (Azure Security Center puede detectar vulnerabilidades emergentes en Linux)

 