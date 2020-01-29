---
title: Agrupación de máquinas con dependencias de máquina con Azure Migrate | Microsoft Docs
description: En este artículo se describe cómo crear una evaluación con dependencias de máquina mediante el servicio Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 10/01/2019
ms.author: hamusa
ms.openlocfilehash: 96d1c44eb9ecb71684e817a89f9376a07dbe3ccb
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2020
ms.locfileid: "76514976"
---
# <a name="set-up-dependency-visualization-for-assessment"></a>Configuración de la visualización de dependencias para la evaluación

En este artículo se describe cómo configurar la asignación de dependencias en Azure Migrate: Server Assessment.

La asignación de dependencias le permite visualizar las dependencias en las máquinas que quiere evaluar y migrar.

- En Azure Migrate: Server Assessment, puede reunir las máquinas para evaluarlas. Esto normalmente se hace con las máquinas que quiere migrar juntas.
- La asignación de dependencias suele usarse cuando quiere evaluar grupos con niveles de confianza más altos.
- Le permite comprobar las dependencias de las máquinas antes de ejecutar una evaluación y una migración.
- El hecho de asignar y visualizar las dependencias le ayuda a planear de forma eficaz la migración a Azure. Le permite garantizar que no se olvida de nada, lo que evita que experimente interrupciones inesperadas durante la migración.
- Con la asignación, puede detectar los sistemas interdependientes que necesitan migrarse juntos. También puede identificar si un sistema en ejecución sigue ofreciendo servicio a los usuarios o si es un candidato para la retirada en lugar de la migración.

[Obtenga más información](concepts-dependency-visualization.md#how-does-it-work) acerca de la visualización de dependencia.

## <a name="before-you-start"></a>Antes de comenzar

- Asegúrese de que ha [creado](how-to-add-tool-first-time.md) un proyecto de Azure Migrate.
- Si ya ha creado un proyecto, asegúrese de que ha [agregado](how-to-assess.md) la herramienta Azure Migrate: Server Assessment.
- Compruebe que ha detectado sus máquinas en Azure Migrate; puede hacerlo, configure un dispositivo de Azure Migrate para [VMware](how-to-set-up-appliance-vmware.md) o [Hyper-V](how-to-set-up-appliance-hyper-v.md). El dispositivo detecta máquinas locales y envía metadatos y datos de rendimiento a Azure Migrate: Server Assessment. [Más información](migrate-appliance.md).


**Características** | **Nota**
--- | ---
Disponibilidad | La visualización de dependencias no está disponible en Azure Government.
Mapa de servicio | La visualización de dependencias usa la solución Service Map en Azure Monitor. [Service Map](../azure-monitor/insights/service-map.md) detecta y muestra automáticamente las conexiones entre los servidores.
Agentes | Para usar la visualización de dependencias, instale los agentes siguientes en las máquinas que quiera asignar:<br/> - [Agente de Log Analytics](../azure-monitor/platform/log-analytics-agent.md) (conocido anteriormente como Microsoft Monitoring Agent [MMA]).<br/> - [Agente de dependencias de Service Map](../azure-monitor/insights/vminsights-enable-overview.md#the-microsoft-dependency-agent).<br/><br/> Para automatizar la instalación de agentes, puede usar una herramienta de implementación, como Configuration Manager, que cuenta con una solución de implementación de agentes para Azure Migrate.
Dependency Agent | Revise la [compatibilidad del agente de dependencias](../azure-monitor/insights/vminsights-enable-overview.md#the-microsoft-dependency-agent) con Windows y Linux.<br/><br/> [Obtenga más información](../azure-monitor/insights/vminsights-enable-hybrid-cloud.md#installation-script-examples) acerca de cómo puede utilizar scripts para instalar el agente de dependencias.
Agente de Log Analytics (MMA) | [Obtenga más información](../azure-monitor/platform/log-analytics-agent.md#install-and-configure-agent) sobre los métodos de instalación de MMA.<br/><br/> En el caso de las máquinas que supervisa System Center Operations Manager 2012 R2 o las versiones posteriores, no hay necesidad de instalar el agente MMA. Service Map se integra con Operations Manager. Puede habilitar la integración con las instrucciones que encontrará [aquí](https://docs.microsoft.com/azure/azure-monitor/insights/service-map-scom#prerequisites). Sin embargo, tenga en cuenta que será necesario instalar el agente de dependencia en estas máquinas.<br/><br/> [Revise](../azure-monitor/platform/log-analytics-agent.md#supported-linux-operating-systems) los sistemas operativos Linux compatibles con el agente de Log Analytics.
Grupos de evaluación | Los grupos de los que desea visualizar las dependencias no deben contener más de diez máquinas. Si tiene más de 10 máquinas, divídalas en grupos más pequeños para visualizar las dependencias.

## <a name="associate-a-log-analytics-workspace"></a>Asociar un área de trabajo de Log Analytics

Si quiere usar la visualización de dependencias, debe asociar un [área de trabajo de Log Analytics](../azure-monitor/platform/manage-access.md) con un proyecto de Azure Migrate.

- Solo puede asociar un área de trabajo en la suscripción del proyecto de Azure Migrate.
- Puede asociar un área de trabajo existente o crear una nueva.
- La primera vez que configure la visualización de dependencias para una máquina, deberá asociar el área de trabajo.
- Solo puede asociar un área de trabajo después de detectar máquinas en el proyecto de Azure Migrate. Para ello, puede configurar un dispositivo Azure Migrate para [VMware](how-to-set-up-appliance-vmware.md) o [Hyper-V](how-to-set-up-appliance-hyper-v.md). El dispositivo detecta máquinas locales y envía metadatos y datos de rendimiento a Azure Migrate: Server Assessment. [Más información](migrate-appliance.md).

Asocie un área de trabajo como se indica a continuación:

1. En **Azure Migrate: Server Assessment**, haga clic en **Información general**. Si aún no ha agregado la herramienta Server Assessment, [hágalo primero](how-to-assess.md).
2. En **Información general**, haga clic en la flecha abajo para expandir **Información esencial**.
3. En **Área de trabajo de OMS**, haga clic en **Requiere configuración**.
4. En **Configurar área de trabajo**, especifique si quiere crear un área de trabajo nueva o utilizar una existente:

    ![Incorporación de un área de trabajo](./media/how-to-create-group-machine-dependencies/workspace.png)

    - Después de especificar un nombre para el área de trabajo nueva, puede elegir la [región](https://azure.microsoft.com/global-infrastructure/regions/) en que se creará.
    - Al asociar un área de trabajo existente, puede elegir entre las disponibles en la misma suscripción del proyecto de migración.
    - Necesita acceso de lectura al área de trabajo para poder asociarla.
    - No puede modificar el área de trabajo asociada a un proyecto una vez se ha adjuntado.

## <a name="download-and-install-the-vm-agents"></a>Descarga e instalación de los agentes en máquinas virtuales

Descargue e instale los agentes en cada máquina local que quiera visualizar con la asignación de dependencias.

1. En **Azure Migrate: Server Assessment**, haga clic en **Servidores detectados**.
2. Para cada máquina para la que quiera usar la visualización de dependencias, haga clic en **Requiere la instalación del agente**.
3. En la página **Dependencias** de una máquina > **Descargue e instale MMA**, descargue el agente adecuado e instálelo como se describe a continuación.
4. En **Download and install Dependency agent** (Descargar e instalar el agente de dependencias), descargue el agente adecuado e instálelo tal y como se describe a continuación.
5. En **Configure MMA agent** (Configurar agente MMA), copie el id. del área de trabajo y la clave. Los necesitará para instalar el agente MMA.

### <a name="install-the-mma"></a>Instalación de MMA

#### <a name="install-the-agent-on-a-windows-machine"></a>Instalación del agente en una máquina Windows

Para instalar al agente en una máquina Windows, siga estos pasos:

1. Haga doble clic en el agente descargado.
2. En la página **principal**, haga clic en **Siguiente**. En la página **Términos de licencia**, haga clic en **Acepto** para aceptar la licencia.
3. En **Carpeta de destino**, mantenga o modifique la carpeta de instalación predeterminada y después haga clic en **Siguiente**.
4. En **Opciones de instalación del agente**, seleccione **Azure Log Analytics** > **Siguiente**.
5. Haga clic en **Agregar** para agregar un área de trabajo de Log Analytics nueva. Pegue la clave y el identificador de área de trabajo que ha copiado desde el portal. Haga clic en **Next**.

El agente se puede instalar desde la línea de comandos o mediante un método automatizado, como Configuration Manager o [Intigua](https://go.microsoft.com/fwlink/?linkid=2104196). [Obtenga más información](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#install-and-configure-agent) sobre el uso de estos métodos para instalar el agente MMA. También se puede instalar el agente de MMA mediante este [script](https://go.microsoft.com/fwlink/?linkid=2104394).

[Más información](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems) sobre la compatibilidad de MMA con sistemas operativos Windows.

#### <a name="install-the-agent-on-a-linux-machine"></a>Instalación del agente en una máquina Linux

Para instalar al agente en una máquina Linux, siga estos pasos:

1. Transfiera el paquete adecuado (x86 o x x64) al equipo Linux mediante scp o sftp.
2. Instale el paquete mediante el argumento --install.

    ```sudo sh ./omsagent-<version>.universal.x64.sh --install -w <workspace id> -s <workspace key>```

[Más información](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-linux-operating-systems) sobre la lista de compatibilidad de MMA con sistemas operativos Linux. 

### <a name="install-the-dependency-agent"></a>Instalación del agente de dependencia
1. Para instalar al agente de dependencia en una máquina Windows, haga doble clic en el archivo de instalación y siga los pasos del asistente.
2. Para instalar el agente de dependencia en una máquina Linux, instale como raíz mediante el siguiente comando:

    ```sh InstallDependencyAgent-Linux64.bin```

[Más información](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-enable-hybrid-cloud#installation-script-examples) acerca de cómo puede utilizar scripts para instalar el agente de dependencia.

[Obtenga más información](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-enable-overview#supported-operating-systems) sobre los sistemas operativos que admite el agente de dependencia.


## <a name="create-a-group-using-dependency-visualization"></a>Creación de un grupo mediante la visualización de dependencias

1. En **Azure Migrate: Server Assessment**, haga clic en **Servidores detectados**.
2. En la columna **Dependencias**, haga clic en **Ver dependencias** para cada máquina que quiera revisar.
3. En el mapa de dependencias, puede ver lo siguiente:
    - Conexiones TCP de entrada (clientes) y de salida (servidores) hacia y desde la máquina.
    - Las máquinas dependientes que no tienen instalados agentes de dependencias se agrupan en función de los números de puerto.
    - Las máquinas dependientes con agentes de dependencias instalados se muestran como cuadros independientes.
    - Los procesos que se ejecutan en la máquina. Expanda el cuadro de cada máquina para ver los procesos.
    - Las propiedades de la máquina (incluido el FQDN, el sistema operativo y la dirección MAC). Haga clic en el cuadro de cada máquina para ver los detalles.

4. Puede consultar las dependencias de distintas duraciones; para ello, haga clic en la duración correspondiente en la etiqueta de intervalo de tiempo. De forma predeterminada, el intervalo es una hora. Puede modificar el intervalo de tiempo o especificar las fechas de inicio y finalización, y la duración.

    > [!NOTE]
    > El intervalo de tiempo puede ser de hasta una hora. Si necesita un intervalo más amplio, use Azure Monitor para consultar los datos dependientes durante un período más prolongado.

5. Después de identificar máquinas dependientes que quiere agrupar, use Control + clic para seleccionar varias máquinas en el mapa y haga clic en **Agrupar máquinas**.
6. Especifique un nombre para el grupo.
7. Verifique que Azure Migrate puede detectar las máquinas dependientes.

    - Si Azure Migrate: Server Assessment no detecta una máquina dependiente, no puede agregarla al grupo.
    - Para agregar una máquina, vuelva a ejecutar la detección y verifique que se detectó.

8. Si desea crear una evaluación para este grupo, seleccione la casilla para crear una evaluación para el grupo.
8. Haga clic en **Aceptar** para guardar el grupo.

Después de crear el grupo, le recomendamos que instale los agentes en todas las máquinas del grupo y, a continuación, visualice las dependencias de todo el grupo.

## <a name="query-dependency-data-in-azure-monitor"></a>Consulta de datos de dependencia en Azure Monitor

Los datos de dependencia que captura Service Map se pueden consultar en el área de trabajo de Log Analytics asociada con el proyecto de Azure Migrate. Log Analytics se usa para escribir y ejecutar consultas de registros en Azure Monitor.

- [Obtenga información sobre cómo](../azure-monitor/insights/service-map.md#log-analytics-records) buscar datos de Service Map en Log Analytics.
- [Obtenga información general](../azure-monitor/log-query/get-started-queries.md) sobre la escritura de consultas de registro en [Log Analytics](../azure-monitor/log-query/get-started-portal.md).

Ejecute una consulta para los datos de dependencia como se indica a continuación:

1. Después de instalar los agentes, vaya al portal y haga clic en **Introducción**.
2. En **Azure Migrate: Server Assessment**, haga clic en **Información general**. Haga clic en la flecha abajo para expandir **Información esencial**.
3. En el **Área de trabajo de OMS**, haga clic en el nombre del área de trabajo.
3. En la página del área de trabajo de Log Analytics > **General**, haga clic en **Registros**.
4. Escriba la consulta y haga clic en **Ejecutar**.

### <a name="sample-queries"></a>Consultas de ejemplo

Le proporcionamos consultas de ejemplo que puede utilizar para extraer datos de dependencia.

- Puede modificar las consultas para extraer los puntos de datos preferidos.
- [Revise](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#log-analytics-records) una lista completa de los registros de datos de dependencia.
- [Revise](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#sample-log-searches) ejemplos consultas adicionales.

#### <a name="sample-review-inbound-connections"></a>Sample: revisión de conexiones entrantes

Revise las conexiones entrantes para un conjunto de máquinas virtuales.

- Los registros de la tabla de métricas de conexión (VMConnection) no representan conexiones de red físicas individuales.
- Varias conexiones de red físicas se agrupan en una conexión lógica.
- [Obtenga más información](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#connections) sobre cómo se agregan los datos de conexión de red física en VMConnection.

```
// the machines of interest
let ips=materialize(ServiceMapComputer_CL
| summarize ips=makeset(todynamic(Ipv4Addresses_s)) by MonitoredMachine=ResourceName_s
| mvexpand ips to typeof(string));
let StartDateTime = datetime(2019-03-25T00:00:00Z);
let EndDateTime = datetime(2019-03-30T01:00:00Z);
VMConnection
| where Direction == 'inbound'
| where TimeGenerated > StartDateTime and TimeGenerated  < EndDateTime
| join kind=inner (ips) on $left.DestinationIp == $right.ips
| summarize sum(LinksEstablished) by Computer, Direction, SourceIp, DestinationIp, DestinationPort
```

#### <a name="sample-summarize-sent-and-received-data"></a>Sample: resumen de los datos enviados y recibidos

En este ejemplo se resume el volumen de datos enviados y recibidos en conexiones salientes entre un conjunto de máquinas.

```
// the machines of interest
let ips=materialize(ServiceMapComputer_CL
| summarize ips=makeset(todynamic(Ipv4Addresses_s)) by MonitoredMachine=ResourceName_s
| mvexpand ips to typeof(string));
let StartDateTime = datetime(2019-03-25T00:00:00Z);
let EndDateTime = datetime(2019-03-30T01:00:00Z);
VMConnection
| where Direction == 'inbound'
| where TimeGenerated > StartDateTime and TimeGenerated  < EndDateTime
| join kind=inner (ips) on $left.DestinationIp == $right.ips
| summarize sum(BytesSent), sum(BytesReceived) by Computer, Direction, SourceIp, DestinationIp, DestinationPort
```

## <a name="next-steps"></a>Pasos siguientes

[Cree una evaluación](how-to-create-assessment.md) para un grupo.
