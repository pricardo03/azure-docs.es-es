---
title: Conectar Operations Manager a Azure Monitor | Microsoft Docs
description: Para mantener su inversión existente en System Center Operations Manager y usar las capacidades ampliadas con Log Analytics, puede integrar Operations Manager con el área de trabajo.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: 245ef71e-15a2-4be8-81a1-60101ee2f6e6
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 11/23/2018
ms.author: magoedte
ms.openlocfilehash: 3e11c4dc8aa082723223fb998e599aa514febd3b
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369976"
---
# <a name="connect-operations-manager-to-azure-monitor"></a>Conectar Operations Manager a Azure Monitor

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

Para mantener su inversión existente en [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/key-concepts?view=sc-om-1807) y usar las capacidades ampliadas con Azure Monitor, puede integrar Operations Manager con el área de trabajo de Log Analytics. Esto le permite que aprovechar las oportunidades de registros en Azure Monitor mientras sigue utilizando Operations Manager para:

* Supervisar el estado de los servicios de TI con Operations Manager
* Mantener la integración con sus soluciones ITSM compatibles con la administración de incidentes y problemas
* Administrar el ciclo de vida de los agentes implementados en máquinas virtuales IaaS en una nube pública y en entornos locales supervisados con Operations Manager

Integración con System Center Operations Manager agrega valor a su estrategia de operaciones de servicio mediante el uso de la velocidad y eficacia de Azure Monitor en recopilar, almacenar y analizar datos de registro de Operations Manager. Supervisar registro de Azure consulta correlacionar ayuda y trabajo para identificar los errores de los problemas y sacar a la superficie reapariciones para respaldar el proceso de administración de problema existente. La flexibilidad del motor de consulta para examinar el rendimiento, eventos y alerta de datos con abundantes paneles y funcionalidades de informes para exponer estos datos de maneras significativas muestra la solidez Azure Monitor ofrece como complemento de Operations Manager.

Los agentes que informan al grupo de administración de Operations Manager recopilan datos de los servidores según el [orígenes de datos de Log Analytics](agent-data-sources.md) y soluciones que ha habilitado en el área de trabajo. Dependiendo de las soluciones habilitadas, sus datos se envía directamente desde un servidor de administración de Operations Manager para el servicio, o debido al volumen de datos recopilados en el sistema administrado con agente, se envían directamente desde el agente a un área de trabajo de Log Analytics. El servidor de administración reenvía los datos directamente al servicio. No se escriben nunca en la base de datos operacional o de almacenamiento de datos. Cuando un servidor de administración pierde conectividad con Azure Monitor, almacena en caché los datos localmente hasta que se restableció la comunicación. Si el servidor de administración está sin conexión debido a mantenimiento planeado o interrupción imprevista, otro servidor de administración del grupo de administración reanudará la conectividad con Azure Monitor.  

El siguiente diagrama muestra la conexión entre los agentes y servidores de administración en el Monitor de Azure, incluidos los puertos y la dirección y un grupo de administración de System Center Operations Manager.   

![oms-operations-manager-integration-diagram](./media/om-agents/oms-operations-manager-connection.png)

Si las directivas de seguridad de TI no permiten que los equipos de la red se conecten a Internet, se pueden configurar los servidores de administración de forma que se conecten a la puerta de enlace de Log Analytics para recibir información de configuración y enviar los datos recopilados según las soluciones que haya habilitado. Para obtener más información e instrucciones sobre cómo configurar el grupo de administración de Operations Manager se comunique a través de una puerta de enlace de Log Analytics a Azure Monitor, consulte [conectar equipos a Azure Monitor con la puerta de enlace de Log Analytics](../../azure-monitor/platform/gateway.md).  

## <a name="prerequisites"></a>Requisitos previos 
Antes de comenzar, revise los siguientes requisitos.

* Azure Monitor solo es compatible con System Center Operations Manager 1807, Operations Manager 1801, Operations Manager 2016, Operations Manager 2012 SP1 UR6 o versiones posteriores y Operations Manager 2012 R2 UR2 o una versión posterior. Se agregó compatibilidad con proxy en Operations Manager 2012 SP1 UR7 y Operations Manager 2012 R2 UR3.
* Todos los agentes de Operations Manager deben cumplir los requisitos mínimos de compatibilidad. Asegúrese de que los agentes están actualizados con los requisitos mínimos ya que, de lo contrario, se podría producir un error de comunicación en el agente de Windows así como errores en el registro de eventos de Operations Manager.
* Un área de trabajo de Log Analytics. Para más información, consulte la [introducción a las áreas de trabajo de Log Analytics](../../azure-monitor/platform/manage-access.md?toc=/azure/azure-monitor/toc.json).
* El usuario se autentica en Azure con una cuenta que es miembro del [rol Colaborador de Log Analytics](../../azure-monitor/platform/manage-access.md#manage-accounts-and-users).  

>[!NOTE]
>Los cambios recientes en las API de Azure impedirá que los clientes la posibilidad de configurar correctamente la integración entre su grupo de administración y supervisión de Azure por primera vez. Para los clientes que ya han integrado su grupo de administración con el servicio, no se verán afectados a menos que necesiten reconfigurar su conexión existente.  
>Se ha publicado un nuevo módulo de administración para las siguientes versiones de Operations Manager:
>  
>* Para System Center Operations Manager 1801, descargue el módulo de administración [aquí](https://www.microsoft.com/download/details.aspx?id=57173)  
>* Para System Center 2016 - Operations Manager, descargue el módulo de administración [aquí](https://www.microsoft.com/download/details.aspx?id=57172)  
>* Para System Center Operations Manager 2012 R2, descargue el módulo de administración de [aquí](https://www.microsoft.com/download/details.aspx?id=57171)  
>
>Esta actualización del módulo de administración no es aplicable a System Center Operations Manager 1807, que es una versión de actualización de la versión 1801 y no una compilación completa del producto.   

### <a name="network"></a>Red
La información siguiente muestra la información de configuración de proxy y firewall requerida para que el agente de Operations Manager, los servidores de administración y consola del operador para comunicarse con Azure Monitor. El tráfico de cada componente es saliente desde la red a Azure Monitor.   

|Recurso | Número de puerto| Omitir inspección de HTTP|  
|---------|------|-----------------------|  
|**Agent**|||  
|\*.ods.opinsights.azure.com| 443 |Sí|  
|\*.oms.opinsights.azure.com| 443|Sí|  
|\*blob.core.windows.net| 443|Sí|  
|\*.azure-automation.net| 443|Sí|  
|**Servidor de administración**|||  
|\*.service.opinsights.azure.com| 443||  
|\*blob.core.windows.net| 443| Sí|  
|\*.ods.opinsights.azure.com| 443| Sí|  
|*.azure-automation.net | 443| Sí|  
|**Consola de Operations Manager a Azure Monitor**|||  
|service.systemcenteradvisor.com| 443||  
|\*.service.opinsights.azure.com| 443||  
|\*.live.com| 80 y 443||  
|\*.microsoft.com| 80 y 443||  
|\*.microsoftonline.com| 80 y 443||  
|\*.mms.microsoft.com| 80 y 443||  
|login.windows.net| 80 y 443||  
|portal.loganalytics.io| 80 y 443||
|api.loganalytics.io| 80 y 443||
|docs.loganalytics.io| 80 y 443||  

### <a name="tls-12-protocol"></a>Protocolo TLS 1.2
Para garantizar la seguridad de datos en tránsito a Azure Monitor, se recomienda encarecidamente configurar el grupo de administración y el agente debe usar al menos seguridad de capa de transporte (TLS) 1.2. Las versiones anteriores de TLS/Capa de sockets seguros (SSL) han demostrado ser vulnerables y, si bien todavía funcionan para permitir la compatibilidad con versiones anteriores, **no se recomiendan**. Para información adicional, revise [Sending data securely using TLS 1.2](../../azure-monitor/platform/data-security.md#sending-data-securely-using-tls-12) (Envío de datos de forma segura mediante TLS 1.2). 

## <a name="connecting-operations-manager-to-azure-monitor"></a>Conectar Operations Manager con Azure Monitor
Ejecute la siguiente serie de pasos para configurar el grupo de administración de Operations Manager para conectarse a una de las áreas de trabajo de Log Analytics.

Durante el registro inicial del grupo de administración de Operations Manager con un área de trabajo de Log Analytics, la opción para especificar la configuración de proxy para el grupo de administración no está disponible en la consola del operador.  El grupo de administración tiene que estar registrado correctamente con el servicio para que esta opción esté disponible.  Para solucionar esto, tiene que actualizar la configuración de proxy del sistema con Netsh en el sistema desde el que se esté ejecutando la consola del operador para configurar la integración y todos los servidores de administración del grupo de administración.  

1. Abra un símbolo del sistema con privilegios elevados.
    a. Vaya a **Inicio** y escriba **cmd**.
   b. Haga clic con el botón derecho en el **símbolo del sistema** y seleccione Ejecutar como administrador\*\*.
1. Escriba el siguiente comando y presione **Entrar**:

    `netsh winhttp set proxy <proxy>:<port>`

Después de completar los pasos siguientes para integrar con Azure Monitor, puede quitar la configuración ejecutando `netsh winhttp reset proxy` y, a continuación, utilice el **configurar servidor proxy** opción en la consola del operador para especificar el proxy o el registro Servidor de puerta de enlace de análisis. 

1. En la consola de Operations Manager, seleccione el área de trabajo **Administración** .
1. Expanda el nodo de Operations Management Suite y haga clic en **Conexión**.
1. Haga clic en el vínculo **Register to Operations Management Suite** (Registrar en Operations Management Suite).
1. En la página **Asistente para incorporación a Operations Management Suite: Autenticación**, escriba la dirección de correo electrónico o el número de teléfono y la contraseña de la cuenta de administrador asociados a su suscripción de OMS y, después, haga clic en **Iniciar sesión**.

   >[!NOTE]
   >El nombre de Operations Management Suite se ha retirado. 
   
1. Una vez autenticado correctamente, en la página **Asistente para incorporación a Operations Management Suite: Seleccionar área de trabajo**, se le pedirá que seleccione el inquilino de Azure, la suscripción y el área de trabajo de Log Analytics. Si tiene más de un área de trabajo, seleccione el que desee registrar con el grupo de administración de Operations Manager en la lista desplegable y haga clic en **Siguiente**.
   
   > [!NOTE]
   > Operations Manager solo admite un área de trabajo de Log Analytics a la vez. La conexión y los equipos que se registraron en Azure Monitor con el área de trabajo anterior se quitan de Azure Monitor.
   > 
   > 
1. En la página **Asistente para incorporación a Operations Management Suite: Resumen**, confirme la configuración y, si es correcta, haga clic en **Crear**.
1. En la página **Asistente para incorporación a Operations Management Suite: Finalizar**, haga clic en **Cerrar**.

### <a name="add-agent-managed-computers"></a>Adición de equipos administrados por agente
Después de configurar la integración con el área de trabajo de Log Analytics, esto solo establece una conexión con el servicio, y no se recopilan datos de los agentes que informan al grupo de administración. Esto no será posible hasta después de configurar los equipos administrados con agente específicos recopilan datos de registro de Azure Monitor. Puede seleccionar los objetos de equipo individualmente o un grupo que contenga objetos de equipos Windows. No puede seleccionar un grupo que contenga instancias de otra clase, como discos lógicos o bases de datos SQL.

1. Abra la consola de Operations Manager y seleccione el área de trabajo **Administración** .
1. Expanda el nodo de Operations Management Suite y haga clic en **Conexión**.
1. Haga clic en el vínculo **Agregar un equipo/grupo** debajo del título Acciones en el lado derecho del panel.
1. En el cuadro de diálogo **Búsqueda de equipos**, puede buscar equipos o grupos supervisados por Operations Manager. Seleccione los equipos o grupos que incorporar a Azure Monitor, haga clic en **agregar**y, a continuación, haga clic en **Aceptar**.

Puede ver los equipos y grupos configurados para recopilar datos desde el nodo Equipos administrados de Operations Management Suite en el área de trabajo **Administración** de la consola de Operations. Desde aquí puede agregar o quitar equipos y grupos según sea necesario.

### <a name="configure-proxy-settings-in-the-operations-console"></a>Configurar el proxy en la Consola del operador
Realice los pasos siguientes si es un servidor proxy interno entre el grupo de administración y supervisión de Azure. Esta configuración se administran desde el grupo de administración centralmente y se distribuye en sistemas administrados con agente que se incluyen en el ámbito para recopilar datos de registro de Azure Monitor.  Esto es beneficioso cuando determinadas soluciones omiten el servidor de administración y envían datos directamente al servicio.

1. Abra la consola de Operations Manager y seleccione el área de trabajo **Administración** .
1. Expanda Operations Management Suite y haga clic en **Conexiones**.
1. En la vista Conexión de OMS, haga clic en **Configurar servidor proxy**.
1. En la página **Asistente para configuración de Operations Management Suite: Proxy Server**, seleccione **Use un servidor proxy para obtener acceso al servicio Operations Management Suite**, escriba la dirección URL con el número de puerto, por ejemplo, http://corpproxy:80 y después haga clic en **Finalizar**.

Si el servidor proxy requiere autenticación, realice los pasos siguientes para configurar las credenciales y que deben propagarse a los equipos administrados que informa a Azure Monitor en el grupo de administración.

1. Abra la consola de Operations Manager y seleccione el área de trabajo **Administración** .
1. En **RunAs Configuration**, seleccione **Perfiles**.
1. Abra el perfil de **proxy del perfil Ejecutar como de System Center Advisor** .
1. En el Asistente para crear perfiles de ejecución, haga clic en Agregar para usar una cuenta de ejecución. Puede crear una [cuenta de ejecución](https://technet.microsoft.com/library/hh321655.aspx) o usar una existente. Esta cuenta debe contar con los permisos suficientes para pasar a través del servidor proxy.
1. Para establecer la cuenta que se va a administrar, elija **una clase seleccionada, un grupo o un objeto**, haga clic en **Seleccionar** y, después, en **Agrupar**. Para abrir el cuadro **Búsqueda de grupos**.
1. Busque y seleccione **Microsoft System Center Advisor Monitoring Server Group**(Grupo de servidores de supervisión de Microsoft System Center Advisor). Haga clic en **Aceptar** después de seleccionar el grupo para cerrar el cuadro **Búsqueda de grupos**.
1. Haga clic en **Aceptar** para cerrar el cuadro **Agregar una ejecución como cuenta**.
1. Haga clic en **Guardar** para completar el asistente y guardar los cambios.

Después de crea la conexión y configurar qué agentes van a recopilar y notificar datos de registro a Azure Monitor, se aplica la configuración siguiente en el grupo de administración, no necesariamente en orden:

* Se crea la cuenta de ejecución **Microsoft.SystemCenter.Advisor.RunAsAccount.Certificate** . Está asociada con el perfil de ejecución **Microsoft System Center Advisor Run As Profile Blob** (Blob del perfil de ejecución de Microsoft System Center Advisor) y está dirigida a dos clases: **Servidor de recopilación** y **Grupo de administración de Operations Manager**.
* Se crean dos conectores.  La primera se denomina **Microsoft.SystemCenter.Advisor.DataConnector** y se configura automáticamente con una suscripción que reenvía todas las alertas generadas desde instancias de todas las clases en el grupo de administración a Azure Monitor. El segundo conector es **Advisor Connector**, que es responsable de comunicarse con Azure Monitor y uso compartido de datos.
* Se agregan los agentes y los grupos que haya seleccionado para recopilar datos del grupo de administración al **grupo de servidores de supervisión de Microsoft System Center Advisor**.

## <a name="management-pack-updates"></a>Actualizaciones del módulo de administración
Una vez completada la configuración, el grupo de administración de Operations Manager establece una conexión con Azure Monitor. El servidor de administración se sincroniza con el servicio web y recibe información de configuración actualizada en forma de módulos de administración para las soluciones que ha habilitado que se integran con Operations Manager. Operations Manager busca actualizaciones de estos módulos de administración y los descargará e importa automáticamente cuando estén disponibles. Hay dos reglas en particular que controlan este comportamiento:

* **Microsoft.SystemCenter.Advisor.MPUpdate** -actualiza los módulos de administración de Azure Monitor base. Se ejecuta cada 12 horas de forma predeterminada.
* **Microsoft.SystemCenter.Advisor.Core.GetIntelligencePacksRule** : Actualiza los módulos de administración de las soluciones habilitados en el área de trabajo. Se ejecuta cada cinco (5) minutos de forma predeterminada.

Puede invalidar estas dos reglas para evitar la descarga automática mediante la deshabilitación o modificar la frecuencia de la frecuencia con el servidor de administración se sincroniza con Azure Monitorto determinan si un módulo de administración está disponible y debe descargarse. Siga los pasos [Cómo reemplazar una regla o monitor](https://technet.microsoft.com/library/hh212869.aspx) para modificar el parámetro **Frequency** con un valor en segundos para cambiar la programación de sincronización o modificar el parámetro **Enabled** para deshabilitar las reglas. Dirige las invalidaciones para todos los objetos de la clase del grupo de administración de Operations Manager.

Para seguir el proceso de control de cambios existente para controlar las versiones del módulo de administración en el grupo de administración de producción, puede deshabilitar las reglas y habilitarlas durante determinadas horas cuando se permiten las actualizaciones. Si tiene un grupo de desarrollo o de administración de control de calidad en el entorno y tiene conectividad a Internet, puede configurar ese grupo de administración con un área de trabajo de Log Analytics para admitir este escenario. Esto le permite revisar y evaluar las versiones iterativas de los módulos de administración de Azure Monitor antes de publicarlas en el grupo de administración de producción.

## <a name="switch-an-operations-manager-group-to-a-new-log-analytics-workspace"></a>Cambiar un grupo de Operations Manager a una nueva área de trabajo de Log Analytics
1. Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).
1. En Azure Portal, haga clic en **Más servicios**, en la esquina inferior izquierda. En la lista de recursos, escriba **Log Analytics**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Seleccione **Log Analytics** y, a continuación, cree un área de trabajo.  
1. Abra la consola de Operations Manager con una cuenta que sea miembro del rol Administradores de Operations Manager y seleccione el área de trabajo **Administración** .
1. Expanda Log Analytics y seleccione **Conexiones**.
1. Seleccione el vínculo **Re-configure Operation Management Suite** (Volver a configurar Operation Management Suite) en el centro del panel.
1. Siga los pasos del **Asistente para incorporación a Log Analytics** y escriba la dirección de correo electrónico o el número de teléfono y la contraseña de la cuenta de administrador asociada a la nueva área de trabajo de Log Analytics.
   
   > [!NOTE]
   > En la página **Asistente para incorporación a Operations Management Suite: Seleccionar área de trabajo**, se presenta el área de trabajo existente que se está usando.
   > 
   > 

## <a name="validate-operations-manager-integration-with-azure-monitor"></a>Validar la integración de Operations Manager con Azure Monitor
Hay varias maneras diferentes, puede comprobar que Azure Monitor para la integración de Operations Manager es correcta.

### <a name="to-confirm-integration-from-the-azure-portal"></a>Para confirmar la integración desde Azure Portal
1. En Azure Portal, haga clic en **Más servicios**, en la esquina inferior izquierda. En la lista de recursos, escriba **Log Analytics**. Cuando comience a escribir, la lista se filtrará en función de la entrada.
1. En la lista de áreas de trabajo de Log Analytics, seleccione el área de trabajo aplicable.  
1. Seleccione **Configuración avanzada**, **Orígenes conectados** y, a continuación, seleccione **System Center**. 
1. En la tabla de System Center Operations Manager, verá el nombre del grupo de administración con el número de agentes y el estado de cuando se recibieron los datos por última vez.
   
   ![oms-settings-connectedsources](./media/om-agents/oms-settings-connectedsources.png)

### <a name="to-confirm-integration-from-the-operations-console"></a>Para confirmar la integración desde la consola de Operations
1. Abra la consola de Operations Manager y seleccione el área de trabajo **Administración** .
1. Seleccione **Módulos de administración** y, en el cuadro de texto **Buscar:**, escriba **Advisor** o **Intelligence**.
1. En función de las soluciones habilitadas, verá el módulo de administración correspondiente en los resultados de búsqueda.  Por ejemplo, si ha habilitado la solución Alert Management, el módulo de administración Alert Management de Microsoft System Center Advisor estará en la lista.
1. Desde la vista **Supervisión**, desplácese hasta la vista **Operations Management Suite\Estado de mantenimiento**.  Seleccione un servidor de administración en el panel **Management Server State** (Estado del servidor de administración) y, en el panel **Vista de detalle**, confirme que el valor de la propiedad **URI del servicio de autenticación** coincide con el identificador del área de trabajo de Log Analytics.
   
   ![oms-opsmgr-mg-authsvcuri-property-ms](./media/om-agents/oms-opsmgr-mg-authsvcuri-property-ms.png)

## <a name="remove-integration-with-azure-monitor"></a>Quitar la integración con Azure Monitor
Cuando ya no necesite la integración entre el grupo de administración de Operations Manager y el área de trabajo de Log Analytics, es necesario realizar varios pasos para quitar correctamente la conexión y la configuración del grupo de administración. El procedimiento siguiente tiene actualizar el área de trabajo de Log Analytics mediante la eliminación de la referencia del grupo de administración, elimina los conectores de Azure Monitor y, a continuación, elimine los módulos de administración compatible con la integración con el servicio.  

Módulos de administración para las soluciones ha habilitado que se integran con Operations Manager y los módulos de administración necesarios para admitir la integración con Azure Monitor no se puede eliminar fácilmente del grupo de administración. Esto es porque algunos de los módulos de administración de Azure Monitor tienen dependencias en otros módulos de administración relacionados. Para eliminar los módulos de administración con una dependencia en otros módulos de administración, descargue el script para [quitar un módulo de administración con dependencias](https://gallery.technet.microsoft.com/scriptcenter/Script-to-remove-a-84f6873e) desde el centro de scripts de TechNet.  

1. Abra el shell de comandos de Operations Manager con una cuenta que sea miembro del rol Administradores de Operations Manager.
   
    > [!WARNING]
    > Compruebe que no tiene ningún módulo de administración personalizado con la palabra Advisor o IntelligencePack en el nombre antes de continuar. De lo contrario, los siguientes pasos los eliminarán del grupo de administración.
    > 

1. Desde el símbolo del shell de comandos, escriba `Get-SCOMManagementPack -name "*Advisor*" | Remove-SCOMManagementPack -ErrorAction SilentlyContinue`
1. Después, escriba `Get-SCOMManagementPack -name “*IntelligencePack*” | Remove-SCOMManagementPack -ErrorAction SilentlyContinue`
1. Para quitar los módulos de administración restantes que tienen una dependencia en otros módulos de administración de System Center Advisor, use el script *RecursiveRemove.ps1* que descargó antes desde el centro de scripts de TechNet.  
 
    > [!NOTE]
    > El paso para quitar los módulos de administración de Advisor con PowerShell no eliminará automáticamente los módulos de administración interna de Microsoft System Center Advisor o Microsoft System Center Advisor.  No intente eliminarlos.  
    >  

1. Abra la consola de operaciones de Operations Manager con una cuenta que sea miembro del rol Administradores de Operations Manager.
1. Bajo **Administración**, seleccione el nodo **Módulos de administración** y, en el cuadro **Buscar**, escriba **Advisor** y compruebe que los siguientes módulos de administración todavía se importan en el grupo de administración:
   
   * Microsoft System Center Advisor
   * Microsoft System Center Advisor Internal

1. En Azure Portal, haga clic en el icono de **Configuración**.
1. Seleccione **Connected Sources**(Orígenes conectados).
1. En la tabla de la sección de System Center Operations Manager, verá el nombre del grupo de administración que desea quitar del área de trabajo. En la columna **Last Data** (Últimos datos), haga clic en **Quitar**.  
   
    > [!NOTE]
    > El vínculo **Quitar** no estará disponible hasta después de 14 días si no hay ninguna actividad detectada en el grupo de administración conectado.  
    > 

1. Aparecerá una ventana que le pide que confirme que desea continuar con la eliminación.  Haga clic en **Sí** para continuar. 

Para eliminar los dos conectores (Microsoft.SystemCenter.Advisor.DataConnector y Advisor Connector), guarde el siguiente script de PowerShell en el equipo y ejecútelo mediante los ejemplos siguientes:

```
    .\OM2012_DeleteConnectors.ps1 “Advisor Connector” <ManagementServerName>
    .\OM2012_DeleteConnectors.ps1 “Microsoft.SystemCenter.Advisor.DataConnector” <ManagementServerName>
```

> [!NOTE]
> El equipo desde el que se ejecuta este script, si no es un servidor de administración, debe tener el shell de comandos de Operations Manager instalado según la versión del grupo de administración.
> 
> 

```
    param(
    [String] $connectorName,
    [String] $msName="localhost"
    )
    $mg = new-object Microsoft.EnterpriseManagement.ManagementGroup $msName
    $admin = $mg.GetConnectorFrameworkAdministration()
    ##########################################################################################
    # Configures a connector with the specified name.
    ##########################################################################################
    function New-Connector([String] $name)
    {
         $connectorForTest = $null;
         foreach($connector in $admin.GetMonitoringConnectors())
    {
    if($connectorName.Name -eq ${name})
    {
         $connectorForTest = Get-SCOMConnector -id $connector.id
    }
    }
    if ($connectorForTest -eq $null)
    {
         $testConnector = New-Object Microsoft.EnterpriseManagement.ConnectorFramework.ConnectorInfo
         $testConnector.Name = $name
         $testConnector.Description = "${name} Description"
         $testConnector.DiscoveryDataIsManaged = $false
         $connectorForTest = $admin.Setup($testConnector)
         $connectorForTest.Initialize();
    }
    return $connectorForTest
    }
    ##########################################################################################
    # Removes a connector with the specified name.
    ##########################################################################################
    function Remove-Connector([String] $name)
    {
        $testConnector = $null
        foreach($connector in $admin.GetMonitoringConnectors())
       {
        if($connector.Name -eq ${name})
       {
         $testConnector = Get-SCOMConnector -id $connector.id
       }
      }
     if ($testConnector -ne $null)
     {
        if($testConnector.Initialized)
     {
     foreach($alert in $testConnector.GetMonitoringAlerts())
     {
       $alert.ConnectorId = $null;
       $alert.Update("Delete Connector");
     }
     $testConnector.Uninitialize()
     }
     $connectorIdForTest = $admin.Cleanup($testConnector)
     }
    }
    ##########################################################################################
    # Delete a connector's Subscription
    ##########################################################################################
    function Delete-Subscription([String] $name)
    {
      foreach($testconnector in $admin.GetMonitoringConnectors())
      {
      if($testconnector.Name -eq $name)
      {
        $connector = Get-SCOMConnector -id $testconnector.id
      }
    }
    $subs = $admin.GetConnectorSubscriptions()
    foreach($sub in $subs)
    {
      if($sub.MonitoringConnectorId -eq $connector.id)
      {
        $admin.DeleteConnectorSubscription($admin.GetConnectorSubscription($sub.Id))
      }
     }
    }
    #New-Connector $connectorName
    write-host "Delete-Subscription"
    Delete-Subscription $connectorName
    write-host "Remove-Connector"
    Remove-Connector $connectorName
```

En el futuro, si planea volver a conectar su grupo de administración a un área de trabajo de Log Analytics, necesitará volver a importar el archivo del módulo de administración `Microsoft.SystemCenter.Advisor.Resources.\<Language>\.mpb`. Según la versión de System Center Operations Manager implementada en el entorno, puede encontrar este archivo en la siguiente ubicación:

* En los medios de origen de la carpeta `\ManagementPacks` para System Center 2016 - Operations Manager y versiones posteriores.
* Desde el paquete acumulativo de actualizaciones más reciente que se aplica al grupo de administración. En Operations Manager 2012, la carpeta de origen es ` %ProgramFiles%\Microsoft System Center 2012\Operations Manager\Server\Management Packs for Update Rollups` y, para la versión 2012 R2, se encuentra en `System Center 2012 R2\Operations Manager\Server\Management Packs for Update Rollups`.

## <a name="next-steps"></a>Pasos siguientes
Para agregar funcionalidad y recopilar datos, vea [soluciones de agregar Azure Monitor desde la Galería de soluciones](../../azure-monitor/insights/solutions.md).


