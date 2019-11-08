---
title: Conexión de datos de Check Point con Azure Sentinel | Microsoft Docs
description: Aprenda a conectar los datos de Check Point a Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2019
ms.author: rkarlin
ms.openlocfilehash: 122adf07b64f8c59008a716263b237072d752955
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73498714"
---
# <a name="connect-check-point-to-azure-sentinel"></a>Conexión de Check Point a Azure Sentinel



En este artículo se explica cómo conectar el dispositivo Check Point a Azure Sentinel. El conector de datos de Check Point permite conectar fácilmente los registros de Check Point con Azure Sentinel para ver paneles, crear alertas personalizadas y mejorar la investigación. El uso de Check Point en Azure Sentinel le proporcionará más información sobre el uso de Internet de su organización y mejorará sus capacidades de operación de seguridad. 


## <a name="how-it-works"></a>Cómo funciona

Para conectar su dispositivo Check Point a Azure Sentinel, implemente un agente en una máquina virtual dedicada o máquina local para admitir la comunicación entre el dispositivo y Azure Sentinel. En el siguiente diagrama se describe la configuración en el caso de una VM Linux en Azure.

 ![CEF en Azure](./media/connect-cef/cef-syslog-azure.png)

Esta configuración también existirá si usa una VM en otra nube o en una máquina local. 

 ![CEF local](./media/connect-cef/cef-syslog-onprem.png)


## <a name="security-considerations"></a>Consideraciones sobre la seguridad

Asegúrese de configurar la seguridad de la máquina de acuerdo con la directiva de seguridad de su organización. Por ejemplo, puede configurar la red para que se alinee con la directiva de seguridad de la red corporativa y cambiar los puertos y protocolos del demonio para que se adapten a sus requisitos. Puede usar las siguientes instrucciones para mejorar la configuración de seguridad de la máquina:   [Protección de máquinas virtuales en Azure](../virtual-machines/linux/security-policy.md), [Procedimientos recomendados de seguridad de la red](../security/fundamentals/network-best-practices.md).

Para usar la comunicación TLS entre la solución de seguridad y la máquina de Syslog, debe configurar el demonio de Syslog (rsyslog o syslog-ng) para que se comunique en TLS: [Cifrado del tráfico de Syslog con TLS -rsyslog](https://www.rsyslog.com/doc/v8-stable/tutorials/tls_cert_summary.html), [Cifrado de los mensajes de registro con TLS –syslog-ng](https://support.oneidentity.com/technical-documents/syslog-ng-open-source-edition/3.22/administration-guide/60#TOPIC-1209298).

 
## <a name="prerequisites"></a>Requisitos previos
Asegúrese de que la máquina Linux que usa como proxy ejecute uno de los siguientes sistemas operativos:

- 64 bits
  - CentOS 6 y 7
  - Amazon Linux 2017.09
  - Oracle Linux 6 y 7
  - Red Hat Enterprise Linux Server 6 y 7
  - Debian GNU/Linux 8 y 9
  - Ubuntu Linux 14.04 LTS, 16.04 LTS y 18.04 LTS
  - SUSE Linux Enterprise Server 12
- 32 bits
   - CentOS 6
   - Oracle Linux 6
   - Red Hat Enterprise Linux Server 6
   - Debian GNU/Linux 8 y 9
   - Ubuntu Linux 14.04 LTS y 16.04 LTS
 
 - Versiones de demonio
   - Syslog-ng: 2.1 - 3.22.1
   - Rsyslog: v8
  
 - RFC de Syslog compatibles
   - RFC de Syslog 3164
   - RFC de Syslog 5424
 
Asegúrese de que la máquina cumpla también con los requisitos siguientes: 
- Permisos
    - Debe tener permisos elevados (sudo) en la máquina. 
- Requisitos de software
    - Asegúrese de que tiene Python en ejecución en el equipo.
## <a name="step-1-deploy-the-agent"></a>PASO 1: Implementar el agente

En este paso, debe seleccionar la máquina Linux que actuará como proxy entre Azure Sentinel y la solución de seguridad. Tendrá que ejecutar un script en el equipo proxy que:
- Instale el agente de Log Analytics y lo configure según sea necesario para escuchar mensajes de Syslog en el puerto 514 a través de TCP y enviar los mensajes CEF al área de trabajo de Azure Sentinel.
- Configure el demonio de Syslog para reenviar los mensajes de CEF al agente de Log Analytics mediante el puerto 25226.
- Ajuste el agente de Syslog para que recopile los datos y los envíe de forma segura a Log Analytics, donde se analizan y enriquecen.
 
 
1. En el portal de Azure Sentinel, haga clic en **Data connectors** (Conectores de datos) y seleccione **Check Point** y, luego, **Open connector page** (Abrir página de conectores). 

1. En **Install and configure the Syslog agent** (Instalar y configurar el agente de Syslog), seleccione el tipo de máquina, ya sea local, de Azure o de otro servicio en la nube. 
   > [!NOTE]
   > Dado que el script del paso siguiente instala el agente de Log Analytics y conecta el equipo al área de trabajo de Azure Sentinel, asegúrese de que esta máquina no esté conectada a ninguna otra área de trabajo.
1. Debe tener permisos elevados (sudo) en la máquina. Asegúrese de que tiene Python en la máquina con el siguiente comando: `python –version`.

1. Ejecute el siguiente script en la máquina proxy.
   `sudo wget https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/DataConnectors/CEF/cef_installer.py&&sudo python cef_installer.py [WorkspaceID] [Workspace Primary Key]`
1. Mientras se ejecuta el script, asegúrese de que no recibe ningún mensaje de error ni advertencia.

 
## <a name="step-2-forward-check-point-logs-to-the-syslog-agent"></a>PASO 2: Reenvío de los registros de Check Point al agente de Syslog

Configure el dispositivo Check Point para reenviar mensajes de Syslog en formato CEF a su área de trabajo de Azure a través del agente de Syslog.

1. Vaya a [Check Point Log Export](https://aka.ms/asi-syslog-checkpoint-forwarding) (Exportación del registro de Check Point).
2. Desplácese hacia abajo hasta **Implementación básica** y siga las instrucciones para configurar la conexión según lo que se indica a continuación:
   - Establezca el **puerto de Syslog** en **514** o en el puerto que establezca en el agente.
     - Reemplace el **nombre** y la **dirección IP del servidor de destino** en la CLI con la dirección IP y el nombre del agente de Syslog.
     - Establezca el formato en **CEF**.
3. Si usa la versión R77.30 o R80.10, desplácese hacia arriba hasta **Instalaciones** y siga las instrucciones para instalar un exportador de registros para su versión.
 
## <a name="step-3-validate-connectivity"></a>PASO 3: Validar conectividad

1. Abra Log Analytics para asegurarse de que los registros se reciben con el esquema CommonSecurityLog.<br> Hasta que los registros empiecen a aparecer en Log Analytics, pueden transcurrir más de 20 minutos. 

1. Antes de ejecutar el script, se recomienda enviar mensajes desde la solución de seguridad para asegurarse de que se reenvían a la máquina proxy de Syslog que ha configurado. 
1. Debe tener permisos elevados (sudo) en la máquina. Asegúrese de que tiene Python en la máquina con el siguiente comando: `python –version`.
1. Ejecute el script siguiente para comprobar la conectividad entre el agente, Azure Sentinel y la solución de seguridad. Este comprueba que el reenvío del demonio esté configurado correctamente, escuche en los puertos correctos y que nada bloquee la comunicación entre el demonio y el agente de Log Analytics. El script también envía mensajes ficticios "TestCommonEventFormat" para comprobar la conectividad de un extremo a otro. <br>
 `sudo wget https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/DataConnectors/CEF/cef_troubleshoot.py&&sudo python cef_troubleshoot.py [WorkspaceID]`




## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar dispositivos de Check Point a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).

