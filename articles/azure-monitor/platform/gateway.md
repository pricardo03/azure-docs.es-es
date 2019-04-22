---
title: Conectar equipos a través de la puerta de enlace de Log Analytics | Microsoft Docs
description: Conecte los dispositivos y equipos supervisados por Operations Manager mediante el uso de la puerta de enlace de Log Analytics para enviar datos al servicio Log Analytics y Azure Automation cuando no tienen acceso a internet.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ae9a1623-d2ba-41d3-bd97-36e65d3ca119
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: magoedte
ms.openlocfilehash: b0b221a9fe6c6482e8759664c297dbd25d0ee776
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59699277"
---
# <a name="connect-computers-without-internet-access-by-using-the-log-analytics-gateway-in-azure-monitor"></a>Conectar equipos sin acceso a internet a través de la puerta de enlace de Log Analytics en Azure Monitor

>[!NOTE]
>Como Microsoft Operations Management Suite (OMS) realiza la transición a Microsoft Azure Monitor, está cambiando la terminología. Este artículo se refiere a la puerta de enlace de OMS como la puerta de enlace de Azure Log Analytics. 
>

En este artículo se describe cómo configurar la comunicación con Azure Automation y Azure Monitor con la puerta de enlace de Log Analytics cuando los equipos que están conectados directamente o que están supervisados por Operations Manager no tienen acceso a internet. 

La puerta de enlace de Log Analytics es un proxy de reenvío de HTTP que admite la tunelización HTTP mediante el comando HTTP CONNECT. Esta puerta de enlace puede recopilar datos y enviarlos a Azure Automation y un área de trabajo de Log Analytics en Azure Monitor en nombre de los equipos que no están conectados a internet.  

La puerta de enlace de Log Analytics admite lo siguiente:

* Informes hasta la misma cuatro Log Analytics de agentes del área de trabajo que están detrás de él y que están configurados con Hybrid Runbook Workers de Azure Automation.  
* Equipos de Windows en el que Microsoft Monitoring Agent está conectado directamente a un área de trabajo de Log Analytics en Azure Monitor.
* Equipos de Linux en el que un agente de Log Analytics para Linux se conecta directamente a un área de trabajo de Log Analytics en Azure Monitor.  
* System Center Operations Manager 2012 SP1 con UR7, Operations Manager 2012 R2 con UR3 o un grupo de administración en Operations Manager 2016 o posterior que se integra con Log Analytics.  

Algunas directivas de seguridad de TI no permiten la conexión a internet para los equipos de red. Estos equipos no conectados podrían ser el punto de dispositivos de venta (PDV) o los servidores que admiten servicios de TI, por ejemplo. Para conectar estos dispositivos a Azure Automation o un área de trabajo de Log Analytics para que pueda administrar y supervisar, configurar para comunicarse directamente con la puerta de enlace de Log Analytics. La puerta de enlace de Log Analytics puede recibir información de configuración y reenviar los datos en su nombre. Si los equipos están configurados con el agente de Log Analytics para conectarse directamente a un área de trabajo de Log Analytics, los equipos se comunicarán en su lugar con la puerta de enlace de Log Analytics.  

La puerta de enlace de Log Analytics transfiere datos desde los agentes al servicio directamente. No analiza ninguno de los datos en tránsito.

Cuando un grupo de administración de Operations Manager se integra con Log Analytics, se pueden configurar los servidores de administración para conectarse a la puerta de enlace de Log Analytics para recibir información de configuración y enviar los datos recopilados, dependiendo de la solución que haya habilitado .  Agentes de Operations Manager envían algunos datos al servidor de administración. Por ejemplo, agentes pueden enviar alertas de Operations Manager, los datos de evaluación de configuración, datos del espacio de instancia y datos de capacidad. Otros datos de gran volumen, como los registros de Internet Information Services (IIS), datos de rendimiento y eventos de seguridad, se envían directamente a la puerta de enlace de Log Analytics. 

Si se implementan uno o más servidores de puerta de enlace de Operations Manager para supervisar sistemas no confiables en una red perimetral o en una red aislada, esos servidores no pueden comunicarse con una puerta de enlace de Log Analytics.  Servidores de puerta de enlace de administrador de operaciones solo pueden informar a un servidor de administración.  Cuando un grupo de administración de Operations Manager está configurado para comunicarse con la puerta de enlace de Log Analytics, la información de configuración de proxy se distribuye automáticamente a todos los equipos administrados con agente que está configurado para recopilar datos de registro de Azure Monitor, incluso si la configuración está vacía.    

Para proporcionar alta disponibilidad para directamente conectados o grupos de administración de operaciones que se comunican con un área de trabajo de Log Analytics a través de la puerta de enlace, use la red equilibrio de carga (NLB) para redirigir y distribuir el tráfico entre varios servidores de puerta de enlace. De este modo, si un servidor de puerta de enlace deja de funcionar, el tráfico se redirige a otro nodo disponible.  

El equipo que ejecuta la puerta de enlace de Log Analytics requiere el agente de Windows de Log Analytics identificar los puntos de conexión de servicio que necesita comunicarse con la puerta de enlace. El agente también se necesita para dirigir la puerta de enlace para informar a las áreas de trabajo mismos que los agentes o grupo de administración de Operations Manager detrás de la puerta de enlace se configuran con. Esta configuración permite que la puerta de enlace y el agente para comunicarse con su área de trabajo asignado.

Una puerta de enlace puede ser varios hosts en hasta cuatro áreas de trabajo. Este es el número total de las áreas de trabajo que admite un agente de Windows.  

Cada agente debe tener conectividad de red a la puerta de enlace para que los agentes puedan transferir automáticamente datos hacia y desde la puerta de enlace. Evite la instalación de la puerta de enlace en un controlador de dominio.

El siguiente diagrama muestra los datos que fluyen desde los agentes directos, a través de la puerta de enlace a Azure Automation y Log Analytics. La configuración de proxy del agente debe coincidir con el puerto configurado con la puerta de enlace de Log Analytics.  

![Diagrama de comunicación de agente directo con los servicios](./media/gateway/oms-omsgateway-agentdirectconnect.png)

El siguiente diagrama muestra el flujo de datos de un grupo de administración de Operations Manager a Log Analytics.   

![Diagrama de comunicación de Operations Manager con Log Analytics](./media/gateway/log-analytics-agent-opsmgrconnect.png)

## <a name="set-up-your-system"></a>Configurar el sistema

Los equipos designados para ejecutar la puerta de enlace de Log Analytics deben tener la siguiente configuración:

* Windows 10, Windows 8.1 o Windows 7
* Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008
* Microsoft .NET Framework 4.5
* Al menos un procesador de 4 núcleos y 8 GB de memoria 
* Un [agente de Log Analytics para Windows](agent-windows.md) que está configurado para informar a la misma área de trabajo que los agentes que se comunican a través de la puerta de enlace

### <a name="language-availability"></a>Disponibilidad de idiomas

La puerta de enlace de Log Analytics está disponible en los siguientes idiomas:

- Chino (simplificado)
- Chino (tradicional)
- Checo
- Neerlandés
- English
- Francés
- Alemán
- Húngaro
- Italiano
- Japonés
- Coreano
- Polaco
- Portugués (Brasil)
- Portugués (Portugal)
- Ruso
- Español (internacional)

### <a name="supported-encryption-protocols"></a>Protocolos de cifrado admitidos
La puerta de enlace de Log Analytics admite solo capa de transporte (TLS) 1.0, 1.1 y 1.2.  No admite capa de Sockets seguros (SSL).  Para garantizar la seguridad de datos en tránsito a Log Analytics, configure la puerta de enlace como mínimo TLS 1.2. Las versiones anteriores de TLS o SSL son vulnerables. Aunque permite actualmente compatibilidad con versiones anteriores, evitar su uso.  

Para información adicional, revise [Sending data securely using TLS 1.2](../../azure-monitor/platform/data-security.md#sending-data-securely-using-tls-12) (Envío de datos de forma segura mediante TLS 1.2). 

### <a name="supported-number-of-agent-connections"></a>Número admitido de conexiones del agente
En la tabla siguiente se muestra aproximadamente cuántos agentes puedan comunicarse con un servidor de puerta de enlace. Soporte técnico se basa en los agentes que cargan aproximadamente 200 KB de datos cada 6 segundos. Para cada agente de prueba, el volumen de datos es aproximadamente 2,7 GB por día.

|Puerta de enlace |Agentes compatibles (aproximado)|  
|--------|----------------------------------|  
|CPU: Intel Xeon Procesador E5-2660 v3 \@ 2,6 GHz 2 núcleos<br> Memoria: 4 GB<br> Ancho de banda de red: 1 Gbps| 600|  
|CPU: Intel Xeon Procesador E5-2660 v3 \@ 2,6 GHz 4 núcleos<br> Memoria: 8 GB<br> Ancho de banda de red: 1 Gbps| 1000|  

## <a name="download-the-log-analytics-gateway"></a>Descargar la puerta de enlace de Log Analytics

Obtener la versión más reciente del archivo de instalación de puerta de enlace de Log Analytics desde [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=54443) o el portal de Azure.

Para obtener la puerta de enlace de Log Analytics desde Azure portal, siga estos pasos:

1. Vaya a la lista de servicios y, luego, seleccione **Log Analytics**. 
1. Seleccione un área de trabajo.
1. En su hoja de área de trabajo, en **General**, seleccione **Inicio rápido**. 
1. En **Elija un origen de datos para conectarse al área de trabajo**, seleccione **Equipos**.
1. En el **Direct Agent** hoja, seleccione **puerta de enlace de descarga de Log Analytics**.
 
   ![Captura de pantalla de los pasos para descargar la puerta de enlace de Log Analytics](./media/gateway/download-gateway.png)

o 

1. En la hoja de área de trabajo en **Configuración**, seleccione **Configuración avanzada**.
1. Vaya a **orígenes conectados** > **servidores Windows** y seleccione **puerta de enlace de descarga de Log Analytics**.

## <a name="install-log-analytics-gateway-using-setup-wizard"></a>Instalar la puerta de enlace de Log Analytics mediante el Asistente para la instalación

Para instalar una puerta de enlace mediante el Asistente para la instalación, siga estos pasos. 

1. En la carpeta de destino, haga doble clic en **Log Analytics gateway.msi**.
1. En la página **principal**, seleccione **Siguiente**.

   ![Página de captura de pantalla de bienvenida del Asistente para instalación de puerta de enlace](./media/gateway/gateway-wizard01.png)

1. En el **contrato de licencia** página, seleccione **acepto los términos del contrato de licencia** que acepte los términos de licencia del Software de Microsoft y, a continuación, seleccione **siguiente**.
1. En la página de **dirección del proxy y puerto**:

    a. Escriba el número de puerto TCP que se usará para la puerta de enlace. Programa de instalación utiliza este número de puerto para configurar una regla de entrada en Firewall de Windows.  El valor predeterminado es 8080.
      El intervalo válido del número de puerto es 1 y 65535. Si el valor especificado no se encuentra en este intervalo, aparece un mensaje de error.

   b. Si el servidor donde está instalada la puerta de enlace necesita comunicarse a través de un proxy, escriba la dirección de proxy que debe conectarse la puerta de enlace. Por ejemplo, escriba: `http://myorgname.corp.contoso.com:80`.  Si deja este campo en blanco, la puerta de enlace intentará conectarse directamente a internet.  Si el servidor proxy requiere autenticación, especifique el nombre de usuario y la contraseña.

   c. Seleccione **Next** (Siguiente).

   ![Captura de pantalla de configuración del proxy de la puerta de enlace](./media/gateway/gateway-wizard02.png)

1. Si no tiene habilitado Microsoft Update, aparece la página de Microsoft Update y puede elegir habilitarlo. Realice una selección y, a continuación, seleccione **siguiente**. De lo contrario, continúe con el paso siguiente.
1. En el **carpeta de destino** página, deje la carpeta predeterminada C:\Program programa\oms Gateway o escriba la ubicación donde desea instalar la puerta de enlace. Luego, seleccione **Siguiente**.
1. En la página **Preparado para instalar**, seleccione **Instalar**. Si el Control de cuentas de usuario solicita permiso para instalar, seleccione **Sí**.
1. Una vez finalizada la instalación, seleccione **finalizar**. Para comprobar que el servicio se está ejecutando, abra el complemento services.msc y compruebe que **OMS Gateway** aparece en la lista de servicios y que su estado es **ejecutando**.

   ![Captura de pantalla de los servicios locales, con la que se está ejecutando la puerta de enlace de OMS](./media/gateway/gateway-service.png)

## <a name="install-the-log-analytics-gateway-using-the-command-line"></a>Instalar la puerta de enlace de Log Analytics mediante la línea de comandos
El archivo descargado para la puerta de enlace es un paquete de instalador de Windows que admite la instalación silenciosa desde la línea de comandos u otro método automatizado. Si no está familiarizado con las opciones de línea de comandos estándar de Windows Installer, consulte [opciones de línea de comandos](https://docs.microsoft.com/windows/desktop/Msi/command-line-options).   

En la tabla siguiente se resalta los parámetros admitidos por el programa de instalación.

|Parámetros| Notas|
|----------|------| 
|NÚMERO DE PUERTO | Número de puerto TCP para la puerta de enlace para escuchar en |
|PROXY | Dirección IP del servidor proxy |
|INSTALLDIR | Ruta de acceso completa para especificar el directorio de instalación de archivos de software de puerta de enlace |
|NOMBRE DE USUARIO | Id. de usuario para autenticarse con el servidor proxy |
|CONTRASEÑA | Contraseña del usuario Id para autenticar con el proxy |
|LicenseAccepted | Especifique un valor de **1** para comprobar que acepte el contrato de licencia |
|HASAUTH | Especifique un valor de **1** cuando se especifican los parámetros de nombre de usuario/contraseña |
|HASPROXY | Especifique un valor de **1** al especificar la dirección IP para **PROXY** parámetro |

Para instalar la puerta de enlace de forma silenciosa y configurarlo con una dirección de proxy específico, el número de puerto, escriba lo siguiente:

```dos
Msiexec.exe /I “oms gateway.msi” /qn PORTNUMBER=8080 PROXY=”10.80.2.200” HASPROXY=1 LicenseAccepted=1 
```

Con la opción de línea de comandos/qn oculta el programa de instalación, /qb muestra el programa de instalación durante la instalación silenciosa.  

Si tiene que proporcionar credenciales para autenticarse con el proxy, escriba lo siguiente:

```dos
Msiexec.exe /I “oms gateway.msi” /qn PORTNUMBER=8080 PROXY=”10.80.2.200” HASPROXY=1 HASAUTH=1 USERNAME=”<username>” PASSWORD=”<password>” LicenseAccepted=1 
```

Después de la instalación, puede confirmar la configuración está aceptados (exlcuding el nombre de usuario y contraseña) mediante los siguientes cmdlets de PowerShell:

- **Get-OMSGatewayConfig** : devuelve la la puerta de enlace está configurado para escuchar en el puerto TCP.
- **Get-OMSGatewayRelayProxy** : devuelve la dirección IP del servidor proxy está configurado para comunicarse con.

## <a name="configure-network-load-balancing"></a>Configuración de equilibrio de carga de red 
Puede configurar la puerta de enlace para lograr alta disponibilidad con equilibrio de carga de red (NLB) mediante Microsoft [equilibrio de carga de red (NLB)](https://docs.microsoft.com/windows-server/networking/technologies/network-load-balancing), [Azure Load Balancer](../../load-balancer/load-balancer-overview.md), o equilibradores de carga basado en hardware. El equilibrador de carga administra el tráfico al redirigir las conexiones solicitadas desde los agentes de Log Analytics o los servidores de administración de Operations Manager mediante sus nodos. Si un servidor de puerta de enlace deja de funcionar, el tráfico se redirige a otros nodos.

### <a name="microsoft-network-load-balancing"></a>Equilibrio de carga de red de Microsoft
Para más información sobre cómo diseñar e implementar un clúster de equilibrio de carga de red de Windows Server 2016, consulte [Equilibrio de carga de red](https://docs.microsoft.com/windows-server/networking/technologies/network-load-balancing). Los pasos siguientes describen cómo configurar un clúster de equilibrio de carga de red de Microsoft.  

1. Inicie sesión en el servidor Windows que sea miembro del clúster NLB con una cuenta administrativa.  
2. Abra el Administrador de equilibrio de carga de red en el Administrador del servidor, haga clic en **Herramientas** y, luego, en **Administrador de equilibrio de carga de red**.
3. Para conectar un servidor de puerta de enlace de Log Analytics con Microsoft Monitoring Agent instalado, haga clic con el botón derecho en la dirección IP del clúster y, después, seleccione **Agregar host al clúster**. 

    ![Administrador de equilibrio: carga de red agregar Host al clúster](./media/gateway/nlb02.png)
 
4. Escriba la dirección IP del servidor de la puerta de enlace al que desea conectarse. 

    ![Administrador de equilibrio de carga de red: Agregar host al clúster: Conectar](./media/gateway/nlb03.png) 

### <a name="azure-load-balancer"></a>Azure Load Balancer
Para obtener información sobre cómo diseñar e implementar un equilibrador de carga de Azure, consulte [¿qué es Azure Load Balancer?](../../load-balancer/load-balancer-overview.md). Para implementar un equilibrador de carga básicos, siga los pasos descritos en este [quickstart](../../load-balancer/quickstart-create-basic-load-balancer-portal.md) excepto los pasos descritos en la sección **crear servidores back-end**.   

> [!NOTE]
> Configuración del equilibrador de carga de Azure mediante el **SKU básica**, requiere que las máquinas virtuales de Azure pertenecen a un conjunto de disponibilidad. Para obtener más información acerca de los conjuntos de disponibilidad, vea [administrar la disponibilidad de las máquinas virtuales de Windows en Azure](../../virtual-machines/windows/manage-availability.md). Para agregar las máquinas virtuales existentes a un conjunto de disponibilidad, consulte [establecer Azure Resource Manager conjunto de disponibilidad](https://gallery.technet.microsoft.com/Set-Azure-Resource-Manager-f7509ec4).
> 

Después de crea el equilibrador de carga, un grupo de back-end debe crearse, que distribuye el tráfico a uno o más servidores de puerta de enlace. Siga los pasos descritos en la sección del artículo de guía de inicio rápido [crear los recursos para el equilibrador de carga](../../load-balancer/quickstart-create-basic-load-balancer-portal.md#create-resources-for-the-load-balancer).  

>[!NOTE]
>Al configurar el sondeo de estado debe configurarse para usar el puerto TCP del servidor de puerta de enlace. El sondeo de estado agrega o quita los servidores de puerta de enlace de la rotación del equilibrador de carga según su respuesta a las comprobaciones de estado dinámicamente. 
>

## <a name="configure-the-log-analytics-agent-and-operations-manager-management-group"></a>Configurar el agente de Log Analytics y el grupo de administración de Operations Manager
En esta sección, verá cómo configurar agentes de Log Analytics directamente conectado, un grupo de administración de Operations Manager o Hybrid Runbook Workers de Azure Automation con la puerta de enlace de Log Analytics para comunicarse con Azure Automation o Log Analytics.  

### <a name="configure-a-standalone-log-analytics-agent"></a>Configurar a un agente de Log Analytics independiente
Al configurar al agente de Log Analytics, reemplace el valor del servidor proxy con la dirección IP del servidor de puerta de enlace de Log Analytics y su número de puerto. Si ha implementado varios servidores de puerta de enlace detrás de un equilibrador de carga, la configuración de proxy del agente de Log Analytics es la dirección IP virtual del equilibrador de carga.  

>[!NOTE]
>Para instalar el agente de Log Analytics en los equipos de Windows que se conectan directamente a Log Analytics y la puerta de enlace, consulte [equipos Windows conectarse al servicio Log Analytics en Azure](agent-windows.md). Para conectar equipos Linux, consulte [configurar un agente de Log Analytics para equipos Linux en un entorno híbrido](../../azure-monitor/learn/quick-collect-linux-computer.md). 
>

Después de instalar al agente en el servidor de puerta de enlace, debe configurarlo para informar al área de trabajo o los agentes del área de trabajo que se comunican con la puerta de enlace. Si no está instalado el agente de Windows de Log Analytics en la puerta de enlace, el evento 300 se escribe en el registro de eventos de puerta de enlace de OMS, que indica que el agente debe instalarse. Si el agente está instalado pero no se configuran para informar a la misma área de trabajo que los agentes que se comunican a través de él, evento 105 se escribe en el mismo registro, que indica que el agente en la puerta de enlace debe configurarse para informar a la misma área de trabajo que los agentes que co mmunicate con la puerta de enlace.

Después de completar la configuración, reinicie el servicio de puerta de enlace de OMS para aplicar los cambios. En caso contrario, la puerta de enlace rechazará a los agentes que intentan comunicarse con Log Analytics y notificarán el evento 105 en el registro de eventos de puerta de enlace de OMS. Esto también ocurre al agregar o quitar un área de trabajo de la configuración del agente en el servidor de puerta de enlace.   

Para obtener información relacionada con Hybrid Runbook Worker de Automation, consulte [automatización de recursos en su centro de datos o en la nube con Hybrid Runbook Worker](../../automation/automation-hybrid-runbook-worker.md).

### <a name="configure-operations-manager-where-all-agents-use-the-same-proxy-server"></a>Configuración de Operations Manager, donde todos los agentes usan el mismo servidor proxy
La configuración de proxy de Operations Manager se aplica automáticamente a todos los agentes que informan a Operations Manager, incluso si la configuración está vacía.  

Para usar OMS Gateway para admitir Operations Manager, debe tener:

* Microsoft Monitoring Agent (versión 8.0.10900.0 o posterior) instalado en el servidor de puerta de enlace de OMS y configurado con las áreas de trabajo de Log Analytics mismos que el grupo de administración está configurado para informar a.
* Conectividad a Internet. Como alternativa, la puerta de enlace de OMS debe estar conectado a un servidor proxy que está conectado a internet.

> [!NOTE]
> Si no se especifica ningún valor para la puerta de enlace, se insertan valores en blanco para todos los agentes.
>

Si se registra el grupo de administración de Operations Manager con un área de trabajo de Log Analytics por primera vez, no verá la opción para especificar la configuración de proxy para el grupo de administración en la consola del operador. Esta opción está disponible solo si el grupo de administración se ha registrado con el servicio.  

Para configurar la integración, actualizar la configuración de proxy del sistema con Netsh en el sistema donde se ejecuta la consola del operador y en todos los servidores de administración del grupo de administración. Siga estos pasos:

1. Abra un símbolo del sistema con privilegios elevados:

    a. Seleccione **iniciar** y escriba **cmd**.  

   b. Haga clic en **símbolo** y seleccione **ejecutar como administrador**.  

1. Escriba el comando siguiente:

   `netsh winhttp set proxy <proxy>:<port>`

Después de completar la integración con Log Analytics, quite el cambio ejecutando `netsh winhttp reset proxy`. A continuación, en la consola del operador, use el **configurar servidor proxy** opción para especificar el servidor de puerta de enlace de Log Analytics. 

1. En la consola de Operations Manager, en **Operations Management Suite**, seleccione **conexión**y, a continuación, seleccione **configurar servidor Proxy**.

   ![Captura de pantalla de Operations Manager, que muestra la selección configurar servidor Proxy](./media/gateway/scom01.png)

1. Seleccione **usar un servidor proxy para acceder a Operations Management Suite** y, a continuación, escriba la dirección IP del servidor de puerta de enlace de Log Analytics o dirección IP virtual del equilibrador de carga. Tenga cuidado al comenzar con el prefijo `http://`.

   ![Captura de pantalla de Operations Manager, que muestra al proxy de dirección del servidor](./media/gateway/scom02.png)

1. Seleccione **Finalizar**. El grupo de administración de Operations Manager está ahora configurado para comunicarse mediante el servidor de puerta de enlace con el servicio Log Analytics.

### <a name="configure-operations-manager-where-specific-agents-use-a-proxy-server"></a>Configuración de Operations Manager, donde determinados agentes usan un servidor proxy
Para entornos grandes o complejos, es posible que desee solo determinados servidores (o grupos) para usar el servidor de puerta de enlace de Log Analytics.  Estos servidores, no se puede actualizar directamente el agente Operations Manager porque el valor global para el grupo de administración sobrescribe este valor.  En su lugar, invalide la regla utilizada para insertar estos valores.  

> [!NOTE] 
> Use esta técnica de configuración si desea permitir para varios servidores de puerta de enlace de Log Analytics en su entorno.  Por ejemplo, puede requerir que se puede especificar en cada región determinados servidores de puerta de enlace de Log Analytics.
>

Para configurar servidores específicos o grupos que utilicen el servidor de puerta de enlace de Log Analytics: 

1. Abra la consola de Operations Manager y seleccione el área de trabajo **Creación**.  
1. En el área de trabajo Creación, seleccione **Reglas**. 
1. En la barra de herramientas de Operations Manager, seleccione el **ámbito** botón. Si este botón no está disponible, asegúrese de que ha seleccionado un objeto, no una carpeta, en el **supervisión** panel. El cuadro de diálogo **Objetos de módulo de administración de ámbito** muestra una lista de clases, grupos u objetos de destino comunes. 
1. En el **buscar** , introduzca **servicio de mantenimiento** y selecciónela en la lista. Seleccione **Aceptar**.  
1. Busque **regla de configuración de Proxy de Advisor**. 
1. En la barra de herramientas de Operations Manager, seleccione **invalida** y, a continuación, elija **invalidar la regla\para un objeto de clase específico: Servicio de mantenimiento** y seleccionar un objeto en la lista.  O bien, cree un grupo personalizado que contiene el objeto de servicio de mantenimiento de los servidores que desea aplicar esta invalidación. A continuación, aplique la invalidación para el grupo personalizado.
1. En el **propiedades de invalidación** diálogo cuadro, agregue una marca de verificación en la **invalidar** columna junto a la **WebProxyAddress** parámetro.  En el **Reemplazar valor** , escriba la dirección URL del servidor de puerta de enlace de Log Analytics. Tenga cuidado al comenzar con el prefijo `http://`.  

    >[!NOTE]
    > No es necesario habilitar la regla. Ha ya administra automáticamente con una invalidación en el módulo de administración de Microsoft System Center Advisor Secure Reference Override que tiene como destino el grupo Microsoft System Center Advisor Monitoring Server.
    > 

1. Seleccione un módulo de administración de la **Seleccionar módulo de administración de destino** lista o cree un nuevo módulo de administración no sellado seleccionando **New**. 
1. Cuando termine, seleccione **Aceptar**. 

### <a name="configure-for-automation-hybrid-runbook-workers"></a>Configurar para Hybrid Runbook Workers de Automation
Si tiene Hybrid Runbook Workers de Automation en su entorno, siga estos pasos para soluciones temporales manuales configurar la puerta de enlace de OMS para admitir los trabajadores.

Para seguir los pasos descritos en esta sección, deberá conocer la región de Azure donde reside la cuenta de Automation. Para buscar esa ubicación:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).
1. Seleccione el servicio Azure Automation.
1. Seleccione la cuenta de Azure Automation apropiada.
1. Vea su región en **Ubicación**.

   ![Captura de pantalla de la ubicación de la cuenta de Automation en Azure portal](./media/gateway/location.png)

Utilice las siguientes tablas para identificar la dirección URL para cada ubicación.

**Direcciones URL del servicio de datos en tiempo de ejecución del trabajo**

| **Ubicación** | **URL** |
| --- | --- |
| Centro-Norte de EE. UU |ncus-jobruntimedata-prod-su1.azure-automation.net |
| Europa occidental |we-jobruntimedata-prod-su1.azure-automation.net |
| Centro-Sur de EE. UU |scus-jobruntimedata-prod-su1.azure-automation.net |
| Este de EE. UU. 2 |eus2-jobruntimedata-prod-su1.azure-automation.net |
| Canadá central |cc-jobruntimedata-prod-su1.azure-automation.net |
| Europa del Norte |ne-jobruntimedata-prod-su1.azure-automation.net |
| Sudeste de Asia |sea-jobruntimedata-prod-su1.azure-automation.net |
| India Central |cid-jobruntimedata-prod-su1.azure-automation.net |
| Japón |jpe-jobruntimedata-prod-su1.azure-automation.net |
| Australia |ase-jobruntimedata-prod-su1.azure-automation.net |

**Direcciones URL de servicio Agente**

| **Ubicación** | **URL** |
| --- | --- |
| Centro-Norte de EE. UU |ncus-agentservice-prod-1.azure-automation.net |
| Europa occidental |we-agentservice-prod-1.azure-automation.net |
| Centro-Sur de EE. UU |scus-agentservice-prod-1.azure-automation.net |
| Este de EE. UU. 2 |eus2-agentservice-prod-1.azure-automation.net |
| Canadá central |cc-agentservice-prod-1.azure-automation.net |
| Europa del Norte |ne-agentservice-prod-1.azure-automation.net |
| Sudeste de Asia |sea-agentservice-prod-1.azure-automation.net |
| India Central |cid-agentservice-prod-1.azure-automation.net |
| Japón |jpe-agentservice-prod-1.azure-automation.net |
| Australia |ase-agentservice-prod-1.azure-automation.net |

Si el equipo se registra automáticamente como Hybrid Runbook Worker, usar la solución Update Management para administrar la revisión. Siga estos pasos:

1. Agregue las direcciones URL del servicio de datos en tiempo de ejecución del trabajo a la lista de hosts permitidos de puerta de enlace de Log Analytics. Por ejemplo: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
1. Reinicie el servicio de puerta de enlace de Log Analytics mediante el siguiente cmdlet de PowerShell: `Restart-Service OMSGatewayService`

Si el equipo está unido a Azure Automation mediante el cmdlet de registro de Hybrid Runbook Worker, siga estos pasos:

1. Agregue la dirección URL de registro del servicio Agente a la lista de hosts permitidos de la puerta de enlace de Log Analytics. Por ejemplo: `Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`
1. Agregue las direcciones URL del servicio de datos en tiempo de ejecución del trabajo a la lista de hosts permitidos de puerta de enlace de Log Analytics. Por ejemplo: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
1. Reinicie el servicio de puerta de enlace de Log Analytics.
    `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Cmdlets de PowerShell útiles
Puede usar cmdlets para completar las tareas para actualizar los valores de configuración de la puerta de enlace del Log Analytics. Para poder usar los cmdlets, asegúrese de:

1. Instale la puerta de enlace de Log Analytics (Microsoft Windows Installer).
1. Abra una ventana de la consola de PowerShell.
1. Importe el módulo, escriba este comando: `Import-Module OMSGateway`
1. Si no se ha producido ningún error en el paso anterior, el módulo se ha importado correctamente y se pueden usar los cmdlets. Escriba `Get-Module OMSGateway`.
1. Después de usar los cmdlets para realizar cambios, reinicie el servicio de puerta de enlace de OMS.

Un error en el paso 3 significa que no se ha importado el módulo. El error podría producirse si PowerShell no puede encontrar el módulo. Puede encontrar el módulo en la ruta de instalación de puerta de enlace de OMS: *C:\Archivos de programa\Microsoft OMS Gateway\PowerShell\OmsGateway*.

| **Cmdlet** | **Parámetros** | **Descripción** | **Ejemplo** |
| --- | --- | --- | --- |  
| `Get-OMSGatewayConfig` |Clave |Obtiene la configuración del servicio |`Get-OMSGatewayConfig` |  
| `Set-OMSGatewayConfig` |Clave (se requiere) <br> Valor |Cambia la configuración del servicio |`Set-OMSGatewayConfig -Name ListenPort -Value 8080` |  
| `Get-OMSGatewayRelayProxy` | |Obtiene la dirección del proxy de retransmisión (ascendente) |`Get-OMSGatewayRelayProxy` |  
| `Set-OMSGatewayRelayProxy` |Dirección<br> Nombre de usuario<br> Contraseña |Establece la dirección (y credencial) del proxy de retransmisión (ascendente) |1. Establezca un proxy de retransmisión y la credencial:<br> `Set-OMSGatewayRelayProxy`<br>`-Address http://www.myproxy.com:8080`<br>`-Username user1 -Password 123` <br><br> 2. Establezca un proxy de retransmisión que no necesite autenticación: `Set-OMSGatewayRelayProxy`<br> `-Address http://www.myproxy.com:8080` <br><br> 3. Desactive la configuración del proxy de retransmisión:<br> `Set-OMSGatewayRelayProxy` <br> `-Address ""` |  
| `Get-OMSGatewayAllowedHost` | |Obtiene el host permitido actualmente (solo el host permitido configurado localmente, que se descargan automáticamente no permite hosts) |`Get-OMSGatewayAllowedHost` | 
| `Add-OMSGatewayAllowedHost` |Host (obligatorio) |Agrega el host a la lista de permitidos |`Add-OMSGatewayAllowedHost -Host www.test.com` |  
| `Remove-OMSGatewayAllowedHost` |Host (obligatorio) |Quita el host de la lista de permitidos |`Remove-OMSGatewayAllowedHost`<br> `-Host www.test.com` |  
| `Add-OMSGatewayAllowedClientCertificate` |Asunto (obligatorio) |Agrega el asunto del certificado de cliente a la lista de permitidos |`Add-OMSGatewayAllowed`<br>`ClientCertificate` <br> `-Subject mycert` |  
| `Remove-OMSGatewayAllowedClientCertificate` |Asunto (obligatorio) |Quita el asunto del certificado de cliente de la lista de permitidos |`Remove-OMSGatewayAllowed` <br> `ClientCertificate` <br> `-Subject mycert` |  
| `Get-OMSGatewayAllowedClientCertificate` | |Obtiene a asuntos de certificado de cliente permitido actualmente (solo los asuntos permitidos configurados localmente, que se descarga automáticamente los asuntos permiten) |`Get-`<br>`OMSGatewayAllowed`<br>`ClientCertificate` |  

## <a name="troubleshooting"></a>solución de problemas
Para recopilar eventos registrados por la puerta de enlace, debe tener instalado el agente de Log Analytics.

![Captura de pantalla de la lista de Visor de eventos en el registro de puerta de enlace de Log Analytics](./media/gateway/event-viewer.png)

### <a name="log-analytics-gateway-event-ids-and-descriptions"></a>Identificadores de evento de puerta de enlace de log Analytics y descripciones

En la tabla siguiente se muestra el Id. de evento y descripciones para los eventos del registro de puerta de enlace de Log Analytics.

| **Id** | **Descripción** |
| --- | --- |
| 400 |Cualquier error de la aplicación que no tenga ningún identificador específico. |
| 401 |Configuración errónea. Por ejemplo, listenPort = "text" en lugar de un entero. |
| 402 |Excepción al analizar los mensajes de protocolo de enlace TLS. |
| 403 |Error de red. Por ejemplo, no se puede conectar al servidor de destino. |
| 100 |Información general. |
| 101 |El servicio se ha iniciado. |
| 102 |El servicio se ha detenido. |
| 103 |Se recibió un comando HTTP CONNECT del cliente. |
| 104 |No es un comando HTTP CONNECT. |
| 105 |Servidor de destino no está en la lista de permitidos o puerto de destino no es seguro (443). <br> <br> Asegúrese de que el agente de MMA en el servidor de puerta de enlace de OMS y los agentes que se comunican con OMS Gateway están conectados a la misma área de trabajo de Log Analytics. |
| 105 |ERROR TcpConnection – Invalid Client certificate: CN = puerta de enlace. <br><br> Asegúrese de que está usando OMS Gateway versión 1.0.395.0 o superior. Asegúrese también de que el agente de MMA en el servidor de puerta de enlace de OMS y los agentes que se comunican con OMS Gateway están conectados a la misma área de trabajo de Log Analytics. |
| 106 |Versión de protocolo TLS/SSL no admitida.<br><br> La puerta de enlace de Log Analytics admite solo TLS 1.0, TLS 1.1 y 1.2. No admite SSL.|
| 107 |Se ha comprobado la sesión de TLS. |

### <a name="performance-counters-to-collect"></a>Para recopilar los contadores de rendimiento

En la tabla siguiente, se muestran los contadores de rendimiento disponibles para la puerta de enlace de Log Analytics. Utilice el Monitor de rendimiento para agregar los contadores.

| **Nombre** | **Descripción** |
| --- | --- |
| Puerta de enlace de Log Analytics/conexión de cliente activa |Número de conexiones de red (TCP) de cliente activas |
| Puerta de enlace de Log Analytics/número de errores |Número de errores |
| Puerta de enlace de Log Analytics/cliente conectado |Número máximo de clientes conectados |
| Puerta de enlace de Log Analytics/número de rechazos |Número de rechazos debido a un error de validación de TLS |

![Interfaz de puerta de enlace de captura de pantalla de Log Analytics, que muestra los contadores de rendimiento](./media/gateway/counters.png)

## <a name="assistance"></a>Obtener ayuda
Cuando haya iniciado sesión en el portal de Azure, puede obtener ayuda con la puerta de enlace de Log Analytics o cualquier otro servicio de Azure o característica.
Para obtener ayuda, seleccione el icono de signo de interrogación en la esquina superior derecha del portal y seleccione **nueva solicitud de soporte técnico**. A continuación, complete el formulario de solicitud de soporte técnico nueva.

![Captura de pantalla de una nueva solicitud de soporte técnico](./media/gateway/support.png)

## <a name="next-steps"></a>Pasos siguientes
[Agregar orígenes de datos](../../azure-monitor/platform/agent-data-sources.md) para recopilar datos desde los orígenes conectados y almacenar los datos en el área de trabajo de Log Analytics.