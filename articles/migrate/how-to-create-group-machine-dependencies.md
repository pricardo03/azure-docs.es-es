---
title: Agrupación de máquinas con dependencias de máquina con Azure Migrate | Microsoft Docs
description: En este artículo se describe cómo crear una evaluación con dependencias de máquina mediante el servicio Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 12/05/2018
ms.author: raynew
ms.openlocfilehash: 686c91669e5eccd7979c248db42d6f5b5079308b
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59280917"
---
# <a name="group-machines-using-machine-dependency-mapping"></a>Agrupación de máquinas con asignación de dependencias de máquina

En este artículo se describe cómo crear un grupo de máquinas para la evaluación de [Azure Migrate](migrate-overview.md) mediante la visualización de dependencias de máquina. Este método se utiliza normalmente cuando se quiere evaluar grupos de máquinas virtuales con mayores niveles de confianza mediante la comprobación cruzada de dependencias de máquina antes de ejecutar una evaluación. La visualización de dependencia puede ayudarle a planear la migración a Azure de forma eficaz. Le ayuda a asegurarse de que no se deja nada atrás y de que no se produzcan interrupciones inesperadas al realizar una migración a Azure. Puede detectar todos los sistemas interdependientes que tienen que migrarse juntos e identificar si un sistema en ejecución sigue ofreciendo servicio a los usuarios o si es un candidato para la retirada en lugar de la migración.

> [!NOTE]
> La funcionalidad de visualización de dependencias no está disponible en Azure Government.

## <a name="prepare-for-dependency-visualization"></a>Preparar la visualización de dependencias
Azure Migrate aprovecha la solución Service Map en registros de Azure Monitor para habilitar la visualización de dependencias de máquinas.

### <a name="associate-a-log-analytics-workspace"></a>Asociar un área de trabajo de Log Analytics
Si quiere aprovechar la visualización de dependencias, necesita asociar un área de trabajo de Log Analytics, actual o nueva, con un proyecto de Azure Migrate. Solo se puede crear o vincular un área de trabajo en la misma suscripción donde se crea el proyecto de migración.

- Para asociar un área de trabajo de Log Analytics a un proyecto, en **Introducción**, vaya a la sección **Essentials** del proyecto y haga clic en **Requiere configuración**.

    ![Asociar un área de trabajo de Log Analytics](./media/concepts-dependency-visualization/associate-workspace.png)

- Al asociar un área de trabajo, obtendrá la opción de crear una o de conectar una existente:
  - Cuando se crea una nueva área de trabajo, hay que especificar un nombre para el área de trabajo. Después, se crea el área de trabajo en una región en la misma [ubicación geográfica de Azure](https://azure.microsoft.com/global-infrastructure/geographies/) que el proyecto de migración.
  - Al asociar un área de trabajo existente, puede elegir entre las disponibles en la misma suscripción del proyecto de migración. Tenga en cuenta que solo se enumeran las áreas de trabajo que se crearon en una región donde [se admita Service Map](https://docs.microsoft.com/azure/azure-monitor/insights/service-map-configure#supported-azure-regions). Para poder asociar un área de trabajo, asegúrese de que tiene acceso de lectura a ella.

> [!NOTE]
> No se puede cambiar el área de trabajo asociada a un proyecto de migración.

### <a name="download-and-install-the-vm-agents"></a>Descarga e instalación de los agentes en máquinas virtuales
Después de configurar un área de trabajo, tiene que descargar e instalar agentes en cada máquina local que vaya a evaluar. Además, si tiene máquinas sin conectividad a Internet, debe descargar e instalar en ellas la [puerta de enlace de Log Analytics](../azure-monitor/platform/gateway.md).

1. En **Introducción**, haga clic en **Administrar** > **Máquinas**y seleccione la máquina requerida.
2. En la columna **Dependencias**, haga clic en **Instalar agentes**.
3. En la página **Dependencias**, descargue e instale el agente de Microsoft Monitoring Agent (MMA), así como el agente de dependencia en cada máquina virtual que vaya a evaluar.
4. Copie la clave y el identificador de área de trabajo. Las necesitará cuando se instala MMA en la máquina local.

> [!NOTE]
> Para automatizar la instalación de agentes, puede usar cualquier herramienta de implementación como System Center Configuration Manager o bien la herramienta de nuestro asociado [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration), que tiene una solución de implementación de agentes para Azure Migrate.

### <a name="install-the-mma"></a>Instalación de MMA

#### <a name="install-the-agent-on-a-windows-machine"></a>Instalación del agente en una máquina Windows

Para instalar al agente en una máquina Windows, siga estos pasos:

1. Haga doble clic en el agente descargado.
2. En la página **principal**, haga clic en **Siguiente**. En la página **Términos de licencia**, haga clic en **Acepto** para aceptar la licencia.
3. En **Carpeta de destino**, mantenga o modifique la carpeta de instalación predeterminada y después haga clic en **Siguiente**.
4. En **Opciones de instalación del agente**, seleccione **Azure Log Analytics** > **Siguiente**.
5. Haga clic en **Agregar** para agregar un área de trabajo nueva de Log Analytics. Pegue la clave y el identificador de área de trabajo que ha copiado desde el portal. Haga clic en **Next**.

Puede instalar al agente desde la línea de comandos o mediante un método automatizado, como System Center Configuration Manager. [Obtenga más información](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#install-and-configure-agent) sobre el uso de estos métodos para instalar el agente MMA.

#### <a name="install-the-agent-on-a-linux-machine"></a>Instalación del agente en una máquina Linux

Para instalar al agente en una máquina Linux, siga estos pasos:

1. Transfiera el paquete adecuado (x86 o x x64) al equipo Linux mediante scp o sftp.
2. Instale el paquete mediante el argumento --install.

    ```sudo sh ./omsagent-<version>.universal.x64.sh --install -w <workspace id> -s <workspace key>```

[Más información](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems) sobre la lista de compatibilidad de MMA con sistemas operativos Linux.

#### <a name="install-the-agent-on-a-machine-monitored-by-scom"></a>Instalación del agente en una máquina supervisada por SCOM

Para las máquinas supervisadas por System Center Operations Manager 2012 R2 o versiones posteriores, no hay necesidad de instalar el agente MMA. Service Map tiene una integración con SCOM que aprovecha SCOM MMA para recopilar los datos de dependencia necesarios. Puede habilitar la integración con las instrucciones que encontrará [aquí](https://docs.microsoft.com/azure/azure-monitor/insights/service-map-scom#prerequisites). Sin embargo, tenga en cuenta que será necesario instalar el agente de dependencia en estas máquinas.


### <a name="install-the-dependency-agent"></a>Instalación del agente de dependencia
1. Para instalar al agente de dependencia en una máquina Windows, haga doble clic en el archivo de instalación y siga los pasos del asistente.
2. Para instalar el agente de dependencia en una máquina Linux, instale como raíz mediante el siguiente comando:

    ```sh InstallDependencyAgent-Linux64.bin```

Obtenga más información sobre la compatibilidad de Dependency Agent para los sistemas operativos [Windows](../azure-monitor/insights/service-map-configure.md#supported-windows-operating-systems)y [Linux](../azure-monitor/insights/service-map-configure.md#supported-linux-operating-systems).

[Más información](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#installation-script-examples) acerca de cómo puede utilizar scripts para instalar el agente de dependencia.


## <a name="create-a-group"></a>Creación de un grupo

1. Después de instalar los agentes, vaya al portal y haga clic en **Administrar** > **Máquinas**.
2. Busque la máquina en que instaló los agentes.
3. La columna **Dependencias** de la máquina debería aparecer ahora como **Ver dependencias**. Haga clic en la columna para ver las dependencias de la máquina.
4. En el mapa de dependencias de la máquina aparecen los siguientes detalles:
    - Conexiones TCP de entrada (clientes) y de salida (servidores) hacia y desde la máquina
        - Las máquinas dependientes que no tienen instalado el agente MMA y de dependencias se agrupan en función de los números de puerto.
        - Las máquinas dependientes que tienen instalado el agente de dependencia y MMA se muestran como casillas diferentes
    - Procesos que se ejecutan dentro de la máquina; aquí puede expandir cada casilla de la máquina para ver los procesos
    - Propiedades, como el nombre de dominio completo, el sistema operativo, la dirección MAC, entre otras, de cada máquina; puede hacer clic en la casilla de cada máquina para ver estos detalles

      ![Ver las dependencias de máquinas](./media/how-to-create-group-machine-dependencies/machine-dependencies.png)

4. Puede consultar las dependencias de distintas duraciones; para ello, haga clic en la duración correspondiente en la etiqueta de intervalo de tiempo. De forma predeterminada, el intervalo es una hora. Puede modificar el intervalo de tiempo o especificar las fechas de inicio y finalización, y la duración.

   > [!NOTE]
   >    Actualmente, la interfaz de usuario de la visualización de dependencias no admite la selección de un intervalo de tiempo superior a una hora. Use Azure Monitor registra en [consultar los datos de dependencia](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) durante un período más largo.

5. Después de identificar máquinas dependientes que desea agrupar, use Ctrl+clic para seleccionar varias máquinas en el mapa y haga clic en **Agrupar máquinas**.
6. Especifique un nombre para el grupo. Verifique que Azure Migrate puede detectar las máquinas dependientes.

    > [!NOTE]
    > Si Azure Migrate no detecta una máquina dependiente, puede agregarla al grupo. Para agregar estas máquinas al grupo, debe volver a ejecutar el proceso de detección con el ámbito correcto en la instancia de vCenter Server y asegurarse de que Azure Migrate detecta la máquina.  

7. Si desea crear una evaluación para este grupo, seleccione la casilla para crear una evaluación para el grupo.
8. Haga clic en **Aceptar** para guardar el grupo.

Una vez creado el grupo, se recomienda instalar los agentes en todas las máquinas del grupo y restringir el grupo mediante la visualización de la dependencia de todo el grupo.

## <a name="query-dependency-data-from-azure-monitor-logs"></a>Consultar datos de dependencia de los registros de Azure Monitor

Los datos de dependencia capturados por Service Map están disponibles para su consulta en el área de trabajo de Log Analytics asociada con el proyecto de Azure Migrate. [Obtenga más información](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#log-analytics-records) acerca de las tablas de datos de Service Map para consultar en Azure Monitor registra. 

Para ejecutar las consultas de Kusto:

1. Después de instalar los agentes, vaya al portal y haga clic en **Introducción**.
2. En **Introducción**, vaya a la sección **Essentials** del proyecto y haga clic en el nombre del área de trabajo que se proporciona junto al **Área de trabajo de OMS**.
3. En la página del área de trabajo de Log Analytics, haga clic en **General** > **Registros**.
4. Escriba la consulta para recopilar datos de dependencia mediante registros de Azure Monitor. Existen consultas de ejemplo para recopilar datos de dependencia [aquí](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#sample-log-searches).
5. Ejecute la consulta haciendo clic en Ejecutar. 

[Obtenga más información](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal) acerca de cómo escribir consultas de Kusto. 

### <a name="sample-azure-monitor-logs-queries"></a>Ejemplo de Azure Monitor registra aquellas consultas

Estos son ejemplos de consultas puede utilizar para extraer datos de dependencia. Tenga en cuenta que las consultas pueden modificarse para extraer los puntos de datos preferida. Está disponible una lista exhaustiva de los campos de registros de datos de dependencia [aquí](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#log-analytics-records)

#### <a name="summarize-inbound-connections-on-a-set-of-machines"></a>Resumir las conexiones entrantes en un conjunto de máquinas

Tenga en cuenta que los registros en la tabla de métricas de conexión, VMConnection, no representan conexiones de red físicos individuales. Varias conexiones de red físicos se agrupan en una conexión lógica. [Obtenga más información](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#connections) acerca de la conexión de red físicos se agregan datos en un registro lógico único VMConnection. 

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

#### <a name="summarize-volume-of-data-sent-and-received-on-inbound-connections-between-a-set-of-machines"></a>Resumir el volumen de datos enviados y recibidos en las conexiones entrantes entre un conjunto de máquinas

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

- [Más información](https://docs.microsoft.com/azure/migrate/resources-faq#dependency-visualization) sobre las preguntas más frecuentes sobre visualización de dependencias.
- [Más información](how-to-create-group-dependencies.md) sobre cómo restringir el grupo mediante la visualización de las dependencias del grupo.
- [Obtenga más información](concepts-assessment-calculation.md) sobre cómo se calculan las evaluaciones.
