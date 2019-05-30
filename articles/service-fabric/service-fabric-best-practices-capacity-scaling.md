---
title: Procedimientos recomendados para el escalado y planeamiento de capacidad de Azure Service Fabric | Microsoft Docs
description: Procedimientos recomendados para el planeamiento y escalado de aplicaciones y clústeres de Service Fabric.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2019
ms.author: pepogors
ms.openlocfilehash: 9bddb6552b11dd506ee3e2c1c416c15da11048b7
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66258752"
---
# <a name="capacity-planning-and-scaling"></a>Escalado y planeamiento de capacidad

Es importante planear la capacidad antes de crear cualquier clúster de Azure Service Fabric o de escalar los recursos de proceso que hospedan al clúster. Para obtener más información sobre el planeamiento de la capacidad, consulte [planeamiento de capacidad del clúster de Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity). Para más recomendado vea una guía para la escalabilidad del clúster [consideraciones sobre escalabilidad de Service Fabric](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric#scalability-considerations)

Además de considerar las características de tipo y el clúster de nodo, debe planear para escalar las operaciones que tarden más de una hora en completarse para un entorno de producción, independientemente del número de máquinas virtuales que se va a agregar.

## <a name="auto-scaling"></a>Ajuste de escala automático
Las operaciones de escalado deben realizarse a través de la implementación de plantillas de recursos de Azure, porque es el procedimiento recomendado para tratar [las configuraciones de recursos como código]( https://docs.microsoft.com/azure/service-fabric/service-fabric-best-practices-infrastructure-as-code)y el uso conjunto de escalado de máquina virtual, el escalado automático dará como resultado la plantilla de Resource Manager con control de versiones definir de manera inexacta el escalado de máquinas virtuales del conjunto de números de instancias; aumenta el riesgo de las futuras implementaciones causando las operaciones de escalado no deseadas y, en general debe usar el escalado automático si:

* Si implementar las plantillas de Resource Manager con la capacidad adecuada declarada no es compatible con su caso de uso.
  * Además del escalado manual, puede configurar un [integración continua y la canalización de entrega de servicios de DevOps de Azure con proyectos de implementación de grupo de recursos de Azure](https://docs.microsoft.com/azure/vs-azure-tools-resource-groups-ci-in-vsts), que se desencadena habitualmente por una aplicación lógica que aprovecha las métricas de rendimiento de máquina virtual consultadas desde [API de REST de Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/rest-api-walkthrough); eficazmente el escalado automático en función de las métricas que desee, al tiempo que optimiza para la suma del valor de Azure Resource Manager.
* Solo será necesario escalar horizontalmente un nodo del conjunto de escalado de máquinas virtuales a la vez.
  * Para escalar horizontalmente por 3 o más nodos a la vez y debería [escalar un clúster de Service Fabric reducir horizontalmente mediante la adición de un conjunto de escalado de máquinas virtuales](https://docs.microsoft.com/azure service-fabric/virtual-machine-scale-set-scale-node-type-scale-out)y es más seguro escalar en y máquina virtual conjuntos de escalado horizontal de 1 nodo a la vez.
* Si tiene un nivel de confiabilidad Silver o superior en su clúster de Service Fabric y un nivel de durabilidad Silver o superior en cualquier conjunto de escalado en el que haya configurado reglas de escalado automático.
  * Capacidad de las reglas de escalado automático (mínimo) debe ser igual o mayor que 5 instancias de máquina virtual y debe ser igual o mayor que el mínimo nivel de confiabilidad para el tipo de nodo principal.

> [!NOTE]
> Tejido de servicio con estado de Azure Service Fabric: / System/InfastructureService/< NODE_TYPE_NAME >, se ejecuta en cada tipo de nodo que tiene Silver o mayor durabilidad, que es el único servicio de sistema que se puede para ejecutar en Azure en cualquiera de los tipos de nodo de clústeres .

## <a name="vertical-scaling-considerations"></a>Consideraciones sobre escalado vertical

[Escalado vertical](https://docs.microsoft.com/azure/service-fabric/virtual-machine-scale-set-scale-node-type-scale-out) un tipo de nodo en Azure Service Fabric requiere un número de pasos y consideraciones. Por ejemplo:

* El clúster debe estar en buen estado antes de escalarlo. En caso contrario, solo se desestabilizar aún más el clúster.
* **Mayor o nivel de durabilidad Silver** es necesaria para todos los tipos de nodo de clúster de Service Fabric que hospedan servicios con estado.

> [!NOTE]
> El tipo de nodo principal que hospeda los servicios del sistema Stateful Service Fabric debe ser la durabilidad Silver nivel o superior. Tras habilitar la durabilidad nivel Silver, las operaciones del clúster tales como las actualizaciones o la adición o remoción de nodos serán más lentas, ya que el sistema realiza optimizaciones para la seguridad de los datos y no para la velocidad de las operaciones.

Un conjunto de escalado de máquinas virtuales de escalado vertical es una operación destructiva. En su lugar, escale horizontalmente el clúster agregando un nuevo conjunto de escalado con la SKU deseada y migre los servicios a la SKU deseada para completar una operación de escalado vertical segura. Cambiar un recurso de conjunto de escalado SKU de máquina virtual es una operación destructiva porque vuelve a las imágenes de los hosts que se encarga de todo el estado guardado de forma local.

El clúster usa las [restricciones de ubicación y propiedades de nodo](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-resource-manager-cluster-description#node-properties-and-placement-constraints) de Service Fabric para determinar en qué ubicación hospedará los servicios de las aplicaciones. Cuando el tipo de nodo principal, el escalado vertical declarar valores idénticos de propiedad para `"nodeTypeRef"`, que se encuentra en el escalado de máquinas virtuales establece la extensión de Service Fabric. En el siguiente fragmento de plantilla de Resource Manager se muestran las propiedades que va a declarar (con el mismo valor para los nuevos conjuntos de escalado aprovisionado que escalará) y que solo se admiten como un estado temporal para el clúster:

```json
"settings": {
   "nodeTypeRef": ["[parameters('primaryNodetypeName')]"]
}
```

> [!NOTE]
> No deje el clúster ejecutándose con varios conjuntos de escalado que usen los mismos valores de la propiedad `nodeTypeRef` durante más tiempo del necesario para que la operación de escalado vertical se realice correctamente.
> Valide siempre las operaciones en entornos de prueba antes de intentar realizar cambios en el entorno de producción. De forma predeterminada, servicios de sistema de clúster de Service Fabric tiene una restricción de colocación para el tipo de nodo principal de destino solo.

Una vez declaradas las restricciones de ubicación y las propiedades de nodo, realice los siguientes pasos en una instancia de máquina virtual a la vez. De este modo, los servicios del sistema (y los servicios con estado) se cerrarán correctamente en la instancia de máquina virtual que se va a quitar a la vez que se crean nuevas réplicas en otros nodos.

1. En PowerShell, ejecute `Disable-ServiceFabricNode` con la intención 'RemoveNode' para deshabilitar el nodo que se va a quitar. Quite el tipo de nodo que tiene el número más alto. Por ejemplo, si tiene un clúster de seis nodos, quite la instancia de máquina virtual "MyNodeType_5".
2. Ejecute `Get-ServiceFabricNode` para asegurarse de que el nodo ya está como deshabilitado. Si no es así, espere hasta que se deshabilite. Esta operación puede tardar un par de horas por cada nodo. No continúe hasta que el nodo esté como deshabilitado.
3. Disminuya en uno el número de máquinas virtuales de ese tipo de nodo. Se quitará la instancia de máquina virtual más alta.
4. repita los pasos del 1 al 3 según vea necesario, pero nunca apague las instancias del nodo principal ni las reduzca a un número inferior al que garantiza el nivel de confiabilidad. Consulte el [planeamiento de la capacidad del clúster de Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) para obtener una lista de instancias recomendadas.

> [!NOTE]
> Es un escenario admitido para cuándo se debe realizar una operación de escalado vertical: ¿Puedo migrar mi clúster de Service Fabric y la aplicación desde el disco no administrado a Managed Disks sin tiempo de inactividad de aplicación. Mediante el aprovisionamiento de una nueva máquina virtual conjuntos de escalado con discos administrados, y realizar una actualización de la aplicación con las restricciones de posición que tienen como destino aprovisiona capacidad; clúster de Service Fabric puede programar la carga de trabajo en la capacidad de nodo de clúster aprovisionado que se implanta por dominio de actualización sin tiempo de inactividad de aplicación. [SKU básica de los equilibradores de carga Azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview#skus) los puntos de conexión del grupo de back-end pueden ser una máquina Virtual en un único conjunto de disponibilidad o conjunto de escalado de máquinas virtuales. Esto significa que no puede usar un equilibrador de carga de SKU básica si mueve la aplicación de Service Fabric sistemas entre conjuntos de escalado, sin causar temporal inaccesibilidad de su tejido de servicio de clúster de punto de conexión de administración, aunque el clúster y su aplicación todavía se está ejecutando; Normalmente usuario aprovisionar un equilibrador de carga de SKU estándar, cuando se realiza un intercambio de dirección IP (VIP) virtuales entre los recursos de un equilibrador de carga de SKU básica y LB de SKU estándar, para mitigar cualquier futura aproximadamente 30 segundos de inaccesibilidad necesario para el intercambio de VIP.

## <a name="horizontal-scaling"></a>Escalado horizontal

El escalado horizontal en Service Fabric puede hacerse [manualmente](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-scale-up-down) o [mediante programación](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-programmatic-scaling).

> [!NOTE]
> Si va a escalar un tipo de nodo con una durabilidad de nivel Silver o Gold, el escalado será lento.

### <a name="scaling-out"></a>Escalado horizontal

Escalar horizontalmente un clúster de Service Fabric aumentando el número de instancias para un conjunto de escalado de máquina virtual concreta. Puede escalar horizontalmente mediante programación si utiliza AzureClient y el identificador del conjunto de escalado para aumentar su capacidad.

```c#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = (int)Math.Min(MaximumNodeCount, scaleSet.Capacity + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
```

Para escalar horizontalmente de forma manual, actualice la capacidad de la propiedad de SKU de deseado [conjunto de escalado de máquinas virtuales](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile) recursos.
```json
"sku": {
    "name": "[parameters('vmNodeType0Size')]",
    "capacity": "[parameters('nt0InstanceCount')]",
    "tier": "Standard"
}
```

### <a name="scaling-in"></a>Reducir horizontalmente

La reducción horizontal requiere más atención que el escalado horizontal. Por ejemplo:

* Los servicios del sistema de Service Fabric se ejecutan en el tipo de nodo principal del clúster. Nunca apague las instancias de esos tipos de nodo ni las reduzca a un número inferior al que garantiza el nivel de confiabilidad. 
* Para los servicios con estado, necesita que un determinado número de nodos estén siempre activos para mantener la disponibilidad y preservar el estado del servicio. Como mínimo, necesita que el número de nodos sea igual al recuento de conjuntos de réplicas de destino de la partición o el servicio.

Para reducir horizontalmente de forma manual, siga estos pasos:

1. En PowerShell, ejecute `Disable-ServiceFabricNode` con la intención 'RemoveNode' para deshabilitar el nodo que se va a quitar. Quite el tipo de nodo que tiene el número más alto. Por ejemplo, si tiene un clúster de seis nodos, quite la instancia de máquina virtual "MyNodeType_5".
2. Ejecute `Get-ServiceFabricNode` para asegurarse de que el nodo ya está como deshabilitado. Si no es así, espere hasta que se deshabilite. Esta operación puede tardar un par de horas por cada nodo. No continúe hasta que el nodo esté como deshabilitado.
3. Disminuya en uno el número de máquinas virtuales de ese tipo de nodo. Se quitará la instancia de máquina virtual más alta.
4. repita los pasos del 1 al 3 según vea necesario, pero nunca apague las instancias del nodo principal ni las reduzca a un número inferior al que garantiza el nivel de confiabilidad. Consulte el [planeamiento de la capacidad del clúster de Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) para obtener una lista de instancias recomendadas.

Para escalar de forma manual, actualice la capacidad de la propiedad de SKU de deseado [conjunto de escalado de máquinas virtuales](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile) recursos.

```json
"sku": {
    "name": "[parameters('vmNodeType0Size')]",
    "capacity": "[parameters('nt0InstanceCount')]",
    "tier": "Standard"
}
```

1. Repita los pasos del 1 al 3 hasta que aprovisione la capacidad que quiera. Nunca reduzca las instancias del nodo principal a un número inferior al que garantiza el nivel de confiabilidad. Para obtener más información acerca de los niveles de confiabilidad y el número de instancias que necesitan, consulte el [planeamiento de la capacidad del clúster de Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity).

Debe preparar el nodo para que se apague con el fin de realizar una reducción vertical mediante programación. Esto implica buscar el nodo que se quitará (el nodo más alto de la instancia) y desactivarlo. Por ejemplo:

```c#
using (var client = new FabricClient())
{
    var mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync())
        .Where(n => n.NodeType.Equals(NodeTypeToScale, StringComparison.OrdinalIgnoreCase))
        .Where(n => n.NodeStatus == System.Fabric.Query.NodeStatus.Up)
        .OrderByDescending(n =>
        {
            var instanceIdIndex = n.NodeName.LastIndexOf("_");
            var instanceIdString = n.NodeName.Substring(instanceIdIndex + 1);
            return int.Parse(instanceIdString);
        })
        .FirstOrDefault();
```

Una vez que identifique el nodo que quitará, desactívelo y quítelo con la misma instancia de `FabricClient` (en este caso, `client`) y el mismo nombre de instancia de nodo (en este caso, `instanceIdString`) que usó en el código anterior:

```c#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);

// Remove the node from the Service Fabric cluster
ServiceEventSource.Current.ServiceMessage(Context, $"Disabling node {mostRecentLiveNode.NodeName}");
await client.ClusterManager.DeactivateNodeAsync(mostRecentLiveNode.NodeName, NodeDeactivationIntent.RemoveNode);

// Wait (up to a timeout) for the node to gracefully shutdown
var timeout = TimeSpan.FromMinutes(5);
var waitStart = DateTime.Now;
while ((mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Up || mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Disabling) &&
        DateTime.Now - waitStart < timeout)
{
    mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync()).FirstOrDefault(n => n.NodeName == mostRecentLiveNode.NodeName);
    await Task.Delay(10 * 1000);
}

// Decrement VMSS capacity
var newCapacity = (int)Math.Max(MinimumNodeCount, scaleSet.Capacity - 1); // Check min count 

scaleSet.Update().WithCapacity(newCapacity).Apply();
```

> [!NOTE]
> Al escalar un clúster horizontalmente verá la instancia de quitado nodo o máquina virtual que se muestra en un estado incorrecto en el Explorador de Service Fabric. Para obtener una explicación de este comportamiento, consulte [comportamientos que se puede observar en Service Fabric Explorer](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-scale-up-down#behaviors-you-may-observe-in-service-fabric-explorer). Puede:
> * Llame a [Remove-ServiceFabricNodeState cmd](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate?view=azureservicefabricps) con el nombre de nodo adecuado.
> * Implementar [aplicación de service fabric escalado automático auxiliar](https://github.com/Azure/service-fabric-autoscale-helper/) en el clúster, lo que garantiza la escala nodos inactivos se borran de Service Fabric Explorer.

## <a name="reliability-levels"></a>Niveles de confiabilidad

El [nivel de confiabilidad](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) es una propiedad de los recursos de clúster de Service Fabric y no puede configurarse de forma diferente para los tipos de nodo individual. Controla el factor de replicación de los servicios del sistema para el clúster y es una configuración a nivel de recursos de clúster. El nivel de confiabilidad determinará el número mínimo de nodos que debe tener el tipo de nodo principal. El nivel de confiabilidad puede adoptar los siguientes valores:

* Platinum: ejecuta los servicios del sistema con un objetivo de recuento de conjunto de réplicas de siete y nueve nodos raíz.
* Gold: ejecuta los servicios del sistema con un objetivo de recuento de conjunto de réplicas de siete y siete nodos raíz.
* Silver: ejecuta los servicios del sistema con un objetivo de recuento de conjunto de réplicas de cinco y cinco nodos raíz.
* Bronze: ejecuta los servicios del sistema con un objetivo de recuento de conjunto de réplicas de tres y tres nodos raíz.

El nivel de confiabilidad mínima recomendada es Silver.

El nivel de confiabilidad se establece en la sección de propiedades del [recurso Microsoft.servicefabric/clusters](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/2018-02-01/clusters) de la siguiente forma:

```json
"properties":{
    "reliabilityLevel": "Silver"
}
```

## <a name="durability-levels"></a>Niveles de durabilidad

> [!WARNING]
> Los tipos de nodos que se ejecutan con la durabilidad Bronze _no obtienen privilegios_. Es decir, los trabajos de infraestructura que afectan a las cargas de trabajo sin estado no se detendrán ni retrasarán, lo que podría afectar a las cargas de trabajo. Utilice el nivel de durabilidad Bronze en los tipos de nodos que ejecutan cargas de trabajo sin estado. Para las cargas de trabajo de producción, ejecute el nivel Silver o superior para garantizar la coherencia del estado. Elija la confiabilidad adecuada según las instrucciones de la [documentación de planeamiento de capacidad](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity).

El nivel de durabilidad se debe establecer en los dos recursos. El perfil de la extensión de la [recurso de conjunto de escalado de máquinas virtuales](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile):

```json
"extensionProfile": {
    "extensions":          {
        "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
        "properties": {
            "settings": {
                "durabilityLevel": "Bronze"
            }
        }
    }
}
```

Y en el valor `nodeTypes` del [recurso Microsoft.ServiceFabric/clusters](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/2018-02-01/clusters): 

```json
"nodeTypes": [
    {
        "name": "[variables('vmNodeType0Name')]",
        "durabilityLevel": "Bronze"
    }
]
```

## <a name="next-steps"></a>Pasos siguientes

* Creación de un clúster en máquinas virtuales o equipos que ejecutan Windows Server: [Creación de un clúster independiente con Windows Server](service-fabric-cluster-creation-for-windows-server.md)
* Creación de un clúster en máquinas virtuales o equipos que ejecutan Linux: [Crear un clúster Linux](service-fabric-cluster-creation-via-portal.md)
* Más información sobre las [opciones de soporte técnico de Service Fabric](service-fabric-support.md)

[Image1]: ./media/service-fabric-best-practices/generate-common-name-cert-portal.png
