---
title: Grupos de selección de ubicación de proximidad de Azure para una latencia de red óptima con aplicaciones SAP | Microsoft Docs
description: Se describen los escenarios de implementación de SAP con grupos de selección de ubicación de proximidad de Azure
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/15/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f97cce2d1a8b2de5634215629ddb997dc8f7196a
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/16/2019
ms.locfileid: "68235170"
---
# <a name="azure-proximity-placement-groups-for-optimal-network-latency-with-sap-applications"></a>Grupos de selección de ubicación de proximidad de Azure para una latencia de red óptima con aplicaciones SAP
Las aplicaciones SAP basadas en la arquitectura SAP NetWeaver o SAP S/4HANA son sensibles a la latencia de red entre la capa de aplicación de SAP y el nivel de base de datos de SAP. La razón de la sensibilidad de estas arquitecturas se basa en el hecho de que la mayor parte de la lógica de negocios se ejecuta en el nivel de aplicación. Como resultado de la ejecución de la lógica de negocios, el nivel de aplicación de SAP emite consultas al nivel de base de datos con una elevada frecuencia de miles y decenas de miles de consultas por segundo. En la mayoría de los casos, la naturaleza de esas consultas es sencilla. A menudo se pueden ejecutar en el nivel de base de datos en 500 microsegundos o incluso menos. El tiempo invertido en la red para enviar una consulta de este tipo desde la capa de aplicación al nivel de base de datos y recibir el conjunto de resultados del nivel de base de datos influye considerablemente en el tiempo que se tarda en ejecutar procesos empresariales. Esta sensibilidad a la latencia de red es la razón por la que se debe dedicar un tiempo a los proyectos de implementación de SAP para lograr una latencia de red óptima. En la [nota de SAP 1100926 sobre preguntas frecuentes de rendimiento de red](https://launchpad.support.sap.com/#/notes/1100926/E), SAP publicó algunas directrices para clasificar la latencia de red.

Por un lado, en muchas regiones de Azure, el número de centros de datos aumentó, a consecuencia también de la introducción de Azure Availability Zones. Por otra parte, los clientes, especialmente en el caso de los sistemas de gama alta de SAP, usaban SKU de máquinas virtuales más especiales de la familia de la serie M o de HANA (instancias grandes). Estos son tipos de máquinas virtuales de Azure que no están presentes en todos los centros de datos de una región de Azure específica. Como resultado de estas dos tendencias, los clientes experimentaron situaciones en las que la latencia de red no se encontraba en el intervalo óptimo y, en algunos casos, producía un rendimiento no óptimo de los sistemas SAP.

Con el fin de evitar este tipo de problemas, Azure ofrece una construcción denominada [Grupo de selección de ubicación de proximidad de Azure](https://docs.microsoft.com/azure/virtual-machines/linux/co-location). Esta nueva funcionalidad ya se ha usado para implementar varios sistemas SAP diferentes. Consulte el artículo referenciado para conocer las restricciones de los grupos de selección de ubicación de proximidad. En este artículo se tratarán los distintos escenarios de SAP en los que se pueden o deben usar los grupos de selección de ubicación de proximidad de Azure.

## <a name="what-are-proximity-placement-groups"></a>Qué son los grupos de selección de ubicación de proximidad 
Un grupo de selección de ubicación de proximidad de Azure es una construcción lógica, que en la fase de definición está enlazada a una región de Azure y a un grupo de recursos de Azure. Durante la implementación de máquinas virtuales, las siguientes hacen referencia a un grupo de selección de ubicación de proximidad:

- La primera máquina virtual de Azure implementada para el establecimiento del centro de datos. La primera máquina virtual se puede ver como una máquina virtual de anclaje que se implementa en un centro de datos basado en algoritmos de asignación de Azure que en última instancia se combinan con las definiciones del usuario para una zona de disponibilidad de Azure específica.
- Todas las máquinas virtuales posteriores implementadas que hacen referencia al grupo de selección de ubicación de proximidad para colocar todas las máquinas virtuales de Azure implementadas posteriormente en el mismo centro de datos en que se colocó la primera máquina virtual.

> [!NOTE]
> Si no hay ningún hardware de host implementado que pueda ejecutar un tipo de máquina virtual específica en el mismo centro de datos en el que se colocó la primera máquina virtual, la implementación del tipo de máquina virtual solicitada no se realizará correctamente y finalizará con un mensaje de error. Esto puede ocurrir en casos en los que una mayoría de máquinas virtuales poco convencionales, como máquinas virtuales con GPU o máquinas virtuales de HPC, debe centrarse en torno a, por ejemplo, una máquina virtual de la serie M que se ha implementado como primer tipo de máquina virtual.

Un único [grupo de recursos de Azure](https://docs.microsoft.com/azure/azure-resource-manager/manage-resources-portal) puede tener varios grupos de selección de ubicación de proximidad asignados. Sin embargo, un grupo de selección de ubicación de proximidad solo se puede asignar a un grupo de recursos de Azure.

Al usar grupos de selección de ubicación de proximidad, debe tener en cuenta lo siguiente:

- Al intentar conseguir un rendimiento óptimo para su sistema SAP y limitarse a un solo centro de datos de Azure para este sistema mediante el uso de grupos de selección de ubicación de proximidad, es posible que no pueda combinar todos los tipos de familias de máquinas virtuales dentro de este grupo de ubicación de proximidad. Esto se debe a que determinado hardware de host necesario para ejecutar de forma exclusiva un cierto tipo de máquina virtual puede no estar presente en el centro de datos en el que se implementó la "máquina virtual de anclaje" del grupo de selección de ubicación.
- En el ciclo de vida de un sistema SAP de este tipo, es posible que se vea obligado a trasladar el sistema a otro centro de datos. Por ejemplo, este traslado podría ser obligatorio en casos en los que haya decidido que la capa de DBMS de HANA de escalabilidad horizontal deba pasar de 4 a 16 nodos, pero ya no haya suficiente capacidad para obtener 12 máquinas virtuales adicionales del tipo que ya ha usado en el mismo centro de datos.
- Debido a una retirada de hardware, Microsoft podría desarrollar capacidades para los tipos de máquina virtual que ha usado en otro centro de datos, en lugar de en el mismo centro de datos. Esta situación podría obligarle a trasladar todas las máquinas virtuales del grupo de selección de ubicación de proximidad a otro centro de datos.


## <a name="azure-proximity-placement-groups-with-sap-systems-using-azure-vms-exclusively"></a>Grupos de selección de ubicación de proximidad de Azure con sistemas SAP que usan máquinas virtuales de Azure exclusivamente
En la mayoría de las implementaciones de sistemas de SAP NetWeaver y S/4HANA en Azure, no se usa [HANA (instancias grandes)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture). En el caso de las implementaciones de estos sistemas, es importante proporcionar el rendimiento óptimo entre el nivel de aplicación de SAP y el nivel de DBMS. Para ello, debe definir un grupo de selección de ubicación de proximidad de Azure solo para este sistema. En la mayoría de las implementaciones de cliente, los clientes optaron por crear un único [grupo de recursos de Azure](https://docs.microsoft.com/azure/azure-resource-manager/manage-resources-portal) para los sistemas SAP. En tal caso, se trataría de una relación de 1:1 entre, por ejemplo, los grupos de recursos del sistema ERP de producción y su grupo de ubicación de proximidad. En otros casos de implementación, los clientes organizaron los grupos de recursos horizontalmente y recopilaron todos los sistemas de producción en un único grupo de recursos. En tal caso, tendría una relación de uno a varios entre el grupo de recursos para sistemas de producción de SAP y varios grupos de selección de ubicación de proximidad de SAP ERP de producción, SAP BW, etc. Se debe evitar la agrupación de varios o incluso todos los sistemas de producción o no de producción de SAP en un solo grupo de selección de ubicación de proximidad. En algunas excepciones, donde un pequeño número de sistemas SAP o un sistema SAP y algunas aplicaciones circundantes deben tener una comunicación de red de baja latencia, puede considerar la posibilidad de mover estos sistemas a un solo grupo de selección de ubicación de proximidad. El motivo de esta recomendación es que cuantos más sistemas se agrupen en un grupo de selección de ubicación de proximidad, mayor será la probabilidad de lo siguiente:

- Necesitará un tipo de máquina virtual que no se puede ejecutar en el centro de datos específico en el que se ha establecido el grupo de selección de ubicación de proximidad.
- Los recursos de ciertas máquinas virtuales poco convencionales, como la serie M, pueden no satisfacerse, ya que con el tiempo se demandan más al agregar software adicional a un grupo de ubicación de proximidad existente.

El uso ideal descrito sería el siguiente:

![Grupos de selección de ubicación de proximidad para todas las máquinas virtuales de Azure](./media/sap-proximity-placement-scenarios/ppg-for-all-azure-vms.png)

En este caso, se agrupan sistemas SAP únicos en un solo grupo de recursos, cada uno con un grupo de selección de ubicación de proximidad. No existe ninguna dependencia respecto a si se usan configuraciones de escalabilidad horizontal de HANA o de escalabilidad vertical de DBMS.


## <a name="azure-proximity-placement-groups-and-hana-large-instances"></a>Grupos de selección de ubicación de proximidad de Azure y HANA (instancias grandes)
En los casos en los que algunos de los sistemas SAP se basan en [HANA (instancias grandes)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) como nivel de aplicación, se pueden lograr mejoras considerables en la latencia de red entre la unidad de HANA (instancias grandes) y las máquinas virtuales de Azure al usar unidades de HANA (instancias grandes) que se han implementado en [sellos o filas de la revisión 4](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture#networking-architecture-for-hana-large-instance). Una de las mejoras es que cuando se implementan las unidades de HANA (instancias grandes) tienen ya implementado un grupo de selección de ubicación de proximidad. Puede usar ese grupo de selección de ubicación de proximidad para implementar las máquinas virtuales del nivel de aplicación. Como resultado, esas máquinas virtuales se van a implementar en el mismo centro de datos que hospeda la unidad de HANA (instancias grandes).

Para detectar si la unidad de HANA (instancias grandes) está implementada en un sello o fila de la revisión 4, consulte el artículo [Control de instancias grandes de HANA en Azure mediante Azure Portal](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-li-portal#look-at-attributes-of-single-hli-unit). En la información general de los atributos de la unidad de HANA (instancias grandes), también puede ver el nombre del grupo de selección de ubicación de proximidad creado en el momento de la implementación para la unidad de HANA (instancias grandes). El nombre que se muestra en la información general de los atributos es el nombre del grupo de selección de ubicación de proximidad que debe usar para implementar las máquinas virtuales del nivel de aplicación.

Al contrario que con los sistemas SAP que solo usan máquinas virtuales de Azure, la decisión de cuántos [grupos de recursos de Azure](https://docs.microsoft.com/azure/azure-resource-manager/manage-resources-portal) se deben usar no le corresponderá por completo al utilizar HANA (instancias grandes). Todas las unidades de HANA (instancias grandes) de un [inquilino de HANA (instancias grandes)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-know-terms) se agrupan en un único grupo de recursos de Azure, tal como se describe [aquí](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-li-portal#display-of-hana-large-instance-units-in-the-azure-portal). A menos que desee realizar una implementación en distintos inquilinos para separar, por ejemplo, producción de no producción o ciertos sistemas, todas las unidades de HANA (instancias grandes) se implementarán en un mismo inquilino de HANA (instancias grandes), que de nuevo tendrá una relación de 1:1 con un grupo de recursos de Azure. Todas las unidades individuales tendrán definido un grupo de selección de ubicación de proximidad independiente. 

Como resultado, la agrupación entre grupos de recursos de Azure y grupos de selección de ubicación de proximidad para un solo inquilino tendrá el siguiente aspecto:

![Grupos de selección de ubicación de proximidad para todas las máquinas virtuales de Azure](./media/sap-proximity-placement-scenarios/ppg-for-hana-large-instance-units.png)


## <a name="short-example-of-deploying-with-azure-proximity-placement-groups"></a>Breve ejemplo de implementación con grupos de selección de ubicación de proximidad de Azure
Para demostrar cómo puede usar los grupos de selección de ubicación de proximidad de Azure para implementar la máquina virtual, aquí se incluye una lista de comandos de PowerShell que demuestran su uso en un primer ejercicio breve con grupos de selección de proximidad de Azure.

El primer paso después de haber iniciado sesión con [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/) es comprobar si se encuentra en la suscripción de Azure correcta que desea usar para la implementación con el comando:

<pre><code>
Get-AzureRmContext
</code></pre>

Si necesita cambiar a una suscripción diferente, puede ejecutar este comando para hacerlo:

<pre><code>
Set-AzureRmContext -Subscription "my PPG test subscription"
</code></pre>

Como tercer paso, desea crear un nuevo grupo de recursos de Azure con este comando:

<pre><code>
New-AzResourceGroup -Name "myfirstppgexercise" -Location "westus2"
</code></pre>

Ahora puede crear el nuevo grupo de selección de ubicación de proximidad con:

<pre><code>
New-AzProximityPlacementGroup -ResourceGroupName "myfirstppgexercise" -Name "letsgetclose" -Location "westus2"
</code></pre>

Ahora puede empezar a implementar su primera máquina virtual en este grupo de selección de ubicación de proximidad con un comando como el siguiente:

<pre><code>
New-AzVm -ResourceGroupName "myfirstppgexercise" -Name "myppganchorvm" -Location "westus2" -OpenPorts 80,3389 -ProximityPlacementGroup "letsgetclose" -Size "Standard_DS11_v2"
</code></pre>

Con el comando anterior, se implementa una máquina virtual basada en Windows. Después de que la implementación de la máquina virtual se haya realizado correctamente, el ámbito del centro de datos del grupo de selección de ubicación de proximidad se define en la región de Azure. Todas las implementaciones de máquina virtual posteriores que hagan referencia al grupo de selección de ubicación de proximidad como en el último comando se implementarán en el mismo centro de datos de Azure, siempre que el tipo de máquina virtual se pueda hospedar en el hardware colocado en ese centro de datos o haya disponible capacidad para ese tipo de máquina virtual.

## <a name="combine-availability-sets-and-availability-zones-with-proximity-placement-groups"></a>Combinación de conjuntos de disponibilidad y zonas de disponibilidad con grupos de selección de ubicación de proximidad 
Con las zonas de disponibilidad para implementaciones de sistemas SAP, una de las desventajas es el hecho de que el nivel de aplicación de SAP no se puede implementar de manera controlada mediante conjuntos de disponibilidad dentro de la zona específica. Dado que desea que el nivel de aplicación de SAP se implemente en las mismas zonas que la capa de DBMS y puesto que no es posible hacer referencia a una zona de disponibilidad y a un conjunto de disponibilidad al implementar una única máquina virtual, se ve obligado a implementar su nivel de aplicación haciendo referencia a una zona y, por tanto, pierde la capacidad de asegurarse de que las máquinas virtuales del nivel de aplicación se distribuyen en distintos dominios de actualización y error. Con la ayuda de los grupos de selección de ubicación de proximidad, puede superar esta restricción. La secuencia de implementaciones tendría el siguiente aspecto:

- Creación de un grupo de selección de ubicación por proximidad
- Implementación de su "máquina virtual de anclaje", normalmente el servidor DBMS, haciendo referencia a una determinada zona de disponibilidad de Azure
- Creación de un conjunto de disponibilidad que haga referencia al grupo de proximidad de Azure (consulte cómo hacerlo a continuación)
- Implementación de las máquinas virtuales del nivel de aplicación haciendo referencia al conjunto de disponibilidad y al grupo de selección de ubicación de proximidad

En lugar de implementar la primera máquina virtual como se mostró anteriormente, hace referencia a una zona de disponibilidad de Azure y al grupo de selección de ubicación de proximidad al implementar la máquina virtual, de la siguiente manera:

<pre><code>
New-AzVm -ResourceGroupName "myfirstppgexercise" -Name "myppganchorvm" -Location "westus2" -OpenPorts 80,3389 -Zone "1" -ProximityPlacementGroup "letsgetclose" -Size "Standard_E16_v3"
</code></pre>

Con una implementación correcta de esta máquina virtual que hospedaría la instancia de base de datos del sistema SAP en una zona de disponibilidad de Azure, el ámbito del grupo de selección de ubicación de proximidad se fija en uno de los centros de datos que representan la zona de disponibilidad definida.

Supondremos que implementa las máquinas virtuales de servicios centrales de la misma manera que las máquinas virtuales de DBMS, haciendo referencia a las mismas zonas que para las máquinas virtuales de DBMS y a los mismos grupos de selección de ubicación de proximidad. En el paso siguiente, debe crear los conjuntos de disponibilidad que desea usar para el nivel de aplicación de su sistema SAP.
Es necesario definir y crear el grupo de selección de ubicación de proximidad. El comando para crear el conjunto de disponibilidad requiere una referencia adicional al identificador del grupo de selección de ubicación de proximidad (no al nombre). Puede obtener el identificador del grupo de selección de ubicación de proximidad con:



<pre><code>
Get-AzProximityPlacementGroup -ResourceGroupName "myfirstppgexercise" -Name "letsgetclose"
</code></pre>

Al crear el conjunto de disponibilidad, debe tener en cuenta parámetros adicionales al usar discos administrados (valores predeterminados, a menos que se especifique lo contrario) y grupos de selección de ubicación de proximidad:

<pre><code>
New-AzAvailabilitySet -ResourceGroupName "myfirstppgexercise" -Name "myppgavset" -Location "westus2" -ProximityPlacementGroupId "/subscriptions/my very long ppg id string" -sku "aligned" -PlatformUpdateDomainCount 3 -PlatformFaultDomainCount 2 
</code></pre>

Idealmente, debe utilizar tres dominios de error. Sin embargo, el número de dominios de error admitidos puede variar de una región a otra. En este caso, el número máximo de dominios de error posibles para las regiones específicas era dos. Para implementar las máquinas virtuales del nivel de aplicación, debe agregar una referencia al nombre del conjunto de disponibilidad y el nombre del grupo de selección de ubicación de proximidad, como se muestra aquí:

<pre><code>
New-AzVm -ResourceGroupName "myfirstppgexercise" -Name "myppgavsetappvm" -Location "westus2" -OpenPorts 80,3389 -AvailabilitySetName "myppgavset" -ProximityPlacementGroup "letsgetclose" -Size "Standard_DS11_v2"
</code></pre>

El resultado final de esta secuencia será una capa de DBMS y los servicios centrales del sistema SAP que se encuentra en una zona de disponibilidad específica y un nivel de aplicación de SAP que se encuentra en varios conjuntos de disponibilidad en los mismos centros de datos de Azure en los que se implementaron las máquinas virtuales de DBMS.

> [!NOTE]
> A medida que implementa una máquina virtual de DBMS en una zona y una segunda máquina virtual de DBMS en otra zona para crear una configuración de alta disponibilidad, será necesario disponer de grupos de selección de ubicación de proximidad diferentes para cada una de las zonas. Igual ocurre con el conjunto de disponibilidad que pueda usar.

## <a name="get-an-existing-system-into-azure-proximity-placement-groups"></a>Colocación de un sistema existente en grupos de selección de ubicación de proximidad de Azure
Dado que ya tiene sistemas SAP implementados, es posible que desee optimizar la latencia de red de algunos de los sistemas críticos y colocar el nivel de aplicación y la capa de DBMS en el mismo centro de datos. En la fase de versión preliminar pública de la funcionalidad de grupos de selección de ubicación de proximidad, es necesario eliminar las máquinas virtuales y crear otras nuevas para realizar este traslado a los grupos de selección de ubicación de proximidad. En esta fase de la funcionalidad, no basta con apagar las máquinas virtuales para poder asignarlas a grupos de selección de ubicación de proximidad.


## <a name="next-steps"></a>Pasos siguientes
Consulte la documentación:

- [Lista de comprobación de planeamiento e implementación de cargas de trabajo de SAP en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-deployment-checklist)
- [Versión preliminar: Implementación de máquinas virtuales en grupos de selección de ubicación de proximidad con la CLI de Azure](https://docs.microsoft.com/azure/virtual-machines/linux/proximity-placement-groups)
- [Versión preliminar: Implementación de máquinas virtuales en grupos de selección de ubicación de proximidad con PowerShell](https://docs.microsoft.com/azure/virtual-machines/windows/proximity-placement-groups)
- [Consideraciones para la implementación de DBMS de Azure Virtual Machines para la carga de trabajo de SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general)

