---
title: Detección de amenazas en máquinas virtuales y servidores en Azure Security Center | Microsoft Docs
description: En este tema se presentan las alertas de máquina virtual y servidor disponibles en Azure Security Center.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: dd2eb069-4c76-4154-96bb-6e6ae553ef46
ms.service: security-center
ms.topic: conceptual
ms.date: 01/05/2020
ms.author: memildin
ms.openlocfilehash: b101fd30ddbdbf0f8fdf6e02394cb10b9af5f4b0
ms.sourcegitcommit: ff9688050000593146b509a5da18fbf64e24fbeb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2020
ms.locfileid: "75666407"
---
# <a name="threat-detection-for-vms-and-servers-in-azure-security-center"></a>Detección de amenazas en máquinas virtuales y servidores en Azure Security Center

En este tema se presentan diferentes tipos de métodos de detección y alertas disponibles para máquinas virtuales y servidores con los siguientes sistemas operativos. Para obtener una lista de versiones admitidas, consulte [Características y plataformas compatibles con Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-os-coverage).

* [Windows](#windows-machines)
* [Linux](#linux-machines)

## Windows <a name="windows-machines"></a>

Azure Security Center se integra con servicios de Azure para supervisar y proteger las máquinas Windows. Security Center presenta las alertas y las sugerencias de corrección de todos estos servicios en un formato fácil de usar.

* La de **ATP de Microsoft Defender** <a name="windows-atp"></a>: Security Center amplía sus plataformas de protección de carga de trabajo en la nube mediante la integración con protección contra amenazas avanzada (ATP) de Microsoft defender. Esta integración ofrece funcionalidades completas de detección y respuesta (EDR) de puntos de conexión.

    > [!NOTE]
    > El sensor de Microsoft Defender ATP se habilita automáticamente en los servidores Windows que usan Security Center.

    Cuando Microsoft Defender ATP detecta una amenaza, desencadena una alerta. La alerta se muestra en el panel de Security Center. En el panel, puede dinamizar hasta la consola de Microsoft Defender ATP para realizar una investigación detallada y descubrir el alcance del ataque. Para más información sobre Microsoft Defender ATP, consulte [Incorporación de servidores al servicio Microsoft Defender ATP](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/configure-server-endpoints).

* **Análisis de volcado de bloqueo** <a name="windows-dump"></a>: cuando se bloquea el software, un volcado de memoria captura una parte de la memoria en el momento del bloqueo.

    Un bloqueo puede deberse a malware o contener malware. Para evitar que lo detecten los productos de seguridad, diversas formas de malware usan ataques sin archivos, lo que evita tener que escribir en el disco o cifrar los componentes de software escritos en el disco. Este tipo de ataque es difícil de detectar con los enfoques tradicionales basados en disco.

    Sin embargo, mediante el análisis de la memoria, puede detectar este tipo de ataque. Mediante el análisis de la memoria del volcado de memoria, Security Center puede detectar las técnicas que está utilizando el ataque. Por ejemplo, el ataque podría estar intentando aprovechar vulnerabilidades del software, acceder a datos confidenciales y persistir clandestinamente dentro de una máquina en peligro. Security Center realiza este trabajo con un impacto mínimo en el rendimiento de los hosts.

    Para obtener una lista de las alertas de análisis de volcado de memoria, consulte la [Tabla de referencia de alertas](alerts-reference.md#alerts-crashdump).

* **La detección de ataques con archivos** <a name="windows-fileless"></a>: ataques de archivos destinados a los puntos de conexión son habituales. Para evitar que se detecten, los ataques sin archivos insertan cargas malintencionadas en la memoria. Las cargas del atacante se conservan dentro de la memoria de los procesos en peligro y realizan una amplia variedad de actividades malintencionadas.

    Con la detección de ataques sin archivos, las técnicas forense de memoria automatizadas identifican kits de herramientas, técnicas y comportamientos de los ataques sin archivos. Esta solución examina periódicamente la máquina en tiempo de ejecución y extrae conclusiones directamente de la memoria de los procesos críticos para la seguridad.

    Asimismo, encuentra indicios de explotación, inserción de código y ejecución de cargas malintencionadas. La detección de ataques sin archivos genera alertas de seguridad detalladas que aceleran la evaluación de prioridades, la correlación y el tiempo de respuesta de bajada de las alertas. Este enfoque complementa a las soluciones EDR basadas en eventos al proporcionar mayor cobertura de detección.

    Para obtener una lista de las alertas de detección de ataques sin archivo, consulte la [Tabla de referencia de alertas](alerts-reference.md#alerts-filelessattackdetect).

> [!NOTE]
> Para simular alertas de Windows, descargue [Azure Security Center Playbook: Alertas de seguridad](https://gallery.technet.microsoft.com/Azure-Security-Center-f621a046).

## Linux <a name="linux-machines"></a>

Security Center recopila registros de auditoría de máquinas Linux mediante **auditd**, uno de los marcos de trabajo de Linux más comunes. auditd se encuentra en el kernel de línea principal. 

* **La integración de alertas y Microsoft Monitoring Agent (MMA) de Linux** <a name="linux-auditd"></a>: el sistema auditado está compuesto por un subsistema de nivel de kernel, que es responsable de supervisar las llamadas del sistema. Las filtra según un conjunto de reglas especificado y escribe mensajes para ellas en un socket. Security Center integra funcionalidades del paquete auditd dentro de Microsoft Monitoring Agent (MMA). Esta integración permite una colección de eventos de auditd en todas las distribuciones de Linux admitidas sin requisitos previos.  

    Los registros de auditd se recopilan, enriquecen y agregan en eventos mediante el agente de MMA de Linux. Security Center agrega continuamente análisis nuevos que usan señales de Linux para detectar comportamientos malintencionados en máquinas Linux locales y en la nube. De manera similar a las funcionalidades de Windows, estos análisis abarcan varios procesos sospechosos, intentos de inicio de sesión dudosos, carga de módulos de kernel y otras actividades. Estas actividades pueden indicar que una máquina está sufriendo un ataque o se ha vulnerado.  

    A continuación se muestran varios ejemplos de análisis que abarcan distintas fases del ciclo de vida del ataque.

    Para obtener una lista de las alertas de Linux, consulte la [Tabla de referencia de alertas](alerts-reference.md#alerts-linux).

> [!NOTE]
> Para simular alertas de Linux descargue [Guía de Azure Security Center: Detecciones de Linux](https://gallery.technet.microsoft.com/Azure-Security-Center-0ac8a5ef).

 
 ## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la detección de Security Center y ver ejemplos, consulte:

* [How Azure Security Center automates the detection of cyber attack](https://azure.microsoft.com/blog/leverage-azure-security-center-to-detect-when-compromised-linux-machines-attack/) (Cómo Azure Security Center automatiza la detección de ataques cibernéticos)
* [How Azure Security Center detects vulnerabilities using administrative tools](https://azure.microsoft.com/blog/azure-security-center-can-detect-emerging-vulnerabilities-in-linux/) (Cómo Azure Security Center detecta vulnerabilidades mediante herramientas administrativas)
* [Leverage Azure Security Center to detect when compromised Linux machines attack](https://azure.microsoft.com/blog/leverage-azure-security-center-to-detect-when-compromised-linux-machines-attack/) (Aprovechar Azure Security Center para detectar ataques que ponen en peligro las máquinas virtuales)
* [Azure Security Center can detect emerging vulnerabilities in Linux](https://azure.microsoft.com/blog/azure-security-center-can-detect-emerging-vulnerabilities-in-linux/) (Azure Security Center puede detectar vulnerabilidades emergentes en Linux)