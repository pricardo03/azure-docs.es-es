---
title: Conecte los datos de Cisco a la versión preliminar de Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo conectar los datos de Cisco para Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 62029b5c-29d3-4336-8a22-a9db8214eb7e
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 5bc57d448b8aa04b8cb6fb16000205fda8964150
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59495709"
---
# <a name="connect-your-cisco-asa-appliance"></a>Conecte el dispositivo Cisco ASA 

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Puede conectar Azure Sentinel a cualquier dispositivo Cisco ASA. Cisco ASA forma nativa se integra con Azure Centinela para la ingesta de datos, por lo que, aunque su dispositivo Cisco no guarda los registros como CEF, Sentinel Azure ingiere ellos del mismo modo que controla los registros CEF. La integración con Azure Sentinel permite ejecutar fácilmente análisis y consultas a través de los datos del archivo de registro de Cisco ASA. 

> [!NOTE]
> Datos se almacenarán en la ubicación geográfica del área de trabajo en el que se ejecuta Azure Sentinel.

## <a name="step-1-connect-your-cisco-asa-appliance-using-an-agent"></a>Paso 1: Conecte el dispositivo Cisco ASA mediante un agente

Para conectar el dispositivo Cisco ASA con Azure Sentinel, deberá implementar un agente en un equipo dedicado (máquina virtual o local) para admitir la comunicación entre el dispositivo y Azure Sentinel. Puede implementar el agente automáticamente o de forma manual. La implementación automática solo está disponible si su máquina dedicada es una nueva máquina virtual que crea en Azure. 

También puede implementar el agente manualmente en una máquina virtual existente, en una máquina virtual en otra nube o en una máquina local.

Para ver un diagrama de red de ambas opciones, consulte [conectar orígenes de datos](connect-data-sources.md).

### <a name="deploy-the-agent-in-azure"></a>Implementar al agente de Azure

1. En el portal de Azure Sentinel, haga clic en **conectores de datos** y seleccione el tipo de dispositivo. 

1. En **configuración del agente de Linux Syslog**:
   - Elija **implementación automática** si desea crear un nuevo equipo que viene preinstalado con el agente de Azure Sentinel e incluye todas la necesaria de la configuración, como se describió anteriormente. Seleccione **implementación automática** y haga clic en **implementación automática del agente**. Esto le llevará a la página de compra para una máquina virtual dedicada que se conecta automáticamente al área de trabajo, es. La máquina virtual es un **estándar D2s v3 (2 vCPU, 8 GB de memoria)** y tiene una dirección IP pública.
      1. En el **implementación personalizada** página, proporcione sus detalles y elija un nombre de usuario y una contraseña y si acepta los términos y condiciones, la máquina virtual de compra.
      1. Configurar su dispositivo para enviar los registros mediante la configuración indicada en la página de conexión. Para el conector genérico Common Event Format, use estos valores:
         - Protocolo = UDP
         - Puerto = 514
         - Instalaciones = Local-4
         - Formato = CEF
   - Elija **implementación Manual** si desea usar una máquina virtual existente como la máquina Linux dedicada en el que se debe instalar el agente de Azure Sentinel. 
      1. En **descargar e instalar el agente de Syslog**, seleccione **máquina virtual Linux de Azure**. 
      1. En el **máquinas virtuales** pantalla que se abre, seleccione la máquina que desea usar y haga clic en **Connect**.
      1. En la pantalla de conector en **Syslog hacia delante y configurar**, establezca si es el demonio Syslog **rsyslog.d** o **syslog-ng**. 
      1. Copie estos comandos y ejecutarlos en su dispositivo:
          - Si seleccionó rsyslog.d:
              
            1. Indicar el demonio Syslog para que escuche en instalaciones local_4 y enviar los mensajes de Syslog del agente de Azure Sentinel usando el puerto 25226. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            
            2. Descargue e instale el [archivo de configuración de security_events](https://aka.ms/asi-syslog-config-file-linux) que configura el agente de Syslog para que escuche en el puerto 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Donde {0} debe reemplazarse por el GUID del área de trabajo.
            
            1. Reiniciar el demonio syslog `sudo service rsyslog restart`
             
          - Si ha seleccionado syslog-ng:

              1. Indicar el demonio Syslog para que escuche en instalaciones local_4 y enviar los mensajes de Syslog del agente de Azure Sentinel usando el puerto 25226. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
              2. Descargue e instale el [archivo de configuración de security_events](https://aka.ms/asi-syslog-config-file-linux) que configura el agente de Syslog para que escuche en el puerto 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Donde {0} debe reemplazarse por el GUID del área de trabajo.

              3. Reiniciar el demonio syslog `sudo service syslog-ng restart`
      2. Reinicie al agente de Syslog con este comando: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
      1. Confirme que hay ningún error en el registro del agente, ejecute este comando: `tail /var/opt/microsoft/omsagent/log/omsagent.log`

### <a name="deploy-the-agent-on-an-on-premises-linux-server"></a>Implementar al agente en servidores Linux locales

Si no usa Azure, implementar manualmente el agente Azure Centinela para ejecutarse en un servidor dedicado de Linux.


1. En el portal de Azure Sentinel, haga clic en **conectores de datos** y seleccione el tipo de dispositivo.
1. Para crear una VM Linux dedicado, en **configuración del agente de Linux Syslog** elija **implementación Manual**.
   1. En **descargar e instalar el agente de Syslog**, seleccione **máquina Linux que no son Azure**. 
   1. En el **agente directo** pantalla que se abre, seleccione **agente para Linux** para descargar el agente o ejecute este comando para descargarlo en su equipo Linux:   `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w {workspace GUID} -s gehIk/GvZHJmqlgewMsIcth8H6VqXLM9YXEpu0BymnZEJb6mEjZzCHhZgCx5jrMB1pVjRCMhn+XTQgDTU3DVtQ== -d opinsights.azure.com`
      1. En la pantalla de conector en **Syslog hacia delante y configurar**, establezca si es el demonio Syslog **rsyslog.d** o **syslog-ng**. 
      1. Copie estos comandos y ejecutarlos en su dispositivo:
         - Si seleccionó rsyslog:
           1. Indicar el demonio Syslog para que escuche en instalaciones local_4 y enviar los mensajes de Syslog del agente de Azure Sentinel usando el puerto 25226. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            
           2. Descargue e instale el [archivo de configuración de security_events](https://aka.ms/asi-syslog-config-file-linux) que configura el agente de Syslog para que escuche en el puerto 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Donde {0} debe reemplazarse por el GUID del área de trabajo.
           3. Reiniciar el demonio syslog `sudo service rsyslog restart`
         - Si ha seleccionado syslog-ng:
            1. Indicar el demonio Syslog para que escuche en instalaciones local_4 y enviar los mensajes de Syslog del agente de Azure Sentinel usando el puerto 25226. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
            2. Descargue e instale el [archivo de configuración de security_events](https://aka.ms/asi-syslog-config-file-linux) que configura el agente de Syslog para que escuche en el puerto 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Donde {0} debe reemplazarse por el GUID del área de trabajo.
            3. Reiniciar el demonio syslog `sudo service syslog-ng restart`
      1. Reinicie al agente de Syslog con este comando: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
      1. Confirme que hay ningún error en el registro del agente, ejecute este comando: `tail /var/opt/microsoft/omsagent/log/omsagent.log`
 
## <a name="step-2-forward-cisco-asa-logs-to-the-syslog-agent"></a>Paso 2: Reenviar los registros de Cisco ASA para el agente de Syslog

Configure Cisco ASA para reenviar los mensajes de Syslog a su área de trabajo de Azure a través del agente de Syslog:

Vaya a [Syslog enviar mensajes a un servidor de Syslog externo](https://aka.ms/asi-syslog-cisco-forwarding)y siga las instrucciones para configurar la conexión. Use estos parámetros cuando se le solicite:
- Establecer **puerto** 514 o el puerto que ha establecido en el agente.
- Establecer **syslog_ip** a la dirección IP del agente.
- Establecer **funcionalidad de registro de** a la instalación se establecen en el agente. De forma predeterminada, el agente establece las instalaciones en 4.


## <a name="step-3-validate-connectivity"></a>Paso 3: Validar conectividad

Puede tardar más de 20 minutos hasta que los registros se empiecen a aparecer en Log Analytics. 

1. Asegúrese de que obtengan los registros para el puerto correcto en el agente de Syslog. Ejecute este comando en el equipo de agente de Syslog: `tcpdump -A -ni any  port 514 -vv` Este comando muestra los registros que se transmiten desde el dispositivo a la máquina de Syslog. Este comando muestra los registros que estén transmitiendo desde el dispositivo a la máquina de Syslog. Asegúrese de que se reciben los registros desde el dispositivo de origen en el puerto adecuado y la instalación correcta.
2. Compruebe que hay comunicación entre el demonio de Syslog y el agente. Ejecute este comando en el equipo de agente de Syslog: `tcpdump -A -ni any  port 25226 -vv` Este comando muestra los registros que se transmiten desde el dispositivo a la máquina de Syslog. Asegúrese de que los registros también se reciben en el agente.
3. Si ambos de esos comandos proporcionan resultados correctos, compruebe si Log Analytics para ver si llegan los registros. Todos los eventos que se transmite por secuencias desde estos dispositivos aparecen en formato sin procesar en Log Analytics en `CommonSecurityLog` tipo.
1. Para comprobar si hay errores o si no que llegan los registros, buscar `tail /var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log`
4. Asegúrese de que el tamaño predeterminado de mensaje de Syslog se limita a 2048 bytes (2KB). Si los registros son demasiado largos, actualice el security_events.conf con este comando: `message_length_limit 4096`
6. Para usar el esquema correspondiente en Log Analytics para los eventos de Cisco, busque **CommonSecurityLog**.




## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido cómo conectar dispositivos Cisco ASA con Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Obtenga información sobre cómo [Obtenga visibilidad sobre sus datos y las posibles amenazas](quickstart-get-visibility.md).
- Introducción a [detección de amenazas con Azure Sentinel](tutorial-detect-threats.md).

