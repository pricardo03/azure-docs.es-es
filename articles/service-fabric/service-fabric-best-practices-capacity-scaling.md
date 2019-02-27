---
title: Procedimientos recomendados para el escalado y planeamiento de capacidad de Azure Service Fabric | Microsoft Docs
description: Procedimientos recomendados para el planeamiento y escalado de aplicaciones y clústeres de Service Fabric.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: jeanpaul.connock
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: 9de6cc224c82bb07fee4d62cd5de1d1964001bab
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2019
ms.locfileid: "56446824"
---
# <a name="capacity-planning-and-scaling"></a>Escalado y planeamiento de capacidad

Es importante planear la capacidad antes de crear cualquier clúster de Azure Service Fabric o de escalar los recursos de proceso que hospedan al clúster. Para obtener más información sobre el planeamiento de la capacidad, consulte [planeamiento de capacidad del clúster de Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity). Además de tomar en cuenta las características de clúster y tipo de nodo, planifique las operaciones de escalado para que tarden más de una hora en completarse en un entorno de producción, independientemente del número de máquinas virtuales que vaya a agregar.

## <a name="auto-scaling"></a>Ajuste de escala automático
Las operaciones de escalado deben realizarse a través de la implementación de plantillas de recursos de Azure, ya que es un procedimiento recomendado para tratar [las configuraciones de recursos como código]( https://docs.microsoft.com/azure/service-fabric/service-fabric-best-practices-infrastructure-as-code). Además, usar el escalado automático de un conjunto de escalado de máquinas virtuales dará como resultado que la plantilla de Resource Manager con control de versiones defina de manera inexacta el número de instancias del conjunto de escalado de máquinas virtuales. Esto eleva el riesgo de que las implementaciones futuras ocasionen operaciones de escalado no deseadas. En general, debe usar el escalado automático solo en los siguientes casos:

* Si implementar las plantillas de Resource Manager con la capacidad adecuada declarada no es compatible con su caso de uso.
  * Además del escalado manual, puede configurar una [integración continua y canalización de entrega en Azure DevOps Services con proyectos del grupo de recursos de Azure]( https://docs.microsoft.com/azure/vs-azure-tools-resource-groups-ci-in-vsts). Normalmente, esta integración se desencadena gracias a una aplicación lógica que aprovecha las métricas de rendimiento de la máquina virtual consultadas desde la [API REST de Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/rest-api-walkthrough), lo que realiza un escalado automático eficaz en función de las métricas que quiera y se optimiza para agregar valor a Azure Resource Manager.
* Solo será necesario escalar horizontalmente un nodo del conjunto de escalado de máquinas virtuales a la vez.
  * Para escalar horizontalmente tres o más nodos a la vez, debe [escalar horizontalmente un clúster de Service Fabric mediante la adición de un conjunto de escalado de máquinas virtuales](https://docs.microsoft.com/azure/service-fabric/virtual-machine-scale-set-scale-node-type-scale-out). Además, es más seguro escalar y reducir horizontalmente los conjuntos de escalado de máquinas virtuales un nodo a la vez.
* Si tiene un nivel de confiabilidad Silver o superior en su clúster de Service Fabric y un nivel de durabilidad Silver o superior en cualquier conjunto de escalado en el que haya configurado reglas de escalado automático.
  * La capacidad [mínima] de las reglas de escalabilidad automática debe ser igual o mayor que cinco instancias de máquina virtual y debe ser igual o mayor que el mínimo del nivel de confiabilidad para el tipo de nodo principal.

> [!NOTE]
> El tejido de servicio con estado de Azure Service Fabric: / System/InfastructureService/< NOMBRE_DEL_TIPO_DE_NODO> se ejecuta en cada tipo de nodo que tiene una durabilidad de nivel Silver o superior, que es el único servicio de sistema que se puede ejecutar en Azure en cualquiera de los tipos de nodo de clústeres. 

## <a name="vertical-scaling-considerations"></a>Consideraciones sobre escalado vertical

Para [escalar verticalmente](https://docs.microsoft.com/azure/service-fabric/virtual-machine-scale-set-scale-node-type-scale-out#upgrade-the-size-and-operating-system-of-the-primary-node-type-vms) un tipo de nodo en Azure Service Fabric, se necesitan una serie de pasos y consideraciones. Por ejemplo: 
* El clúster debe estar en buen estado antes de escalarlo. En caso contrario, solo se desestabilizará aún más el clúster.
* Se requiere un **nivel de durabilidad Silver o superior** en todos los tipos de nodo de los clústeres de Service Fabric que hospedan servicios con estado.

> [!NOTE]
> El tipo de nodo principal que hospeda los servicios del sistema con estado de Service Fabric deben tener un nivel de durabilidad Silver o superior. Tras habilitar la durabilidad nivel Silver, las operaciones del clúster tales como las actualizaciones o la adición o remoción de nodos serán más lentas, ya que el sistema realiza optimizaciones para la seguridad de los datos y no para la velocidad de las operaciones.

Realizar el escalado vertical de un conjunto de escalado de máquinas virtuales es una operación destructiva. En su lugar, escale horizontalmente el clúster agregando un nuevo conjunto de escalado con la SKU deseada y migre los servicios a la SKU deseada para completar una operación de escalado vertical segura. Cambiar la SKU del recurso de conjunto de escalado de máquinas virtuales es una operación destructiva ya que restablece la imagen inicial de los hosts, eliminando todos los estados que persistan de forma local.

El clúster usa las [restricciones de ubicación y propiedades de nodo](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-resource-manager-cluster-description#node-properties-and-placement-constraints) de Service Fabric para determinar en qué ubicación hospedará los servicios de las aplicaciones. Al escalar verticalmente el tipo de nodo principal, declare valores de propiedad idénticos para `"nodeTypeRef"`, que se encuentra en la extensión de Service Fabric para el conjunto de escalado de máquinas virtuales. En el siguiente fragmento de plantilla de Resource Manager se muestran las propiedades que va a declarar (con el mismo valor para los nuevos conjuntos de escalado aprovisionado que escalará) y que solo se admiten como un estado temporal para el clúster:

```json
"settings": {
   "nodeTypeRef": ["[parameters('primaryNodetypeName')]"]
}
```

> [!NOTE]
> No deje el clúster ejecutándose con varios conjuntos de escalado que usen los mismos valores de la propiedad `nodeTypeRef` durante más tiempo del necesario para que la operación de escalado vertical se realice correctamente.
> Valide siempre las operaciones en entornos de prueba antes de intentar realizar cambios en el entorno de producción. De forma predeterminada, los servicios de sistema del clúster de Service Fabric tienen una restricción de ubicación dirigida solo a los tipos de nodo principal.

Una vez declaradas las restricciones de ubicación y las propiedades de nodo, realice los siguientes pasos en una instancia de máquina virtual a la vez. De este modo, los servicios del sistema (y los servicios con estado) se cerrarán correctamente en la instancia de máquina virtual que se va a quitar a la vez que se crean nuevas réplicas en otros nodos.
1. En PowerShell, ejecute `Disable-ServiceFabricNode` con la intención 'RemoveNode' para deshabilitar el nodo que se va a quitar. Quite el tipo de nodo que tiene el número más alto. Por ejemplo, si tiene un clúster de seis nodos, quite la instancia de máquina virtual "MyNodeType_5".
2. Ejecute `Get-ServiceFabricNode` para asegurarse de que el nodo ya está como deshabilitado. Si no es así, espere hasta que se deshabilite. Esta operación puede tardar un par de horas por cada nodo. No continúe hasta que el nodo esté como deshabilitado.
3. Disminuya en uno el número de máquinas virtuales de ese tipo de nodo. Se quitará la instancia de máquina virtual más alta.
4. repita los pasos del 1 al 3 según vea necesario, pero nunca apague las instancias del nodo principal ni las reduzca a un número inferior al que garantiza el nivel de confiabilidad. Consulte el [planeamiento de la capacidad del clúster de Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) para obtener una lista de instancias recomendadas.

## <a name="horizontal-scaling"></a>Escalado horizontal

El escalado horizontal en Service Fabric puede hacerse [manualmente](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-scale-up-down) o [mediante programación](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-programmatic-scaling).

> [!NOTE]
> Si va a escalar un tipo de nodo con una durabilidad de nivel Silver o Gold, el escalado será lento.

### <a name="scaling-out"></a>Escalado horizontal

Puede escalar horizontalmente un clúster de Service Fabric al aumentar el número de instancias para un determinado conjunto de escalado de máquinas virtuales. Puede escalar horizontalmente mediante programación si utiliza AzureClient y el identificador del conjunto de escalado para aumentar su capacidad.

```c#
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = (int)Math.Min(MaximumNodeCount, scaleSet.Capacity + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
```

Para escalar horizontalmente de forma manual, actualice la capacidad de la propiedad SKU del recurso de [conjunto de escalado de máquinas virtuales](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile) deseado.
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

Para reducir horizontalmente de forma manual, actualice la capacidad de la propiedad SKU del recurso de [conjunto de escalado de máquinas virtuales](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile) deseado.

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

## <a name="reliability-levels"></a>Niveles de confiabilidad

El [nivel de confiabilidad](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) es una propiedad de los recursos de clúster de Service Fabric y no puede configurarse de forma diferente para los tipos de nodos individuales. Controla el factor de replicación de los servicios del sistema para el clúster y es una configuración a nivel de recursos de clúster. El nivel de confiabilidad determinará el número mínimo de nodos que debe tener el tipo de nodo principal. El nivel de confiabilidad puede adoptar los siguientes valores:
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

El nivel de durabilidad se debe establecer en los dos recursos. En el perfil de extensión del [recurso del conjunto de escalado de máquinas virtuales](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile), agregue:

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
