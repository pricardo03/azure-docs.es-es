---
title: Mover a Azure Virtual Machines de proceso del sistema central
description: Comparación de los recursos favorablemente a la capacidad de gran sistema de cálculo de Azure para que pueda migrar y modernizar aplicaciones z14 de IBM.
author: njray
ms.author: larryme
ms.date: 04/02/2019
ms.topic: article
ms.service: multiple
ms.openlocfilehash: 8aea4a74ba84855f011dada70ea75ec0d5fb64fe
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "58896522"
---
# <a name="move-mainframe-compute-to-azure"></a>Mover el proceso del sistema central a Azure

Grandes sistemas tienen una reputación de gran confiabilidad y disponibilidad y siguen siendo la red troncal de confianza de muchas empresas. Está a menudo pensaron que escalabilidad casi ilimitada y la capacidad de proceso. Sin embargo, algunas empresas hayan superado la capacidad de los mainframes disponibles más grandes. Si esto le suena, Azure ofrece ahorros de infraestructura, alcance y agilidad.

Para ejecutar cargas de trabajo de mainframes en Microsoft Azure, debe saber cómo su mainframe compute comparación de funcionalidades a Azure. En función de un gran sistema z14 de IBM (el modelo más reciente cuando se redactó este documento), este artículo explica cómo obtener resultados comparables en Azure.

Para comenzar, considere la posibilidad de los entornos en paralelo. La siguiente ilustración compara un entorno de mainframe para ejecutar aplicaciones en un entorno de hospedaje de Azure.

![Servicios de Azure y emulación entornos oferta comparable admiten y simplifica la migración](media/mainframe-compute-azure.png)

A menudo se usa la eficacia de mainframes sistemas (OLTP) que controlar millones de actualizaciones para miles de usuarios de procesamiento de transacciones en línea. Estas aplicaciones suelen usar software de procesamiento de transacciones, el control de pantalla y la entrada de formulario. Puede usar un Customer Information Control System (CICS), sistema de administración de información (IMS) o paquete de interfaz de transacciones (TIP).

Como se muestra en la ilustración, un emulador de TPM en Azure puede controlar las cargas de trabajo CICS e IMS. Un emulador de proceso por lotes del sistema en Azure realiza la función de lenguaje de Control de tareas (JCL). Bases de datos de Azure, como Azure SQL Database se migran datos de gran sistema. Servicios de Azure u otro software que se hospeda en Azure Virtual Machines puede usarse para la administración del sistema.

## <a name="mainframe-compute-at-a-glance"></a>Proceso del sistema central con un solo vistazo

En el gran sistema z14, los procesadores se organizan en hasta cuatro *alimentadores*. Un *cajón* es simplemente un clúster de procesadores y conjuntos de chips. Cada Gaveta puede tener seis chips de procesador central activo (CP), y cada CP tiene 10 chips de controlador (SC) del sistema. En la terminología de Intel x86, hay seis sockets por cuatro alimentadores, cajón y 10 núcleos por socket. Esta arquitectura proporciona un equivalente aproximado de 24 sockets y 240 núcleos, máximo, para un z14.

Rápido CP z14 tiene una velocidad de reloj de 5,2 GHz. Normalmente, se entrega un z14 con CPs en el cuadro. Se activaron según sea necesario. Normalmente, un cliente se cobra por al menos cuatro horas de tiempo de ejecución al mes a pesar del uso real.

Un procesador de mainframe puede configurarse como uno de los siguientes tipos:

- Procesador de (GP) de propósito general
- Procesador de información integrada del sistema z (zIIP)
- Instalación integrada para el procesador de Linux (IFL)
- Procesador del sistema de ayuda (SAP)
- Procesador de instalaciones de acoplamiento (ICF) integrado

## <a name="scaling-mainframe-compute-up-and-out"></a>Proceso de mainframe escalado vertical y horizontalmente

Grandes sistemas IBM ofrecen la posibilidad de escalar hasta 240 núcleos (el tamaño z14 actual para un único sistema). Además, IBM mainframes pueden escalar horizontalmente a través de una característica denominada acoplamiento Facility (CF). El CF permite múltiples sistemas (mainframes) tener acceso a los mismos datos simultáneamente. Usando el CF, los procesadores de gran sistema mainframe Parallel Sysplex tecnología grupos en los clústeres. Cuando se ha escrito esta guía, la característica de Parallel Sysplex admite 32 agrupaciones de 64 procesadores. Hasta 2048 procesadores se puede agrupar de esta manera para escalar horizontalmente la capacidad de proceso.

Los clústeres de cálculo compartir datos con acceso directo permite que una CF. Se utiliza para bloquear información, información de la caché y la lista de recursos de datos compartido. Un paralelo Sysplex mediante CFs uno o más puede considerarse como un "compartida todo" clúster de cálculo de escalabilidad horizontal. Para obtener más información acerca de estas características, consulte [Parallel Sysplex en IBM Z](https://www.ibm.com/it-infrastructure/z/technologies/parallel-sysplex-resources) en el sitio Web de IBM.

Las aplicaciones pueden usar estas características para ofrecer escalado horizontal del rendimiento y alta disponibilidad. Para obtener información acerca de cómo CICS puede utilizar Parallel Sysplex con CF, descargue el [IBM CICS y las instalaciones de acoplamiento: Más allá del simple](http://www.redbooks.ibm.com/redbooks/pdfs/sg248420.pdf) libro rojo.

## <a name="azure-compute-at-a-glance"></a>Proceso de Azure con un solo vistazo

Algunas personas pensar equivocadamente que los servidores basados en Intel no son tan eficaces como grandes sistemas. Sin embargo, los nuevos sistemas de densidad de núcleo, basados en Intel tienen como mucho la capacidad como grandes sistemas de proceso. Esta sección describen las opciones Azure de (IaaS) de infraestructura como servicio de proceso y almacenamiento. Azure ofrece opciones también de plataforma como servicio (PaaS), pero en este artículo se centra en las opciones de IaaS que proporcionan la capacidad de mainframe comparable.

Máquinas virtuales de Azure proporcionan potencia de proceso en un intervalo de tamaños y tipos. En Azure, una CPU virtual (vCPU) equivale aproximadamente a un núcleo en un gran sistema.

Actualmente, los tamaños de intervalo de máquina Virtual de Azure ofrece desde 1 a 128 vCPU. Tipos de máquinas virtuales (VM) están optimizados para cargas de trabajo concretas. Por ejemplo, la siguiente lista muestra los tipos de máquina virtual (actuales cuando se redactó este documento) y sus usos recomendados:

| Tamaño     | Tipo y descripción                                                                 |
|----------|--------------------------------------------------------------------------------------|
| Serie D | Uso general con 64 vCPU y un máximo de velocidad de reloj de 3,5 GHz                           |
| Serie E | Memoria optimizada con hasta 64 vCPU                                                 |
| Serie F | Proceso optimizado con hasta 64 vCPU y velocidad del reloj de 3..7 GHz                       |
| Serie H | Optimizado para aplicaciones de informática de alto rendimiento (HPC)                          |
| Serie L | Almacenamiento optimizado para aplicaciones de alto rendimiento respaldadas por bases de datos NoSQL |
| Serie M | Proceso más grande optimizadas para memoria y las máquinas virtuales con hasta 128 vCPU                        |

Para obtener más información acerca de máquinas virtuales disponibles, consulte [series de máquinas virtuales](https://azure.microsoft.com/pricing/details/virtual-machines/series/).

Un mainframe z14 puede tener hasta 240 núcleos. Sin embargo, mainframes z14 casi nunca usan todos los núcleos para una sola aplicación o carga de trabajo. En su lugar, un gran sistema aísla las cargas de trabajo en particiones lógicas (LPARs) y las LPARs tienen clasificaciones, MIPS (millones de instrucciones por segundo) o MSU (unidad de servicio millones). Al determinar el tamaño de máquina virtual comparable necesario para ejecutar una carga de trabajo de mainframes en Azure, clasificación de factor en MIPS (o MSU).

Los siguientes son estimaciones generales:

-   150 MIPS por vCPU

-   MIPS 1.000 por procesador

Para determinar el tamaño de máquina virtual correcto para una carga de trabajo en una LPAR, optimizar la carga de trabajo de la máquina virtual. A continuación, determine el número de vCPU que sea necesitado. Una estimación conservadora es 150 MIPS por vCPU. Según esta estimación, por ejemplo, una máquina virtual de la serie F con 16 vCPU podría admitir fácilmente una carga de trabajo de IBM Db2 procedentes de una LPAR con MIPS 2.400.

## <a name="azure-compute-scale-up"></a>Escalado de proceso de Azure

Las máquinas virtuales de la serie M pueden escalar hasta 128 vCPU (en el momento en que se escribió este artículo). Usar la estimación conservadora de MIPS 150 por vCPU, la máquina virtual de la serie M es igual a aproximadamente 19.000 MIPS. La regla general para estimar MIPS para un gran sistema es MIPS 1000 por cada procesador. Un mainframe z14 puede tener hasta 24 procesadores y proporcionar aproximadamente 24000 MIPS para un sistema central único.

El mainframe z14 único mayor tiene aproximadamente 5.000 MIPS más que la máquina virtual más grande disponible en Azure. Aún es importante comparar cómo se implementan las cargas de trabajo. Si un sistema (mainframe) tiene una aplicación y una base de datos relacional, normalmente se implementen en el mismo mainframe físico, cada uno en su propio LPAR. La misma solución en Azure a menudo se implementa mediante una máquina virtual para la aplicación y una VM independiente, el tamaño apropiado para la base de datos.

Por ejemplo, si un sistema de vCPU M64 es compatible con la aplicación y una vCPU m 96 se usa para la base de datos, se necesita aproximadamente 150 vCPU, o aproximadamente 24000 MIPS como se muestra en la figura siguiente.

![Comparación de las implementaciones de carga de trabajo de 24.000 MIPS](media/mainframe-compute-mips.png)

El enfoque es migrar LPARs a las máquinas virtuales individuales. A continuación, Azure fácilmente escala hasta el tamaño necesario para la mayoría de las aplicaciones que se implementa en un sistema central único.

## <a name="azure-compute-scale-out"></a>Ampliación del proceso de Azure

Una de las ventajas de una solución basada en Azure es la capacidad para escalar horizontalmente. Capacidad disponible para una aplicación de proceso de escalado hace casi ilimitada. Azure admite varios métodos para escalar horizontalmente la potencia de proceso:

- **Equilibrio de carga en un clúster.** En este escenario, una aplicación puede utilizar un [equilibrador de carga](/azure/load-balancer/load-balancer-overview) o de resource manager para distribuir la carga de trabajo entre varias máquinas virtuales en un clúster. Si se necesita capacidad más procesamiento, máquinas virtuales adicionales se agregan al clúster.

- **Conjuntos de escalado de máquina virtual.** En este escenario de ráfaga, una aplicación puede escalar adicional [recursos de proceso](/azure/virtual-machine-scale-sets/overview) según el uso de la máquina virtual. Cuando la demanda cae, el número de máquinas virtuales en un conjunto de escalado también puede ir hacia abajo, lo que garantiza un uso eficaz de la capacidad de proceso.

- **Escalado de PaaS.** Recursos de proceso de escalado de las ofertas de PaaS de Azure. Por ejemplo, [Azure Service Fabric](/azure/service-fabric/service-fabric-overview) asigna recursos de proceso para cumplir con aumentos en el volumen de solicitudes.

- **Clústeres de Kubernetes.** Las aplicaciones en Azure pueden usar [clústeres de Kubernetes](/azure/aks/concepts-clusters-workloads) para servicios de proceso para los recursos especificados. Azure Kubernetes Service (AKS) es un servicio administrado que organiza los nodos de Kubernetes, grupos y clústeres en Azure.

Para elegir el método adecuado para el escalado horizontal de los recursos de proceso, es importante entender cómo Azure y mainframes difieren. La clave es cómo, o si, comparten los recursos de proceso de datos. En Azure, datos (de forma predeterminada) no se comparten normalmente por varias máquinas virtuales. Si el uso compartido de datos requiere un clúster de proceso de varias máquinas virtuales en una implementación escalada, los datos compartidos deben residir en un recurso que admite esta funcionalidad. En Azure, uso compartido de datos implica almacenamiento tal como se describe en la sección siguiente.

## <a name="azure-compute-optimization"></a>Optimización del proceso de Azure

Puede optimizar cada nivel de procesamiento en una arquitectura de Azure. Utilice el tipo más apropiado de máquinas virtuales y las características para cada entorno. La siguiente ilustración muestra un patrón posibles para la implementación de máquinas virtuales en Azure para admitir una aplicación CICS que usa Db2. En el sitio principal, las máquinas virtuales de producción, preproducción y pruebas se implementan con alta disponibilidad. El sitio secundario es para recuperación ante desastres y copia de seguridad.

Cada nivel también puede proporcionar ante desastres adecuada los servicios de recuperación. Por ejemplo, las máquinas virtuales de producción y base de datos pueden requerir una recuperación activa o semiactiva, mientras que las máquinas virtuales de desarrollo y pruebas permiten una recuperación pasiva.

![Implementación de alta disponibilidad que admite la recuperación ante desastres](media/mainframe-compute-dr.png)

## <a name="next-steps"></a>Pasos siguientes

- [Migración del sistema central](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/overview)
- [Mainframe rehospedaje en Azure Virtual Machines](/azure/virtual-machines/workloads/mainframe-rehosting/overview)
- [Mover el almacenamiento de mainframe a Azure](mainframe-storage-Azure.md)

### <a name="ibm-resources"></a>Recursos de IBM

- [Sysplex paralela en IBM Z](https://www.ibm.com/it-infrastructure/z/technologies/parallel-sysplex-resources)
- [IBM CICS y las instalaciones de acoplamiento: Más allá de los conceptos básicos](http://www.redbooks.ibm.com/redbooks/pdfs/sg248420.pdf)
- [Creación de los usuarios necesarios para una instalación de Db2 pureScale Feature](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.qb.server.doc/doc/t0055374.html?pos=2)
- [Mandato db2icrt - Crear instancia](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.admin.cmd.doc/doc/r0002057.html)
- [PureScale Clustered solución de base de datos de Db2](http://www.ibmbigdatahub.com/blog/db2-purescale-clustered-database-solution-part-1)
- [IBM Data Studio](https://www.ibm.com/developerworks/downloads/im/data/index.html/)

### <a name="azure-government"></a>Azure Government

- [En la nube de Microsoft Azure Government para aplicaciones del sistema central](https://azure.microsoft.com/resources/microsoft-azure-government-cloud-for-mainframe-applications/)
- [Microsoft y FedRAMP](https://www.microsoft.com/TrustCenter/Compliance/FedRAMP)

### <a name="more-migration-resources"></a>Más recursos de migración

- [Platform Modernization Alliance: IBM Db2 en Azure](https://www.platformmodernization.org/pages/ibmdb2azure.aspx)
- [Azure Virtual Data Center Lift and Shift Guide](https://azure.microsoft.com/resources/azure-virtual-datacenter-lift-and-shift-guide/) (Centro de datos virtual de Azure: guía para una migración lift-and-shift)
- [iSCSI en GlusterFS](https://docs.gluster.org/en/latest/Administrator%20Guide/GlusterFS%20iSCSI/)
