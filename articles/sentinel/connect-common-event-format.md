---
title: Conexión de datos CEF a la versión preliminar de Azure Sentinel | Microsoft Docs
description: Aprenda a conectar datos CEF a Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: cbf5003b-76cf-446f-adb6-6d816beca70f
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/02/2019
ms.author: rkarlin
ms.openlocfilehash: f9435c4b7649e9b97c209fb554f62228cde95034
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2019
ms.locfileid: "67612389"
---
# <a name="connect-your-external-solution-using-common-event-format"></a>Conexión de su solución externa con Common Event Format

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Puede conectar Azure Sentinel con una solución externa que le permite guardar archivos de registro en Syslog. Si su dispositivo le permite guardar registros como Common Event Format (CEF) de Syslog, la integración con Azure Sentinel permite ejecutar fácilmente análisis y consultas en los datos.

> [!NOTE] 
> Los datos se almacenan en la ubicación geográfica del área de trabajo en la que se ejecute Azure Sentinel.

## <a name="how-it-works"></a>Cómo funciona

La conexión entre Azure Sentinel y su dispositivo CEF se lleva a cabo en tres pasos:

1. Debe establecer estos valores en el dispositivo para que envíe los registros necesarios en el formato necesario al agente de Syslog de Azure Sentinel. Puede modificar estos parámetros en su dispositivo, siempre y cuando modifique también el demonio de Syslog en el agente de Azure Sentinel.
    - Protocolo = UDP
    - Puerto = 514
    - Recurso = Local-4
    - Formato = CEF
2. El agente de Syslog recopila los datos y los envía de forma segura a Log Analytics, donde se analizan y enriquecen.
3. El agente almacena los datos en un área de trabajo de Log Analytics, por lo que pueden consultarse según sea necesario, mediante análisis, reglas de correlación y paneles.

> [!NOTE]
> El agente puede recopilar registros de varios orígenes, pero debe estar instalado en el equipo proxy dedicado.

## <a name="step-1-connect-to-your-cef-appliance-via-dedicated-azure-vm"></a>Paso 1: Conectarse al dispositivo CEF mediante una máquina virtual de Azure dedicada

Para conectar su dispositivo Fortinet a Azure Sentinel, implemente un agente en una máquina virtual dedicada o máquina local para admitir la comunicación entre el dispositivo y Azure Sentinel. Puede implementar el agente automáticamente o de forma manual. La implementación automática se basa en plantillas de Resource Manager y se puede usar solo si su equipo Linux dedicado es una máquina virtual nueva que está creando en Azure.

 ![CEF en Azure](./media/connect-cef/cef-syslog-azure.png)

También puede implementar el agente manualmente en una máquina virtual existente, en una máquina virtual en otra nube o en una máquina local. 

 ![CEF local](./media/connect-cef/cef-syslog-onprem.png)

### <a name="deploy-the-agent-in-azure"></a>Implementación del agente en Azure


1. En el portal de Azure Sentinel, haga clic en **Data connectors** (Conectores de datos) y seleccione su tipo de dispositivo. 

1. En **Linux Syslog agent configuration** (Configuración de agente de Syslog de Linux):
   - Elija **Implementación automática** si desea crear una nueva máquina preinstalada con el agente de Azure Sentinel que incluya toda la configuración necesaria, como se ha descrito anteriormente. Seleccione **Implementación automática** y **Automatic agent deployment** (Implementación del agente automática). Esta acción le dirigirá a la página de compra de una máquina virtual Linux dedicada conectada automáticamente a su área de trabajo. La máquina virtual es un **D2s v3 estándar (2 vCPU, 8 GB de memoria)** y tiene una dirección IP pública.
      1. En la página **Implementación personalizada**, proporcione sus datos, elija un nombre de usuario y una contraseña y, si acepta los términos y condiciones, compre la máquina virtual.
      1. Configure su dispositivo para enviar registros mediante la configuración que se muestra en la página de conexión. Para el conector de Common Event Format genérico, use esta configuración:
         - Protocolo = UDP
         - Puerto = 514
         - Recurso = Local-4
         - Formato = CEF
   - Elija **Implementación manual** si desea usar una máquina virtual existente como la máquina Linux dedicada en la que se debe instalar el agente de Azure Sentinel. 
      1. En **Download and install the Syslog agent** (Descargar e instalar el agente de Syslog), seleccione **Azure Linux virtual machine** (Máquina virtual Linux de Azure). 
      1. En la pantalla **Máquinas virtuales**, seleccione la máquina que desea usar y haga clic en **Conectar**.
      1. En la pantalla del conector, en **Configure and forward Syslog** (Configurar y reenviar Syslog), establezca si su demonio Syslog es **rsyslog.d** o **syslog-ng**. 
      1. Copie estos comandos y ejecútelos en su dispositivo:
          - Si seleccionó rsyslog.d:
              
            1. Indique al demonio Syslog que escuche en el recurso local_4 y envíe los mensajes de Syslog al agente de Azure Sentinel mediante el puerto 25226. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            
            2. Descargue e instale el [archivo de configuración security_events](https://aka.ms/asi-syslog-config-file-linux) que configura el agente de Syslog para que escuche en el puerto 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Donde {0} debe reemplazarse por el GUID del área de trabajo.
            
            1. Reinicio del demonio Syslog`sudo service rsyslog restart`<br> Para más información, consulte la [documentación de ryslog](https://www.rsyslog.com/doc/v8-stable/tutorials/tls_cert_summary.html).
           
          - Si seleccionó syslog-ng:

              1. Indique al demonio Syslog que escuche en el recurso local_4 y envíe los mensajes de Syslog al agente de Azure Sentinel mediante el puerto 25226. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
              2. Descargue e instale el [archivo de configuración security_events](https://aka.ms/asi-syslog-config-file-linux) que configura el agente de Syslog para que escuche en el puerto 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Donde {0} debe reemplazarse por el GUID del área de trabajo.

              3. Reinicio del demonio Syslog `sudo service syslog-ng restart` <br>Para más información, consulte la [documentación de syslog-ng](https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.16/mutual-authentication-using-tls/2).
      2. Reinicie el agente de Syslog mediante este comando: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
      1. Confirme que no hay ningún error en el registro del agente ejecutando este comando: `tail /var/opt/microsoft/omsagent/log/omsagent.log`

 Para usar el esquema correspondiente en Log Analytics para los eventos CEF, busque `CommonSecurityLog`.


### <a name="deploy-the-agent-on-an-on-premises-linux-server"></a>Implementación del agente en un servidor Linux local

Si no usa Azure, implemente manualmente el agente de Azure Sentinel para su ejecución en un servidor Linux dedicado.


1. En el portal de Azure Sentinel, haga clic en **Data connectors** (Conectores de datos) y seleccione su tipo de dispositivo.
1. Para crear una máquina virtual Linux dedicada, en **Linux Syslog agent configuration** (Configuración de agente de Syslog de Linux), elija **Implementación manual**.
   1. En **Download and install the Syslog agent** (Descargar e instalar el agente de Syslog), seleccione **Non-Azure Linux machine** (Máquina Linux que no es de Azure). 
   1. En la pantalla **Agente directo** que se abre, seleccione **Agente para Linux** para descargar el agente o ejecutar este comando para descargarlo en su máquina Linux: `wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w {workspace GUID} -s gehIk/GvZHJmqlgewMsIcth8H6VqXLM9YXEpu0BymnZEJb6mEjZzCHhZgCx5jrMB1pVjRCMhn+XTQgDTU3DVtQ== -d opinsights.azure.com`
      1. En la pantalla del conector, en **Configure and forward Syslog** (Configurar y reenviar Syslog), establezca si su demonio Syslog es **rsyslog.d** o **syslog-ng**. 
      1. Copie estos comandos y ejecútelos en su dispositivo:
         - Si seleccionó rsyslog:
           1. Indique al demonio Syslog que escuche en el recurso local_4 y envíe los mensajes de Syslog al agente de Azure Sentinel mediante el puerto 25226. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            
           2. Descargue e instale el [archivo de configuración security_events](https://aka.ms/asi-syslog-config-file-linux) que configura el agente de Syslog para que escuche en el puerto 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Donde {0} debe reemplazarse por el GUID del área de trabajo.
           3. Reinicio del demonio Syslog `sudo service rsyslog restart`
         - Si seleccionó syslog-ng:
            1. Indique al demonio Syslog que escuche en el recurso local_4 y envíe los mensajes de Syslog al agente de Azure Sentinel mediante el puerto 25226. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
            2. Descargue e instale el [archivo de configuración security_events](https://aka.ms/asi-syslog-config-file-linux) que configura el agente de Syslog para que escuche en el puerto 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` Donde {0} debe reemplazarse por el GUID del área de trabajo.
            3. Reinicio del demonio Syslog `sudo service syslog-ng restart`
      1. Reinicie el agente de Syslog mediante este comando: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`
      1. Confirme que no hay ningún error en el registro del agente ejecutando este comando: `tail /var/opt/microsoft/omsagent/log/omsagent.log`
  
 Para usar el esquema correspondiente en Log Analytics para los eventos CEF, busque `CommonSecurityLog`.

## <a name="step-2-forward-common-event-format-cef-logs-to-syslog-agent"></a>Paso 2: Reenviar los registros de Common Event Format (CEF) al agente de Syslog

Establezca su solución de seguridad para enviar mensajes de Syslog en formato CEF a su agente de Syslog. Asegúrese de usar los mismos parámetros que aparecen en la configuración del agente. Por lo general, son:

- Puerto 514
- Recurso local4

## <a name="step-3-validate-connectivity"></a>Paso 3: Validar conectividad

Hasta que los registros empiecen a aparecer en Log Analytics, pueden transcurrir más de 20 minutos. 

1. Asegúrese de usar el recurso adecuado. El recurso debe ser el mismo en su dispositivo y en Azure Sentinel. Puede comprobar qué archivo de recurso usa en Azure Sentinel y modificarlo en el archivo `security-config-omsagent.conf`. 

2. Asegúrese de que sus registros tienen acceso al puerto adecuado en el agente de Syslog. Ejecute este comando en la máquina del agente de Syslog: `tcpdump -A -ni any  port 514 -vv` Este comando le muestra los registros que se transmiten a la máquina Syslog desde el dispositivo. Asegúrese de que los registros se reciben del dispositivo de origen en el puerto y el recurso adecuados.

3. Asegúrese de que los registros que envía cumplen el [RFC 5424](https://tools.ietf.org/html/rfc542).

4. En el equipo que ejecuta el agente de Syslog, asegúrese de que los puertos 514 y 25226 están abiertos y escuchan mediante el comando `netstat -a -n:`. Para obtener más información sobre cómo usar este comando, consulte la [página man de Linux netstat(8)](https://linux.die.net/man/8/netstat). Si escucha correctamente, verá:

   ![Puertos de Azure Sentinel](./media/connect-cef/ports.png) 

5. Asegúrese de que el demonio se establece para escuchar en el puerto 514, donde envía los registros.
    - Para rsyslog:<br>Asegúrese de que el archivo `/etc/rsyslog.conf` incluye esta configuración:

           # provides UDP syslog reception
           module(load="imudp")
           input(type="imudp" port="514")
        
           # provides TCP syslog reception
           module(load="imtcp")
           input(type="imtcp" port="514")

      Para más información, consulte [imudp: módulo de entrada de Syslog de UDP](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imudp.html#imudp-udp-syslog-input-module) e [imtcp: módulo de entrada de Syslog de TCP](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imtcp.html#imtcp-tcp-syslog-input-module).

   - Para syslog-ng:<br>Asegúrese de que el archivo `/etc/syslog-ng/syslog-ng.conf` incluye esta configuración:

           # source s_network {
            network( transport(UDP) port(514));
             };
     Para más información, consulte [imudp: módulo de entrada de Syslog de UDP] (Para más información, consulte la [guía de administración de la edición Open Source Edition 3.16 de syslog-ng](https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.16/administration-guide/19#TOPIC-956455).

1. Compruebe que hay comunicación entre el demonio Syslog y el agente de Syslog. Ejecute este comando en la máquina del agente de Syslog: `tcpdump -A -ni any  port 25226 -vv` Este comando muestra los registros que se transmiten desde el dispositivo a la máquina de Syslog. Asegúrese de que los registros también se reciben en el agente.

6. Si ambos comandos proporcionaron resultados correctos, compruebe Log Analytics para ver si llegan sus registros. Todos los eventos transmitidos desde estos dispositivos aparecen con formato sin procesar en Log Analytics en el tipo `CommonSecurityLog`.

7. Para comprobar si hay errores o si los registros no llegan, eche un vistazo a `tail /var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log`. Si indica que hay errores de coincidencia de formato de registro, vaya a `/etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"`, examine el archivo `security_events.conf` y asegúrese de que los registros coinciden con el formato regex que ve en este archivo.

8. Asegúrese de que el tamaño predeterminado de los mensajes de Syslog se limita a 2048 bytes (2 KB). Si los registros son demasiado largos, actualice security_events.conf mediante este comando: `message_length_limit 4096`


## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar dispositivos CEF a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats.md).

