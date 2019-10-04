---
title: Conexión de datos de Palo Alto Networks con Azure Sentinel | Microsoft Docs
description: Aprenda a conectar datos de Palo Alto Networks a Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: a4b21d67-1a72-40ec-bfce-d79a8707b4e1
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: rkarlin
ms.openlocfilehash: 659f36a036d8a165b0c2b28830ae2312adb56c56
ms.sourcegitcommit: 992e070a9f10bf43333c66a608428fcf9bddc130
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2019
ms.locfileid: "71240234"
---
# <a name="connect-your-palo-alto-networks-appliance"></a>Conexión del dispositivo de Palo Alto Networks



Puede conectar Azure Sentinel a cualquier dispositivo de Palo Alto Networks guardando los archivos de registro como Common Error Format (CEF) de Syslog. La integración con Azure Sentinel permite ejecutar análisis y consultas fácilmente en los datos de archivo de Palo Alto Networks. Para obtener más información sobre cómo ingiere Azure Sentinel datos CEF, consulte el artículo sobre cómo [conectar dispositivos CEF](connect-common-event-format.md).

> [!NOTE]
> Los datos se almacenarán en la ubicación geográfica del área de trabajo en la que Azure Sentinel se ejecute.

## <a name="step-1-connect-your-palo-alto-networks-appliance-using-an-agent"></a>Paso 1: Conectar el dispositivo de Palo Alto Networks mediante un agente

Para conectar su dispositivo de Palo Alto Networks a Azure Sentinel, debe implementar un agente en una máquina dedicada (máquina virtual o local) para admitir la comunicación entre el dispositivo y Azure Sentinel. 

También puede implementar el agente manualmente en una máquina virtual existente, en una máquina virtual en otra nube o en una máquina local.

> [!NOTE]
> Asegúrese de configurar la seguridad de la máquina de acuerdo con la directiva de seguridad de su organización. Por ejemplo, puede configurar la red para que se alinee con la directiva de seguridad de la red corporativa y cambiar los puertos y protocolos del demonio para que se adapten a sus requisitos. 

Para ver un diagrama de red de ambas opciones, consulte [Conexión con orígenes de datos](connect-data-sources.md#agent-options).


### <a name="deploy-the-agent"></a>Implementar el agente 

1. En el portal de Azure Sentinel, haga clic en **Data connectors** (Conectores de datos) y seleccione **Palo Alto Networks** y, luego, **Open connector page** (Abrir página de conectores). 

1. En **Download and install the Syslog agent** (Descargar e instalar el agente de Syslog), seleccione el tipo de máquina, ya sea en Azure o local. 
1. En la pantalla **Máquinas virtuales** que se abre, seleccione la máquina que quiere usar y haga clic en **Conectar**.
1. Si elige **Download and install agent for Azure Linux virtual machines** (Descargar e instalar el agente para máquinas virtuales Linux de Azure), seleccione la máquina y haga clic en **Conectar**. Si eligió **Download and install agent for non-Azure Linux virtual machines** (Descargar e instalar el agente para máquinas virtuales Linux que no son de Azure), en la pantalla de **Agente directo**, ejecute el script en **Descargar e incorporar Agente para Linux**.      1. En la pantalla del conector, en **Configure and forward Syslog** (Configurar y reenviar Syslog), establezca si su demonio Syslog es **rsyslog.d** o **syslog-ng**. 
1. Copie estos comandos y ejecútelos en su dispositivo:
     - Si seleccionó rsyslog.d:
              
       1. Indique al demonio Syslog que escuche en el recurso local_4 y envíe los mensajes de Syslog al agente de Azure Sentinel mediante el puerto 25226. `sudo bash -c "printf 'local4.debug  @127.0.0.1:25226' > /etc/rsyslog.d/security-config-omsagent.conf"`
            
       2. Descargue e instale el [archivo de configuración security_events](https://aka.ms/asi-syslog-config-file-linux) que configura el agente de Syslog para que escuche en el puerto 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` en que {0} debe reemplazarse por el GUID del área de trabajo.
            
       1. Reinicie el demonio Syslog `sudo service rsyslog restart`.
             
    - Si seleccionó syslog-ng:

         1. Indique al demonio Syslog que escuche en el recurso local_4 y envíe los mensajes de Syslog al agente de Azure Sentinel mediante el puerto 25226. `sudo bash -c "printf 'filter f_local4_oms { facility(local4); };\n  destination security_oms { tcp(\"127.0.0.1\" port(25226)); };\n  log { source(src); filter(f_local4_oms); destination(security_oms); };' > /etc/syslog-ng/security-config-omsagent.conf"`
         2. Descargue e instale el [archivo de configuración security_events](https://aka.ms/asi-syslog-config-file-linux) que configura el agente de Syslog para que escuche en el puerto 25226. `sudo wget -O /etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"` en que {0} debe reemplazarse por el GUID del área de trabajo.

         3. Reinicio del demonio Syslog `sudo service syslog-ng restart`.
 1. Reinicie el agente de Syslog mediante este comando: `sudo /opt/microsoft/omsagent/bin/service_control restart [{workspace GUID}]`.
 1. Confirme que no hay ningún error en el registro del agente ejecutando este comando: `tail /var/opt/microsoft/omsagent/log/omsagent.log`.


 
## <a name="step-2-forward-palo-alto-networks-logs-to-the-syslog-agent"></a>Paso 2: Reenviar los registros de Palo Alto Networks al agente de Syslog

Configure Palo Alto Networks para reenviar mensajes de Syslog en formato CEF a su área de trabajo de Azure a través del agente de Syslog:
1.  Vaya a [Common Event Format (CEF) Configuration Guides](https://docs.paloaltonetworks.com/resources/cef) (Guías de configuración de Common Event Format [CEF]) y descargue el PDF para su tipo de dispositivo. Siga todas las instrucciones de la guía para configurar su dispositivo de Palo Alto Networks a fin de recopilar eventos CEF. 

1.  Vaya a [Configure Syslog monitoring](https://aka.ms/asi-syslog-paloalto-forwarding) (Configuración de supervisión de Syslog) y siga los pasos 2 y 3 para configurar el reenvío de eventos CEF de su dispositivo de Palo Alto Networks a Azure Sentinel.

    1. Asegúrese de establecer el **formato de servidor de Syslog** en **BSD**.
    1. Asegúrese de establecer el **número de recurso**  en el mismo valor que estableció en el agente de Syslog.

       > [!NOTE]
       > Las operaciones de copiar y pegar desde el archivo PDF pueden cambiar el texto e insertar caracteres aleatorios. Para evitarlo, copie el texto en un editor y quite los caracteres que pueden interrumpir el formato del registro antes de pegarlo, como puede ver en este ejemplo.
 
        ![Problema al copiar texto CEF](./media/connect-cef/paloalto-text-prob1.png)

2. Para usar el esquema correspondiente en Log Analytics para encontrar eventos de Palo Alto Networks, busque **CommonSecurityLog**.

## <a name="step-3-validate-connectivity"></a>Paso 3: Validar conectividad

Hasta que los registros empiecen a aparecer en Log Analytics, pueden transcurrir más de 20 minutos. 

1. Asegúrese de usar el recurso adecuado. El recurso debe ser el mismo en su dispositivo y en Azure Sentinel. Puede comprobar qué archivo de recurso usa en Azure Sentinel y modificarlo en el archivo `security-config-omsagent.conf`. 

2. Asegúrese de que sus registros tienen acceso al puerto adecuado en el agente de Syslog. Ejecute este comando en la máquina del agente de Syslog: `tcpdump -A -ni any  port 514 -vv` Este comando le muestra los registros que se transmiten a la máquina Syslog desde el dispositivo. Asegúrese de que los registros se reciben del dispositivo de origen en el puerto y el recurso adecuados.

3. Asegúrese de que los registros que envía cumplen con [RFC 3164](https://tools.ietf.org/html/rfc3164).

4. En el equipo que ejecuta el agente de Syslog, asegúrese de que los puertos 514 y 25226 están abiertos y escuchan mediante el comando `netstat -a -n:`. Para obtener más información sobre cómo usar este comando, consulte la [página man de Linux netstat(8)](https://linux.die.net/man/8/netstat). Si escucha correctamente, verá:

   ![Puertos de Azure Sentinel](./media/connect-cef/ports.png) 

5. Asegúrese de que el demonio se establece en el puerto 514, donde envía los registros.
    - Para rsyslog:<br>Asegúrese de que el archivo `/etc/rsyslog.conf` incluye esta configuración:

           # provides UDP syslog reception
           module(load="imudp")
           input(type="imudp" port="514")
        
           # provides TCP syslog reception
           module(load="imtcp")
           input(type="imtcp" port="514")

      Para obtener más información, consulte las páginas [imudp: UDP Syslog Input Module](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imudp.html#imudp-udp-syslog-input-module) (imudp: módulo de entrada de Syslog de UDP) e [imtcp: TCP Syslog Input Module](https://www.rsyslog.com/doc/v8-stable/configuration/modules/imtcp.html#imtcp-tcp-syslog-input-module) (imtcp: módulo de entrada de Syslog de TCP).

   - Para syslog-ng:<br>Asegúrese de que el archivo `/etc/syslog-ng/syslog-ng.conf` incluye esta configuración:

           # source s_network {
            network( transport(UDP) port(514));
             };
     Para más información, consulte la [guía de administración de la edición Open Source Edition 3.16 de syslog-ng](https://www.syslog-ng.com/technical-documents/doc/syslog-ng-open-source-edition/3.16/administration-guide/19#TOPIC-956455).

1. Compruebe que haya comunicación entre el demonio Syslog y el agente. Ejecute este comando en la máquina del agente de Syslog: `tcpdump -A -ni any  port 25226 -vv` Este comando le muestra los registros que se transmiten a la máquina Syslog desde el dispositivo. Asegúrese de que los registros también se reciben en el agente.

6. Si ambos comandos proporcionaron resultados correctos, compruebe Log Analytics para ver si llegan sus registros. Todos los eventos transmitidos desde estos dispositivos aparecen con formato sin procesar en Log Analytics en el tipo `CommonSecurityLog`.

7. Para comprobar si hay errores o si los registros no llegan, eche un vistazo a `tail /var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log`. Si indica que hay errores de coincidencia de formato de registro, vaya a `/etc/opt/microsoft/omsagent/{0}/conf/omsagent.d/security_events.conf "https://aka.ms/syslog-config-file-linux"`, examine el archivo `security_events.conf` y asegúrese de que los registros coinciden con el formato regex que ve en este archivo.

8. Asegúrese de que el tamaño predeterminado de los mensajes de Syslog se limita a 2048 bytes (2 KB). Si los registros son demasiado largos, actualice security_events.conf mediante este comando: `message_length_limit 4096`.








## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar dispositivos de Palo Alto Networks a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).

