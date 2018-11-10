---
title: Planear un sistema de Avere vFXT - Azure
description: Aquí se explica el plan que se debe realizar antes de implementar Avere vFXT para Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: f0e5523565dc561ed457dbc340835ad1889cb876
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50669872"
---
# <a name="plan-your-avere-vfxt-system"></a>Planear un sistema de Avere vFXT

En este artículo se explica cómo planear una nueva instancia de Avere vFXT para el clúster de Azure y así asegurarse de que el clúster creado tiene la posición y el tamaño adecuados para sus necesidades. 

Antes de ir a Azure Marketplace o crear las máquinas virtuales, piense en la manera en que el clúster interactuará con otros elementos de Azure. Planifique dónde se ubicarán los recursos de clúster en su red y subredes privadas, y decida dónde estará el almacenamiento de back-end. Asegúrese de que los nodos de clúster que cree sean lo suficientemente potentes para admitir su flujo de trabajo. 

Siga leyendo para obtener más información.

## <a name="resource-group-and-network-infrastructure"></a>Infraestructura de red y grupo de recursos

Piense dónde ubicará los elementos de su implementación de Avere vFXT para Azure. En el siguiente diagrama se muestra una posible disposición para los componentes de Avere vFXT para Azure:

![Diagrama que muestra el controlador y las máquinas virtuales del clúster dentro de una subred. En torno al límite de la subred hay un límite de red virtual. Igualmente, en la red virtual hay un hexágono que representa el punto de conexión del servicio de almacenamiento; tenga en cuenta que se conecta con una flecha discontinua a una instancia de Blob Storage que está fuera de la red virtual.](media/avere-vfxt-components-option.png)

Siga estas instrucciones cuando planifique la infraestructura de red de su sistema Avere vFXT:

* Todos los elementos se deben administrar con una nueva suscripción creada para la implementación de Avere vFXT. Esta estrategia simplifica el seguimiento del costo y la limpieza, y también le ayuda a dividir las cuotas de recursos. Debido a que Avere vFXT se usa con una gran cantidad de clientes, si aísla los clientes y el clúster en una sola suscripción protegerá otras cargas de trabajo críticas de posibles limitaciones de recursos durante el aprovisionamiento de clientes.

* Busque sus sistemas de procesamiento de clientes cerca del clúster de vFXT. El almacenamiento de back-end puede ser aún más remoto.  

* Para simplificar, ubique el clúster de vFXT y la máquina virtual del controlador de clúster en la misma red virtual (vnet) y en el mismo grupo de recursos. Recuerde que también deben usar la misma cuenta de almacenamiento. 

* El clúster debe estar ubicado en su propia subred para evitar conflictos de direcciones IP con los clientes o con los recursos del proceso. 

## <a name="ip-address-requirements"></a>Requisitos de las direcciones IP 

Asegúrese de que la subred del clúster tenga un rango de direcciones IP lo suficientemente grande como para admitir el clúster. 

El clúster de Avere vFXT usa las siguientes direcciones IP:

* Una dirección IP de gestión de clúster. Esta dirección puede moverse de un nodo a otro en el clúster, pero siempre está disponible para que pueda conectarse a la herramienta de configuración del panel de control de Avere.
* Para cada nodo del clúster:
  * Al menos una dirección IP orientada al cliente. (El *servidor virtual* del clúster se encarga de administrar todas las direcciones orientadas al cliente y puede moverlas entre nodos según sea necesario).
  * Una dirección IP para la comunicación del clúster.
  * Una dirección IP de la instancia (asignada a la máquina virtual).

Si usa Azure Blob Storage, también puede requerir direcciones IP de la red virtual de su clúster:  

* Una cuenta de Azure Blob Storage requiere al menos cinco direcciones IP. Tenga en cuenta este requisito si ubica Blob Storage en la misma red virtual que su clúster.
* Si usa una instancia de Azure Blob Storage que esté fuera de la red virtual del clúster, debe crear un punto de conexión de servicio de almacenamiento en la red virtual. Tenga en cuenta que este punto de conexión no usa una dirección IP.

Asimismo, tiene la opción de ubicar los recursos de red y Blob Storage (si lo va a usar) en diferentes grupos de recursos del clúster.

## <a name="vfxt-node-sizes"></a>Tamaños de nodos de vFXT 

Las máquinas virtuales que sirven como nodos de clúster determinan el rendimiento de la solicitud y la capacidad de almacenamiento de la caché. Puede elegir entre dos tipos de instancia, cada una con diferentes características de memoria, procesador y almacenamiento local. 

Cada nodo de vFXT será idéntico. Es decir, si crea un clúster de tres nodos, tendrá tres máquinas virtuales del mismo tipo y tamaño. 

| Tipo de instancia | vCPU | Memoria  | Almacenamiento local de SSD  | Discos de datos máx. | Rendimiento del disco sin almacenamiento en la caché | NIC (recuento) |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_D16s_v3 | 16  | 64 GiB  | 128 GB  | 32 | 25 600 IOPS <br/> 384 MBps | 8 000 MBps (8) |
| Standard_E32s_v3 | 32  | 256 GiB | 512 GB  | 32 | 51 200 IOPS <br/> 768 MBps | 16 000 MBps (8)  |

La caché de disco por nodo se puede configurar y puede oscilar entre los 1000 GB y los 8000 GB. 1 TB por nodo es el tamaño de caché recomendado para los nodos Standard_D16s_v3; además es recomendable que tenga 4 TB por nodo para los nodos Standard_E32s_v3.

Para obtener información adicional sobre estas máquinas virtuales, lea los siguientes documentos de Microsoft Azure:

* [Tamaños de máquina virtual de uso general](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general)
* [Tamaños de máquina virtual optimizada para memoria](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory)

## <a name="account-quota"></a>Cuotas de la cuenta

Asegúrese de que su suscripción tenga la capacidad de ejecutar el clúster de Avere vFXT, así como los sistemas de informática o cliente que vaya a usar. Lea [Cuota del clúster de vFXT](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster) para obtener más detalles.

## <a name="back-end-data-storage"></a>Almacenamiento de datos de back-end

Cuando no está en la caché, ¿el conjunto de trabajo se almacenará en un nuevo contenedor de blobs o en una nube o sistema de almacenamiento de hardware ya existente?

Si quiere usar Azure Blob Storage para el back-end, debe crear un nuevo contenedor como parte de la creación del clúster de vFXT. Use el script de implementación ``create-cloud-backed-container`` y proporcione la cuenta de almacenamiento para el nuevo contenedor de blobs. Esta opción crea y configura el nuevo contenedor de modo que esté listo para su uso tan pronto como el clúster esté listo. Lea [Creación de nodos y configuración del clúster](avere-vfxt-deploy.md#create-nodes-and-configure-the-cluster) para obtener más detalles.

> [!NOTE]
> Solo los contenedores de Blob Storage vacíos se pueden usar como archivadores principales del sistema Avere vFXT. vFXT debe poder administrar su almacén de objetos sin necesidad de guardar los datos existentes. 
>
> Lea [Moving data to the vFXT cluster](avere-vfxt-data-ingest.md) (Mover datos al clúster de vFXT) para aprender a copiar los datos en el nuevo contenedor del clúster de manera eficaz mediante el uso de máquinas cliente y la caché de Avere vFXT.

Si quiere usar un sistema de almacenamiento local existente, debe agregarlo al clúster de vFXT después de crearlo. El script de implementación ``create-minimal-cluster`` crea un clúster de vFXT sin almacenamiento de back-end. Lea [Configure storage](avere-vfxt-add-storage.md) (Configurar el almacenamiento) para obtener instrucciones detalladas sobre cómo agregar un sistema de almacenamiento existente al clúster de Avere vFXT. 

## <a name="next-step-understand-the-deployment-process"></a>Siguiente paso: comprender el proceso de implementación

La [información general de la implementación](avere-vfxt-deploy-overview.md) le proporciona una visión general de todos los pasos necesarios para crear una instancia Avere vFXT para el sistema Azure y tenerla lista para proporcionar datos.  