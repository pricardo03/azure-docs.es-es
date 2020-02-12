---
title: Monitor de conexión (versión preliminar) | Microsoft Docs
description: Obtenga información acerca de cómo usar Monitor de conexión (versión preliminar) para supervisar la comunicación de red en un entorno distribuido
services: network-watcher
documentationcenter: na
author: vinynigam
manager: agummadi
editor: ''
tags: azure-resource-manager
Customer intent: I need to monitor communication between a VM and another VM. If the communication fails, I need to know why, so that I can resolve the problem.
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/27/2020
ms.author: vinigam
ms.custom: mvc
ms.openlocfilehash: c993a08a4163d50a9632055da355e39b5bdde004
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77026452"
---
# <a name="overview"></a>Información general

Monitor de conexión (versión preliminar) ofrece funcionalidades de supervisión de conexión integrales unificadas en Azure Network Watcher para implementaciones híbridas y en la nube de Azure. Azure Network Watcher proporciona herramientas para supervisar, diagnosticar y ver las métricas relacionadas con la conectividad de las implementaciones de Azure.

Casos de uso clave:

- Tiene una VM de servidor front-end web que se comunica con una VM de servidor de bases de datos en una aplicación de varios niveles. Quiere comprobar la conectividad de red entre las dos VM.
- Quiere que las VM en la región Este de EE. UU. hagan ping a las VM en la región Centro de EE. UU. y comparar las latencias de red entre regiones.
- Tiene varios sitios de oficina locales en ciudades como Seattle que se conectan a direcciones URL de Office 365. Quiere comparar las latencias que experimentan los usuarios con las direcciones URL de Office 365 de Seattle y Ashburn.
- Tiene una configuración de aplicación híbrida que necesita conectividad a un punto de conexión de Azure Storage. Quiere comparar las latencias entre un sitio local y la aplicación de Azure que se conectan al mismo punto de conexión de Azure Storage.
- Quiere comprobar la conectividad de las VM de Azure que hospedan la aplicación en la nube a las instalaciones locales.

En esta fase de versión preliminar, la solución reúne lo mejor de las dos funcionalidades clave: [Monitor de conexión](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#monitor-communication-between-a-virtual-machine-and-an-endpoint) de Network Watcher y [Monitor de conectividad de servicio](https://docs.microsoft.com/azure/azure-monitor/insights/network-performance-monitor-service-connectivity) de Network Performance Monitor.

Aspectos destacados:

* Experiencia unificada e intuitiva para las necesidades de supervisión híbrida y de Azure
* Supervisión de conectividad entre regiones y entre áreas de trabajo
* Frecuencias de sondeo más altas y una mejor visibilidad del rendimiento de la red
* Alertas más rápidas para las implementaciones híbridas
* Compatibilidad con comprobaciones de conectividad basadas en HTTP, TCP e ICMP
* Las métricas y Log Analytics admiten configuraciones de pruebas de Azure y que no son de Azure

![Monitor de conexión](./media/connection-monitor-2-preview/hero-graphic.png)

Siga los pasos que se mencionan a continuación para iniciar la supervisión con Monitor de conexión (versión preliminar)

## <a name="step-1-install-monitoring-agents"></a>Paso 1: Instalar los agentes de supervisión

Monitor de conexión se basa en ejecutables ligeros para ejecutar las comprobaciones de conectividad.  Se admiten las comprobaciones de conectividad desde entornos locales y de Azure. El ejecutable específico que se va a usar depende de si la VM se hospeda en Azure o en un entorno local.

### <a name="agents-for-azure-virtual-machines"></a>Agentes para máquinas virtuales de Azure

Para que Monitor de conexión reconozca las VM de Azure como origen para la supervisión, debe instalar en ellas la extensión para máquinas virtuales del agente de Network Watcher (también conocida como extensión de Network Watcher). La extensión de del agente de Network Watcher es un requisito para desencadenar la supervisión de un extremo a otro, así como otras funcionalidades avanzadas en máquinas virtuales de Azure. Puede [crear una VM e instalar la extensión de Network Watcher](https://docs.microsoft.com/azure/network-watcher/connection-monitor#create-the-first-vm)[en ella](https://docs.microsoft.com/azure/network-watcher/connection-monitor#create-the-first-vm).  También puede instalar, configurar y solucionar problemas de la extensión de Network Watcher para [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/network-watcher-linux) y [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/network-watcher-windows) por separado.

Si las reglas de firewall o NSG bloquean la comunicación entre el origen y el destino, Monitor de conexión detectará el problema y lo mostrará como mensaje de diagnóstico en la topología. Para habilitar la supervisión de la conexión, asegúrese de que las reglas de firewall y NSG permitan los paquetes a través de TCP o ICMP entre el origen y el destino.

### <a name="agents-for-on-premise-machines"></a>Agentes para máquinas locales

Para que Monitor de conexión reconozca las máquinas locales como orígenes para la supervisión, deberá instalar el agente de Log Analytics en las máquinas y habilitar la solución Network Performance Monitor. Estos agentes están vinculados a áreas de trabajo de Log Analytics y necesitan que el identificador de área de trabajo y la clave principal estén configurados antes de poder iniciar la supervisión.

Para instalar el agente de Log Analytics para máquinas Windows, siga las instrucciones que se mencionan en [este vínculo](https://docs.microsoft.com/azure/virtual-machines/extensions/oms-windows).

Asegúrese de que el destino sea accesible si hay firewalls o dispositivos de red virtual (NVA) en la ruta de acceso.

## <a name="step-2-enable-network-watcher-on-your-subscription"></a>Paso 2: Habilitar Network Watcher en la suscripción

Todas las suscripciones con una red virtual se habilitan con Network Watcher. Al crear una red virtual en la suscripción, Network Watcher se habilitará automáticamente en la región y suscripción de esa red virtual. El hecho de habilitar Network Watcher de forma automática no afecta a sus recursos ni supone ningún cargo adicional. Asegúrese de que Network Watcher no esté explícitamente deshabilitado en la suscripción. Para más información, consulte [Habilitación de Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="step-3-create-connection-monitor"></a>Paso 3: Crear un monitor de conexión 

_Monitor de conexión_ supervisa la comunicación a intervalos regulares y le informa de los cambios de disponibilidad, latencia y topología de red entre los agentes de origen y los puntos de conexión de destino. Los orígenes pueden ser VM de Azure o máquinas locales que tengan instalado un agente de supervisión. Los puntos de conexión de destino pueden ser direcciones URL de Office 365, direcciones URL de Dynamics 365, direcciones URL personalizadas, identificadores de recursos de VM de Azure, IPv4, IPv6, FQDN o cualquier nombre de dominio.

### <a name="accessing-connection-monitor-preview"></a>Acceso a Monitor de conexión (versión preliminar)

1. Acceda a Network Watcher con el siguiente vínculo:[https://ms.portal.azure.com/?Microsoft\_Azure\_Network\_connectionmonitorpreview=true#blade/Microsoft\_Azure\_Network/NetworkWatcherMenuBlade/connectionMonitorPreview](https://ms.portal.azure.com/?Microsoft_Azure_Network_connectionmonitorpreview=true#blade/Microsoft_Azure_Network/NetworkWatcherMenuBlade/connectionMonitorPreview)
2. En la sección Supervisión del panel izquierdo de Network Watcher, haga clic en la pestaña "Monitor de conexión (vista previa)". Esta pestaña solo estará visible si se tiene acceso a Network Watcher mediante el vínculo especificado en el paso 1.
3. Puede ver todos los monitores de conexión que se crean con la experiencia de Monitor de conexión (versión preliminar). Todos los monitores de conexión creados con la experiencia clásica de la pestaña Monitor de conexión estarán visibles en la pestaña Monitor de conexión.

    ![Crear un monitor de conexión](./media/connection-monitor-2-preview/cm-resource-view.png)


### <a name="creating-a-connection-monitor"></a>Creación de un monitor de conexión

Los monitores de conexión creados con Monitor de conexión (versión preliminar) proporcionan la capacidad de agregar VM de Azure y locales como orígenes, y supervisar la conectividad a los puntos de conexión, lo que puede abarcar Azure o cualquier otra dirección URL/IP.

A continuación se indican las entidades de un monitor de conexión:

* Recurso de monitor de conexión: Recurso de Azure específico de la región. Todas las entidades que se mencionan a continuación son propiedades de un recurso de monitor de conexión.
* Puntos de conexión: Todos los orígenes y destinos que participan en las comprobaciones de conectividad se denominan puntos de conexión. Ejemplos de punto de conexión: VM de Azure, agentes locales, direcciones URL, direcciones IP.
* Configuración de prueba: Cada configuración de prueba es específica del protocolo. En función del protocolo elegido, puede definir el puerto, los umbrales, la frecuencia de las pruebas y otros parámetros.
* Grupo de prueba: Cada grupo de prueba contiene puntos de conexión de origen, puntos de conexión de destino y configuraciones de prueba. Cada monitor de conexión puede contener más de un grupo de prueba.
* Prueba: Una combinación de un punto de conexión de origen, un punto de conexión de destino y una configuración de prueba componen una prueba. La prueba es el nivel más bajo en el que están disponibles los datos de supervisión (% de comprobaciones con errores y RTT)

 ![Crear un monitor de conexión](./media/connection-monitor-2-preview/cm-tg-2.png)

#### <a name="from-portal"></a>Desde el portal

Para crear un monitor de conexión, siga los pasos que se indican a continuación:

1. En el panel de Monitor de conexión (versión preliminar), haga clic en "Crear" en la esquina superior izquierda.
2. En la pestaña Básico, escriba la información del monitor de conexión.
   1. Nombre del monitor de conexión: Nombre del monitor de conexión. Aquí se aplican las reglas de nomenclatura estándar para los recursos de Azure.
   2. Suscripción: Elija una suscripción para el monitor de conexión.
   3. Región: Elija una región para el recurso de monitor de conexión. Solo puede seleccionar las VM de origen que se crean en esta región.
   4. Configuración del área de trabajo: Puede usar el área de trabajo predeterminada creada por el monitor de conexión para almacenar los datos de supervisión haciendo clic en la casilla predeterminada. Para elegir un área de trabajo personalizada, desactive esta casilla. Elija la suscripción y la región para seleccionar el área de trabajo que contendrá los datos de supervisión.
   5. Haga clic en "Next: Test Groups" (Siguiente: Grupos de prueba) para agregar grupos de prueba.

      ![Crear un monitor de conexión](./media/connection-monitor-2-preview/create-cm-basics.png)

3. En la pestaña Grupos de prueba, haga clic en "+ Grupo de prueba" para agregar un grupo de prueba. Use _Crear grupos de prueba en el monitor de conexión_ para agregar grupos de prueba. Haga clic en "Revisar y crear" para revisar el monitor de conexión.

   ![Crear un monitor de conexión](./media/connection-monitor-2-preview/create-tg.png)

4. En la pestaña "Revisar y crear", revise la información básica y los grupos de prueba antes de crear el monitor de conexión. Para editar el monitor de conexión de la vista "Revisar y crear":
   1. Para editar los detalles básicos, use el icono de lápiz como se especifica en el cuadro 1 de la imagen 2.
   2. Para editar los grupos de prueba individuales, haga clic en el grupo de pruebas que desea editar para abrir el grupo de prueba en modo de edición.
   3. Costo actual/mes indicaba el costo durante la versión preliminar. Actualmente no se cobra el uso de Monitor de conexión, por lo que esta columna mostrará cero. Costo real/mes indica el precio que se cobrará después de la disponibilidad general. Tenga en cuenta que los cargos de ingesta de Log Analytics se aplicarán incluso durante la versión preliminar.

5. En la pestaña "Revisar y crear", haga clic en el botón "Crear" para crear el monitor de conexión.

   ![Crear un monitor de conexión](./media/connection-monitor-2-preview/review-create-cm.png)

6.  Monitor de conexión (versión preliminar) creará el recurso de monitor de conexión en segundo plano.

#### <a name="from-armclient"></a>Desde Armclient

```armclient
$connectionMonitorName = "sampleConnectionMonitor"

$ARM = "[https://](https://apac01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fbrazilus.management.azure.com&amp;data=02%7C01%7CManasi.Sant%40microsoft.com%7Cd900da4ed7f24366842108d68022159b%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636837281231186904&amp;sdata=qHL8zWjkobY9MatRpAVbODwboKSQAqqEFOMnjmfyOnU%3D&amp;reserved=0)management.azure.com"

$SUB = "subscriptions/\&lt;subscription id 1\&gt;"

$NW = "resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher\_centraluseuap"

$body =

"{

location: 'eastus',

properties: {

endpoints: [{

name: 'workspace',

resourceId: '/subscriptions/\&lt;subscription id\&gt;/resourcegroups/\&lt;resource group\&gt;/providers/Microsoft.OperationalInsights/workspaces/sampleWorkspace',

filter: {

 items: [{

type: 'AgentAddress',

address: '\&lt;FQDN of your on-premise agent'

}]

}

          },

 {

name: 'vm1',

resourceId: '/subscriptions/\&lt;subscription id\&gt;/resourceGroups/\&lt;resource group\&gt;/providers/Microsoft.Compute/virtualMachines/\&lt;vm-name\&gt;'

},

 {

name: 'vm2',

resourceId: '/subscriptions/\&lt;subscription id\&gt;/resourceGroups/\&lt;resource group\&gt;/providers/Microsoft.Compute/virtualMachines/\&lt;vm-name\&gt;'

   },

{

name: 'azure portal'

address: '\&lt;URL\&gt;'

   },

 {

    name: 'ip',

     address: '\&lt;IP\&gt;'

 }

  ],

  testGroups: [{

    name: 'Connectivity to Azure Portal and Public IP',

    testConfigurations: ['http', 'https', 'tcpEnabled', 'icmpEnabled'],

    sources: ['vm1', 'workspace'],

    destinations: ['azure portal', 'ip']

   },

{

    name: 'Connectivty from Azure VM 1 to Azure VM 2',

    testConfigurations: ['http', 'https', 'tcpDisabled', 'icmpDisabled'],

    sources: ['vm1'],

    destinations: ['vm2'],

    disable: true

   }

  ],

  testConfigurations: [{

    name: 'http',

    testFrequencySec: 60,

    protocol: 'HTTP',

    successThreshold: {

     checksFailedPercent: 50,

     roundTripTimeMs: 3.4

    }

   }, {

    name: 'https',

    testFrequencySec: 60,

    protocol: 'HTTP',

    httpConfiguration: {

     preferHTTPS: true

    },

    successThreshold: {

     checksFailedPercent: 50,

     roundTripTimeMs: 3.4

    }

   }, {

    name: 'tcpEnabled',

    testFrequencySec: 30,

    protocol: 'TCP',

    tcpConfiguration: {

     port: 80

    },

    successThreshold: {

     checksFailedPercent: 30,

     roundTripTimeMs: 5.2

    }

   }, {

    name: 'icmpEnabled',

    testFrequencySec: 90,

    protocol: 'ICMP',

    successThreshold: {

     checksFailedPercent: 50,

     roundTripTimeMs: 3.4

    }

   }, {

    name: 'icmpDisabled',

    testFrequencySec: 120,

    protocol: 'ICMP',

    icmpConfiguration: {

     disableTraceRoute: true

    },

    successThreshold: {

     checksFailedPercent: 50,

     roundTripTimeMs: 3.4

    }

   }, {

    name: 'tcpDisabled',

    testFrequencySec: 45,

    protocol: 'TCP',

    tcpConfiguration: {

     port: 80,

     disableTraceRoute: true

    },

    successThreshold: {

     checksFailedPercent: 30,

     roundTripTimeMs: 5.2

    }

   }

  ]

 }

} "
```

Comando de implementación:
```
armclient PUT $ARM/$SUB/$NW/connectionMonitors/$connectionMonitorName/?api-version=2019-07-01 $body -verbose
```

### <a name="creating-test-groups-in-connection-monitor"></a>Crear grupos de prueba en el monitor de conexión

Cada grupo de prueba del monitor de conexión incluye orígenes y destinos cuyos parámetros de red se prueban para conocer las comprobaciones con errores y RTT a través de las configuraciones de prueba.

#### <a name="from-portal"></a>Desde el portal

Para crear un grupo de prueba en un monitor de conexión, especifique el valor de los campos que se mencionan a continuación:

1. Disable Test Group (Deshabilitar grupo de prueba): Al activar este campo, se deshabilitará la supervisión de todos los orígenes y destinos especificados en el grupo de prueba. Verá esta opción desactivada de forma predeterminada.
2. Nombre: Nombre del grupo de prueba.
3. Orígenes: Puede especificar VM de Azure y máquinas locales como orígenes si los agentes están instalados en ellas. Consulte el paso 1 para instalar el agente específico de su origen.
   1. Haga clic en la pestaña "Agentes de Azure" para seleccionar agentes de Azure. Solo verá las VM enumeradas que están enlazadas a la región que especificó en el momento de crear el monitor de conexión. De forma predeterminada, las VM se agrupan en la suscripción a la que pertenecen, y los grupos se contraen. Puede explorar en profundidad desde el nivel de las suscripciones a otros niveles de la jerarquía:

      ```Subscription -\&gt; resource groups -\&gt; VNETs -\&gt; Subnets -\&gt; VMs with agents Y```

      También puede cambiar el valor del campo "Agrupar por" para iniciar el árbol desde cualquier otro nivel. Por ejemplo:  Agrupar por: VNET mostrará las VM con agentes en las redes virtuales de la jerarquía -\&gt; Subnets -\&gt; VMs with agents.

      ![Agregar orígenes](./media/connection-monitor-2-preview/add-azure-sources.png)

   2. Haga clic en la pestaña "Non-Azure Agents" (Agentes que no son de Azure) para seleccionar agentes locales. De forma predeterminada, verá los agentes agrupados en áreas de trabajo de una región. Solo se mostrarán las áreas de trabajo que tengan configurada la solución Network Performance Monitor. Agregue la solución Network Performance Monitor al área de trabajo desde [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview). También puede usar el proceso descrito en [Agregar soluciones de Azure Monitor desde la Galería de soluciones](https://docs.microsoft.com/azure/azure-monitor/insights/solutions). De forma predeterminada, verá la región seleccionada en la pestaña Información básica de la vista Crear un monitor de conexión. Puede cambiar la región y elegir agentes de áreas de trabajo de la región recién seleccionada. También puede cambiar el valor del campo "Agrupar por" para agrupar en subredes.

      ![Orígenes que no son de Azure](./media/connection-monitor-2-preview/add-non-azure-sources.png)


   3. Haga clic en "Revisar" para revisar los agentes de Azure y los que no son de Azure seleccionados.

      ![Revisar orígenes](./media/connection-monitor-2-preview/review-sources.png)

   4. Haga clic en "Listo" cuando haya terminado de seleccionar los orígenes.

4. Destinos: Puede supervisar la conectividad a las VM de Azure o a cualquier punto de conexión (IP pública, dirección URL, FQDN) al especificarlo como destino. En un único grupo de prueba, puede agregar VM de Azure, direcciones URL de O365, direcciones URL de D365 o puntos de conexión personalizados.

   1. Haga clic en la pestaña "VM de Azure" para seleccionar las VM de Azure como destinos. De forma predeterminada, verá las VM de Azure agrupadas en la jerarquía de suscripciones en la misma región que se seleccionó en la pestaña Información básica de la vista Crear un monitor de conexión. Puede cambiar la región y elegir VM de Azure de la región recién seleccionada. Puede explorar en profundidad desde el nivel de las suscripciones a otros niveles de la jerarquía, como Agentes de Azure.

      ![Agregar destinos](./media/connection-monitor-2-preview/add-azure-dests1.png)<br>

      ![Agregar destinos 2](./media/connection-monitor-2-preview/add-azure-dests2.png)

   2. Haga clic en la pestaña "Puntos de conexión" para seleccionar las VM de Azure como destinos. La lista de puntos de conexión se rellenará con las direcciones URL de prueba de O365 y D365, agrupadas por nombre.  También puede elegir un punto de conexión creado en otros grupos de prueba en el mismo monitor de conexión. Para agregar un nuevo punto de conexión, haga clic en "+ Punto de conexión" en la esquina superior derecha de la pantalla y proporcione la dirección URL/IP/FQDN y el nombre del punto de conexión.

      ![Agregar puntos de conexión](./media/connection-monitor-2-preview/add-endpoints.png)

   3. Haga clic en "Revisar" para revisar los agentes de Azure y los que no son de Azure seleccionados.
   4. Haga clic en "Listo" cuando haya terminado de seleccionar los orígenes.

5. Configuración de prueba: Puede asociar cualquier número de configuraciones de prueba a un grupo de prueba determinado. El portal lo restringe a una configuración de prueba por grupo de prueba, pero use Armclient para agregar más.
   1. Nombre: Nombre de la configuración de prueba.
   2. Protocolo: Puede elegir entre TCP, ICMP y HTTP. Para cambiar HTTP a HTTPS, seleccione HTTP como protocolo y 443 como puerto.
   3. Create Network Test Configuration (Crear configuración de prueba de red): Solo verá esta casilla si selecciona HTTP en el campo Protocolo. Habilite este campo para crear otra configuración de pruebas con los mismos orígenes y destinos especificados en los pasos 3 y 4 sobre protocolo TCP/ICMP. La configuración de prueba recién creada se denomina "\&lt;nombre especificado en 5.a\&gt;\_networkTestConfig".
   4. Deshabilitar traceroute: Este campo será aplicable a los grupos de prueba con TCP o ICMP como protocolo.  Active este campo para evitar que los orígenes descubran la topología y el tiempo de ida y vuelta de salto a salto.
   5. Puerto de destino: Puede personalizar este campo para colocarlo en un puerto de destino de su elección.
   6. Frecuencia de prueba: Este campo determina la frecuencia con la que los orígenes harán ping a los destinos en el protocolo y el puerto especificados anteriormente. Puede elegir entre 30 segundos, 1 minuto, 5 minutos, 15 minutos y 30 minutos. Los orígenes probarán la conectividad a los destinos según el valor que elija.  Por ejemplo, si selecciona 30 segundos, los orígenes comprobarán la conectividad con el destino al menos una vez en un período de 30 segundos.
   7. Health Thresholds (Umbrales de estado): Puede establecer umbrales en los parámetros de red que se mencionan a continuación.
      1. Comprobaciones erróneas en %: Porcentaje de comprobaciones erróneas cuando los orígenes comprueban la conectividad con el destino a través de los criterios especificados anteriormente. Para el protocolo TCP/ICMP, las comprobaciones erróneas en % pueden ser equivalentes al % de pérdida de paquetes. En el caso del protocolo HTTP, este campo representa el número de solicitudes HTTP que no obtuvieron una respuesta.
      2. RTT en milisegundos: Tiempo de ida y vuelta en milisegundos cuando los orígenes se conectan al destino a través de la configuración de prueba especificada anteriormente.

      ![Agregar grupo de prueba](./media/connection-monitor-2-preview/add-test-config.png)

Todos los orígenes y destinos agregados a un grupo de prueba con la configuración de prueba especificada se dividen en pruebas individuales. Por ejemplo:

* Grupo de prueba: TG1
* Orígenes: 3 (A, B, C)
* Destinos: 2 (D, E)
* Configuración de prueba: 2 (Config 1, Config 2)
* Pruebas creadas: Total = 12

| **Número de prueba** | **Origen** | **Destino** | **Nombre de la configuración de prueba** |
| --- | --- | --- | --- |
| 1 | Un | D | Config 1 |
| 2 | Un | D | Config 2 |
| 3 | Un | E | Config 1 |
| 4 | Un | E | Config 2 |
| 5 | B | D | Config 1 |
| 6 | B | D | Config 2 |
| 7 | B | E | Config 1 |
| 8 | B | E | Config 2 |
| 9 | C | D | Config 1 |
| 10 | C | D | Config 2 |
| 11 | C | E | Config 1 |
| 12 | C | E | Config 2 |

### <a name="scale-limits"></a>Límites de escalado

* N.º máx. de monitores de conexión por suscripción por región: 100
* N.º máx. de grupos de prueba por monitor de conexión: 20
* N.º máx. de orígenes + destinos por monitor de conexión: 100
* N.º máx. de configuraciones de prueba por monitor de conexión: 20 a través de Armclient, 2 a través del portal.

## <a name="step-4--data-analysis-and-alerts"></a>Paso 4:  Análisis de datos y alertas

Una vez que se crea un monitor de conexión, los orígenes comprueban la conectividad con los destinos en función de la configuración de prueba especificada.

### <a name="checks-in-a-test"></a>Comprobaciones en una prueba

En función del protocolo seleccionado por un usuario en la configuración de prueba, Monitor de conexión (versión preliminar) ejecuta una serie de comprobaciones para el par origen-destino a la frecuencia de prueba elegida.

Si se selecciona HTTP, el servicio calcula el número de respuestas HTTP que devolvieron un código de respuesta para determinar el porcentaje de comprobaciones erróneas.  Para calcular el RTT, se mide el tiempo necesario para recibir la respuesta de una llamada HTTP.

Si se selecciona TCP o ICMP, el servicio calcula el porcentaje de paquetes para determinar el porcentaje de comprobaciones erróneas. Para calcular el RTT, se mide el tiempo necesario para recibir la ACK para los paquetes enviados. Si ha habilitado los datos de traceroute para las pruebas de red, puede ver la pérdida y la latencia de salto a salto para la red local.

### <a name="states-of-a-test"></a>Estados de una prueba

En función de los datos devueltos por las comprobaciones de una prueba, cada prueba puede tener los estados siguientes:

* Correcto = cuando los valores reales de Comprobaciones erróneas (%) y RTT están dentro de los umbrales especificados.
* Error = cuando los valores reales de Comprobaciones erróneas (%) y RTT cruzan los umbrales especificados. Si no se especifica ningún umbral, una prueba se marca como error cuando las comprobaciones erróneas (%) = 100 %.
* Advertencia = cuando no se especifican los criterios para comprobaciones erróneas (%). En tal caso, Monitor de conexión (versión preliminar) usa un criterio de establecimiento automático como umbral y, cuando se infringe el umbral, el estado de la prueba se establece en "Advertencia".

### <a name="data-collection-analysis-and-alerts"></a>Recopilación de datos, análisis y alertas

Todos los datos recopilados por Monitor de conexión (versión preliminar) se almacenan en el área de trabajo de Log Analytics configurada en el momento de la creación del monitor de conexión. Los datos de supervisión también están disponibles en métricas de Azure Monitor. Puede usar Log Analytics para mantener los datos de supervisión siempre que desee, pero Azure Monitor almacena las métricas de forma predeterminada durante 30 días **.** A continuación, puede [establecer alertas basadas en métricas para los datos](https://azure.microsoft.com/blog/monitor-at-scale-in-azure-monitor-with-multi-resource-metric-alerts/).

#### <a name="monitoring-dashboards-in-connection-monitor-solution"></a>Paneles de supervisión en la solución Monitor de conexión

Verá una lista de los monitores de conexión a los que tiene acceso para una determinada selección de suscripciones, regiones, marcas de tiempo, orígenes y tipos de destino.

Cuando navega a Monitor de conexión (versión preliminar) desde el servicio Network Watcher, puede elegir **Ver por**:

* Monitor de conexión (valor predeterminado): Lista de todos los monitores de conexión creados para las suscripciones, regiones, marca de tiempo, orígenes y tipos de destino elegidos.
* Grupos de prueba: Lista de todos los grupos de prueba creados para las suscripciones, regiones, marca de tiempo, orígenes y tipos de destino elegidos. Estos grupos de prueba no se filtran por monitor de conexión.
* Pruebas: Lista de todas las pruebas para las suscripciones, regiones, marca de tiempo, orígenes y tipos de destino elegidos. Estas pruebas no se filtran por monitor de conexión ni grupo de prueba.

En el panel, puede expandir cada monitor de conexión en grupos de prueba, y cada grupo de prueba en las distintas pruebas individuales que se ejecutan en él. Se marca como [1] en la imagen siguiente.

Puede filtrar esta lista en función de lo siguiente:

* Filtros de nivel superior: Suscripciones, regiones, marca de tiempo, origen y tipos de destino. Se marca como [2] en la imagen siguiente.
* Filtros basados en estado: Filtro de segundo nivel en estado del monitor de conexión/grupo de prueba/prueba. Se marca como [3] en la imagen siguiente.
* Campo de búsqueda: Elija "Todo" para realizar una búsqueda genérica. Para buscar en una entidad específica, use la lista desplegable para restringir los resultados de la búsqueda. Se marca como [4] en la imagen siguiente.

![Filtrar pruebas](./media/connection-monitor-2-preview/cm-view.png)

Por ejemplo:

1. Para ver todas las pruebas en todo Monitor de conexión (versión preliminar) donde la dirección IP de origen = 10.192.64.56:
   1. Cambie Ver por a "Pruebas".
   2. Busque = 10.192.64.56.
   3. Use la lista desplegable junto a valor para seleccionar "Orígenes".
2. Para filtrar solo las pruebas con errores en todo Monitor de conexión (versión preliminar) donde la dirección IP de origen = 10.192.64.56
   1. Cambie Ver por a "Pruebas".
   2. Seleccione "Error" en los filtros basados en el estado.
   3. Busque el campo = 10.192.64.56.
   4. Use la lista desplegable junto a valor para seleccionar "Orígenes".
3. Para filtrar solo las pruebas con errores en todo Monitor de conexión (versión preliminar) donde el destino es outlook.office365.com.
   1. Cambie Ver por a "Pruebas".
   2. Seleccione "Error" en los filtros basados en el estado.
   3. Busque el campo = outlook.office365.com.
   4. Use la lista desplegable junto a valor para seleccionar "Destinos".

   ![Pruebas con error](./media/connection-monitor-2-preview/tests-view.png)

Para ver las tendencias de las comprobaciones erróneas (%) y RTT para:

1. Monitor de conexión
   1. Haga clic en el monitor de conexión que desea investigar en detalle.
   2. De forma predeterminada, verá los datos de supervisión por "Grupos de prueba".

      ![Ver métricas por](./media/connection-monitor-2-preview/cm-drill-landing.png)

   3. Elija el grupo de prueba que desea investigar en detalle.

      ![Métricas por grupo de prueba](./media/connection-monitor-2-preview/cm-drill-select-tg.png)

   4. Verá las 5 primeras pruebas con error en Comprobaciones erróneas (%) o RTT (ms) para el grupo de prueba que eligió en el paso anterior. Para cada prueba, verá las líneas de tendencia de las comprobaciones erróneas (%) y RTT (ms).
   5. Seleccione una prueba de la lista anterior o elija otra prueba para investigar en detalle.
   6. En el intervalo de tiempo seleccionado, en el caso de las comprobaciones erróneas (%), verá el umbral y los valores reales. En el caso de RTT (ms), verá los valores de umbral, promedio, mínimo y máximo.

      ![RTT](./media/connection-monitor-2-preview/cm-drill-charts.png)

  7. Cambie el intervalo de tiempo para ver más datos.
  8. Puede cambiar la vista en el paso b y elegir ver por orígenes, destinos o configuraciones de prueba. A continuación, elija un origen basado en pruebas con error e investigue las 5 principales pruebas con error.  Por ejemplo: Elija Ver por: Orígenes y destinos para investigar todas las pruebas que se ejecutan entre esa combinación en el monitor de conexión seleccionado.

      ![RTT2](./media/connection-monitor-2-preview/cm-drill-select-source.png)

2. Grupo de prueba
   1. Haga clic en el grupo de prueba que desea investigar en detalle.
   2. De forma predeterminada, verá los datos de supervisión por "Origen + destino + configuración de prueba (prueba)".

      ![RTT3](./media/connection-monitor-2-preview/tg-drill.png)

   3. Elija la prueba que desea investigar en detalle.
   4. En el intervalo de tiempo seleccionado, en el caso de las comprobaciones erróneas (%), verá el umbral y los valores reales. En el caso de RTT (ms), verá los valores de umbral, promedio, mínimo y máximo. También verá alertas desencadenadas específicas a la prueba seleccionada.
   5. Cambie el intervalo de tiempo para ver más datos.
   6. Puede cambiar la vista en el paso b y elegir ver por orígenes, destinos o configuraciones de prueba. A continuación, elija una entidad para investigar las 5 principales pruebas con error.  Por ejemplo: Elija Ver por: Orígenes y destinos para investigar todas las pruebas que se ejecutan entre esa combinación en el monitor de conexión seleccionado.

3. Prueba
   1. Haga clic en el Origen + Destino + Configuración de prueba que desea investigar en detalle.
   2. En el intervalo de tiempo seleccionado, en el caso de las comprobaciones erróneas (%), verá el umbral y los valores reales. En el caso de RTT (ms), verá los valores de umbral, promedio, mínimo y máximo. También verá alertas desencadenadas específicas a la prueba seleccionada.

      ![Test1](./media/connection-monitor-2-preview/test-drill.png)

   3. También puede hacer clic en "Topología" para ver la topología de red en cualquier momento.

      ![Test2](./media/connection-monitor-2-preview/test-topo.png)

   4. Puede hacer clic en cualquier salto de vínculo para la red de Azure para ver los problemas identificados por el monitor de conexión. Esta funcionalidad no está disponible para las redes locales en este momento.

       ![Test3](./media/connection-monitor-2-preview/test-topo-hop.png)

#### <a name="log-queries-in-azure-monitor-log-analytics"></a>Registrar consultas en Log Analytics de Azure Monitor

Utilice Log Analytics para crear vistas personalizadas de los datos de supervisión. Todos los datos que se muestran en la UI se rellenan a partir de Log Analytics. Puede realizar un análisis interactivo de los datos en el repositorio y correlacionar los datos procedentes de distintos orígenes, como el estado del agente y otras aplicaciones basadas en Log Analytics. También puede exportar los datos a Excel, Power BI o un vínculo compartible.

#### <a name="metrics-in-azure-monitor"></a>Métricas en Azure Monitor

Para los monitores de conexión que se crearon antes de la experiencia de Monitor de conexión (versión preliminar), las cuatro métricas estarán disponibles. En el caso de los monitores de conexión creados mediante la experiencia de Monitor de conexión (versión preliminar), los datos solo estarán disponibles para las métricas etiquetadas con "(versión preliminar)".

Tipo de recurso: Microsoft.Network/networkWatchers/connectionMonitors

| Métrica | Nombre de métrica para mostrar | Unidad | Tipo de agregación | Descripción | Dimensions |
| --- | --- | --- | --- | --- | --- |
| ProbesFailedPercent | % de sondeos con error | Percent | Average | % de sondeos de supervisión de conectividad con error | Sin dimensiones |
| AverageRoundtripMs | Prom. Tiempo de ida y vuelta (ms) | MilliSeconds | Average | Promedio de tiempo de ida y vuelta de red (ms) para los sondeos de supervisión de conectividad que se envían entre el origen y el destino |             Sin dimensiones |
| ChecksFailedPercent (versión preliminar) | Comprobaciones erróneas (%) (versión preliminar) | Percent | Average | Porcentaje de comprobaciones erróneas para una prueba |Lista: - ConnectionMonitorResourceId - SourceAddress - SourceName - SourceResourceId - SourceType - Protocol - DestinationAddress - DestinationName - DestinationResourceId - DestinationType -DestinationPort - TestGroupName - TestConfigurationName - Region |
| RoundTripTimeMs (versión preliminar) | Tiempo de ida y vuelta (ms) (versión preliminar) | Milisegundos | Average | Tiempo de ida y vuelta (ms) para las comprobaciones enviadas entre el origen y el destino. Este valor no se calcula como promedio. | Lista: - ConnectionMonitorResourceId - SourceAddress - SourceName - SourceResourceId - SourceType - Protocol - DestinationAddress - DestinationName - DestinationResourceId - DestinationType - DestinationPort - TestGroupName - TestConfigurationName - Region |

 ![Métricas de supervisión](./media/connection-monitor-2-preview/monitor-metrics.png)

#### <a name="metric-alerts-in-azure-monitor"></a>Alertas de métricas en Azure Monitor

Para crear una alerta:

1. Elija el recurso de monitor de conexión creado mediante Monitor de conexión (versión preliminar).
2. Asegúrese de que aparezca "Métrica" como tipo de señal para el recurso seleccionado en el paso anterior.
3. En Agregar condición, elija el Nombre de señal como ChecksFailedPercent(Preview) o RoundTripTimeMs(Preview), y el Tipo de señal como Métricas. Por ejemplo: Elija ChecksFailedPercent(Preview)
4. Se mostrarán todas las dimensiones aplicables según las métricas.  Elija el nombre de la dimensión y el valor de la dimensión. Por ejemplo: Elija la Dirección de origen y proporcione la dirección IP de cualquier origen implicado en el recurso de monitor de conexión elegido en el paso 1.
5. En Lógica de alerta, elija:
   1. Tipo de condición: Estática
   2. Condición y umbral
   3. Granularidad de agregación y Frecuencia de evaluación: Monitor de conexión (vista previa) actualiza los datos cada 1 minuto.
6.  En acciones, elija el grupo de acciones.
7. Proporcione los detalles de la alerta.
8. Crear regla de alertas

   ![Alertas](./media/connection-monitor-2-preview/mdm-alerts.jpg)

## <a name="step-5-diagnose-issues-in-your-network"></a>Paso 5: Diagnosticar problemas en la red

El monitor de conexión le ayudará a diagnosticar problemas correspondientes al recurso de monitor de conexión y en la red. Los agentes de Log Analytics que instaló en el paso 1 detectarán los problemas de la red híbrida, y la extensión de Network Watcher detectará los problemas de Azure.  Los problemas de la red híbrida se podrán ver en la página Diagnósticos, y los problemas de red de Azure se podrán ver en la topología de red.

En el caso de las redes con VM locales como origen, se detecta:

* Se ha agotado el tiempo de espera para la solicitud
* Punto de conexión no resuelto por DNS: temporal o persistente. Dirección URL no válida.
* No se encontraron hosts.
* El origen no puede conectarse al destino. Destino no accesible a través de ICMP.
* Problema relacionado con certificados: Se necesita un certificado de cliente para autenticar al agente, no se puede acceder a la lista de reubicación de certificados, el nombre de host del punto de conexión no coincide con el nombre de firmante o nombre alternativo del firmante del certificado, falta el certificado raíz en el almacén de entidades de certificación de confianza del equipo local, el certificado SSL expiró, no es válido, se revocó, es incompatible.

En el caso de las redes con VM de Azure son orígenes, se detecta:

* Problemas del agente: Se detuvo el agente, error de resolución de DNS, no hay ninguna aplicación/agente de escucha escuchando en el puerto de destino, no se pudo abrir el socket
* Problemas de estado de VM: Iniciándose, deteniéndose, se detuvo, desasignándose, se desasignó, reiniciándose, sin asignar
* Falta la entrada de la tabla ARP.
* Tráfico bloqueado debido a problemas de firewall local, reglas de NSG.
* Puerta de enlace de red virtual: Faltan rutas, el túnel entre dos puertas de enlace está desconectado o falta, o el túnel no encuentra la segunda puerta de enlace, no se encontró información de emparejamiento
* Falta la ruta en MS Edge.
* Tráfico detenido debido a rutas del sistema o UDR.
* BGP no habilitado en la conexión de puerta de enlace.
* Error en el sondeo de DIP en Load Balancer.
