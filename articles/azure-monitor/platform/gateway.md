---
title: Conectar equipos mediante la puerta de enlace de Log Analytics | Microsoft Docs
description: Conecte los dispositivos y los equipos supervisados por Operations Manager con la puerta de enlace de Log Analytics para enviar datos al servicio de Log Analytics y Azure Automation cuando no tengan acceso a Internet.
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
ms.date: 02/06/2019
ms.author: magoedte
ms.openlocfilehash: e1c225eb77a76015520690916db0399487ffe9e7
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55822619"
---
# <a name="connect-computers-without-internet-access-using-the-log-analytics-gateway"></a>Conectar equipos sin acceso a Internet mediante la puerta de enlace de Log Analytics

>[!NOTE]
>Como parte de la transición continuada de Microsoft Operations Management Suite (OMS) a Azure Monitor, la puerta de enlace de OMS se conocerá como la puerta de enlace de Log Analytics. 
>

En este documento, se describe cómo configurar la comunicación con Azure Automation y Log Analytics mediante la puerta de enlace de Log Analytics cuando los equipos conectados directamente o supervisados por Operations Manager no tengan acceso a Internet.  La puerta de enlace de Log Analytics, que es un proxy de reenvío HTTP que admite la tunelización de HTTP con el comando HTTP CONNECT, puede recopilar datos y enviarlos a Azure Automation y Log Analytics en su nombre.  

La puerta de enlace de Log Analytics admite lo siguiente:

* Envío de informes a las cuatro áreas de trabajo de Log Analytics con las que están configurados los agentes de dicha puerta de enlace  
* Hybrid Runbook Workers de Azure Automation  
* Equipos Windows con Microsoft Monitoring Agent directamente conectado a un área de trabajo de Log Analytics
* Equipos con Linux con el agente de Log Analytics para Linux conectado directamente a un área de trabajo de Log Analytics  
* System Center Operations Manager 2012 SP1 con UR7, Operations Manager 2012 R2 con UR3, el grupo de administración de Operations Manager 2016 o posterior integrado con Log Analytics.  

Si las directivas de seguridad de TI no permiten a los equipos de la red conectarse a Internet (como dispositivos de punto de venta [PDV] o servidores que admiten servicios de TI), pero necesita conectarlos a Azure Automation o Log Analytics para administrarlos y supervisarlos, se pueden configurar de forma que se comuniquen directamente con la puerta de enlace de Log Analytics para recibir la configuración y reenviar los datos en su nombre.  Si estos equipos están configurados con el agente de Log Analytics para conectarse directamente a un área de trabajo de Log Analytics, todos los equipos se comunicarán en su lugar con la puerta de enlace de Log Analytics.  La puerta de enlace transfiere los datos de los agentes al servicio directamente, sin analizar ninguno de los datos en tránsito.

Cuando un grupo de administración de Operations Manager se integra en Log Analytics, se pueden configurar los servidores de administración para que se conecten a la puerta de enlace de Log Analytics para recibir información de configuración y enviar los datos recopilados según la solución que haya habilitado.  Los agentes de Operations Manager envían algunos datos, como las alertas, la evaluación de la configuración, el espacio de las instancias y los datos de capacidad de Operations Manager, al servidor de administración. Otros datos de gran volumen (como registros, rendimiento y eventos de seguridad de IIS) se envían directamente a la puerta de enlace de Log Analytics.  Si ha implementado uno o más servidores de puerta de enlace de Operations Manager en una red perimetral u otra red aislada para supervisar sistemas no confiables, no puede comunicarse con la puerta de enlace de Log Analytics.  Los servidores de puerta de enlace de Operations Manager solo pueden informar a un servidor de administración.  Cuando se configura un grupo de administración de Operations Manager para comunicarse con la puerta de enlace de Log Analytics, la información de configuración de proxy se distribuye automáticamente a todos los equipos administrados por agente y configurados para recopilar datos para Log Analytics, incluso si la configuración está vacía.    

Para proporcionar alta disponibilidad a grupos directamente conectados o grupos de Operations Management que se comunican con Log Analytics mediante la puerta de enlace, puede utilizar el equilibrio de carga de red para redirigir y distribuir el tráfico entre varios servidores de puerta de enlace.  Si un servidor de puerta de enlace deja de funcionar, el tráfico se redirige a otro nodo disponible.  

Se requiere el agente de Log Analytics para Windows en el equipo que ejecuta la puerta de enlace de Log Analytics para identificar los puntos de conexión de servicio que necesita para comunicarse. Además, dicho agente es necesario para informar a las mismas áreas de trabajo con las que se configuraron los agentes o grupos de administración de Operations Manager de la puerta de enlace. Este es un requisito para que la puerta de enlace permita que dichos agentes o grupos se comuniquen con su área de trabajo asignada. Una puerta de enlace puede hospedarse en hasta cuatro áreas de trabajo, ya que este es el número total de áreas de trabajo que admite el agente de Windows.  

Cada agente debe tener conectividad de red a la puerta de enlace para que los agentes puedan transferirle datos y recibirlos de ella automáticamente. No se recomienda instalar la puerta de enlace en un controlador de dominio.

El siguiente diagrama muestra el flujo de datos desde los agentes directos a Azure Automation y Log Analytics mediante el servidor de puerta de enlace. Los agentes tienen que hacer coincidir su configuración de proxy con el mismo puerto configurado en la puerta de enlace de Log Analytics.  

![diagrama de comunicación de agente directo con los servicios](./media/gateway/oms-omsgateway-agentdirectconnect.png)

El siguiente diagrama muestra el flujo de datos de un grupo de administración de Operations Manager a Log Analytics.   

![Diagrama de comunicación de Operations Manager con Log Analytics](./media/gateway/log-analytics-agent-opsmgrconnect.png)

## <a name="prerequisites"></a>Requisitos previos

Al designar un equipo para que ejecute la puerta de enlace de Log Analytics, necesita lo siguiente:

* Windows 10, Windows 8.1, Windows 7
* Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008
* .Net Framework 4.5
* Procesador de 4 núcleos y 8 GB de memoria como mínimo 
* El [agente de Windows para Log Analytics](agent-windows.md) debe estar instalado y configurado para informar a la misma área de trabajo que los agentes que se comunican a través de la puerta de enlace.  

### <a name="language-availability"></a>Disponibilidad de idiomas

La puerta de enlace de Log Analytics está disponible en los idiomas siguientes:

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
La puerta de enlace de Log Analytics solo admite Seguridad de la capa de transporte (TLS) 1.0, 1.1 y 1.2.  No se admite Capa de sockets seguros (SSL).  Para garantizar la seguridad de los datos en tránsito a Log Analytics, se recomienda encarecidamente configurar la puerta de enlace para que use al menos Seguridad de la capa de transporte (TLS) 1.2. Las versiones anteriores de TLS/Capa de sockets seguros (SSL) han demostrado ser vulnerables y, si bien todavía funcionan para permitir la compatibilidad con versiones anteriores, **no se recomiendan**.  Para información adicional, revise [Sending data securely using TLS 1.2](../../azure-monitor/platform/data-security.md#sending-data-securely-using-tls-12) (Envío de datos de forma segura mediante TLS 1.2). 

### <a name="supported-number-of-agent-connections"></a>Número admitido de conexiones del agente
En la siguiente tabla se resalta el número admitido de agentes que se comunican con un servidor de puerta de enlace.  Esta compatibilidad se basa en los agentes que cargan aproximadamente 200 KB de datos cada 6 segundos. El volumen de datos por agente de prueba es de unos 2,7 GB por día.

|Puerta de enlace |Número aproximado de agentes admitidos|  
|--------|----------------------------------|  
|- CPU: Intel XEON CPU E5-2660 v3 \@ 2,6 GHz de 2 núcleos<br> - Memoria: 4 GB<br> - Ancho de banda de la red: 1 Gbps| 600|  
|- CPU: Intel XEON CPU E5-2660 v3 \@ 2,6 GHz de 4 núcleos<br> - Memoria: 8 GB<br> - Ancho de banda de la red: 1 Gbps| 1000|  

## <a name="download-the-log-analytics-gateway"></a>Descargar la puerta de enlace de Log Analytics

Hay dos maneras de descargar la versión más reciente del archivo de instalación de la puerta de enlace de Log Analytics.

1. Descargarla del [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=54443).

1. Descargarla desde Azure Portal.  Después de iniciar sesión en Azure Portal:  

   1. Vaya a la lista de servicios y, luego, seleccione **Log Analytics**.  
   1. Seleccione un área de trabajo.
   1. En su hoja de área de trabajo, en **General**, haga clic en **Inicio rápido**.
   1. En **Elegir un origen de datos para conectarse al área de trabajo**, haga clic en **Equipos**.
   1. En la hoja **Agente directo**, haga clic en **Descargar puerta de enlace de Log Analytics**.<br><br> ![descargar puerta de enlace de Log Analytics](./media/gateway/download-gateway.png)

o 

   1. En la hoja de área de trabajo en **Configuración**, haga clic en **Configuración avanzada**.
   1. Vaya a **Orígenes conectados** > **Servidores de Windows** y haga clic en **Descargar puerta de enlace de Log Analytics**.

## <a name="install-the-log-analytics-gateway"></a>Instalar la puerta de enlace de Log Analytics

Para instalar una puerta de enlace, realice los pasos siguientes.  Si tiene instalada una versión anterior, anteriormente denominada *Reenviador de Log Analytics*, se actualizará a esta versión.  

1. En la carpeta de destino, haga doble clic en **Log Analytics gateway.msi**.
1. En la página **principal**, haga clic en **Siguiente**.<br><br> ![Asistente para la instalación de la puerta de enlace](./media/gateway/gateway-wizard01.png)<br> 
1. En la página del **contrato de licencia**, seleccione **I accept the terms in the License Agreement** (Acepto los términos del Contrato de licencia) para aceptar el CLUF y, después, haga clic en **Siguiente**.
1. En la página de **dirección del proxy y puerto**:
   1. Escriba el número de puerto TCP que se va a usar para la puerta de enlace. El programa de instalación configura una regla de entrada con este número de puerto en el firewall de Windows.  El valor predeterminado es 8080.
      El intervalo válido del número de puerto es 1 - 65535. Si el valor especificado no se encuentra en este intervalo, aparece un mensaje de error.
   1. Opcionalmente, si el servidor en que está instalada la puerta de enlace necesita comunicarse a través de un proxy, escriba la dirección del proxy al que debe conectarse la puerta de enlace. Por ejemplo, `http://myorgname.corp.contoso.com:80`.  Si está en blanco, la puerta de enlace intentará conectarse a Internet directamente.  Si el servidor proxy requiere autenticación, especifique el nombre de usuario y la contraseña.<br><br> ![Configuración del proxy en el Asistente de la puerta de enlace](./media/gateway/gateway-wizard02.png)<br>   
   1. Haga clic en **Next**.
1. Si Microsoft Update no está habilitado, aparecerá la página de Microsoft Update, donde puede elegir habilitarlo. Elija la opción que desee y haga clic en **Siguiente**. De lo contrario, continúe con el paso siguiente.
1. En la página de la **carpeta de destino**, deje la carpeta predeterminada C:\Archivos de programa\OMS Gateway o escriba la ubicación en la que desea instalar la puerta de enlace y haga clic en **Siguiente**.
1. En la página **Preparado para instalar...**, haga clic en **Instalar**. Puede aparecer un Control de cuentas de usuario que solicite permiso para realizar la instalación. Si aparece, haga clic en **Sí**.
1. Una vez completada la instalación, haga clic en **Finalizar**. Para comprobar que el servicio se está ejecutando, abra el complemento services.msc y compruebe que **OMS Gateway** aparece en la lista de servicios y que su estado sea **En ejecución**.<br><br> ![Servicios: puerta de enlace de Log Analytics](./media/gateway/gateway-service.png)  


## <a name="configure-network-load-balancing"></a>Configuración de equilibrio de carga de red 
Puede configurar la puerta de enlace para lograr alta disponibilidad con equilibrio de carga de red (NLB) mediante Equilibrio de carga de red de Microsoft (NLB) o equilibradores de carga basados en hardware.  El equilibrador de carga administra el tráfico al redirigir las conexiones solicitadas desde los agentes de Log Analytics o los servidores de administración de Operations Manager mediante sus nodos. Si un servidor de puerta de enlace deja de funcionar, el tráfico se redirige a otros nodos.

Para más información sobre cómo diseñar e implementar un clúster de equilibrio de carga de red de Windows Server 2016, consulte [Equilibrio de carga de red](https://technet.microsoft.com/windows-server-docs/networking/technologies/network-load-balancing).  Los pasos siguientes describen cómo configurar un clúster de equilibrio de carga de red de Microsoft.  

1. Inicie sesión en el servidor Windows que sea miembro del clúster NLB con una cuenta administrativa.  
1. Abra el Administrador de equilibrio de carga de red en el Administrador del servidor, haga clic en **Herramientas** y, luego, en **Administrador de equilibrio de carga de red**.
1. Para conectar un servidor de puerta de enlace de Log Analytics con la instancia de Microsoft Monitoring Agent instalada, haga clic con el botón derecho en la dirección IP del clúster y, después, seleccione **Agregar host al clúster**.<br><br> ![Administrador de equilibrio de carga de red – Agregar host al clúster](./media/gateway/nlb02.png)<br> 
1. Escriba la dirección IP del servidor de la puerta de enlace al que desea conectarse.<br><br> ![Administrador de equilibrio de carga de red: Agregar host al clúster: Conectar](./media/gateway/nlb03.png) 
    
## <a name="configure-log-analytics-agent-and-operations-manager-management-group"></a>Configurar el agente de Log Analytics y el grupo de administración de Operations Manager
En la sección siguiente, se indican los pasos para configurar los agentes de Log Analytics conectados directamente, un grupo de administración de Operations Manager o Hybrid Runbook Workers de Azure Automation con la puerta de enlace de Log Analytics para comunicarse con Azure Automation o Log Analytics.  

### <a name="configure-standalone-log-analytics-agent"></a>Configurar el agente de Log Analytics independiente
Para obtener información sobre los requisitos y pasos para instalar el agente de Log Analytics en la puerta de enlace y en los equipos con Windows mediante una conexión directa a Log Analytics, consulte [Conexión de equipos Windows a Log Analytics](agent-windows.md) o bien, para equipos con Linux, consulte [Conexión de equipos Linux a Log Analytics](../../azure-monitor/learn/quick-collect-linux-computer.md). En lugar de especificar un servidor proxy al configurar el agente, cambie ese valor por la dirección IP del servidor de puerta de enlace de Log Analytics y el número de puerto. Si ha implementado varios servidores de puerta de enlace detrás de un equilibrador de carga de red, la configuración de proxy del agente de Log Analytics es la dirección IP virtual de NLB.  

Después de instalar al agente en el servidor de la puerta de enlace, puede configurarlo para que informe al área de trabajo o a los agentes de las áreas de trabajo que se comunican con la puerta de enlace. Si el agente de Windows de Log Analytics no está instalado en la puerta de enlace, se escribe el evento 300 en el **Registro de puerta de enlace de OMS** con un mensaje que indica que el agente debe instalarse. Si el agente está instalado, pero no está configurado para informar a las mismas áreas de trabajo que los agentes que se comunican a través de la puerta de enlace, se escribirá el evento 105 en el mismo registro de eventos, indicando que el agente en la puerta de enlace debe configurarse para llevar a cabo dicha comunicación.

Después de completar la configuración, deberá reiniciar el servicio de la **puerta de enlace de OMS** para que los cambios surtan efecto. En caso contrario, la puerta de enlace rechazará los intentos de los agentes por comunicarse con Log Analytics e informará sobre el evento con identificador 105 en el **Registro de puerta de enlace de OMS**. Esto se aplica también al agregar o quitar un área de trabajo de la configuración de agente en el servidor de puerta de enlace.   

Para información relacionada con el Hybrid Runbook Worker de Automation, consulte la [Implementación de Hybrid Runbook Worker](../../automation/automation-hybrid-runbook-worker.md).

### <a name="configure-operations-manager---all-agents-use-the-same-proxy-server"></a>Configuración de Operations Manager: todos los agentes usan el mismo servidor proxy
Configure Operations Manager para agregar la puerta de enlace del servidor.  La configuración de proxy de Operations Manager se aplica automáticamente a todos los agentes que informan a Operations Manager, incluso si la configuración está vacía.  

Si desea utilizar la puerta de enlace para que sea compatible con Operations Manager, debe tener:

* Microsoft Monitoring Agent (versión del agente: **8.0.10900.0** o posterior) instalado en el servidor de la puerta de enlace y configurado con las mismas áreas de trabajo de Log Analytics que configuró para que el grupo de administración enviara informes.
* La puerta de enlace debe tener conectividad a Internet o estar conectada a un servidor proxy que la tenga.

> [!NOTE]
> Si no especifica ningún valor para la puerta de enlace, se insertan valores en blanco para todos los agentes.
> 

Si se trata de la primera vez que se registra el grupo de administración de Operations Manager con un área de trabajo de Log Analytics, la opción para especificar la configuración de proxy para el grupo de administración no está disponible en la consola del operador.  El grupo de administración tiene que estar registrado correctamente con el servicio para que esta opción esté disponible.  Actualice la configuración de proxy del sistema con Netsh en el sistema desde el que se esté ejecutando la consola del operador para configurar la integración y todos los servidores de administración del grupo de administración.  

1. Abra un símbolo del sistema con privilegios elevados.

     a. Vaya a **Inicio** y escriba **cmd**.  
    b. Haga clic con el botón derecho en el **símbolo del sistema** y seleccione **Ejecutar como administrador**.  

1. Escriba el siguiente comando y presione **Entrar**:

    `netsh winhttp set proxy <proxy>:<port>`

Después de completar la integración con Log Analytics, puede quitar el cambio; para hacerlo, ejecute `netsh winhttp reset proxy` y use la opción **Configurar servidor proxy** en la Consola del operador para especificar el servidor de puerta de enlace de Log Analytics. 

1. Abra la consola de Operations Manager y en **Operations Management Suite**, haga clic en **Conexión** y en **Configurar servidor proxy**.<br><br> ![Operations Manager: Configurar servidor proxy](./media/gateway/scom01.png)<br> 
1. Seleccione **Use un servidor proxy para obtener acceso al servicio Operations Management Suite** y, después, escriba la dirección IP del servidor de puerta de enlace de Log Analytics o la dirección IP virtual de NLB. Asegúrese de que su prefijo es `http://`.<br><br> ![Operations Manager: dirección de servidor proxy](./media/gateway/scom02.png)<br> 
1. Haga clic en **Finalizar** El grupo de administración de Operations Manager está ahora configurado para comunicarse mediante el servidor de puerta de enlace con el servicio Log Analytics.

### <a name="configure-operations-manager---specific-agents-use-proxy-server"></a>Configuración de Operations Manager: determinados agentes usan el servidor proxy
En entornos grandes o complejos, puede que solo quiera tener determinados servidores (o grupos) que usen el servidor de puerta de enlace de Log Analytics.  En estos servidores, no se puede actualizar el agente Operations Manager directamente, ya que el valor global para el grupo de administración sobrescribe este valor.  En su lugar, debe invalidar la regla utilizada para insertar estos valores.  

> [!NOTE] 
> Esta misma técnica de configuración se puede usar para permitir el uso de varios servidores de puerta de enlace de Log Analytics en su entorno.  Por ejemplo, puede exigir que se especifiquen determinados servidores de puerta de enlace de Log Analytics en cada región.
>  

1. Abra la consola de Operations Manager y seleccione el área de trabajo **Creación**.  
1. En el área de trabajo Creación, seleccione **Reglas** y haga clic en el botón **Ámbito** de la barra de herramientas de Operations Manager. Si este botón no está disponible, compruebe que tiene un objeto, y no una carpeta, seleccionado en el panel Supervisión. El cuadro de diálogo **Objetos de módulo de administración de ámbito** muestra una lista de clases, grupos u objetos de destino comunes. 
1. Escriba **Servicio de mantenimiento** en el campo **Buscar** y selecciónelo en la lista.  Haga clic en **OK**.  
1. Busque la regla **Advisor Proxy Setting Rule** (Regla de configuración de proxy de Advisor) y en la barra de herramientas de la consola de operaciones, haga clic en **Invalidaciones**, seleccione **Override the Rule\For a specific object of class: Health Service** (Invalidar la regla\Para un objeto de clase específico: servicio de mantenimiento) y, luego, un objeto específico de la lista.  Si lo desea, puede crear un grupo personalizado que incluya el objeto de servicio de mantenimiento de los servidores a los que desea aplicar esta invalidación y, después, aplicar la invalidación para ese grupo.
1. En el cuadro de diálogo **Propiedades de invalidación**, haga clic para poner una marca de verificación en la columna **Invalidar** junto al parámetro **WebProxyAddress**.  En el campo **Valor de invalidación**, escriba la dirección URL del servidor de puerta de enlace de Log Analytics y asegúrese de que el prefijo sea `http://`.  

    >[!NOTE]
    > No es necesario habilitar la regla, porque ya se administra automáticamente con una invalidación incluida en el módulo de administración de reemplazo de referencia segura de Microsoft System Center Advisor dirigido al grupo de servidores de supervisión de Microsoft System Center Advisor.
    >   

1. Seleccione un módulo de administración de la lista **Seleccionar módulo de administración de destino** o cree un nuevo módulo de administración no sellado haciendo clic en **Nuevo**. 
1. Cuando haya completado los cambios, haga clic en **Aceptar**. 

### <a name="configure-for-automation-hybrid-workers"></a>Configuración de Hybrid Workers de Automation
Si tiene Hybrid Runbook Workers de Automation en su entorno, los pasos siguientes proporcionan soluciones temporales manuales para configurar la puerta de enlace y que esta sea compatible con él.

En los pasos siguientes, es preciso saber en qué región de Azure reside la cuenta de Automation. Para buscar la ubicación:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).
1. Seleccione el servicio Azure Automation.
1. Seleccione la cuenta de Azure Automation apropiada.
1. Vea su región en **Ubicación**.<br><br> ![Azure Portal – Ubicación de la cuenta de Automation](./media/gateway/location.png)  

Utilice las siguientes tablas para identificar la dirección URL de cada ubicación:

**Direcciones URL del servicio de datos del tiempo de ejecución del trabajo**

| **ubicación** | **URL** |
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

| **ubicación** | **URL** |
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

Si el equipo se registra automáticamente como Hybrid Runbook Worker para que las revisiones se apliquen mediante la solución de administración de actualizaciones, siga estos pasos:

1. Agregue las direcciones URL del servicio de datos en tiempo de ejecución del trabajo a la lista de hosts permitidos de puerta de enlace de Log Analytics. Por ejemplo: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
1. Reinicie el servicio de puerta de enlace de Log Analytics mediante el siguiente cmdlet de PowerShell: `Restart-Service OMSGatewayService`

Si el equipo está incorporado en Azure Automation mediante el cmdlet de registro de Hybrid Runbook Worker, siga estos pasos:

1. Agregue la dirección URL de registro del servicio Agente a la lista de hosts permitidos de la puerta de enlace de Log Analytics. Por ejemplo: `Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`
1. Agregue las direcciones URL del servicio de datos en tiempo de ejecución del trabajo a la lista de hosts permitidos de puerta de enlace de Log Analytics. Por ejemplo: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
1. Reinicie el servicio de puerta de enlace de Log Analytics.
    `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Cmdlets de PowerShell útiles
Los cmdlets pueden ayudarle a completar las tareas necesarias para actualizar las opciones de configuración de la puerta de enlace de Log Analytics. Antes de usarlos, asegúrese de:

1. Instalar la puerta de enlace de Log Analytics (MSI).
1. Abra una ventana de la consola de PowerShell.
1. Para importar el módulo, escriba este comando: `Import-Module OMSGateway`
1. Si no se ha producido ningún error en el paso anterior, el módulo se ha importado correctamente y se pueden usar los cmdlets. Escriba `Get-Module OMSGateway`
1. Después de realizar los cambios mediante los cmdlets, asegúrese de reiniciar el servicio Gateway.

Un error en el paso 3 indica que el módulo no se ha importado. El error puede producirse si PowerShell no puede encontrar el módulo. Puede encontrarlo en la ruta de instalación de la puerta de enlace: *C:\Archivos de programa\Microsoft OMS Gateway\PowerShell\OmsGateway*.

| **Cmdlet** | **Parámetros** | **Descripción** | **Ejemplo** |
| --- | --- | --- | --- |  
| `Get-OMSGatewayConfig` |Clave |Obtiene la configuración del servicio |`Get-OMSGatewayConfig` |  
| `Set-OMSGatewayConfig` |Clave (se requiere) <br> Valor |Cambia la configuración del servicio |`Set-OMSGatewayConfig -Name ListenPort -Value 8080` |  
| `Get-OMSGatewayRelayProxy` | |Obtiene la dirección del proxy de retransmisión (ascendente) |`Get-OMSGatewayRelayProxy` |  
| `Set-OMSGatewayRelayProxy` |Dirección<br> Nombre de usuario<br> Contraseña |Establece la dirección (y credencial) del proxy de retransmisión (ascendente) |1. Establezca un proxy de retransmisión y la credencial:<br> `Set-OMSGatewayRelayProxy`<br>`-Address http://www.myproxy.com:8080`<br>`-Username user1 -Password 123` <br><br> 2. Establezca un proxy de retransmisión que no necesite autenticación: `Set-OMSGatewayRelayProxy`<br> `-Address http://www.myproxy.com:8080` <br><br> 3. Desactive la configuración del proxy de retransmisión:<br> `Set-OMSGatewayRelayProxy` <br> `-Address ""` |  
| `Get-OMSGatewayAllowedHost` | |Obtiene el host permitido actualmente (solo el host permitido configurado localmente, no se incluyen los hosts permitidos que se descargan automáticamente) |`Get-OMSGatewayAllowedHost` | 
| `Add-OMSGatewayAllowedHost` |Host (obligatorio) |Agrega el host a la lista de permitidos |`Add-OMSGatewayAllowedHost -Host www.test.com` |  
| `Remove-OMSGatewayAllowedHost` |Host (obligatorio) |Quita el host de la lista de permitidos |`Remove-OMSGatewayAllowedHost`<br> `-Host www.test.com` |  
| `Add-OMSGatewayAllowedClientCertificate` |Asunto (obligatorio) |Agrega el asunto del certificado de cliente a la lista de permitidos |`Add-OMSGatewayAllowed`<br>`ClientCertificate` <br> `-Subject mycert` |  
| `Remove-OMSGatewayAllowedClientCertificate` |Asunto (obligatorio) |Quita el asunto del certificado de cliente de la lista de permitidos |`Remove-OMSGatewayAllowed` <br> `ClientCertificate` <br> `-Subject mycert` |  
| `Get-OMSGatewayAllowedClientCertificate` | |Obtiene los asuntos de certificado de cliente permitidos actualmente (solo los asuntos permitidos configurados localmente, no se incluyen los asuntos permitidos que se descargan automáticamente) |`Get-`<br>`OMSGatewayAllowed`<br>`ClientCertificate` |  

## <a name="troubleshooting"></a>solución de problemas
Para recopilar eventos registrados por la puerta de enlace, también necesita tener instalado el agente de Log Analytics.<br><br> ![Visor de eventos: registro de la puerta de enlace de Log Analytics](./media/gateway/event-viewer.png)

**Identificadores de evento y descripciones de la puerta de enlace de Log Analytics**

En la tabla siguiente, se muestran los identificadores de evento y las descripciones de los eventos de registro de la puerta de enlace de Log Analytics.

| **Id** | **Descripción** |
| --- | --- |
| 400 |Cualquier error de la aplicación que no tiene un identificador específico |
| 401 |Configuración errónea. Por ejemplo: listenPort = "text", en lugar de un entero |
| 402 |Excepción al analizar los mensajes de protocolo de enlace TLS |
| 403 |Error de red. Por ejemplo: no se puede conectar al servidor de destino |
| 100 |Información general |
| 101 |El servicio se ha iniciado |
| 102 |El servicio se ha detenido |
| 103 |Se ha recibido un comando HTTP CONNECT del cliente |
| 104 |No es un comando HTTP CONNECT |
| 105 |El servidor de destino no está en la lista de permitidos o el puerto de destino no es un puerto seguro (443) <br> <br> Asegúrese de que el agente MMA del servidor de puerta de enlace y los agentes que se comunican con la puerta de enlace están conectados a la misma área de trabajo de Log Analytics. |
| 105 |ERROR TcpConnection – Invalid Client certificate: CN=Gateway <br><br> Asegúrese de que: <br>    <br> - Utiliza una puerta de enlace con el número de versión 1.0.395.0, o uno superior. <br> - Tanto el agente MMA del servidor de puerta de enlace como los agentes que se comunican con la puerta de enlace están conectados a la misma área de trabajo de Log Analytics. |
| 106 |La puerta de enlace de Log Analytics solo admite TLS 1.0, 1.1 y 1.2.  No admite SSL. Para las versiones de protocolo TLS/SSL no admitidas, la puerta de enlace de Log Analytics genera el identificador de evento 106.|
| 107 |Se ha comprobado la sesión de TLS |

**Contadores de rendimiento que se recopilan**

En la tabla siguiente, se muestran los contadores de rendimiento disponibles para la puerta de enlace de Log Analytics. Los contadores se pueden agregar mediante el Monitor de rendimiento.

| **Nombre** | **Descripción** |
| --- | --- |
| Puerta de enlace de Log Analytics/conexión de cliente activa |Número de conexiones de red (TCP) de cliente activas |
| Puerta de enlace de Log Analytics/número de errores |Número de errores |
| Puerta de enlace de Log Analytics/cliente conectado |Número máximo de clientes conectados |
| Puerta de enlace de Log Analytics/número de rechazos |Número de rechazos debido a un error de validación de TLS |

![Contadores de rendimiento de la puerta de enlace de Log Analytics](./media/gateway/counters.png)

## <a name="get-assistance"></a>Obtención de ayuda
Después de iniciar sesión en Azure Portal, puede crear una solicitud de asistencia con la puerta de enlace de Log Analytics o con cualquier otro servicio o característica de Azure.
Para solicitar asistencia, haga clic en el signo de interrogación en la esquina superior derecha del portal y, después, en **Nueva solicitud de soporte técnico**. A continuación, complete el formulario de solicitud de soporte técnico nuevo.

![Nueva solicitud de soporte](./media/gateway/support.png)

## <a name="next-steps"></a>Pasos siguientes
[Agregue orígenes de datos](../../azure-monitor/platform/agent-data-sources.md) para recopilar datos de los orígenes conectados y almacénelos en el área de trabajo de Log Analytics.
