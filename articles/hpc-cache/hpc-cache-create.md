---
title: Creación de una instancia de Azure HPC Cache
description: Cómo crear una instancia de Azure HPC Cache
author: ekpgh
ms.service: hpc-cache
ms.topic: tutorial
ms.date: 09/06/2019
ms.author: v-erkell
ms.openlocfilehash: e1b69f17d964647944f23f4d16a0a1a5f112b60d
ms.sourcegitcommit: 0fab4c4f2940e4c7b2ac5a93fcc52d2d5f7ff367
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2019
ms.locfileid: "71037067"
---
# <a name="create-an-azure-hpc-cache"></a>Creación de una instancia de Azure HPC Cache

Uso de Azure Portal para crear una memoria caché.

![captura de pantalla de información general de caché en Azure Portal, con el botón crear en la parte inferior](media/hpc-cache-home-page.png)

## <a name="define-basic-details"></a>Definición de los detalles básicos

![captura de pantalla de la página de detalles del proyecto en Azure Portal](media/hpc-cache-create-basics.png)

En **Detalles del proyecto**, seleccione la suscripción y el grupo de recursos que hospedará Azure HPC Cache. Asegúrese de que la suscripción está en la lista de [acceso de la versión preliminar](hpc-cache-prereqs.md#azure-subscription).

En **Detalles del servicio**, establezca el nombre de la memoria caché y estos otros atributos:

* Ubicación: seleccione alguna de las [regiones admitidas](hpc-cache-overview.md#region-availability).
* Red virtual: puede optar por crear una nueva red virtual o seleccionar una existente.
* Subred: elija o cree una subred con al menos 64 direcciones IP (/24) que solo se usarán para la instancia de Azure HPC Cache.

## <a name="set-cache-capacity"></a>Establecimiento de la capacidad de la memoria caché
<!-- change link in GUI -->

En la página **Caché**, debe establecer la capacidad de la instancia de Azure HPC Cache. Este valor determina la cantidad de datos que puede contener la memoria caché y la rapidez con la que puede atender las solicitudes de cliente. Después del período de versión preliminar pública, la capacidad también afectará al costo de la memoria caché.

La capacidad de la memoria caché se mide en operaciones de entrada/salida por segundo (IOPS). Para elegir la capacidad, establezca estos dos valores:

* La velocidad de transferencia de datos máxima para la memoria caché (rendimiento), en GB/segundo
* La cantidad de almacenamiento asignado a los datos en caché, en TB

Elija uno de los valores de rendimiento disponibles y los tamaños de almacenamiento en caché. La capacidad de IOPS se calcula y se muestra debajo de los selectores de valor.

Tenga en cuenta que la velocidad de transferencia de datos real depende de la carga de trabajo, las velocidades de red y el tipo de destinos de almacenamiento. Si un archivo no está en la memoria caché o está marcado como obsoleto, el servicio usará parte del rendimiento para recuperarlo del almacenamiento back-end. El valor que elija establece el rendimiento máximo para toda la memoria caché y no todo está disponible para las solicitudes de cliente.

Para el almacenamiento en caché, Azure HPC Cache administra los archivos que se almacenan en caché y se cargan previamente para maximizar las tasas de aciertos de caché. El contenido de la caché se evalúa continuamente y los archivos se mueven al almacenamiento a largo plazo cuando se accede a ellos con menos frecuencia. Elija un tamaño de almacenamiento en caché que pueda contener el conjunto activo de archivos de trabajo con espacio adicional para los metadatos y otras sobrecargas.

![captura de pantalla de la página de tamaño de caché](media/hpc-cache-create-iops.png)

## <a name="add-storage-targets"></a>Incorporación de destinos de almacenamiento

Los destinos de almacenamiento son el almacenamiento de back-end a largo plazo para el contenido de la memoria caché.

Puede definir los destinos de almacenamiento durante la creación de la memoria caché, pero también puede agregarlos después con el vínculo de la sección **Configurar** de la página de la memoria caché en el portal.

![captura de pantalla de la pestaña destinos de almacenamiento](media/hpc-cache-storage-targets-pop.png)

Haga clic en el **vínculo Agregar destino de almacenamiento** para definir los sistemas de almacenamiento de back-end. El almacenamiento puede estar en contenedores de blobs de Azure o en sistemas NFS locales.

Puede definir hasta diez destinos de almacenamiento diferentes.

Las instrucciones paso a paso para agregar un destino de almacenamiento se pueden encontrar en el artículo [Incorporación de almacenamiento](hpc-cache-add-storage.md). El procedimiento es diferente para las exportaciones de NFS o Blob Storage.

A continuación se incluyen algunas sugerencias: 

* Para ambos tipos de almacenamiento, debe especificar cómo buscar el sistema de almacenamiento de back-end (ya sea una dirección NFS o un nombre de contenedor de blobs) y la ruta de acceso del espacio de nombres orientado al cliente.

* Al crear un destino de almacenamiento de blobs, asegúrese de que la memoria caché tiene permisos de acceso a la cuenta de almacenamiento, tal y como se describe en [Incorporación de los roles de control de acceso](hpc-cache-add-storage.md#add-the-access-control-roles-to-your-account). Si no está seguro de que la configuración de roles se vaya a realizar correctamente, primero cree la memoria caché y después agregue el almacenamiento de blobs.

* Al crear un destino de almacenamiento NFS, especifique un [modelo de uso](hpc-cache-add-storage.md#choose-a-usage-model). La configuración del modelo de uso ayuda a la memoria caché a optimizar el flujo de trabajo.

## <a name="add-resource-tags-optional"></a>Incorporación de etiquetas de recursos (opcional)

La página **Etiquetas** permite agregar [etiquetas de recursos ](https://go.microsoft.com/fwlink/?linkid=873112) a Azure HPC Cache. 

## <a name="finish-creating-the-cache"></a>Terminar de crear la memoria caché

Después de configurar la nueva memoria caché, haga clic en la pestaña **Revisar y crear**. El portal valida las selecciones y le permite revisar las opciones. Si todo es correcto, haga clic en **Crear**. 

La creación de la memoria caché tarda unos 10 minutos. Puede realizar el seguimiento del progreso en las notificaciones de Azure Portal. 

![Captura de pantalla de las páginas de "implementación en curso" y "notificaciones" de creación de la memoria caché en el portal](media/hpc-cache-deploy-status.png)

Cuando finaliza la creación, aparece una notificación con un vínculo a la nueva instancia de Azure HPC Cache y la memoria caché aparece en la lista **Recursos** de la suscripción. 

![captura de pantalla de la instancia de Azure HPC Cache en Azure Portal](media/hpc-cache-new-overview.png)

## <a name="next-steps"></a>Pasos siguientes

Una vez que la memoria caché aparece en la lista **Recursos**, puede montarla para el acceso de cliente, utilizarla para trasladar los datos del espacio de trabajo a un nuevo destino de Azure Blob Storage o definir orígenes de datos adicionales.

* [Montaje de Azure HPC Cache](hpc-cache-mount.md)
* [Movimiento de datos a Azure Blob Storage para Azure HPC Cache](hpc-cache-ingest.md)
* [Incorporación de destinos de almacenamiento](hpc-cache-add-storage.md)
